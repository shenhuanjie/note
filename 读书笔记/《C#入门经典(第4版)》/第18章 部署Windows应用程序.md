# 第18章 部署Windows应用程序

安装Windows应用程序有几种方式，简单的应用程序可以使用简单的xcopy部署来安装，但对于上百个客户的安装，xcopy部署就不顶用了。在这种情况下，可以使用ClickOnce部署，也可以使用Microsoft安装应用程序。

在ClickOnce部署中，可以通过单击某个网站的链接来安装应用程序。此时，用户选择安装应用程序的目录，如果需要一些组成表项，就应使用Microsoft Installer部署选项。

本章介绍安装Windows应用程序的两个选项，主要内容如下：

* 部署的基础知识
* ClickOnce部署
* Visual Studio部署和安装项目类型
* Windows安装程序的特性
* 使用Visual Studio 2008创建Windows安装程序包

## 18.8 小结

本章介绍了ClickOnce部署的用法和Windows Installer的功能，以及如何使用Visual Studio 2008创建Installer软件包。Windows Installer更易于进行标准化的安装、卸载和修复。

ClickOnce是一种新技术，它使得安装Windows应用程序更容易，且不需要以系统管理员的身份进行登录。ClickOnce提供了简单的部署和客户应用程序的更新。

如果需要的功能比ClickOnce能提供的还多，就应使用Windows Installer。Visual Studio 2008 Installer在功能上收到了限制，不能提供Windows Installer的全部功能，但是对于许多应用程序而言，Visual Studio 2008 Installer提供的功能就足够了。有几个编辑器可以配置生成的Windows Installer文件。使用File System编辑器可以指定所有的文件和快捷方式，Launch Conditions编辑器可以定义一些必须的先期安装内容，File Types编辑器用于为应用程序注册文件扩展名，User Interface编辑器更易于修改用于安装的对话框。

本章学习了：

* 使用ClickOnce部署的原因及其功能
* 如何为Windows应用程序配置ClickOnce部署
* 如何使用Visual Studio创建安装项目
* 如何使用安装项目的FileStyle、File Types、Launch Condition和User Interface编辑器。