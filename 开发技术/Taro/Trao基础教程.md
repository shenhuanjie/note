# Trao基础教程-Trao、Taro UI开发微信小程序

## （一）初始化项目

taro开发微信小程序,taro框架目前是移动端的混合全能框架,对于微信小程序平台编译优秀,taro结合webpack可以编译微信小程序,anroid,ios,H5等平台代码,taro ui是基于taro的一套ui界面库。本栏博客介绍如何使用使Trao和Taro UI框架来开发移动端微信小程序.

1. 使用yarn或者npm来初始化项目

```cmd
$ npm install -g @tarojs/cli   tip:这里可以换成cnpm
$ yarn global add @tarojs/cli
 
//注意yarn与node是需要提前安装好的
```

2. 创建一个taro模版项目

```cmd
 taro init taro-pro    
 //使用命令行进入到相应的目录之中 若需要新建一个文件夹请执行
 mkdir taro-pro && cd taro-pro
```

项目创建完成后直接一路回车或者提示的按照提示操作即可

3. 项目创建完成后必须安装依赖

```node
cnpm install
```

4. 编译项目

这里以小程序的方式编译代码，然后预览

```cmd
npm run dev:weapp
```

5. 使用微信开发工具预览

打开开发者工具后选定dist目录作为小程序的开发目录即可

## （二）使用Taro ui

安装Taro ui

```cmd
$ cd taro-pro
$ npm install taro-ui --save //加入依赖清单
```

在组件中使用

```js
//以头像组件为例
 
---------------pages／index------------------
 
import { AtAvatar } from 'taro-ui'
 
<AtAvatar image='https://jdc.jd.com/img/200'></AtAvatar>
```

注意在微信开发工具里设置不校验合法域名 或者把 jdc.jd.com 加入到微信开发者管理后台中！