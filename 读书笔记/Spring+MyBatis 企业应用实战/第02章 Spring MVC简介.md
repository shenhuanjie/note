# 第2章 Spring MVC简介

**本章要点**

* Model1和Model2
* MVC思想及其优点
* Spring MVC的优势
* Spring MVC的前端控制器DispatcherServlet
* 开发第一个Spring MVC应用
* 基于Controller接口的控制器
* 基于注解的控制器
* Spring MVC的工作流程

## 2.1 MVC思想概述

### 2.1.1 传统Model1和Modle2

Java Web应用的结构经理了Model1和Modle2两个时代，从Model1发展到Model2既是技术发展的必然，也是无数程序员的心血结晶。

在Model1模式下，整个Web应用几乎全部由JSP页面组成，JSP页面接收处理客户端请求，对请求处理后直接做出响应。用少量的JavaBean来处理数据库连接、数据库访问等操作。

Model1模式的实现比较简单，适用于快速开发小规模项目。但从工程化的角度看，它的局限性非常明显：JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常第，增加了应用的扩展和维护的难度。

早期由大量JSP页面所开发出来的Web应用，大都采用了Model1架构。实际上，早期绝大部分ASP应用也属于这种Model1架构。

Model2是基于MVC架构的设计模式。在Model2架构中，Servlet作为前端控制器，负责接收客户端发送的请求。在Servlet中只包含控制逻辑和简单的前端处理；然后，调用后端JavaBean来完成实际的逻辑处理；最后，将其转发到相应的JSP页面来处理显示逻辑。其具体的实现方式如图2.1所示。

正如在图2.1中看到的，Model2下的JSP不再承担控制器的责任，它仅仅是表现层角色，仅仅用于将结果呈现给用户。JSP页面的请求与Servlet（控制器）交互，而Servlet负责与后台的JavaBean通信。在Model2模式下，模型（Model）由JavaBean充当，视图（View）由JSP页面充当，而控制器（Controller）则由Servlet充当。

由于引入了MVC模式，使得Model2具有组件化的特点，从而更适用于大规模应用的开发，但也增加了应用开发的复杂程度。原本需要一个简单地JSP页面就能实现的应用，在Model2中被分解成多个协同工作的部分，程序员需要花更多的时间才能真正掌握其设计和实现过程。

Model2是MVC设计思想下的架构。下面简要介绍MVC设计思想的优势。

>**提示：** 
>
> 对于非常小型的Web站点，如果后期的更新、维护工作不是特别多，则可以使用Model1模式来开发应用，而不是Model2模式。虽然Model2提供了更好地可扩展性及可维护性，但其增加了前期开发成本。从某种程度上讲，Model2为了降低系统后期维护的复杂度，而导致前期开发的高复杂度。

### 2.1.2 MVC思想及其优势

MVC并不是Java所特有的设计思想，也并不是Web应用所特有的实现，它是所有面向对象程序设计语言都应该遵守的规范。

MVC思想将一个应用分成三个基本部分：Model（模型）、View（视图）和Controller（控制器），这三个部分以最少的耦合协同工作，从而提高应用的可扩展性及可维护性。

在经典的MVC模式中，事件由控制器处理，控制器根据事件的类型改变模型或视图，反之亦然。具体地说，每个模型对应一系列的视图列表，这种对应关系通常采用注册来完成，即把多个视图注册到同一个模型，当模型发生改变时，模型向所有注册过的视图发送通知，接下来，视图从对应的模型中获取信息，然后完成视图显示的更新。

从设计模式的角度来看，MVC思想非常类似于观察者模式，但其与观察者模式存在少许差别：在观察者模式下，观察者和被观察者可以是两个互相对等的对象；但在MVC中，被观察者往往只是单纯的数据体，而观察者这是单纯的视图页面。

概况起来，MVC有如下特点：

* 多个视图可以对应一个模型。按MVC设计模式，一个模型对应多个视图，可以减少代码的复制及代码的维护量，这样，一旦模型发生改变，也易于维护。
* 模型返回的数据与显示逻辑分离。模型数据可以应用任何的显示技术，例如，使用JSP页面、Velocity模板或者直接产生Excel文档等。
* 应用被分隔为三层，这降低了各层之间的耦合，提供了应用的可扩展性。
* 控制层的概念也很有效，由于它把不同的模型和不同的视图组合在一起，完全不同的请求。因此，控制层可以说包含了用户请求权限的概念。
* MVC更符合软件工程化管理的精神。不同层各司其职，每一层的组件具有相同的特征，这有利于通过工程化和工具化的方法产生管理程序代码。

