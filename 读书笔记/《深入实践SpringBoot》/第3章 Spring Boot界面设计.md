# 第3章 Spring Boot界面设计

用Spring Boot框架设计Web显示界面，我们还是使用MVC（Model View Controller，模型 - 视图 - 控制器）的概念，将数据管理、事件控制和界面显示进行分层处理，实现多层结构设计。界面设计，即视图的设计，主要是组织和处理显示的内容，界面上的事件响应最终交给了控制器进行处理，由控制器决定是否调用模型进行数据的存取操作，然后再将结果返回给合适的视图显示。

本章的实例工程使用分模块管理，如表3-1所示，即将数据管理独立成为一个工程模块，专门负责数据库管理方面的功能。而界面设计模块主要负责控制器和视图设计方面的功能。

| 项目         | 工程  | 功能                |
| ------------ | ----- | ------------------- |
| 数据管理模块 | data  | 实现使用Neo4j数据库 |
| 界面设计模块 | webui | 控制器和视图设计    |

## 3.1 模型设计

数据管理模块实现了MVC中模型的设计，主要负责实体建模和数据库持久化等方面的功能。在本章的实例中，将使用上一章的Neo4j数据库的例子，对电影数据进行管理。回顾一下，有两个节点实体（电影和演员）和一个关系实体（角色）。其中，关系实体体现了节点实体之间的关系，即一个演员在一部电影中扮演一个角色。实体建模和持久化与上一章的实现差不多。只不过为了适应本章的内容，电影节点实体和角色关系实体的建模在属性上做了些许调整。另外针对Neo4j数据库的分页查询也做了一些调整和优化。

### 3.1.1 节点实体建模

如代码清单3-1所示，在电影节点实体建模中做了一些调整，即增加一个photo属性，用来存放电影剧照，并将关系类型更改为“扮演”。需要注意的是，Neo4j还没有日期格式的数据类型，所以在读取日期类型的数据时，使用注解@DateTimeFormat进行格式转换，而在保存时，使用注解@DateLong将它转换成Long类型的数据进行存储。

``` java
package com.spring.boot.data.domain;

import com.fasterxml.jackson.annotation.JsonIdentityInfo;
import com.voodoodyne.jackson.jsog.JSOGGenerator;
import org.neo4j.ogm.annotation.GraphId;
import org.neo4j.ogm.annotation.NodeEntity;
import org.neo4j.ogm.annotation.Relationship;
import org.neo4j.ogm.annotation.typeconversion.DateLong;
import org.springframework.format.annotation.DateTimeFormat;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * Movie
 *
 * @author shenhuanjie
 * @date 2019/5/13 16:45
 */
@JsonIdentityInfo(generator = JSOGGenerator.class)
@NodeEntity
public class Movie {
    @GraphId
    Long id;
    private String name;
    private String photo;
    @DateLong
    @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private Date createDate;

    @Relationship(type = "扮演", direction = Relationship.INCOMING)
    List<Role> roles = new ArrayList<>();

    public Role addRole(Actor actor, String name) {
        Role role = new Role(actor, this, name);
        this.roles.add(role);
        return role;
    }

    public Movie() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public List<Role> getRoles() {
        return roles;
    }

    public void setRoles(List<Role> roles) {
        this.roles = roles;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPhoto() {
        return photo;
    }

    public void setPhoto(String photo) {
        this.photo = photo;
    }

    public Date getCreateDate() {
        return createDate;
    }

    public void setCreateDate(Date createDate) {
        this.createDate = createDate;
    }
}
```

### 3.1.2 关系实体建模

电影实体对应的角色关系实体建模的关系类型也同样做了调整而改为“扮演”，如代码清单3-2所示。

```
package com.spring.boot.data.domain;

import com.fasterxml.jackson.annotation.JsonIdentityInfo;
import com.voodoodyne.jackson.jsog.JSOGGenerator;
import org.neo4j.ogm.annotation.EndNode;
import org.neo4j.ogm.annotation.GraphId;
import org.neo4j.ogm.annotation.RelationshipEntity;
import org.neo4j.ogm.annotation.StartNode;

/**
 * Role
 *
 * @author shenhuanjie
 * @date 2019/5/13 16:51
 */
@JsonIdentityInfo(generator = JSOGGenerator.class)
@RelationshipEntity(type = "扮演")
public class Role {
    @GraphId
    Long id;
    String name;
    @StartNode
    Actor actor;
    @EndNode
    Movie movie;

    public Role() {
    }

    public Role(Actor actor, Movie movie, String name) {
        this.actor = actor;
        this.movie = movie;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Actor getActor() {
        return actor;
    }

    public Movie getMovie() {
        return movie;
    }

}
```

