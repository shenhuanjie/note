# 微信小程序如何让text内容空格

```html
<text decode="{{true}}" space="{{true}}">&nbsp;&nbsp;</text>
```

由于小程序有这需求，就是让显示的一段话首行空两个空格，所以网上查了一下。

顺便也小结一下：

 在text标签中一定得加上`decode="{{true}}"`，然后在需要显示空格的地方放`&nbsp;`

想空几个空格就放几个`&nbsp;`