相对于早期的MVC思想，Web模式下的MVC思想则又存在一些变化。对于一个普通应用程序，可以将视图注册给模型，但模型数据发生改变时，即时通知视图页面发生改变；而对于Web应用，即使将多个JSP页面注册给一个模型，但当模型发生变化时，模型也无法主动给JSP页面发送消息（因为Web应用都是基于请求/响应模式的），只有当用户请求浏览器该页面时，控制器才负责调用模型数据来更新JSP页面。

>**提示**
>
>MVC思想与观察者模式有一定的相似之处，但并不完全相同。经典的MVC思想与Web应用的MVC思想也存在一定的差别，引起差别的主要原因是因为Web应用是一种请求/响应模式下应用，对应请求/响应应用，如果用户不对应发出请求，视图将无法主动更新自己。

## 2.2 Struts2和Spring MVC

### 2.2.1 Spring MVC的优势

Spring框架提供了构建Web应用程序的全功能MVC模块——Spring MVC。Spring MVC框架提供了一个DispatcherServlet作用前端控制器来分派请求，同时提供灵活的配置处理程序映射、视图解析、语言环境和主题解析，并支持文件上传。Spring MVC还包含多种视图技术，例如Java Server Pages（JSP）、Velocity、Tiles、iText和POI等。Spring MVC分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

Spring MVC具有如下特点：

* Spring MVC拥有强大的灵活性、非侵入性和可配置性。
* Spring MVC提供一个前端控制器DispatcherServlet，开发者无须额外开发控制器对象。
* Spring MVC分工明确，包括控制器、验证器、命令对象、模型对象、处理程序映射视图解析器，等等，每一个功能实现由一个专门的对象负责完成。
* Spring MVC可以自动绑定用户输入，并正确地转换数据类型。例如，Spring MVC能自动解析字符串，并将其设置为模型的int或float类型的属性。
* Spring MVC使用一个名称/值的Map对象实现更加灵活的模型数据传输。
* Spring MVC内置了常见的校验器，可以校验用户输入，如果校验不通过，则重定向回输入表单。输入校验是可选的，并且支持编程方式及声明方式。
* Spring MVC支持国际化，支持根据用户区域显示多国语言，并且国际化的配置非常简单。
* Spring MVC支持多种视图技术，最常见的有JSP技术以及其他技术，包括Velocity和FreeMarker。
* Spring提供了一个简单而强大的JSP标签库，支持数据绑定功能，使得编写JSP页面更加容易。

## 2.3 开发第一个Spring MVC应用

本书成书之时，Spring的最新稳定版本是4.2.0，本书的代码都是基于该版本的。建议读者也下载该版本或者更高版本的Spring。

### 2.3.1 Spring的下载和安装

Spring是一个独立的框架，它不需要依赖于任何Web服务器或容器。它即可在独立的Java SE项目中使用，也可以在Java Web项目中使用。

下载和安装Spring框架可按如下步骤进行：

1. 登录`https://repo.spring.io/libs-release-local/`站点，该页面显示一个目录列表，读者沿着`org->springframework->spring`路径进入，即可看到Spring框架各版本的压缩包的下载链接。下载Spring的最新稳定版4.2.0。
2. 下载完成，得到一个`spring-framework-4.2.0.RELEASE-dist.zip`压缩文件，解压该压缩文件得到一个名为`spring-framework-4.2.0.RELEASE`的文件夹，该文件夹下有2如下几个子文件夹：
	
    * **docs。** 该文件夹下存放Spring的相关文档，包含开发指南、API参考文档。
    * **libs。** 该文件夹下的JAR分为三类：1. Spring 框架class文件的JAR包；2. Spring框架源文件的压缩包，文件名以-source结尾；3. Spring框架API文档的压缩包，文件名以-javadoc结尾。
    * **schemas。** 该文件夹下包含了Spring各种配置文件的XML Schema文档。
    * **readme.txt、notice.txt、license.txt** 等说明文档。
3. 将libs文件夹下所需模块的class文件的JAR包复制添加到项目的类加载路径中，既可通过环境变量的方式来添加，也可以使用Ant或IDE工具管理应用程序的类加载路径。如果需要发布该应用，则将这些JAR包一同发布即可。如果没有太多要求，建议将libs文件夹下所有模块的class文件的JAR包添加进去。
4. 除此之外，Spring的核心容器必须依赖于common-logging的JAR包，因此还应该登录`http://commons.apache.org/`站点，沿着`Releases->Logging`路径进入，下载最新的`commons-logging`工具，下载完成得到一个`commons-logging-1.2-bin.zip`压缩文件，将该文件解压路径下的`commons-logging-1.2.jar`也添加到项目的类加载路径中。

