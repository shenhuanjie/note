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

```xml
$ cat ~/.ssh/id_rsa.pub
ssh-rsa 【公开密钥的内容】 shenhuanjie@live.cn
```

添加成功之后，创建账户时所用的邮箱会接到一封提示“公共密钥添加完成”的邮件。

完成以上设置后，就可以用手中的私人密钥与GitHub进行认证和通信了。让我们来实际试一试。

```xml
$ ssh -T git@github.com
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:【fingerpring值】.
Are you sure you want to continue connecting (yes/no)? 【输入yes】
```

出现如下结果即为成功。

```xml
Hi shenhuanjie! You've successfully authenticated, but GitHub does not provide shell access.
```

