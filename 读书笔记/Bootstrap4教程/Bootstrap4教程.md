# Bootstrap4 教程

![img](assets/bootstrap-stack.png)

Bootstrap 是全球最受欢迎的前端组件库，用于开发响应式布局、移动设备优先的 WEB 项目。

Bootstrap4 目前是 Bootstrap 的最新版本，是一套用于 HTML、CSS 和 JS 开发的开源工具集。利用我们提供的 Sass 变量和大量 mixin、响应式栅格系统、可扩展的预制组件、基于 jQuery 的强大的插件系统，能够快速为你的想法开发出原型或者构建整个 app 。

<!--more-->

## 谁适合阅读本教程？

只要您具备 HTML 和 CSS 的基础知识，您就可以阅读本教程，进而开发出自己的网站。在您学习完本教程后，您即可达到使用 Bootstrap 开发 Web 项目的中等水平。

## 阅读本教程前，您需要了解的知识：

在您开始阅读本教程之前，您必须具备 HTML 、 CSS 和 JavaScript 的基础知识。如果您还不了解这些概念，那么建议您先阅读我们的这些教程:

- [HTML 教程](http://www.runoob.com/html/html-tutorial.html)
- [CSS 教程](http://www.runoob.com/css/css-tutorial.html)
- [JavaScript 教程](http://www.runoob.com/js/js-tutorial.html)

## Bootstrap4 实例

```html
<!DOCTYPE html>
<html>

	<head>
		<title>Bootstrap 实例</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/4.1.0/css/bootstrap.min.css">
		<script src="https://cdn.staticfile.org/jquery/3.2.1/jquery.min.js"></script>
		<script src="https://cdn.staticfile.org/popper.js/1.12.5/umd/popper.min.js"></script>
		<script src="https://cdn.staticfile.org/twitter-bootstrap/4.1.0/js/bootstrap.min.js"></script>
	</head>

	<body>
		<div class="jumbotron text-center">
			<h1>我的第一个 Bootstrap 页面</h1>
			<p>重置浏览器大小查看效果!</p>
		</div>
		<div class="container">
			<div class="row">
				<div class="col-sm-4">
					<h3>第一列</h3>
					<p>菜鸟教程</p>
					<p>学的不仅是技术，更是梦想！！！</p>
				</div>
				<div class="col-sm-4">
					<h3>第二列</h3>
					<p>菜鸟教程..</p>
					<p>学的不仅是技术，更是梦想！！！</p>
				</div>
				<div class="col-sm-4">
					<h3>第三列</h3>
					<p>菜鸟教程..</p>
					<p>学的不仅是技术，更是梦想！！！</p>
				</div>
			</div>
		</div>
	</body>

</html>
```

## Bootstrap4 与 Bootstrap3

Bootstrap4 是 Bootstrap 的最新版本，与 Bootstrap3 相比拥有了更多的具体的类以及把一些有关的部分变成了相关的组件。同时 Bootstrap.min.css 的体积减少了40%以上。

Bootstrap4 放弃了对 IE8 以及 iOS 6 的支持，现在仅仅支持 IE9 以上 以及 iOS 7 以上版本的浏览器。如果对于其中需要用到以前的浏览器，那么请使用 [Bootstrap3](http://www.runoob.com/bootstrap/bootstrap-tutorial.html)。