### 3.1.3 分页查询设计

对于新型的Neo4j数据库来说，由于它的资源库遵循了JPA的规范标准来设计，在分页查询方面有的地方还不是很完善，所以在分页查询中，设计了一个服务类来处理，如代码清单3-3所示。其中，使用`Class<T>`传入调用的实体对象，使用Pageable传入页数设定和排序字段设定的参数，使用Filters传入查询的一些节点属性设定的参数。

```java
package com.spring.boot.data.service;

import org.neo4j.ogm.cypher.Filters;
import org.neo4j.ogm.cypher.query.Pagination;
import org.neo4j.ogm.cypher.query.SortOrder;
import org.neo4j.ogm.session.Session;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageImpl;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;

/**
 * PageService
 *
 * @author shenhuanjie
 * @date 2019/5/13 16:54
 */
@Service
public class PageService<T> {
    @Autowired
    private Session session;


    public Page<T> findAll(Class<T> clazz, Pageable pageable, Filters filters) {
        Collection data = this.session.loadAll(clazz, filters, convert(pageable.getSort()), new Pagination(pageable.getPageNumber(), pageable.getPageSize()), 1);
        return updatePage(pageable, new ArrayList(data));
    }

    private Page<T> updatePage(Pageable pageable, List<T> results) {
        int pageSize = pageable.getPageSize();
        int pageOffset = pageable.getOffset();
        int total = pageOffset + results.size() + (results.size() == pageSize ? pageSize : 0);
        return new PageImpl(results, pageable, (long) total);
    }

    private SortOrder convert(Sort sort) {
        SortOrder sortOrder = new SortOrder();
        if (sort != null) {
            Iterator var3 = sort.iterator();

            while (var3.hasNext()) {
                Sort.Order order = (Sort.Order) var3.next();
                if (order.isAscending()) {
                    sortOrder.add(new String[]{order.getProperty()});
                } else {
                    sortOrder.add(SortOrder.Direction.DESC, new String[]{order.getProperty()});
                }
            }
        }
        return sortOrder;
    }
}
```

## 3.2 控制器设计

怎样将视图上的操作与模型——数据管理模块联系起来，这中间始终是控制器在起着通信桥梁的作用，它响应视图上的操作事件，然后根据需要决定是否访问数据管理模块，最后再将结果返回给合适的视图，由视图处理显示。下面将按照电影控制器的设计来说明控制器中增删查改的实现方法，演员控制器的设计与此类似，不再赘述。

### 3.2.1 新建控制器

接收新建电影的请求，以及输入一部电影的数据后的最后提交，由新建控制器进行处理。在控制器上将执行两个操作，第一个操作将返回一个新建电影的视图，第二个操作接收界面中的输入数据，并调用数据管理模块进行保存，如代码清单3-4所示。其中，create函数将返回一个新建电影的视图，它不调用数据管理模块，save函数将需要保存的数据通过调用数据管理模块存储至数据库中，并返回一个成功标志。注意，为了简化设计，将电影剧照的图片文件做了预定义处理。

```java
@RequestMapping("/new")
public ModelAndView create(ModelMap model) {
    String[] files = {"/images/movie/西游记.jpg", "/images/movie/西游记续集.jpg"};
    model.addAttribute("files", files);
    return new ModelAndView("movie/new");
}

@RequestMapping(value = "/save", method = RequestMethod.POST)
public String save(Movie movie) throws Exception {
    movieRepository.save(movie);
    logger.info("新增->ID={}", movie.getId());
    return "1";
}
```

### 3.2.2 查看控制器

查看一个电影的详细信息时，控制器首先使用请求的电影ID向数据管理模块请求数据，然后将取得的数据输出到一个显示视图上，如代码清单3-5所示。

```java
@RequestMapping(value = "/{id}")
public ModelAndView show(ModelMap model, @PathVariable Long id) {
    Movie movie = movieRepository.findOne(id);
    model.addAttribute("movie", movie);
    return new ModelAndView("movie/show");
}
```

### 3.2.3 修改控制器