完成上面4个步骤后，接下来即可在Java Web应用中使用Spring MVC框架了。

### 2.3.2 Spring MVC的DispatcherServlet

在许多的MVC框架中，都包含一个用于调度控制的Servlet。Spring MVC也提供了一个名为`org.springframework.web.servlet.DispatcherServlet`的Servlet充当前端控制器，所有的请求驱动都围绕这个DispatcherServlet来分派请求。

DispatcherServlet是一个Servlet（它继承自HttpServlet基类），因此使用时需要把它配置在Web应用的部署描述符web.xml文件当中，配置信息如下：

```xml
    <servlet>
        <!--Servlet的名称-->
        <servlet-name>dispatcher</servlet-name>
        <!--Servlet对应的Java类-->
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <!--当前Servlet的参数信息-->
        <init-param>
            <!--contextConfigLocation是参数名称，该参数的值包含SpringMVC的配置文件路径-->
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/dispatcher-servlet.xml</param-value>
        </init-param>
        <!--在Web应用启动时立即加载Servlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--Servlet映射声明-->
    <servlet-mapping>
        <!--请求对应的Servlet的名称-->
        <servlet-name>dispatcher</servlet-name>
        <!--监听当前域的所有请求-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

以上是标注Java EE Servlet的配置。配置了一个DispatcherServlet，该Servlet在Web应用程序启动时立即加载，DispatcherServlet加载时会需要一个Spring MVC的配置文件，默认情况下，应用会去应用程序文件夹的WEB-INF下查找对应的`[servlet-name]-servlet.xml`文件，例如本例的`<servlet-name>`是springmvc，默认查找的就是`/WEB-INF/springmvc-servlet.xml`。

也可以把Spring MVC的配置文件放到应用程序文件夹下的任何地方，用servlet元素的init-param子元素进行描述，本例的param-name元素的值contextConfigLocation表示参数名称，param-value元素的值/WEB-INF/springmvc-config.xml文件则表示Spring MVC的配置文件路径和名称。则DispatcherServlet会查找/WEB-INF/springmvc-config.xml文件，作为Spring MVC的配置文件，解析该文件内容并根据文件配置信息创建一个WebApplicationContext容器对象，也称为上下文环境。WebApplicationContext继承自ApplicationContext容器，它的初始化方式和BeanFactory、ApplicationContext有所区别，因为WebApplicationContext需要ServletContext实例，也就是说，它必须在拥有Web容器的前提下才能完成启动Spring Web应用上下文的工作。有了WebApplicationContext容器，开发者就可以很自然地使用Spring的IOC、AOP等其他功能呢。

## 2.3.3 基于Controller接口的控制器

DispatcherServlet在Spring当中充当一个前端控制器的角色，它的核心功能是分发请求。请求会被分发给对应处理的Java类，Spring MVC中成为Handle。在Spring 2.5版本以前，开发一个Handle的唯一方法是实现`org.springframework.web.servlet.mvc.Controller`接口。Controller接口必须实现一个方法，该方法的签名如下：

```java
ModelAndView handleRequest(HttpServletRequest request,HttpServletResponse response)throws Exception
```

Controller接口的实现类可以通过handleRequest方法传递的参数访问对应请求的HttpServletRequest和HttpServletResponse对象，处理完业务请求之后，必须返回一个包含模型对象和视图路径的ModelAndView对象。

>**提示：**
>
>Controller接口的实现类只能处理一个单一请求动作，而Spring 2.5之后新增的基于注解的控制器可以支持同时处理多个请求动作，并且无须实现任何接口，其更加灵活，之后会详细介绍。

接下来我们演示一个基于Controller接口的Spring MVC控制器的Web应用，以便展示Spring MVC是如何工作的。

#### 示例：第一个Spring MVC应用

**1、增加Spring的支持**

首先，使用Eclipse新建一个Dynamic Web Project，也就是新建一个动态Web项目，命名为SpringMVCTest。

为了让Web应用具有Spring支持的功能，将`spring-framework-4.2.0.RELEASE`解压文件夹下的libs文件夹下所有Spring框架的class文件的JAR包和Spring所有依赖的commons-logging-1.2.jar复制到Web应用的lib文件夹下，也就是SpringMVCTest\WebContent\WEB-INF\lib路径下。

返回Eclipse主界面，此时在Eclipse主界面的左上角资源导航树中会看到SpringMVCTest结点，选中该结点，然后按F5键，将看到Eclipse主界面的左上角资源导航树中出现如图2.3所示的界面。

看到如图2.3所示的界面，即表明该Web应用已经加入了Spring的必需类库。但还需要修改web.xml文件，让该文件负责加载Spring框架。

**2、配置前端控制器DispatcherServlet**

在如图2.3所示的导航树中，单击WebContent->WEB-INF结点前的加号，展开该结点，会看到该结点下包含的web.xml文件子节点。

单击web.xml文件结点，编辑该文件，配置Spring MVC的前端控制器DispatcherServlet。配置信息如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--定义Spring MVC的前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/springmvc-config.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--让Spring MVC的前端控制器拦截所有请求-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```


