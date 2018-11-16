# 第3章 使用GitHub的前期准备

本章将为各位讲解使用GitHub前期需要做一些准备。

## 3.1 使用前的准备

### 创建账户

首先让我们来创建GitHub账户。请打开创建账户的页面。

我们会看到如图3.1所示的页面。在Username一栏中用英文和数字输入要创建的ID，您的公开页面的URL（https://github.com/shenhuanjie）会用到这个ID。其他项目也请按照页面要求输入。

填写完所有项目后点击Create an account，就能完成账户的创建。账户常见完成后会直接进入登录状态，用户可以立即使用GitHub。登录状态下用户名会显示在页面的右上方。
<!--more-->
### 设置头像

在GitHub上随处可见的头像（账户独有的标识）是通过Gravatar服务显示的。使用过WordPress的读者可能对它有所了解。

只要使用创建GitHub账户时注册的邮箱在Gravatar上设置头像，GitHub的头像就会变成您设置好的样子。

头像并不是使用GitHub时的硬性要求，但如果为代码配上编码者相貌或标识，会让人觉得安心，同时还可能让对方对您产生兴趣。毕竟我们要使用的是能将关注点聚集在人身上的GitHub，所以建议各位积极地设置头像。

### 设置SSH Key

GitHub上连接已有仓库时的认证，是通过使用了SSH的公开密钥认证方式进行的。现在让我们来创建公开密钥认证所需的SSH Key，并将其添加至GitHub。已经创建过的读者，请用先优的密钥进行设置。

运行下面的命令创建SSH Key。

```xml
$ ssh-keygen -t rsa -C "【your_email@example.com】"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/shenh/.ssh/id_rsa):【按回车键】
Enter passphrase (empty for no passphrase):【输入密码】
Enter same passphrase again:【再次输入密码】
```

“your_email@example.com”的部分请改成您在创建账户时用的邮箱地址。密码需要在认证时输入，请选择复杂度高并且容易记忆的组合。

输入密码后会出现以下结果。

```xml
Your identification has been saved in /c/Users/shenh/.ssh/id_rsa.
Your public key has been saved in /c/Users/shenh/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:【fingerprint值】 shenhuanjie@live.cn
The key's randomart image is:
+---[RSA 2048]----+
|+=O=.  ..=+      |
|.= o  ..+oo      |
【略】
```

id_rsa文件是私有密钥，id_rsa.pub是公有密钥。

### 添加公开密钥

在GitHub中添加公开密钥，今后就可以用私有密钥进行认证了。

点击右上角的账户设定按钮（Account Settings），选择SSH Keys菜单。点击Add SSH Key之后，会出现如图3.2的输入框。在Title中输入适当的密钥名称。Key部分请粘贴id_rsa.pub文件里的内容。id_rsa.pub的内容可以用如下方法查看。

```bash
$ cat ~/.ssh/id_rsa.pub
ssh-rsa 【公开密钥的内容】 shenhuanjie@live.cn
```

添加成功之后，创建账户时所用的邮箱会接到一封提示“公共密钥添加完成”的邮件。

完成以上设置后，就可以用手中的私人密钥与GitHub进行认证和通信了。让我们来实际试一试。

```bash
$ ssh -T git@github.com
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:【fingerpring值】.
Are you sure you want to continue connecting (yes/no)? 【输入yes】
```

出现如下结果即为成功。

```bash
Hi shenhuanjie! You've successfully authenticated, but GitHub does not provide shell access.
```

### 使用社区功能

既然说GitHub能够以人为焦点，那么在创建账户后不妨试试Follow（关注）别人。在用户信息页面的右上角点击如图3.3所示的按钮即可。

这样一来，您所Follow的用户的活动就会显示在您的控制面板页面中。您可以通过这种方法知道那个人在GitHub上都做了些什么。

对于仓库，也可以使用Watch功能获取最新的开发信息。如果您经常使用某个软件正在GitHub上进行开发，不妨去Watch一下。

关于这部分的内容还将在第5章中详细讲解。

## 3.2 实际动手使用

### 创建仓库

实际创建一个公开的仓库。点击右上角工具栏的New repository（图3.4）图标，创建新的仓库。

#### Repository name

参考图3.5，在Repository name中输入仓库的名称。这里我们输入Hello-World。

#### Description

Description栏中可以设置仓库的说明。这一栏不是必需项，可以留空。

#### Public、Private

在这一栏可以选择Public还是Private。这里我们选择Public，创建公开仓库，仓库内的所有内容都会被公开。

选择Private可以创建非公开仓库，用户可以设置访问权限，但这项服务是收费的。

#### Initialize this repository with a README

在Initialize this repository with a README选项上打钩，随后GitHub会自动初始化仓库并设置README文件，让用户可以立刻clone这个仓库。如果想向GitHub添加手中已有的Git仓库，建议不要勾选，直接手动push。

#### Add .gitignore

下方左侧的下拉菜单非常方便，通过它可以在初始化时自动生成.gitignore文件。这个设定会帮我们把不需要在Git仓库中进行版本管理的文件记录在.gitignore文件中，省去了每次根据框架进行设置的麻烦。下拉菜单中包含了主要的语言及框架，选择今后将要使用的即可。由于本书中我们并不使用任何框架，所以不做选择。

#### Add a license

