# 第3章 Spring MVC的常用注解

**本章要点**

* @Controller注解
* @RequestMapping注解
* @RequestParam注解
* @PathVariable注解
* @RequestHeader注解
* @CookieValue注解
* @SessionAttributes注解
* @ModelAttribute注解
* 转换JSON数据
* 转换XML数据

Spring 从2.5版开始引入注解，用户可以使用@Controller、@RequestMapping、@RequestParam/@ModelAttribute等类似这样的注解。到目前为止，Spring的版本虽然发生了很大的变化，但注解的特性却一直延续下来，并不断扩展，让广大开发者的工作变得更轻松。这都离不开Annotation的强大作用，本章将重点讲解Spring MVC 4中常用的注解。

## 3.1 @Controller注解

org.springframework.stereotype.Controller注解类型用于指示Spring类的实例是一个控制器，使用@Controller注解的类不需要继续特定的父类或者实现特定的接口，相对之前的版本实现Controller接口变得更加简单。而且Controller接口的实现类智能处理一个单一请求动作，而@Controller注解的控制器可以支持同时处理多个请求动作，更加灵活。

@Controller用于标记一个类，使用它标记的类就是一个SpringMVC Controller对象，即一个控制器类。Spring使用扫描机制查找应用程序中所有基于注解的控制器类。分发处理器会扫描使用了该注解的类的方法，并检测该方法是否使用了@RequestMapping注解，而使用@RequestMapping注解的方法才是真正处理请求的处理器。为了保证Spring能找到控制器，需要完成两件事情：

* 在Spring MVC 的配置文件的头文件中引入spring-context。
* 使用`<context:component-scan/>`元素，该元素的功能为：启动包扫描功能，以便注册带有@Controller、@Service、@repository、@Component等注解的类成为Spring 的Bean。base-package属性指定了需要扫描的类包，类包及其递归子包中所有的类都会被处理。

配置文件如下所示

```xml
<!--spring可以自动去扫描base-pack下面的包或者子包下面的Java文件，如果扫描到有Spring的相关注解的类，则把这些内注册为Spring的bean-->
<context:component-scan base-package="org.fkit.controller"/>
```

应该讲所有控制器类都放在基本包下，并且指定扫描该包，即org.fkit.controller，而不应该指定扫描org.fkit包，以免Spring MVC扫描了无关的包。

**示例：@Controller注解的使用**

新建一个项目ControllerTest，加入所需的.jar文件，示例代码如下：

```java
package org.fkit.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloWorldController {
    @RequestMapping("/helloWorld")
    public String helloWorld(Model model){
        model.addAttribute("message","Hello World!");
        return "helloWorld";
    }
}
```