web.xml文件的内容告诉Web容器，将使用Spring MVC的DispatcherServlet，并通过配置url-pattern元素的值为"/"，将所有URL映射到该Servlet。

**3、配置Spring MVC的Controller**

接下来是Spring MVC的配置文件，配置信息如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--配置Handle，映射"/hello"请求-->
<bean name="/hello" class="org.fkit.controller.HelloController"/>
<!--处理映射器将bean的name作为url进行查找，需要在配置Handle时指定name（即Url）-->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<!--SimpleControllerHandlerAdapter是一个处理器适配器，所有处理适配器都要实现HandlerAdapter接口-->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
<!--视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>
</beans>
```

springmvc-config.xml文件声明了HelloController业务控制器类，并将其映射到/hello请求。

此处还配置了一个处理映射器BeanNameUrlHandlerMapping，这样可以Bean的名称为url进行查找；同时配置了处理器适配器SimpleControllerHandlerAdapter，来完成对HelloController类的handleRequest方法的调用；最后配置了视图解析器InternalResourceViewResolver来解析视图，将View呈现给用户。需要注意的是，在Spring 4.0之后，如果不配置处理器映射器、处理器适配器和视图解析器，也会使用默认的完成Spring 内部MVC工作，笔者在此处显示配置处理过程，是希望读者能够了解Spring MVC的每一个动作，之后则可以更好地理解Spring MVC的工作流程。

**4、Controller类的实现**

HelloController类实现了Controller接口，用来处理/hello请求。示例代码如下：

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * HelloController是一个实现Controller接口的控制器，
 * 可以处理一个单一的请求动作
 */
public class HelloController implements Controller {

    private static final Log logger = LogFactory.getLog(HelloController.class);

    /**
     * handleRequest是Controller接口必须实现的方法。
     * 该方法的参数是对应请求的HttpServletRequest和HttpServletResponse。
     * 该方法必须返回一个包含视图名或视图名和模型的ModelAndView对象。
     *
     * @param httpServletRequest
     * @param httpServletResponse
     * @return
     */
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) {
        logger.info("handleRequest 被调用");
        // 创建准备返回的ModelAndView对象，该对象通常包含了返回视图名、模型的名称以及模型对象。
        ModelAndView modelAndView = new ModelAndView();
        // 添加模型数据，可以是任意的POJO对象
        modelAndView.addObject("message", "Hello World!");
        // 设置逻辑视图名，视图解析器会根据该名字解析到具体的视图页面
        modelAndView.setViewName("/WEB-INF/content/welcome.jsp");
        // 返回ModelAndView对象。
        return modelAndView;
    }
}
```

HelloController是一个实现Controller接口的控制器，它可以处理一个单一的请求动作。handleRequest是Controller接口必须实现的方法，Controller调用该方法来处理请求。该方法的参数是对应请求的HttpServletRequest和HttpServletresponse，该方法必须返回一个包含视图名或视图名和模型的ModelAndView对象。本例返回的模型中包含了一个名为message的字符串对象，返回的视图路径为/WEB-INF/content/welcome.jsp，因此，请求将被转发到welcome.jsp页面。

>**提示：**
>
>Spring MVC建议把所有的视图页面存放在WEB-INF文件夹下，这样可以保护视图页面，避免直接向视图页面发送请求。上面的HelloController类的handleRequest方法处理完请求后，Spring MVC会调用/WEB-INF/content文件夹下的welcome.jsp呈现视图。

**5、View页面**

SpringMVCTest包含一个视图页面welcome.jsp，用来显示欢迎信息。

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>
    </head>
    <body>
        ${message}
    </body>
