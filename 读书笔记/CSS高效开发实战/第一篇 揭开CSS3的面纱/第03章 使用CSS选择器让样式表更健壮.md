# 第3章 使用CSS选择器让样式表更健壮

本章主要介绍CSS选择器的知识，选择器可以帮助我们选择特定的某个HTML元素，或有共同规律的某组HTML元素，从而可以为选中的元素添加样式。合理地使用选择器可以使项目拥有更好的可维护性。

本章主要知识点：

* CSS的基础选择器。
* CSS的伪类选择器。
* 选择器的应用案例。

## 3.1 基础选择器

本节主要介绍最常用的一些选择器，这些选择器在任何浏览器下都支持，是CSS学习中必须掌握的基础内容。

### 3.1.1 标签选择器

标签选择器是最简单的选择器，它的命名只要和对应的HTML标签相同即可，例如：

```css
h1{
    font-size:30px;
    color:#333;
}
```

这里的h1就是标签选择器，在实际项目中，标签选择器一般用于定义全局样式。和Office中的Word类似，在全局定义中预定义好正文和标签的样式、段落之间的间距、图片的最大宽度和对齐方式等。全局定义只需要定义一次，就可以在本文档的任何地方使用，如果修改，也只需要修改一次。合理地使用标签选择器定义全局样式，可以大大减轻开发和维护的工作量。

下面的示例是节选Bootstrap框架汇中关于标题的全局定义，读者可以研习一下标签选择器在实际项目中的应用。

```css
h1,h2,h3,h4,h5,h6{
    margin:10px 0;
    font-famliy:inherit;
    font-weight:bold;
    line-heiht:20px;
    color:inherit;
    text-rendering:optimizeleglbility;
}

h1,h2,h3{
    line-height:40px
}

h1{font-size:38.5px;}
h2{font-size:31.5px;}
h3{font-size:24.5px;}
h4{font-size:17.5px;}
h5{font-size:14px;}
h6{font-size:11.9px;}
```

### 3.1.2 类选择器

类选择器也称为class选择器，它的语法非常简单，在class名称前面加上一个“.“符号。例如：

```html
<div class="red content">
    
</div>
```

```css
.red{
    background:red;
}
.content{
    height:100px;
    width:100%;
}
```

由于一个HTML标签可以定义多个class属性，因此在实际应用中，类选择器称为了最灵活、应用最广泛的选择器。基本上任何样式定义都可以通过为元素追加class属性，然后定义该class的样式来完成，可谓是“万金油”方法。

> **注意：**从代码的可读性、可维护性角度来讲，不要滥用类选择器，尽量不要为一个标签添加多于两个的class属性。尽量为class指定有意义的命名，避免x1、x2、y1、y2这样的无意义命名。

下面仍然节选一段Bootstrap中的代码供读者参考：

```css
.icon-heart{
    background-position:-96px 0;
}
.icon-star{
    background-position:-120px 0;
}
.icon-star-empty{
    background-position:-144px 0;
}
.icon-user{
    background-position:-168px 0;
}
.icon-film{
    background-position:-192px 0;
}
```

注意看这里的命名规则，icon开头表示这个class是在描述一个icon（小图标），而后缀则表示这个icon的意义，比如icon-heart一看就知道是一个心型图标的意思，这样即使不看实际效果，也能知道这段CSS的作用，大大提高了代码的可读性。

### 3.1.3 id选择器

id选择器的语法是一个“#”号加上id的名称，例如：

```html
<div id="user_123">
    
</div>
```

```css
#user_123{
    width:120px;
    line-height:30px;
    height:30px;
}
```

一个HTML元素只能对应一个id，所以id选择器在灵活性上不如class选择器，因此在实战中很少会直接在CSS文件中为id定义样式。

id选择器在实战中一般有两个用途：

* id选择器拥有最高的权重，因此可以用于覆盖之前的一些定义。
* 和后台数据对应，从而配合JavaScript进行一些逻辑操作。

### 3.1.4 通配符选择器

通配符的意思就是用一个符号来代替某些字符，例如在Word中要搜索以com开头的所有单词，可以用“`com*`”来做搜索关键字，这个*表示任意字符，这个时候可能就会搜到computer、compact、combo等以com开头的单词。

CSS从CSS2时代开始就引入了一种简单选择器——通配符选择器（universal selector），它以星号（*）开始，该选择器可以与任意元素匹配。例如，下面的规则可以使文档中的每个元素都显示为红色：

```css
*{
    color:red;
}
```

这个声明等价于列出了文档中所有元素的一个分组选择器。

通配符选择器在实际开发中可用于定义全局样式，不过使用标签选择器也能获得类似效果，例如：

```css
body{
    color:red;
}
html{
    color:red;
}
```

> **注意：**通配符选择器的权重是最低的，因此只要有其他的定义，使用通配符选择器进行的定义就会被覆盖。

### 3.1.5 子元素选择器

子元素选择器用于表示某些特定HTML嵌套关系时的样式展现，其语法关键词是一个“>“符号。例如：

```html
<li>
    <a href='#'>www.baidu.com</a>
</li>
<li>
    <div>
    	<a href='#'>www.baidu.com</a>
    </div>
</li>
```

```css
li>a{
    color:red;
}
```

“>“左边是父元素，右边是子元素。上面的代码就表示第一个列表项（`<li>`）中的链接（`<a>`）为蓝色，而其他地方的链接则不受影响。

> **注意：**如果两个元素不是严格的“父子关系”，则使用子元素选择器的定义不会生效。例如上面HTML代码中的第2个列表项的文字就不会设置为蓝色。

### 3.1.6 后代元素选择器

后代元素选择器类似于子元素选择器，只不过它的要求不那么严格。它的语法关键词