HelloWorldController是一个基于@Controller注解的控制器，@RequestMapping注释用来映射一个请求，value=“/helloWorld”表示请求由helloWorld方法进行处理。helloWorld方法接收一个org.springframework.ui.Model类型的参数，本例在model中添加了一个名为“message”的字符串对象，该对象可以在返回的视图当中通过request对象获取。最后，方法返回一个字符串“helloWorld”作为视图名称。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--spring可以自动去扫描base-pack下面的包或者子包下面的Java文件，如果扫描到有Spring的相关注解的类，则把这些内注册为Spring的bean-->
    <context:component-scan base-package="org.fkit.controller"/>
    <!--视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix">
            <value>/WEB-INF/content</value>
        </property>
        <!--后缀-->
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
</beans>
```

由于使用了注解类型，因此不需要再在配置文件中使用XML描述Bean。`<context:component-sacn base-package="org.fkit.controller"/>`指定需要Spring 扫描org.fkit.controller包及其子包下面的所有Java文件。最后配置了视图解析器InternalResourceViewResolver来解析视图，将View呈现给用户。视图解析器中配置的prefix属性表示视图的前缀，suffix表示视图的后缀，返回的视图字符串是”helloWorld”，经过视图解析器之后，则视图的完整路径为：/WEB-INF/content/helloWorld.jsp。需要注意的是，此处没有配置处理映射器和处理器适配器，当用户没有配置这两项时，Spring 会使用默认的处理映射器和处理器适配器处理请求。

此外，还需要在web.xml文件中配置Spring MVC的前端控制器DispatcherServlet，因为每次配置基本一致，故此处不再赘述，读者可自行配置。

部署ControllerTest这个Web应用，在浏览器中输入如下URL来测试应用：

```tiki wiki
http://localhost:8080/helloWorld
```

会看到如图3.1所示的界面，表示Spring MVC访问成功。

![1541404671383](assets/1541404671383.png)

Spring MVC中用于参数绑定的注解很多，都在org.springframework.web.bind.annotation包中，根据它们处理的request的不同内容部分可以分为四类（主要讲解常用类型）。

* **处理request body部分的注解：** @RequestParam、@RequestBody。
* **处理request uri部分的注解：** @PathVariable。
* **处理request header部分的注解：** @RequestHeader、@CookieValue。
* **处理attribute类型的注解：** @SessionAttributes、@ModelAttribute。

## 3.2 @RequestMapping注解

### 3.2.1 @RequestMapping注解

开发者需要在控制器内部为每一个请求动作开发相应的处理方法。org.springframework.web.bind.annotation.RequestMapping注解类型知识Spring用哪一个类或方法来处理请求动作，该注解可用于类或方法。

> **提示：**
>
> @RequestMapping虽然也在org.springframework.web.bind.annotation下面，但是严格来说，它并不属于参数绑定注解。

@RequestMapping可以用来注释一个控制器类，在这种情况下，所有方法都将映射为相对于类级别的请求，表示该控制器处理的所有请求都被映射到value属性所指示的路径下。示例代码如下：

```java
package org.fkit.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping(value = "/user")
public class UserController {
    @RequestMapping(value = "/register")
    public String register() {
        return "register";
    }