</html>
```

此处的JSP页面使用了EL表达式来简化页面开发，关于EL表达式的使用可参考“附录A EL表达式和JSTL标签库”内容。

**6、测试应用**

使用Eclipse部署SpingMVCTest这个Web应用，在浏览器中输入如下URL来测试应用：

```xml
http://localhost:8080/hello
```

将会看到如图2.4所示的界面。表示Spring MVC访问成功。

>**提示：**
>
>使用MVC框架就应该严格遵守MVC思想。MVC框架不赞成浏览器直接访问Web应用的视图页面，用户的所有请求都只应向控制器发送，由控制器调用模型组件、视图组件向用户呈现数据。

#### 示例：基于注解的控制器

Spring 2.5版本开始新增了可基于注解的控制器，也就是说控制器不用实现Controller接口，通过注释类型来描述。下面将SpringMVCTest这个Web应用进行修改，演示一个基于注解的控制器Spring MVC的Web应用。

新建一个Dynamic Web Project，也就是新建一个动态项目，命名为AnnotationTest。所有步骤和2.3.3小节的“第一个Spring MVC应用”示例一样，只是修改两个地方。

**1、Controller类的实现**

HelloController类不需要Controller接口，改为使用注解类型来描述，用来处理/hello请求。示例代码如下：

```java
package org.fkit.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

/**
 * HelloController是一个基于注解的控制器
 * 可以同时处理多个请求动作，并且无须实现任何接口
 * org.springframework.stereotype.Controller注解用于指示该类是一个控制器
 */
@Controller
public class HelloController {
    private static final Log logger = LogFactory.getLog(HelloController.class);

    /**
     * org.springframework.web.bind.annotation.RequestMapping注解
     * 用来映射请求的URL和请求的方法等。本例用来映射"/hello"
     * hello只是一个普通方法。
     * 该方法返回一个包含视图名或视图名和模型的ModelAndView对象。
     *
     * @return
     */
    @RequestMapping(value = "/hello")
    public ModelAndView hello() {
        logger.info("hello方法，被调用");
        //创建准备返回的ModelAndView对象，该对象通常包含了返回视图名、模型的名称以及模型对象
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据，可以是任意的POJO对象
        modelAndView.addObject("message", "Hello World！");
        //设置逻辑视图名，视图解析器会根据该名字解析到具体的视图界面
        modelAndView.setViewName("/WEB-INF/content/welcome.jsp");
        //返回ModelAndView对象。
        return modelAndView;
    }
}
```

HelloController是一个基于注解的控制器，org.springframework.stereotype.Controller注解类型用于指示Spring类的实例是一个控制器。org.springframework.web.bind.annotation.RequestMapping注释类型用来映射一个请求和请求的方法，value="/hello"表示请求由hello方法进行处理。方法返回一个包含视图名和视图名和模型的ModelAndView对象，这和2.3.4节中的示例一样。

**2、修改Spring MVC的配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--spring可以自动去扫描base-pack下面的包或者子包下面的Java文件，如果扫描到有Spring的相关注解的类，则把这些内注册为Spring的bean-->
    <context:component-scan base-package="org.fkit.controller"/>
    <!--配置annotation类型的处理映射器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <!--配置annotation类型的处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>
</beans>
```

由于使用了注解类型，因此不需要再在配置文件中使用XML描述Bean。Spring使用扫描机制查找应用程序中所有基于注解的控制器类。`<context:component-scan base-package="org.fkit.controller"/>`指定需要Spring扫描org.fkit.controller包及其子包下面的所有Java文件。

此处还配置了一个annotation类型的处理器映射器RequestMappingHandlerMapping，它根据请求查找映射；同时配置了annotation类型的处理器适配器RequestMappingHandlerAdapter，来完成对HelloController类的@RequestMapping标注方法的调用；最后配置了视图解析器InternalResourceViewResolver来解析视图，将View呈现给用户。需要注意的是，在Spring 4.0之后，处理映射器、处理器适配器的配置还可以使用更简单地方式，笔者在此处显示配置处理过程，是希望读者能够了解Spring MVC的每一个动作，之后则可以更好地理解Spring MVC的工作流程。

**3、测试**

使用Eclipse部署AnnotationTest这个Web应用，在浏览器中输入如下URL来测试应用：

```html
http://localhost:8080/hello
```
会看到如图2.4所示的界面，表示Spring MVC访问成功。

## 2.4 详解DispatcherServlet

2.3节中的第一个Spring MVC应用已经成功运行了。那么，前端控制器DispatcherServlet截获请求后做了什么工作呢？DispatcherServlet又是如何分派请求的呢？

分析DispatcherServlet的源代码如下：

