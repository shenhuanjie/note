# Bootstrap4 颜色

Bootstrap 4 提供了一些有代表意义的颜色类：.text-muted, .text-primary, .text-success, .text-info, .text-warning, .text-danger, .text-secondary, .text-white, .text-dark and .text-light：

<!--more-->
```html
<div class="container">
    <h2>代表指定意义的文本颜色</h2>
    <p class="text-muted">柔和的文本。</p>
    <p class="text-primary">重要的文本。</p>
    <p class="text-success">执行成功的文本。</p>
    <p class="text-info">代表一些提示信息的文本。</p>
    <p class="text-warning">警告文本。</p>
    <p class="text-danger">危险操作文本。</p>
    <p class="text-secondary">副标题。</p>
    <p class="text-dark">深灰色文字。</p>
    <p class="text-light">浅灰色文本（白色背景上看不清楚）。</p>
    <p class="text-white">白色文本（白色背景上看不清楚）。</p>
</div>
```

**在链接中使用**

```html
<div class="container">
  <h2>文本颜色</h2>
  <p>鼠标移动到链接。</p>
  <a href="#" class="text-muted">柔和的链接。</a>
  <a href="#" class="text-primary">主要链接。</a>
  <a href="#" class="text-success">成功链接。</a>
  <a href="#" class="text-info">信息文本链接。</a>
  <a href="#" class="text-warning">警告链接。</a>
  <a href="#" class="text-danger">危险链接。</a>
  <a href="#" class="text-secondary">副标题链接。</a>
  <a href="#" class="text-dark">深灰色链接。</a>
  <a href="#" class="text-light">浅灰色链接。</a>
</div>
```

## 背景颜色

提供背景颜色的类有: .bg-primary, .bg-success, .bg-info, .bg-warning, .bg-danger, .bg-secondary, .bg-dark 和 .bg-light。

注意背景颜色不会设置文本的颜色，在一些实例中你需要与 .text-* 类一起使用。

```html
<div class="container">
  <h2>背景颜色</h2>
  <p class="bg-primary text-white">重要的背景颜色。</p>
  <p class="bg-success text-white">执行成功背景颜色。</p>
  <p class="bg-info text-white">信息提示背景颜色。</p>
  <p class="bg-warning text-white">警告背景颜色</p>
  <p class="bg-danger text-white">危险背景颜色。</p>
  <p class="bg-secondary text-white">副标题背景颜色。</p>
  <p class="bg-dark text-white">深灰背景颜色。</p>
  <p class="bg-light text-dark">浅灰背景颜色。</p>
</div>
```