    @RequestMapping(value = "/login")
    public String login() {
        return "login";
    }
}
```

由于UserController类加了value=“/user”的@RequestMapping注解，因此所有相关路径都要加上“/user”，此时方法被映射到了如下请求URL：

```wiki
http://localhost:8080/user/register
http://localhost:8080/user/login
```

使用@RequestMapping注解可以指定如表3.1所示的属性

| 属性     | 类型            | 是否必要 | 说明                                                         |
| :------- | :-------------- | :------: | :----------------------------------------------------------- |
| value    | String[]        |    否    | 用于将指定请求的实际地址映射到方法上                         |
| name     | String          |    否    | 给映射地址指定一个别名                                       |
| method   | RequestMethod[] |    否    | 映射指定请求的方法类型，包括GET、POST、HEAD、OPTIONS、PUT、PATCH、DELETE、TRACE |
| consumes | String[]        |    否    | 指定处理请求的提交内容类型（Content-Type），例如application/json、text/html等 |
| produces | String[]        |    否    | 指定返回的内容类型，返回的内容类型必须是request请求头（Accept）中所包含的类型 |
| params   | String[]        |    否    | 指定request中必须包含某些参数值时，才让该方法处理            |
| headers  | String[]        |    否    | 指定request中必须包含某些指定的header值，才能让该方法处理请求 |
| Path     | String[]        |    否    | 在Servlet环境中只有:uri路径映射（例如”/myPath.do”）。也支持如ant的基于路径模式（例如“/myPath/*“）。在方法层面上，支持相对路径（例如”edit.do“） |

@RequestMapping注解支持的常用属性示例如下。

**1. value属性**

@RequestMapping用来映射一个请求和一种方法。可以使用@RequestMapping注释一个方法或类。

一个采用@RequestMapping注释的方法将成为一个请求处理方法，例如：

```java
@RequestMapping(value="/hello")
public ModelAndView hello(){
    return ...;
}
```

该示例使用@RequestMapping注释的value属性将URL映射到方法上。在这个例子中，将hello映射到hello方法上，使用如下URL访问应用时将由hello方法进行处理。

```wiki
http://localhost:8080/context/hello
```

由于value属性是@RequestMapping注释的默认属性，因此，如果只有唯一的属性，则可以省略属性名，即如下两个标注含义相同。

```wiki
@RequestMapping(value="/hello")
@RequestMapping("/hello")
```

但如果有超过一个属性，就必须写上value属性名称。

value属性的值也可以是一个空字符串，此时该方法被映射到如下请求URL：

```wiki
http://localhost:8080/context
```

**2. method属性**

该属性用来指示该方法仅仅处理哪些HTTP请求方式。

```java
@RequestMapping(value="/hello",method=RequestMethod.POST)
```

以上代码method=RequestMethod.POST表示该方法只支持POST请求。

也可以同时支持多个HTTP请求方式。如：

```java
@RequestMapping(value="/hello",method=[RequestMethod.POST,RequestMethod.GET])
```

如果没有指定method属性值，则请求处理方法可以处理任意HTTP请求方式。

**3. consumes属性**

该属性指定处理请求的提交内容类型（Content-Type）

```java
@RequestMapping(value="/hello",method=RequestMethod.POST,consumes="application/json")
```

表示方法仅处理request Content-Type为“application/json”类型的请求。

**4. produces属性**

该属性指定返回的内容类型，返回的内容类型必须是request请求头（Accept）中所包含的类型。

```java
@RequestMapping(value="/hello",method=RequestMethod.POST,produces="application/json")
```

方法仅处理request请求中Accept头中包含了“application/json”的请求，同时指明了返回的内容类型为application/json。

**5. params属性**

该属性指定request中必须包含某些参数值时，才让该方法处理。

```java
@RequestMapping(value="/hello",method=RequestMethod.POST,params="myParam=myValue")
```

方法仅处理其中名为“myParam”、值为”myValue“的请求。

**6. headers属性**

该属性指定request中必须包含某些指定的header值，才能让该方法处理请求。

```java
@RequestMapping(value="/hello",method=RequestMethod.POST,headers="Referer=http://www.fkit.org/")
```

方法仅处理request的header中包含了“Referer”请求头和对应值为“http://www.fkit.org/“的请求。

### 3.2.2 请求处理方法可出现的参数类型

每个请求处理方法可以有多个不同类型的参数。

如果需要访问HttpServletRequest对象，则可以添加HttpServletRequest作为参数，Spring会将对象正确地传递给方法：

```java
@RequestMapping(value = "/login")
public String login(HttpServletRequest request) {
    return "login";
}
```

如果需要访问HttpSession对象，则可以添加HttpSession作为参数，Spring会将对象正确地传递给方法：

```java
@RequestMapping(value = "/login")
public String login(HttpSession session) {
    return "login";
}
```

下面是可以在请求处理方法中出现的参数类型：

* javax.servlet.ServletRequest或javax.servlet.http.HttpServletRequest
* javax.servlet.ServletResponse或javax.servlet.http.HttpServletResponse
* javax.servlet.http.HttpSession
* org.springframework.web.context.request.WebRequest或org.springframework.web.context.request.NativeWebREquest
* java.util.Locale
* java.io.InputStream或Java.io.Reader
* java.io.OutputStream或Java.io.Writer
* java.security.Principal
* `HttpEntity<?>`
* java.util.Map
* org.springframework.ui.Model
* org.springframework.ui.ModelMap
* org.springframework.web.servlet.mvc.support.RedirectAttributes
* org.springframework.validation.Errors
* org.springframework.validation.BindingResult
* org.springframework.web.bind.support.SessionStatus
* org.springframework.web.util.UriComponentsBuilder
* @PathVariable、@@MatrixVariable注解
* @RequestParam、@RequestHeader、@RequestBody、@RequestPart注解

其中最终要的是org.springframework.ui.Model类型。这不是一个ServletAPI类型，而是一个Spring MVC类型，其中包含了Map对象用来存储数据。如果方法中添加了Model参数，则每次调用请求处理方法时，Spring MVC都会创建Model对象，并将其作为参数传递给方法。

### 3.2.3 请求处理方法可返回的类型

每个请求处理方法可以返回一下类型的返回结果。

* org.springframework.web.portlet.ModelAndView
* org.springframework.ui.Model
* `java.util.Map<k,v>`
* org.springframework.web.servlet.View
* java.lang.String
* HttpEntity或ResponseEntity
* java.util.concurrent.Callable
* org.springframework.web.context.request.async.DeferredREsult
* void

### 3.2.4 Model和ModelAndView

在请求处理方法可出现和返回的参数类型中，最重要的就是Model和ModelAndView了。对于MVC框架，控制器（Controller）执行业务逻辑，用于产生模型数据（Model），而视图（View）则用于渲染模型数据。

如何将模型数据传递给视图是Spring MVC框架的一项重要工作，Spring MVC提供了多种途径输出模型数据，如：

* Model和ModelMap
* ModelAndView
* @ModelAttribute
* @SessionAttributes

下面将重点介绍Model、ModelMap以及ModelAndView，@ModelAttribute和@SessionAttributes将在3.3节中重点介绍。

**1. Model和ModelMap**

Spring MVC在内部使用了一个org.springframework.ui.Model接口存储模型数据，它的功能类似java.util.Map接口，但是比Map易于使用。org.springframework.ui.ModelMap接口实现了Map接口。

Spring MVC在调用处理方法之前会创建一个隐含的模型对象，作为模型数据的存储容器。如果处理方法的参数为Model或ModelMap类型，则Spring MVC会将隐含模型的引用传递给这些参数。在处理方法内部，开发者就可以通过这个参数对象访问模型中的所有数据，也可以向模型中添加新的属性数据。

在处理方法中，Model和ModelMap对象都可以使用如下方法添加模型数据：

* addObject(String attributeName,Object attributeValue);

**示例：Model和ModelMap的使用**

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class User1Controller {
    private static final Log logger = LogFactory.getLog(UserController.class);

    //@ModelAttribute修饰的方法会先于login调用，该方法用于接收前台jsp页面传入的参数
    @ModelAttribute
    public void userModel(String loginname, String password, Model model) {
        logger.info("userModel");
        //创建User对象存储jsp页面传入的参数
        User user = new User();
        user.setLoginname(loginname);
        user.setPassword(password);
        //将User对象添加到Model当中
        model.addAttribute("user", user);
    }

    @RequestMapping(value = "/login1")
    public String login(Model model) {
        logger.info("login");
        //从Model当中取出之前传入的名为user的对象
        User user = (User) model.asMap().get("user");
        System.out.println(user);
        //设置user对象的username属性
        user.setUsername("测试");
        return "result1";
    }
}
```