```java    
	protected void initStrategies(ApplicationContext context){
        initMultipartResolver(context);//初始化上传文件解析器
        initLocalResolver(context);//初始化本地化解析器
        initThemeResolver(context);//初始化主题解析器
        initHandleMappings(context);//初始化处理器映射器，将请求映射到处理器
        initHandlerAdapters(context);//初始化处理器适配器
        initHandlerExceptionResolvers(context);//初始化处理器异常解析器，如果执行过程中遇到异常将交给HandlerExceptionResolver来解析
        initREquestToViewnameTranslator(context);//初始化请求到视图名称解析器
        initViewResolvers(context);//初始化视图解析器，通过ViewResolver解析逻辑视图名到具体视图实现
        initFlashMapmanager(context);//初始化flash映射管理器
    }
```

>**提示**
>
>org/springframework/web/servlet/DispatcherServlet是Spring框架的源代码，读者可在配套资源中找到Spring源代码或自行下载。

initStrategies方法将在WebApplicationContext初始化后自动执行，自动扫描上下文的Bean，根据名称或类型匹配的机制查找自定义的组件，如果没有找到则会装配一套Spring的默认组件。在org.springframework.web.servlet路径下有一个DispatcherServlet.properties配置文件，该文件制定了DispatcherServlet所使用的默认组件。

```xml
# Default implementation classes for DispatcherServlet's strategy interfaces.
# Used as fallback when no matching beans are found in the DispatcherServlet context.
# Not meant to be customized by application developers.

# 本地化解析器
org.springframework.web.servlet.LocaleResolver=org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver
# 主题解析器
org.springframework.web.servlet.ThemeResolver=org.springframework.web.servlet.theme.FixedThemeResolver
# 处理器映射器（共2个）
org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
	org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping
# 处理器适配器（共3个）
org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
	org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
	org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter
# 异常处理器（共3个）
org.springframework.web.servlet.HandlerExceptionResolver=org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerExceptionResolver,\
	org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver,\
	org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver
# 视图名称解析器
org.springframework.web.servlet.RequestToViewNameTranslator=org.springframework.web.servlet.view.DefaultRequestToViewNameTranslator
# 视图解析器
org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver
# FlashMap映射管理器
org.springframework.web.servlet.FlashMapManager=org.springframework.web.servlet.support.SessionFlashMapManager
```

如果开发者希望使用自定义类型的组件，则只需要在Spring配置文件中配置自定义的Bean组件即可。Spring MVC如果发现上下文中用户指定义的组件，就不会使用默认的组件。

以下是DispatcherServlet装配每种组件的细节。

**本地化解析器。** 只允许一个实例

* （1）查找名为localeResolver、类型为LocaleResolver的Bean作为该类型组件。
* （2）如果没有找到，则使用默认的实现类AcceptHeaderLocaleResolver作为该类型组件。

**主题解析器。** 只允许一个实例

* （1）查找名为themeResolver、类型为ThemeResolver的Bean作为该类型组件。
* （2）如果没有找到，则使用默认的实现类FixedThemeResolver作为该类型组件。

**处理器映射器。** 允许多个实例

* （1）如果detectAllHandlerMappings的属性为true（默认为true），则根据类型匹配机制查找上下文以及Spring容器中所有类型为HandlerMapping的Bean，将它们作为该类型组件。
* （2）如果detectAllHandlerMappings的属性为false，则查找名为handlerMapping、类型为HandlerMapping的Bean作为该类型组件。
* （3）如果通过以上两种方式都没有找到，则使用BeannameUrlHandlerMapping实现类创建该类型的组件。

**处理器适配器。** 允许多个实例

* （1）如果detectAllHandlerAdapters的属性为true（默认为true），则根据类型匹配机制查找上下文以及Spring容器中所有类型为HandlerAdapter的Bean，将它们作为该类型组件。
* （2）如果detectAllHandlerAdapters的属性为false，则查找名为handlerAdapter、类型为HandlerAdapter的Bean作为该类型组件。
* （3）如果通过以上两种方式都没有找到，则使用DispatcherServlet.properties配置文件中指定的三个实现类分别创建一个适配器，并将其添加到适配器列表中。

**处理器异常解析器。** 允许多个实例

* （1）如果detectAllHandlerExceptionResolvers的属性为true（默认为true），则根据类型匹配机制查找上下文及Spring容器中所有类型为HandlerExceptionResolver的Bean，将它们作为该类型组件。
* （2）如果odetectAllHandlerExceptionResolvers的属性为false，则查找名为handlerExceptionResolver、类型为HandlerExceptionResolver的Bean作为该类型组件。
* （3）如果通过以上两种方式都没有找到，则查找DispatcherServlet.properties配置文件中定义的默认实现类，注意，该文件中没有对应处理器异常解析器的默认实现类，用户可以自定义处理器异常解析器的实现类，将之添加到DispatcherServlet.properties配置文件当中。