右侧的下拉菜单可以选择要添加的许可协议文件。如果这个仓库中包含的代码已经确定了许可协议，那么请在这里进行选择。随后将自动生成包含许可协议内容的LICENSE文件，用来表明该仓库内容的许可协议。

输入选择都完成后，点击Create repository按钮，完成仓库的创建。

### 连接仓库

下面这个URL便是刚刚创建的仓库的页面。

```xml
https://github.com/shenhuanjie/Hello-World
```

#### README.md

README.md在初始化时已经生成好了。README.md文件内容会自动显示在仓库的首页当中。因此，人们一般会在这个文件中标明本仓库所包含的软件的概要、使用流程、许可协议等信息。如果使用Markdown语言进行描述，还可以添加标记，提高可读性。

#### GitHub Flavored Markdown

在GitHub上进行交流时用到的Issue、评论、Wiki，都可以用Markdown语法表述，从而进行标记。准确地说应该是GitHub Flavored Markdown（GFM）语法。该语法虽然是GitHub在Markdown语法基础上扩充而来的，但一般情况下只要按照原来的Markdown语法进行描述就可以。

关于Markdown语法的解说，网上也有相关资料可查。各位不妨一边参考一边实际尝试。

使用GitHub后，很多文档都需要用Markdown来书写。也就是说，全世界有大量程序员都在使用Markdown，因此掌握这种语法已经成为程序员的标准技能之一。请各位也务必学会Markdown语法。

### 公开代码

#### clone已有仓库

接下来我们将尝试在已有仓库中添加代码并加以公开。首先将已有仓库clone到身边的开发环境中。clone时指定的路径请参考图3.6.

```bash
$ git clone git@github.com:shenhuanjie/Hello-World.git
Cloning into 'Hello-World'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

$ cd Hello-World
```

这里会要求输入GitHub上设置的公开密钥的密码。认证成功后，仓库便会被clone至仓库名的目录中。将想要公开的代码提交至这个仓库再push到GitHub的仓库中，代码便会被公开。

```php
<?php
	echo "Hello World!";
?>
```

由于hello_word.php还没有添加到Git仓库，所以显示为Untracked files。

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hello_world.php

nothing added to commit but untracked files present (use "git add" to track)
```

#### 提交

将hello_world.php提交至仓库。这样一来，这个文件就进入了版本管理系统的管理之下。今后的更改管理都交由Git进行。

```bash
$ git add hello_world.php
$ git commit -m "Add hello world script by php"
[master b865050] Add hello world script by php
 1 file changed, 3 insertions(+)
 create mode 100644 hello_world.php
```

通过git add命令将文件加入暂存区，再通过git commit命令提交。

添加成功后，可以通过git log命令查看提交日志。

```bash
$ git log
commit b8650502ff59441fcbfbd035842826f3fec058aa (HEAD -> master)
Author: shenhuanjie <shenhuanjie@live.cn>
Date:   Mon Nov 12 22:10:51 2018 +0800

    Add hello world script by php

commit f724d88486fab3ac8c5a12d9b0b9a1810b979c92 (origin/master, origin/HEAD)
Author: SHEN.HUANJIE <shenhuanjie@live.cn>
Date:   Mon Nov 12 21:40:44 2018 +0800

    Initial commit
```

> **专栏：公开时的许可协议**
>
> 即便在GitHub上公开了源代码，也不代表著作者放弃了著作权等权利。代码的权利持有人请选择合适的许可协议。在GitHub上，有修正BSD许可协议、Apache许可协议等多种许可协议供人们选择，不过大多数软件都使用MIT许可协议。
>
> MIT许可协议具有以下特征。
>
> 被授权人权利：被授权人有权利使用、复制、修改、合并、出版发行、散布、再授权和/或销售软件及软件的副本，及授予被供应人同等权利，唯服从以下义务。
>
> 被授权人义务：在软件和软件的所有副本中都必须包含以上版权声明和许可声明。
>
> 其他重要特性：此许可协议并非属copyleft的自由软件许可协议条款，允许在自由及开源代码软件及非自由软件（proprietary software）所使用。
>
> MIT的内容可依照程序著作权则的需求更改内容。此亦为MIT与BSD（The BSD license，3-clause BSD license）本质上不同处。
>
> MIT许可协议可与其他许可协议并存。另外，MIT条款也是自由软件基金会（FSF）所认可的自由软件许可协议条款，与GPL兼容。
>
>
>
> ——MIT许可证，Wikipedia，http://zh.wikipedia.org/，2015年3月27日获取
>
> ----
>
> 详细内容请参阅[原文](http://www.opensource.org/licenses/mit-license.php)。
>
> 实际使用时，只需将LICENSE文件加入仓库，并在README.md文件中声明使用了何种协议即可。
>
> 使用没有声明许可协议的软件时，以防万一最好直接联系作者。

#### 进行push

之后只要执行push，GitHub上的仓库就会被更新。

```bash
$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 322 bytes | 161.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:shenhuanjie/Hello-World.git
   f724d88..b865050  master -> master
```

这样一来代码就在GitHub上公开了。不妨实际连接http://github.com/shenhuanjie/Hello-World查看一下。Git更加详细的操作请查阅第4章。

## 3.3 小结

本章讲解了初次在GitHub建立仓库以及公开代码的流程。完成这些，各位就踏入了GitHub的世界。