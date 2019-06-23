# 第3章 Maven使用入门

**本章内容**

* 编写POM
* 编写主代码
* 编写测试代码
* 打包和运行
* 使用Archetype生成项目骨架
* m2eclipse简单使用
* NetBeans Maven插件简单使用
* 小结

到目前为止，已经大概了解并安装好了Maven，现在，我们开始创建一个最简单的Hello World项目。如果你是初次接触Maven，建议按照本章的内容一步步地编写代码并执行，其中可能你会碰到一些概念暂时难以理解，不用着急，记下这些疑难点，相信本书的后续章节会帮你逐一解答。

## 3.1 编写POM

现在先为Hello World项目编写一个简单的pom.xml。

首先创建一个名为hello-world的文件夹，打开该文件夹，新建一个名为pom.xml的文件，输入其内容，如代码清单3-1所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xis:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.juvenxu.mvnbook</groupId>
    <artifacId>hello-world</artifacId>
    <version>1.0</version>
    <name>Maven Hello World Project</name>
</project>
```

代码的第一行是XML头，指定了该xml文档的版本和编码方式。紧接着是project元素，project是所有pom.xml的根元素，它还声明了一些POM相关的命名空间及xsd元素，虽然这些属性不是必须的，但使用这些属性能够让第三方工具（如IDE中的XML编辑器）帮助我们快速编辑POM。

根元素下的第一个元素modleVersion指定了当前POM模型的版本，对于Maven2及Maven3来说，它只能是4.0.0。

这段代码中最重要的是包含groupId、artifactId和version的三行。这三个元素定义了一个项目基本的坐标，在Maven的世界，任何的jar、pom或者war都是以基于这些基本的坐标进行区分的。

* **groupId**定义了项目属于哪个组，这个组往往和项目所在的组织或公司存在关联。
* **artifactId**定义了当前Maven项目在组中唯一的ID。
* **version**指定了Hello World项目当前的版本——1.0.SNAPSHOT。
* **name**元素声明了一个对于用户更为友好的项目名称，虽然这不是必须的，但还是推荐为每个POM声明name，以方便信息交流。

## 3.2 编写主代码

```java
package com.juvenxu.mvnbook.helloworld;

public class HelloWorld {
    public String sayHello() {
        return "Hello Maven";
    }

    public static void main(String[] args) {
        System.out.println(new HelloWorld().sayHello());
    }
}
```

代码编写完毕后，使用Maven进行编译，在项目根目录下运行命令`mvn clean compile`会得到如下输出：

```bash
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.juvenxu.mvnbook:hello-world >-------------------
[INFO] Building Maven Hello World Project 1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ hello-world ---
[INFO] Deleting C:\...\Maven实战\HelloWorld\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ hello-world ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\...\Maven实战\HelloWorld\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ hello-world ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\...\Maven实战\HelloWorld\target\classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.732 s
[INFO] Finished at: 2019-06-10T15:44:49+08:00
[INFO] ------------------------------------------------------------------------
```

### 3.3 编写测试代码

为了使项目结构保持清晰，主代码与测试代码应该分别位于独立的目录中。

在Java世界中，由Kent Beck 和Erich Gamma建立的JUnit是事实上的单元测试标准。要使用JUnit，首先需要为Hello World项目添加一个JUnit依赖，修改项目的POM如代码清单3-3所示：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.juvenxu.mvnbook</groupId>
    <artifactId>hello-world</artifactId>
    <version>1.0</version>
    <name>Maven Hello World Project</name>
    <description>《Maven实战》第3章实例</description>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.7</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <properties>
        <!-- Maven编译环境:JDK1.8 -->
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.source>1.8</maven.compiler.source>
        <!-- 项目资源编码格式:UTF-8 -->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

代码中添加了dependencies元素，该元素下可以包含多个dependency元素以声明项目的依赖。

上述POM代码中还有一个值为test的元素scope，scope为依赖范围，若依赖范围为test则表示该依赖只对测试有效。换句话说，测试代码中的import JUnit代码是没有问题的，但是如果在主代码中用import JUnit代码，就会造成编译错误。如果不声明依赖范围，那么默认值就是compile，表示该依赖对主代码和测试代码都有效。

```java
package com.juvenxu.mvnbook.helloworld;

import static org.junit.Assert.assertEquals;

import org.junit.Test;

public class HelloWorldTest {
    @Test
    public void testSayHello() {
        HelloWorld helloWorld = new HelloWorld();
        String result = helloWorld.sayHello();
        assertEquals("Hello Maven", result);
    }
}
```

一个典型的单元测试包含三个步骤：

1. 准备测试类及数据；
2. 执行要测试的行为；
3. 检查结果。

## 3.4 打包和运行

将项目进行编译、测试之后，下一个重要步骤就是打包（package）。简单地执行命令`mvn clean package`进行打包，可以看到如下输出：

```powershell
Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO]
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ hello-world ---
[INFO] Building jar: C:\...\Maven实战\Archetype\hello-world\target\hello-world-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 9.877 s
[INFO] Finished at: 2019-06-10T16:36:16+08:00
[INFO] ------------------------------------------------------------------------
```



类似地，Maven会在打包之前执行编译、测试等操作。

## 3.5 使用Archetype生成项目骨架

我们使用`maven archetype`来创建该项目的骨架。

```powershell
mvn archetype:generate
```

## 3.6 m2eclipse简单使用

### 3.6.1 导入Maven项目

### 3.6.2 创建Maven项目

### 3.6.3 运行mvn命令

## 3.7 NetBeans Maven插件简单使用

### 3.7.1 打开Maven项目

### 3.7.2 创建Maven项目

### 3.7.3 运行mvn命令

## 3.8 小结

本章以尽可能简单且详细方式叙述了一个 HelloWorld项目，重点解释了POM的基本内容、Maven项目的基本结构以及构建项目基本的Maven命令。在此基础上，还介绍了如何使用Archetype快速创建项目骨架。最后讲述的是如何在Eclipse和NetBeans中导入、创建及构建Maven项目。