**视图名称解析器。** 只允许一个实例

* （1）查找名为viewNameTranslator、类型为RequestToViewnameTranslator的Bean作为该类型组件。
* （2）如果没有找到，则使用默认的实现类DefaultRequestToViewNameTranslator作为该类型的组件。

**视图解析器。** 允许多个实例

* （1）如果detectAllViewResolvers的属性为true（默认为true），则根据类型匹配机制查找上下文以及Spring容器中所有类型为ViewResolver的Bean，将它们作为该类型组件。
* （2）如果detectAllViewResolvers的属性为false，则查找名为viewResolvers、类型为ViewResolver的Bean作为该类型组件。
* （3）如果通过以上两种方式都没有找到，则查找DispatcherServlet.properties配置文件中定义的默认实现类InternalResourceViewResolver作为该类型的组件。

**文件上传解析器。** 只允许一个实例

* （1）查找名为muliipartResolver、类型为MuliipartResolver的Bean作为该类型组件。
* （2）如果用户没有在上下文中显式定义MuliipartResolver类型的组件，则DispatcherServlet将不会加载该类型的组件。

**FlashMap映射管理器。**

* （1）查找名为FlashMapManager、类型为SessionFlashMapManager的Bean作为该类型组件，用于管理FlashMap，即数据默认存储在HttpSession中。

DispatcherServlet装配的各种组件，有些只允许一个实例，比如文件上传解析器MuliipartResolver、本地化解析器LocalResolver等；有些则允许多个实例，如处理映射器HandlerMapping、处理器适配器HandlerAdapter等，读者需要注意这一点。

如果同一类型的组件存在多个，那么它们之间的优先级顺序如何确定呢？因为这些组件实现了org.springframework.core.Ordered接口，所以可以通过Order属性确定优先级的顺序，值越小的优先级越高。

## 2.5 Spring MVC执行的流程

下面将对开发Spring MVC应用的过程进行总结，以期让读者对Spring MVC有一个大致的了解。

### 2.5.1 Spring MVC应用的开发步骤

下面简单介绍Spring MVC 应用的开发步骤。

1、在web.xml文件中定义前端控制器DispatcherServlet来拦截用户请求。

由于Web应用是基于请求/响应架构的应用，所以不管哪个MVC Web框架，都需要在web.xml中配置该框架的核心Servlet或Filter，这样才可以让该框架介入Web应用中。

例如，开发Spring MVC应用的第1步就是在web.xml文件中增加如下配置片段：

