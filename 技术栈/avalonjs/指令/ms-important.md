#  important绑定

这个指令是用于圈定某个VM的作用域范围(换言之,这个元素的outerHTML会被扫描编译,所有`ms-*`及双花括号替换成vm中的内容),ms-important的属性值只能是某个VM的$id

ms-important的元素节点下面的其他节点也可以使用[ms-controller](http://avalonjs.coding.me/directives/ms-controller.html)或ms-important

> 与ms-controller不一同的是,当某个属性在ms-important的VM找不到时,就不会所上寻找

 

> 不要在ms-for内使用ms-important.

ms-important这特性有利协作开发,每个人的VM都不会影响其他人,并能大大提高性能

ms-important只能用于ms-controller的元素里面

```html
  <div ms-important='aaa'>
      <div ms-controller='ccc'>
           <div ms-important='ddd'>

          </div>
          </div>
      <div ms-controller='bbb'>

      </div>
  </div>
```