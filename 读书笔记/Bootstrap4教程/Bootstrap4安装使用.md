# Bootstrap4 安装使用

我们可以通过以下两种方式来安装 Bootstrap4：

- 使用 Bootstrap 4 CDN。

- 从官网 [getbootstrap.com](https://getbootstrap.com/docs/4.1/getting-started/download/) 下载 Bootstrap 4。

  ![img](assets/4D026288-A0DB-4CB3-9921-5EABC450C650.jpg)

<!--more-->
### Bootstrap 4 CDN

国内推荐使用 Staticfile CDN 上的库：

```html
<!-- 新 Bootstrap4 核心 CSS 文件 -->
<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/4.1.0/css/bootstrap.min.css">
 
<!-- jQuery文件。务必在bootstrap.min.js 之前引入 -->
<script src="https://cdn.staticfile.org/jquery/3.2.1/jquery.min.js"></script>
 
<!-- popper.min.js 用于弹窗、提示、下拉菜单 -->
<script src="https://cdn.staticfile.org/popper.js/1.12.5/umd/popper.min.js"></script>
 
<!-- 最新的 Bootstrap4 核心 JavaScript 文件 -->
<script src="https://cdn.staticfile.org/twitter-bootstrap/4.1.0/js/bootstrap.min.js"></script>
```

此外，你还可以使用以下的 CDN 服务：

- 国内推荐使用 : <https://www.staticfile.org/>
- 国际推荐使用：<https://cdnjs.com/>

### 下载 Bootstrap 4

你可以去官网 <https://getbootstrap.com/> 下载 Bootstrap4 资源库。

**注：**此外你还可以通过包的管理工具 npm、 gem、 composer 等来安装：

```bash
npm install bootstrap@4.0.0-beta.2
gem 'bootstrap', '~> 4.0.0.beta2'
composer require twbs/bootstrap:4.0.0-beta.2
```

## 创建第一个 Bootstrap 4 页面

### 1、添加 HTML5 doctype

Bootstrap 要求使用 HTML5 文件类型，所以需要添加 HTML5 doctype 声明。

HTML5 doctype 在文档头部声明，并设置对应编码:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"> 
  </head>
</html>
```

### 移动设备优先

为了让 Bootstrap 开发的网站对移动设备友好，确保适当的绘制和触屏缩放，需要在网页的 head 之中添加 viewport meta 标签，如下所示：

```html
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

`width=device-width` 表示宽度是设备屏幕的宽度。

`initial-scale=1` 表示初始的缩放比例。

shrink-to-fit=no 自动适应手机屏幕的宽度。

## 容器类

Bootstrap 4 需要一个容器元素来包裹网站的内容。

我们可以使用以下两个容器类：

- .container 类用于固定宽度并支持响应式布局的容器。
- .container-fluid 类用于 100% 宽度，占据全部视口（viewport）的容器。

![img](assets/176B67B9-013C-429C-8FD0-BC2409011545.jpg)

## 两个 Bootstrap 4 页面

* Bootstrap4 .container 实例

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

		<div class="container">
			<h1>我的第一个 Bootstrap 页面</h1>
			<p>这是一些文本。</p>
		</div>

	</body>

</html>
```

以下实例展示了占据全部视口（viewport）的容器。

* Bootstrap4 .container-fluid 实例

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
		<div class="container-fluid">
			<h1>我的第一个 Bootstrap 页面</h1>
			<p>使用了 .container-fluid，100% 宽度，占据全部视口（viewport）的容器。</p>
		</div>
	</body>
</html>
```