```xml
	<servlet>
        <!--Servlet的名称-->
        <servlet-name>dispatcher</servlet-name>
        <!--Servlet对应的Java类-->
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <!--当前Servlet的参数信息-->
        <init-param>
            <!--contextConfigLocation是参数名称，该参数的值包含SpringMVC的配置文件路径-->
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/dispatcher-servlet.xml</param-value>
        </init-param>
        <!--在Web应用启动时立即加载Servlet-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--Servlet映射声明-->
    <servlet-mapping>
        <!--请求对应的Servlet的名称-->
        <servlet-name>dispatcher</servlet-name>
        <!--监听当前域的所有请求-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

2、如果需要以POST方式提交请求，则定义包含表单数据的JSP页面。如果仅仅只是以GET方式发送请求，则无须经过这一步。

3、定义处理用户请求的Handle类，可以实现Controller接口或使用@Controller注解。

这一步也是所有MVC框架中必不可少的，因为这个DispatcherServlet就是MVC中的C，也就是前端控制器，该控制器负责接收请求，并将请求分发给对应的Handle，即实现Controller接口的Java类，而该Java类负责调用后台业务逻辑代码来处理请求。

>**提示：**
>
>可能有读者会产生疑问：Controller并未接收到用户请求，它怎么能够处理用户的请求呢？MVC框架的底层机制是：前端Servlet接收到用户请求后，通常会对用户请求进行简单预处理，例如解析、封装参数等，然后通过反射来创建Controller实例，并调用Controller的指定方法（实现Controller接口的是handleRequest方法，而使用基于注解的控制器可以是任意方法）来处理用户请求。
>
>这里又产生一个问题：当Servlet拦截用户请求后，它如何知道创建哪个Controller接口的实例呢？有两种解决方案。
>
>利用xml配置文件：例如在xml配置文件中描述hello请求对应使用HelloController类。这就可以让MVC框架知道创建那个Controller接口的实例。
>
>利用注解：例如使用注解@Controller描述一个类，并使用注解@RequestMapping(value="/hello“）描述hello请求对应的方法。这样也可以让MVC框架知道创建哪个Controller接口的实例并调用哪个方法处理请求。

根据上面的介绍不难发现，在Spring MVC框架中，控制器实际上由两个部分共同组成，即拦截所有用户请求和处理请求的通用代码都由前端控制器DispatcherServlet完成，而实际的业务控制（诸如调用后台业务逻辑代码，返回处理结果等）则由Controller处理。

4、配置Handle。Java领域的绝大部分MVC框架都非常喜欢使用xml文件来进行配置管理，则在以前是一种思维定势。即配置哪个请求对应哪个Controller进行处理，从而让前端控制器根据该配置来创建合适的Controller实例，并调用该Controller的业务控制方法。

例如，可以采用如下代码片段来配置Handle：

```xml
<!--配置Handle，映射"/hello"请求-->
<bean name="/hello" class="org.fkit.controller.HelloController"/>
```

在Spring 2.5之后，推荐使用注解来配置Handle：

```java
@Controller
public class HelloController{
@RequestMapping(value="/hello")
	public ModelAndView hello(){
    	……
    }
}
```

上面的配置片段指定如果用户请求URL为hello，则使用org.fkit.controller.HelloController来处理。现在几乎所有的MVC框架都使用”约定优于配置“的思想，也就是采用约定方式来规定用户请求地址和Handle之间的对应关系。

5、编写视图资源

当Handle处理用户请求结束后，通常会返回一个ModelAndView对象，该对象中应该包含返回的视图名或视图名和模型，这个视图名就代表需要显示的物理视图资源。如果Handle需要把一些数据传给视图资源，则可以通过模型对象。

经过上面5个步骤，即可基本完成一个Spring MVC处理流程的开发，也就是可以执行一次完整的请求->响应过程。

### 2.5.2 Spring MVC执行的流程

上一节所介绍的Spring MVC应用的开发流程实际上是按请求->响应的流程来开发的，下面通过一个流程图来介绍请求->响应的完整流程。图2.5显示了一次请求->响应的完整流程。

按照图2.5中所标识的序号，Spring MVC请求->响应的完整工作流程如下：

1. 用户向服务器发送请求，请求被Spring的前端控制器DispatcherServlet截获。
2. DispatcherServlet对请求URL（统一资源定位符）进行解析，得到URI（请求资源标识符）。然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关对象，包括Handler对象以及Handler对象对应的拦截器，这些对象会被封装到一个HandlerExeCutionChain对象当中返回。
3. DIspatcherServlet根据获得的Handler，选择一个合适的HandlerAdapter。HandlerAdapter的设计符合面向对象中的单一职责原则，代码架构清晰，便于维护，最重要的是，代码可服用性高。HandlerAdapter会被用于处理多种Handler，调用Handler实际处理请求的方法，例如hello方法。
4. 提取请求中的模型数据，开始执行Handler（Controller）。在填充Handler的入参过程中，根据配置，Spring将帮你做一些额外的工作。
   * **消息转换。** 将请求（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息。
   * **数据转换。** 对请求消息进行数据格式化，如String转换成Integer、Double等。
   * **数据格式化。** 对请求消息进行数据格式化，如将字符串转换成格式化数字或格式化日期等。
   * **数据验证。** 验证数据的有效性（长度、格式等），验证结果存储在BindingResult或Error中。
5. Handler执行完成后，先DispatcherServlet返回一个ModelAndView对象，ModelAndView对象中应该包含视图名或视图名和模型。
6. 根据返回的ModelAndView对象，选择一个合适的ViewResolver（视图解析器）返回给DispatcherServlet。
7. ViewREsolver结合Model和View来渲染视图。
8. 将视图渲染结果返回给客户端。

以上8个步骤：DispatcherServlet、HandlerMapping、HandlerAdapter和ViewResolver等对象协同工作，完成Spring MVC请求->响应的整个流程，这些对象所完成的工作对于开发者来说都是不可见的，开发者并不需要关心这些对象是如何工作的，开发者只需要在Handler（Controller）当中完成对请求的业务处理。

## 2.6 本章小结

本章介绍了Spring MVC的入门知识，包括如何使用Spring MVC开发一个简单的Web应用。在Spring MVC中，开发者无须编写自己的前端控制器，使用Spring提供的DispatcherServlet就可以分派请求。Spring MVC传统风格的控制器开发方式是实现Controller接口，从Spring 2.5版本开始，提供了一个更好的控制器开发方式，即采用注解类型。最后，详细分析了Spring MVC请求->响应的完整工作流程。

第3章将重点介绍基于注解的控制器。