@ModelAttribute修饰的userModel方法会先于login调用，它把请求参数赋给对应变量，可以向方法中的Model添加对象，前提是要在方法签名中加入一个Model类型的参数。

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.fkit.entity.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class User2Controller {
    private static final Log logger = LogFactory.getLog(User2Controller.class);

    //@ModelAttribute修饰的方法会先于login调用，该方法用于接收前台jsp页面传入的参数
    @ModelAttribute
    public void userModel(String loginname, String password, ModelMap modelMap) {
        logger.info("userMode2");
        //创建User对象存储jsp页面传入的参数
        User user = new User();
        user.setLoginname(loginname);
        user.setPassword(password);
        //将User对象添加到Model当中
        modelMap.addAttribute("user", user);
    }

    @RequestMapping(value = "/login2")
    public String login(ModelMap modelMap) {
        logger.info("login2");
        //从Model当中取出之前传入的名为user的对象
        User user = (User) modelMap.get("user");
        System.out.println(user);
        //设置user对象的username属性
        user.setUsername("测试");
        return "result2";
    }
}
```

User2Controller和User1Controller的代码功能基本一致，只是存储对象由Model改成了ModelMap。

**2. ModelAndView**

控制器处理方法的返回值如果是ModelAndView，则其既包含模型数据信息，也包含视图信息，这样Spring MVC将使用包含的视图对模型数据进行渲染。可以简单地将模型数据看成一个`Map<String,Object>`对象。

在处理方法中可以使用ModelAndView对象的如下方法添加模型数据：

```java
addObject(String attributeName,Object attributeValue);
```

可以通过如下方法设置视图：

```java
setViewName(String viewName)
```

**示例：ModelAndView的使用**

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.fkit.entity.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class User3Controller {
    private static final Log logger = LogFactory.getLog(User3Controller.class);

    @ModelAttribute
    public void userModel(String loginname, String password, ModelAndView mv) {
        logger.info("userMode3");
        User user = new User();
        user.setLoginname(loginname);
        user.setPassword(password);
        mv.addObject("user", user);
    }

    @RequestMapping(value = "/login3")
    public ModelAndView login(ModelAndView mv) {
        logger.info("login3");
        //从ModelAndView的Model当中取出之前存入的名为user的对象
        User user = (User) mv.getModel().get("user");
        System.out.println(user);
        //设置user对象的username属性
        user.setUsername("测试");
        //设置返回的视图名称
        mv.setViewName("result3");
        return mv;
    }
}
```

