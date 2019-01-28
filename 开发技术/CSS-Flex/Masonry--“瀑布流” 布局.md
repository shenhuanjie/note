# Masonry--“瀑布流” 布局

Masonry.js是一个以“瀑布流”布局呈现网页元素的JS库，它可以使多个不规则宽高的元素以恰当的顺序排列 ，增加美观度。

基本的使用方法如下（以下内容翻译自masonry官网： <https://masonry.desandro.com/>）：

### **HTML：**

1. 导入masonry.js库
2. 以两层结构包裹将要使用这种布局的元素，结构如下：

```html
<div class="grid">
  <div class="grid-item">...</div>
  <div class="grid-item grid-item--width2">...</div>
  <div class="grid-item">...</div>
  ...
</div>
```

在.grid-item里面分别放置显示的内容元素。

### **CSS:**

item元素的尺寸设置全部在CSS内进行，如：

```css
.grid-item {
    width: 200px; 
}
.grid-item--width2 { 
    width: 400px; 
}
```

### **初始化：**

提供三种初始化的方式，可按任意选择

1. 以Jquery插件的方式

```javascript
$('.grid').masonry({
   itemSelector: '.grid-item',
   columnWidth: 200
});
```

 对选中元素应用masonry方法，并设置初始化参数（参数为对 象格式）。

2. 以原生JS方式初始化：

```javascript
var elem = document.querySelector('.grid');
var msnry = new Masonry( elem, {
　//属性设置
  itemSelector: '.grid-item',
  columnWidth: 200
});

var msnry = new Masonry( '.grid', {
  //属性设置
});
```

第一个参数为外层容器元素（可以是一个原生DOM对象；或者是能唯一确定该元素的，以“’选择器”语 法表示的字符串），第二个参数是初始化配置对象。

3. 直接在HTML中初始化：

```html
<div class="grid" data-masonry='{ "itemSelector": ".grid-item", "columnWidth": 200 }'>
```

在自定义属性data-masonry里直接初始化配置对象。选择这种方式 的话，就无需另写JS了。

**注意：** 配置对象必须是有效的JSON格式对象，键名必须使用双引号包围，而因此 data-masonry的值需要使用单引号。

另外，在Masonry v3中初始化是对外层容器元素添加js-masonry类 ，并在data-masonry-options中进行的初始化，Masonry v4能够向下兼 容这种写法。

##  **布局**

所有尺寸与样式设置都由CSS完成，如：

```css
.grid-item {
  float: left;
  width: 80px;
  height: 60px;
  border: 2px solid hsla(0, 0%, 0%, 0.5);
}

.grid-item--width2 {
    width: 160px; 
}
.grid-item--height2 { 
    height: 140px; 
}
```

### **响应式布局**

item的尺寸可以设置为百分比来满足响应式设计的需求，如：

```html
<div class="grid">
  <!-- width of .grid-sizer used for columnWidth -->
  <div class="grid-sizer"></div>
  <div class="grid-item"></div>
  <div class="grid-item grid-item--width2"></div>
  ...
</div>
```

```css
/* 元素宽度为布局空间宽度的20% */
.grid-sizer,
.grid-item { 
    width: 20%; 
}

/* 40% */
.grid-item--width2 { 
    width: 40%; 
}
```

```javascript
$('.grid').masonry({
  // 将itemSelector设为.grid-item，这样.grid-sizer就不会被作为item的宽度使用
  itemSelector: '.grid-item',
  // 直接以类来设置属性
  columnWidth: '.grid-sizer',
  percentPosition: true
})
```

### **imagesLoaded**

未加载完全的图片可能会脱离Masonry的布局，并导致item元素发生重叠，而使用imagesLoaded可以解决这个问题。 imagesLoaded是一款独立的JS脚本，你可以在[这里](https://imagesloaded.desandro.com/)看到更多与imagesloaded相关的信息。

> **例子：** https://codepen.io/desandro/pen/bIFyl

初始化Masonry，然后在每张图片加载完成后触发layout，如：

```javascript
// 初始化 Masonry
var $grid = $('.grid').masonry({
  // 属性设置...
});
// 每张图片加载完成后触发layout
$grid.imagesLoaded().progress( function() {
  $grid.masonry('layout');
});
```

或者，你可以在所有图片加载完成后初始化Masonry，如：

```javascript
var $grid = $('.grid').imagesLoaded( function() {
  // 在所有图片加载完成后初始化Masonry
  $grid.masonry({
    // 属性设置...
  });
});
```

