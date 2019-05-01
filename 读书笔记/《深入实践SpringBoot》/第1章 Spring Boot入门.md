# 第1章 Spring Boot入门

在使用Spring Boot框架进行各种开发体验之前，要先配置好开发环境。首先安装JDK，然后选择一个开发工具，如Eclipse IDE和IntelliJ IDEA（以下简称IDEA）都是不错的选择。对于开发工具的选择，本书极力推荐使用IDEA，因为它为Spring Boot提供了许多更好和更贴切的支持，本书的实例都是使用IDEA创建的。同时，还需要安装Apache Maven和Git客户端。所有这些准备好之后，我们就能开始使用Spring Boot了。

## 1.1 配置开发环境

下面的开发环境配置主要以使用Windows操作系统为例，如果你使用的是其他操作系统，请对照其相关配置进行操作。

### 1.1.1 安装JDK

JDK（Java SE Development Kit）需要1.8及以上版本，可以从Java的官网<https://www.oracle.com/techetwork/java/javase/downloads/>下载安装包。如果访问官网速度慢的话，也可以通过百度搜索JDK，然后在百度软件中心下载符合你的Windows版本和配置的JDK1.8安装包。

安装完成后，配置环境变量JAVA_HOME，例如，使用路径D:\Program Files\Java\jdk1.8.0_25（如果你安装的是这个目录的话）。JAVA_HOME配置