若要实现对电影的修改及保存操作，需要先将电影的数据展示在视图界面上，然后接收界面的操作，调用数据管理模块将更改的数据保存至数据库中，如代码清单3-6所示。其中，为了简化设计，将剧照中的图片文件和电影角色名称做了预定义处理。修改数据时，由于从界面传回的电影对象中，丢失了其角色关系的数据（这是OGM的缺点），所以再次查询一次数据库，以取得一个电影的完整数据，然后再执行修改的操作。

``` java
@RequestMapping(value = "/edit/{id}")
public ModelAndView update(ModelMap model, @PathVariable Long id) {
    Movie movie = movieRepository.findOne(id);
    String[] files = {"/images/movie/西游记.jpg", "/images/movie/西游记续集.jpg"};
    String[] rolelist = new String[]{"唐僧", "孙悟空", "猪八戒", "沙僧"};
    Iterable<Actor> actors = actorRepository.findAll();

    model.addAttribute("files", files);
    model.addAttribute("rolelist", rolelist);
    model.addAttribute("movie", movie);
    model.addAttribute("actors", actors);

    return new ModelAndView("movie/edit");
}

@RequestMapping(method = RequestMethod.POST, value = "/update")
public String update(Movie movie, HttpServletRequest request) throws Exception {
    String rolename = request.getParameter("rolename");
    String actorid = request.getParameter("actorid");

    Movie old = movieRepository.findOne(movie.getId());
    old.setName(movie.getName());
    old.setPhoto(movie.getPhoto());
    old.setCreateDate(movie.getCreateDate());

    if (!StringUtils.isEmpty(rolename) && !StringUtils.isEmpty(actorid)) {
        Actor actor = actorRepository.findOne(new Long(actorid));
        old.addRole(actor, rolename);
    }
    movieRepository.save(old);
    logger.info("修改->ID=" + old.getId());
    return "1";
}
```

### 3.2.4 删除控制器

删除电影时，从界面上接收电影的ID参数，然后调用数据管理模块将电影删除，如代码清单3-7所示。

```java
@RequestMapping(value = "/delete/{id}", method = RequestMethod.GET)
public String delete(@PathVariable Long id) throws Exception {
    Movie movie = movieRepository.findOne(id);
    movieRepository.delete(movie);
    logger.info("删除->ID=" + id);
    return "1";
}
```

### 3.2.5 分页查询控制器

列表数据的查询使用分页的方法，按提供的查询字段参数、页码、页大小及其排序字段等参数，通过调用数据管理模块进行查询，然后返回一个分页对象Page，如代码清单-8所示。这里的分页查询调用了3.1.3节定义的分页查询服务类。

```java
@RequestMapping(value = "/list")
public Page<Movie> list(HttpServletRequest request) throws Exception {
    String name = request.getParameter("name");
    String page = request.getParameter("page");
    String size = request.getParameter("size");
    Pageable pageable = new PageRequest(page == null ? 0 : Integer.parseInt(page), size == null ? 10 : Integer.parseInt(size),
            new Sort(Sort.Direction.DESC, "id"));

    Filters filters = new Filters();
    if (!StringUtils.isEmpty(name)) {
        Filter filter = new Filter("name", name);
        filters.add(filter);
    }

    return pageService.findAll(Movie.class, pageable, filters);
}
```

## 3.3 使用Thymeleaf模版

完成了模型和控制器的设计之后，接下来的工作就是视图设计了。在视图设计中主要使用Thymeleaf模版来实现。在进行视图设计之前，先了解一下Thymeleaf模版的功能。

Thymeleaf是一个优秀的面向Java的XML/XHTML/HTML 5页面模版，并具有丰富的标签语言和函数。使用Spring Boot 框架进行界面设计，一般都会选择Thymeleaf模版。

### 3.3.1 Thymeleaf配置

要使用Thymeleaf模版，首先，必须在工程的Maven管理中引入它的依赖：“spring-boot-starter-thymeleaf“，如代码清单3-9所示。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

其次，必须配置使用Thymeleaf模版的一些参数。在一般的Web项目中都会使用如代码清单3-10所示的配置，其中，prefix指定了HTML文件存放在webapp的/WEB-INF/views/目录下面，或者也可以指定其他路径，其他一些参数的设置其实是使用了Thymeleaf的默认配置。

在实例中，为了更方便将项目发布成jar文件，我们将使用Thymeleaf自动配置中的默认配置选项，即只要在资源文件夹resources中增加一个templates目录即可，这个目录用来存放HTML文件。