User3Controller和User1Controller的代码功能基本一致，只是存储对象改成了ModelAndView。

## 3.3 参数绑定注解

### 3.3.1 @RequestParam注解

org.springframework.web.bind.annotation.RequestParam注解类型用于将指定的请求参数赋值给方法中的形参。

使用@RequestParam注解可指定如表3.2所示的属性。

| 属性         | 类型    | 是否必要 | 说明                           |
| ------------ | ------- | -------- | ------------------------------ |
| name         | String  | 否       | 指定请求头绑定的名称           |
| value        | String  | 否       | name属性的名                   |
| required     | boolean | 否       | 指示参数是否必须绑定           |
| defaultValue | String  | 否       | 如果没有传递参数而使用的默认值 |

请求处理方法参数的可选类型为Java基本数据类型和String。示例代码如下：

```java
package org.fkit.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class UserController {
    @RequestMapping(value = "/login")
    public ModelAndView login(
            @RequestParam("loginname") String loginname,
            @RequestParam("password") String password) {
        return ……;
    }
}
```

假设请求如下：

```wiki
http://localhost:8080/context/login?loginname=jack&password=123456
```

以上代码会将请求中的loginname参数的值“jack”赋给loginname变量，password参数的值“123456”赋给password变量。

@RequestParam还有如下写法：

```java
@RequestParam(value="loginname",required=true,defaultValue="admin")
```

其中required参数不是必须的，默认为true。

