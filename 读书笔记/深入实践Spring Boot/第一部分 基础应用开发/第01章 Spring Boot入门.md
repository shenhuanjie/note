# 第1章 Spring Boot入门

在使用Spring Boot框架进行各种开发体验之前，要先配置好开发环境。首先安装JDK，然后选择一个开发工具，如Eclipse IDE和IntelliJ IDEA（以下简称IDEA）都是不错的选择。对于开发工具的选择，本书极力推荐使用IDEA，因为它为Spring Boot提供了许多更好和更贴切的支持，本书的实例都是使用IDEA创建的。同时，还需要安装Apache Maven和Git客户端。所有这些都准备好之后，我们就能开始使用Spring Boot了。
<!--more-->
## 1.1 配置开发环境

下面的开发环境配置主要以使用Windows操作系统为例，如果你使用的是其他操作系统，请对照其相关配置进行操作。

### 1.1.1 安装JDK

JDK（Java SE Development Kit）需要1.8及以上版本，可以从Java的[官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载安装包。如果访问官网速度慢的话，也可以通过百度搜索JDK，然后在百度软件中心下载符合你的Windows版本和配置的JDK1.8安装包。

安装完成后，配置环境变量JAVA_HOME，例如，使用路径`D:\Program Files\Java\jkd1.8.0_25`（如果你安装的是这个目录的话）。`JAVA_HOME`配置好之后，将`%JAVA_HOME%\bin`加入系统的环境变量path中。完成后，打开一个命令行窗口，输入命令java -version，如果能正确输出版本号则说明安装成功了。输出版本的信息如下：

```shell
java version "1.8.0_161"
Java(TM) SE Runtime Environment (build 1.8.0_161-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.161-b12, mixed mode)
```

### 1.1.2 安装InterlliJ IDEA

IDEA需要14.0以上的版本，可以从其官网http://jetbrains.com/ 下载免费版，本书的实例是使用IDEA14.1.15版本开发的。IDEA已经包含Maven插件，版本是3.05，这已经能够适用我们开发的要求。安装完成后，打开IDEA，将显示如图1-1所示的欢迎界面，在这里可以看到IDEA的版本号。

![图1-1 InterlliJ IDEA欢迎界面](https://i.loli.net/2018/11/19/5bf2729db10f1.png) 

### 1.1.3 安装Apache Maven

为了能够在命令行窗口中使用Maven来管理工程，可以安装一个Maven管理工具。通过Maven的官网http://maven.apache.org/download.cgi  下载3.0.5以上的版本，下载完成后加压缩即可，例如，解压到D盘上是不错的做法，然后将Maven的安装路径（如`D:\apache-maven-3.2.3\bin`）也加入Windows的环境变量path中。安装完成后，在命令行窗口中执行指令：mvn -v，将输出如下的版本信息以及系统的一些环境信息。

```shell
Maven home: C:\Program Files\Maven\bin\..
Java version: 1.8.0_161, vendor: Oracle Corporation
Java home: C:\Program Files\Java\jdk1.8.0_161\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

建议更改IDEA中的Maven资源库的存放路径，可以先在Maven安装路径中创建一个资源库目录，如repository。然后打开Maven的配置文件，即安装目录conf中的settings.xml，找到下列代码，将路径更改为repository所在的位置，并保存在注释符下面。

例如找到下列代码行：

```xml
<localRepository>/path/to/local/repo</localRepository>
```

复制出来改为如下所示：

```xml
<localRepository>D:\apache-maven-3.2.3\repository</localRepository>
```

改好后可以拷贝一份settings.xml放置在${user.home}/.m2/下面，这样做可以不用修改IDEA的Maven这个配置。在图1-2所示的Maven配置界面中，User Settings File保存了默认位置，Local Repository使用了上面配置的路径`D:\apache-maven-3.2.3\repository`，而Maven程序还是使用了IDEA自带的版本。

![image.png](https://i.loli.net/2018/11/19/5bf272f145138.png) 

### 1.1.4 安装Git客户端

由于本书的实例工程都存放在GitHub（`https://github.com/`）中，所以还需要在GitHub中免费注册一个用户（可以通过E-mail直接注册免费用户），以方便在IDEA中从GitHub检出本书的实例工程。当然，如果不想注册，通过普通下载的方法也能取得实例工程的源代码。GitHub是世界级的代码库服务器，如果你愿意，也可以将它作为你的代码库服务器，在这里还可以搜索到全世界的开发者分享出来的源程序。图1-3是打开GitHub的首页。

![image.png](https://i.loli.net/2018/11/19/5bf273078c438.png) 

IDEA还需要Git客户端程序的支持。可以从其官网`https://git-scm.com/download/`下载Git客户端安装包。安装非常简单，按提示单击“下一步”并选择好安装路径即可。安装完成后，在Windows的资源管理器中，单击鼠标右键弹出的菜单中将会多出如下几个选择菜单：

```
Git Init Here
Git Gui
Git Bash
```

其中Git Bash是一个带有UNIX指令行窗口，在这里可以执行一些Git指令，用来提交或者检出项目。

在IDEA中对Git的设置，只要指定git.exe执行文件的位置即可。图1-4是IDEA中Git客户端的配置，其中Git的路径被设置在`D:\Program Files\Git\bin\git.exe`中，这主要由安装Git客户端的位置而定。

![image.png](https://i.loli.net/2018/11/19/5bf2731640c07.png) 

如果已经在GitHub中注册了用户，即可以打开如图1-5所示的GitHub配置，输入用户名和密码，然后单击Test按钮，如果设置正确的话将会返回连接成功的提示。

![image.png](https://i.loli.net/2018/11/19/5bf2731e5760f.png) 

> **提示：**上面IDEA的一些设置界面都可以单击工具栏上的Settings按钮打开，打开File菜单，选择Settings同样也可以打开。

## 1.2 创建项目工程

现在，可以尝试使用IDEA来创建一个项目工程。如果是第一次打开IDEA。可以选择Create New Project创建一个新工程。如果已经打开了IDEA，在File菜单中选择New Project，也能打开New Project对话框，如图1-6所示。使用IDEA创建一个Spring Boot项目有很多方法，这里只介绍使用Maven和Spring Initalizr两种方法来创建一个新项目。一般使用Maven来新建一个项目，因为这样更容易按我们的要求配置一个项目。

![image.png](https://i.loli.net/2018/11/19/5bf273327c983.png) 

### 1.2.1 使用Maven新建项目

使用Maven新建一个项目主要有以下三个步骤。

**1. 选择项目类型**

在图1-6中的Project SDK下拉列表框中选择前面安装的Java 1.8，如果下拉列表中不存在Java 1.8，可以单击New按钮，找到安装Java的位置，选择它。然后在左面侧边栏的项目类型中，选择Maven项目，即可使用Maven作为项目的管理工具。至于Maven中的archetype，因为我们并不打算使用其中任何一种类型，所以不用勾选，然后单击Next进入下一步。

**2. 输入GroupId和ArtifactId**

在GroupId输入框中输入“springboot.example”，在ArtifactId输入框中输入”spring-boot-hello“，Version输入框中保存默认值，如图1-7所示，单击Next进入下一步。

![image.png](https://i.loli.net/2018/11/19/5bf273429330c.png) 

**3. 指定项目名称和存放路径**

在Project location编辑框中选择和更改存放路径，在Project name输入框中输入与ArtifactId相同的项目名称：“spring-boot-hello”，如图1-8所示。

![image.png](https://i.loli.net/2018/11/19/5bf273501d4e8.png) 

单击Finish，完成项目创建，这样将在当前窗口中打开一个新项目，如图1-9所示。其中，在工程根目录中生成了一个pom.xml，即Maven的项目对象模型（Project Object Model），并生成了源代码目录java、资源目录resources和测试目录test等，即生成了一个项目的一些初始配置和目录结构。

![image.png](https://i.loli.net/2018/11/19/5bf2735da424e.png) 

下一节将使用这个项目工程来创建第一个使用Spring Boot开发框架的应用实例。

### 1.2.2 使用Spring Initializr新建项目

新建一个Spring Boot项目，也可以使用Spring Initializr的方式，这种方式很简单，如图1-10所示。注意Initializr Service URL为`https://start.spring.io`，这将会连接网络，以查询Spring Boot的当前可用版本和组件列表。使用这种方式新建项目大体上也需要三个步骤。

![image.png](https://i.loli.net/2018/11/19/5bf27376c261d.png) 

**1. 选择类型**

可以使用默认选项，注意Type为Maven Project，Java Version为1.8，Packaging为Jar，如图1-11所示。单击Next进入下一步。

![image.png](https://i.loli.net/2018/11/19/5bf273801d2be.png) 

**2. 选择Spring Boot版本和组件**

选择Spring Boot版本和Spring Boot组件，例如，在Spring Boot Version中选择1.3.5，并勾选Web项目组件，如图1-12所示，然后单击Next进入下一步。

![image.png](https://i.loli.net/2018/11/19/5bf2738ab6767.png) 

**3. 输入项目名称**

选择存放路径后输入项目名称，如图1-13所示，这里使用demo作为项目的名称。

![image.png](https://i.loli.net/2018/11/19/5bf273963d505.png) 

单击Finish，将创建一个初始化项目，如图1-14所示。这个项目不但有完整的目录结构，还有一个完整的Maven配置，并且生成了一个默认的主程序，几乎所有的准备工作都已经就绪，并且可以立即运行（虽然没有提供任何可用的服务）。这也是Spring Boot引以为傲的地方，即创建一个应用可以不用编写任何代码，只管运行即可。

![image.png](https://i.loli.net/2018/11/19/5bf2739e3a3c4.png) 

## 1.3 使用Spring Boot

任何应用的开发都需要对项目的创建、运行和发布等进行管理，使用Spring Boot框架进行开发，可以选择使用Maven或Gradle等项目管理工具。在这里我们使用的是Maven。

### 1.3.1 Maven依赖管理

使用Maven，通过导入Spring Boot的starter模块，可以将许多程序依赖包自动导入工程中。使用Maven的parent   POM，还可以更容易地管理依赖的版本和使用默认的配置，工程中的模块也可以很方便地继承它。例如，使用1.2.1节创建的工程，修改pom.xml文件，使用如代码清单1-1所示的简单Maven配置，基本上就能为一个使用Spring Boot开发框架的Web项目开发提供所需的相关依赖。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>springboot.example</groupId>
    <artifactId>spring-boot-hello</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.2.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

这里只使用了一个依赖配置spring-boot-starter-web和一个parent配置spring-boot-starter-parent，在工程的外部库（External Libraries）列表中，它自动引入的依赖包如代码清单1-2所示。

![image.png](https://i.loli.net/2018/11/19/5bf271be126a1.png) 

在工程的外部库列表中，Spring Boot已经导入了整个springframework依赖，以及autoconfigure、logging、slf4j、jackson、tomcat插件等，所以这些都是一个Web项目可能需要用到的东西（包括你已经考虑到的和没有考虑的），它真是一个聪明的助手。

### 1.3.2 一个简单的实例

Spring Boot的官方文档中提供了一个最简单的Web实例程序，这个实例只使用了几行代码，如代码清单1-3所示。虽然简单，但实际上这已经可以算作是一个完整的Web项目了。

```java
package springboot.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class Application {
    @RequestMapping("/")
    String home() {
        return "hello";
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

这个简单实例，首先是一个Spring Boot应用的程序入口，或者叫作主程序，其中使用了一个注解@SpringBootApplication来标注它是一个Spring Boot应用，main方法使它成为一个主程序，将在应用启动时首先被执行。其次，注解@RestController同时标注这个程序还是一个控制器，如果在浏览器中访问应用根目录，它将调用home方法，并输出字符串：hello。

## 1.4 运行与发布

本章实例工程的完整代码可以使用IDEA直接从GitHub的`https://github.com/chenfromsz/spring-boot-hello.git`中检出，如图1-15所示，单击Clone按钮将整个项目复制到本地。

![image.png](https://i.loli.net/2018/11/19/5bf273b851397.png) 

### 1.4.1 在IDEA环境中运行

在IDEA中打开Run菜单，选择Edit Configuration打开Run/Debug Configurations对话框，在配置界面的左边侧边栏中选择增加一个Application或Spring Boot配置项目，然后在工作目录中选择工程所在的根目录，主程序选择代码清单1-3创建的类：springboot.example.Application，并将配置保存为hello，如图1-16所示。

然后选择Run或Debug运行hello配置项目。如果启动成功，将在控制台中输出类似如下信息：

![image.png](https://i.loli.net/2018/11/19/5bf271dd641e9.png) 

从上面的输出中可以看出，Tomcat默认开启了8080端口。要访问这个应用提供的服务，可以在浏览器的地址栏中输入`http://localhost:8080/`。这样就可以看到我们期望的输出字符：hello。

![image.png](https://i.loli.net/2018/11/19/5bf273cc8231d.png) 

### 1.4.2 将应用打包发布

上面操作演示了在IDEA环境中如何运行一个应用。如果我们想把应用发布出去，需要怎么做呢？可以将代码清单1-1中的Maven配置增加一个发布插件来实现。如代码清单1-4所示，增加了一个打包插件：spring-boot-maven-plugin，并增加了一行打包的配置：`<packaging>jar</packaging>`，这行配置指定将应用工程打包成jar文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>springboot.example</groupId>
    <artifactId>spring-boot-hello</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.3.2.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

这样就可以在IDEA中增加一个打包的配置，打开Run/Debug Configurations对话框，选择增加配置一个Maven打包项目，在工作目录中选择工程所在根目录，在命令行中输入package，并将配置保存为mvn，如图1-17所示。

![image.png](https://i.loli.net/2018/11/19/5bf273e13391e.png) 

运行mvn打包项目，就可以将实例工程打包，打包的文件将输出在工程的target目录中。

如果已经按照1.1.3节的说明安装了Maven，也可以直接使用Maven的命令打包。打开一个命令行窗口，将路径切换到工程根目录中，直接在命令行输入mvn package，同样也能将项目打包成jar文件。执行结果如下：

```bash
……
[INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ spring-boot-hello ---
[INFO] Building jar: C:\Users\shenh\Documents\GitHub\Spring\SpringBoot\spring-boot-hello\target\spring-boot-hello-1.0-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:1.3.2.RELEASE:repackage (default) @ spring-boot-hello ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 11.497 s
[INFO] Finished at: 2018-11-19T15:41:53+08:00
[INFO] ------------------------------------------------------------------------

Process finished with exit code 0

```

打包成功后，在工程的target目录中将会生成jar文件spring-boot-hello-1.0-SNAPSHOT.jar。在命令行窗口中切换到target目录中，运行如下指令，就能启动应用。

```bash
java -jar spring-boot-hello-1.0-SNAPSHOT.jar
```

如果希望按照传统的做法，将工程发布成war文件，应当将代码清单1-4的Maven配置`<packaging>jar</packageing>`改成`<packaging>war</packaging>`，这样就可以打包成war文件。打包完成后将war文件放置在Tomcat的webapp路径中，启动Tomcat就能自动运行程序。

这里需要注意的是，如果自主使用Tomcat运行应用，在安装JDK时必须配置JAVA_HOME环境变量，同时JDK要求1.8以上的版本，Tomcat必须是8.0以上的版本。

我更加喜欢打包成jar，然后使用Spring Boot的嵌入插件Tomcat运行应用。本书所有实例都可以打包成jar直接运行。即使对于一个包含很多页面、图片、脚本等资源的复杂应用系统，这种方法也是可行的，并且打包成jar，更方便项目发布在Docker上运行，这些将在后面的章节中详细介绍。

## 1.5 关于Spring Boot配置

关于Spring Boot配置，可以在工程的resources文件夹中创建一个application.properties或application.yml文件，这个文件会被发布在classpath中，并且被Spring Boot自动读取。这里推荐使用application.yml文件，因为它提供了结构化及其嵌套的格式，例如，可以按如下所示配置上面的工程，将默认端口改为80，并且将Tomcat的字符集定义为UTF-8。

```yaml
server:
  port: 80
  tomcat:
    uri-encoding: utf-8
```

如果要使用application.properties文件，上面的配置就要改成如下所示的样子，其结果完全相同。

```xml
server.port = 80
server.tomcat.uri-enconding = UTF-8
```

使用这个配置文件可以直接使用Spring Boot预定义的一些配置参数，关于其他配置参数的详细说明和描述可以查看官方的文档说明：[https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html) 。在后面的开发中将在用得到的地方选择使用这些预定义的配置参数。即使没有预定义的配置参数可用，也能很容易地按照应用的需要自定义一些配置参数，这将在后续的章节中详细介绍。

## 1.6 小结

本章主要介绍了Spring Boot开发环境的搭建，以及一些开发工具的安装配置，内容难免有点枯燥。然后创建并运行一个非常简单的实例工程，让性急的读者一睹Spring Boot的芳容。

本章实例工程只是使用Spring Boot框架进行开发的非常简单的入门指引。因为Spring Boot开发框架是一个非常轻量级的开发框架，所以也有人把它叫做微框架，从入门指引中可以看出，使用Spring Boot框架开发应用不但入门容易，而且其蕴藏的无比强大的功能，使开发过程也变得更加容易。

下面，让我们使用Spring Boot框架进行一些更加有趣的开发吧。这一章只是小试牛刀而已，在后续章节中将使用Spring Boot框架来开始一些真正的开发。