> **参考：** [SpringMVC的各种参数绑定方式](http://www.shenhuanjie.com/2018/11/05/various-parametric-binding-methods-of-springmvc/)

**示例：@RequestMapping和@RequestParam注解的使用

新建一个项目RequestMappingTest，加入需要的jar文件，示例代码如下：

```java
package org.fkit.entity;

import java.io.Serializable;

public class User implements Serializable {
    //私有字段
    private String loginname;
    private String password;
    private String username;

    //公共构造器
    public User() {
        super();
    }

    //set/get方法
    public String getLoginname() {
        return loginname;
    }

    public void setLoginname(String loginname) {
        this.loginname = loginname;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

User是一个域对象，用来接收并封装前台页面传递过来的数据。

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.fkit.entity.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.ArrayList;
import java.util.List;

// Controller注解用于指示该类是一个控制器，可以同时处理多个请求
@Controller
// RequestMapping可以用来注释一个控制类，此时，所有方法都将映射为相对于类级别的请求
//表示该控制器处理的所有请求都被映射到value属性所指示的路径下
@RequestMapping(value = "/user")
public class UserController {
    // 静态List<User>集合，此处代替数据库用来保存注册的用户信息
    private static List<User> userList;

    // UserController类的构造器，初始化List<User>集合
    public UserController() {
        super();
        userList = new ArrayList<User>();
    }

    // 静态的日志类LogFactory
    private static final Log logger = LogFactory.getLog(UserController.class);

    // 该方法映射的请求为http://localhost:8080/RequestMappingTest/user/register，该
    // 方法支持GET请求
    @RequestMapping(value = "/register", method = RequestMethod.GET)
    public String registerForm() {
        logger.info("register GET 方法被调用……");
        // 跳转到注册页面
        return "/registerForm";
    }

    // 该方法映射的请求为http://localhost:8080/RequestMappingTest/user/register，该
    // 方法支持POST请求
    @RequestMapping(value = "/register", method = RequestMethod.POST)
    // 将请求中的loginname参数的值赋给loginname变量，password和username同样处理
    public String register(
            @RequestParam("loginname") String loginname,
            @RequestParam("password") String password,
            @RequestParam("username") String username) {
        logger.info("register POST 方法被调用……");
        // 创建User对象
        User user = new User();
        user.setLoginname(loginname);
        user.setPassword(password);
        user.setUsername(username);
        // 模拟数据库存储User信息
        userList.add(user);
        // 跳转到登录页面
        return "/loginForm";
    }

    // 该方法映射的请求为http://localhost:8080/RequestMappingTest/user/login
    @RequestMapping("/login")
    public String login(
            //将请求中的loginname参数的值赋给loginname变量，password同样处理
            @RequestParam("loginname") String loginname,
            @RequestParam("password") String password,
            Model model) {
        logger.info("登录名：" + loginname + " 密码：" + password);
        //到集合中查找用户是否存在，此处用来模拟数据库验证
        for (User user : userList) {
            if (user.getLoginname().equals(loginname) && user.getPassword().equals(password)) {
                model.addAttribute("user", user);
                return "/welcome";
            }
        }
        return "/loginForm";
    }

}
```

UserController类的代码解释如下：

1. UserController类使用了@Controller注解，是一个控制类。
2. UserController类上面使用了@RequestMapping(value=“/user”)注解，表示该控制器处理的所有请求都被映射到user路径下。
3. 本来没有使用数据库存储用户注册信息，所以定义了一个静态的List集合userList用来代替数据库存储用户数据。
4. registerForm方法使用了@RequestMapping(value=“/register”,method=RequestMethod.GET)注解，表示该方法映射的请求为http://localhost:8080/RequestMappingTest/user/register，并且只支持GET请求。该方法返回字符串“registerForm”，参考springmvc-config.xml中的配置信息，可以知道该方法只是跳转到registerForm.jsp注册页面。
5. register方法使用了@RequestMapping(value=“/register”,method=RequestMethod.POST)注解，表示该方法映射的请求为http://localhost:8080/RequestMappingTest/user/register，并且只支持POST请求。该方法使用@RequestParam注解将指定的请求参数赋值给方法中形参，之后创建了一个User对象保存用户传递的注册信息，最后将User对象存储到userList集合当中，之后的登录页面就可以到userList集合当中进行用户登录业务逻辑的判断。该方法返回字符串“loginForm”，并跳转到loginForm.jsp登录页面。
6. login方法使用了@RequestMapping(“/login”)注解，表示该方法映射的请求为http://localhost:8080/RequestMappingTest/user/login，这里没有设置method属性表示支持所有方式的请求。该方法也使用@RequestParam注解将指定的请求参数赋值给方法中的形参。之后到集合中查找用户是否存在，此处用来模拟数据库验证。login方法中还有一个参数Model对象，调用该对象的addAttribute方法可以将数据添加到request当中（关于Model对象的知识，在第4章重点介绍）。最后，如果用户登录成功则返回字符串“welcome”，并跳转到welcome.jsp欢迎页面；登录失败则返回字符串“loginForm”，并跳转到loginForm.jsp登录页面。

> **提示：**
>
> registerForm方法和register方法虽然映射的请求一样，但是registerForm方法支持的是GET请求，而register方法支持的是POST请求。

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>注册页面</title>
</head>
<body>
<h3>注册页面</h3>
<br>
<form action="/user/register" method="post">
    <table>
        <tr>
            <td><label for="loginname">登录名：</label></td>
            <td><input type="text" id="loginname" name="loginname"></td>
        </tr>
        <tr>
            <td><label for="password">密码：</label></td>
            <td><input type="password" id="password" name="password"></td>
        </tr>
        <tr>
            <td><label for="username">真实姓名：</label></td>
            <td><input type="text" id="username" name="username"></td>
        </tr>
        <tr>
            <td><input type="submit" id="submit" value="注册"></td>
        </tr>
    </table>
</form>
</body>
</html>
```

registerForm.jsp是一个注册页面，用户可以输入登录名、密码和真实姓名，该表单被提交到register请求。注意，这里使用的是POST方式，响应请求的是UserController类的register方法。

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>登录页面</title>
</head>
<body>
<h3>登录页面</h3>
<br>
<form action="/user/login" method="post">
    <table>
        <tr>
            <td><label for="loginname">登录名：</label></td>
            <td><input type="text" id="loginname" name="loginname"></td>
        </tr>
        <tr>
            <td><label for="password">密码：</label></td>
            <td><input type="password" id="password" name="password"></td>
        </tr>
        <tr>
            <td><input type="submit" id="submit" value="登录"></td>
        </tr>
    </table>
</form>
</body>
</html>
```

loginForm.jsp是一个登录页面，用户可以输入登录名和密码，该表单被提交到login请求。这里使用的是POST方式，响应请求的是UserController类的login方法。

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>欢迎</title>
</head>
<body>
<h3>欢迎[${requestScope.user.username}]登录</h3>
</body>
</html>
```

welcome.jsp是一个欢迎页面，用户登录成功后跳转到该页面，该页面使用EL表达式访问request当中的user对象的username属性。

此外，还需要在web.xml文件中配置Spring MVC的前端控制器DispatcherServlet，因为每次配置基本一致，故此处不再赘述，读者可自行配置。

部署RequestMappingTest这个Web应用，在浏览器中输入如下URL来测试应用：

```wiki
http://localhost:8080/user/register
```

> **提示：**
>
> 可能出现的中文乱码情况，需要在web.xml文件中增加编码过滤器
>
> ```xml
> <!-- 编码过滤器 -->
>     <filter>
>         <filter-name>characterEncodingFilter</filter-name>
>         <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
>         <init-param>
>             <param-name>encoding</param-name>
>             <param-value>UTF-8</param-value>
>         </init-param>
>         <init-param>
>             <param-name>forceEncoding</param-name>
>             <param-value>true</param-value>
>         </init-param>
>     </filter>
>     <filter-mapping>
>         <filter-name>characterEncodingFilter</filter-name>
>         <url-pattern>/*</url-pattern>
>     </filter-mapping>
> ```

可看到如图3.2所示的界面，表示Spring MVC成功跳转到注册界面registerForm.jsp。

输入登录名“test”，密码“123456”，真实姓名“测试用户”，单击“注册”按钮。请求将会被提交到UserController类的register方法进行注册，注册的用户信息会保存到UserController类的userList静态集合中。注册成功，将会跳转到如图3.3所示的登录界面。

输入登录名“test”，密码“123456”，单击“登录”按钮。请求将会被提交到UserController类的login方法进行登录验证，验证成功，将会跳转到如图3.4所示的欢迎界面。