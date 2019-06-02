# 循环绑定

avalon2.0的ms-for绑定集齐了ms-repeat, ms-each, ms-with的所有功能, 并且更好用, 性能提升七八倍

ms-for可以同时循环对象与数组

```html
<ul>
    <li ms-for="el in @aaa">{{el}}</li>
</ul>
```

现在采用类似angular的语法, in前面如果只有一个变量,那么它就是数组元素或对象的属性名

```javascript
vm.aaa = ['aaa','bbb','ccc']
vm.bbb = {a: 1, b: 2, c: 3}
<ul>
    <li ms-for="(aaa, el) in @aaa">{{aaa}}-{{el}}</li>
</ul
<ul>
    <li ms-for="(k, v) in @bbb">{{k}}-{{v}}</li>
</ul>
```

依次输出的LI元素内容为0-aaa,1-bbb,2-ccc, a-1,b-2,c-3

in 前面有两个变量, 它们需要放在小括号里,以逗号隔开, 那么分别代表数组有索引值与元素, 或对象的键名与键值, 这个与jQuery或avalon的each方法的回调参数一致。

> 小括号里面的变量是随便起的,主要能符合JS变量命名规范就行,当然,也不要与window, this这样变量冲突.

```html
 <li ms-for="($index, el) in @arr">{{$index}}-{{el}}</li>
 <li ms-for="($key, $val) in @obj">{{$key}}-{{$val}}</li>
```

写成这样,就与avalon1.*很相像了

ms-for还可以配套**data-for-rendered**回调,当列表渲染好时执行此方法

```html
<ul>
   <li ms-for="el in @arr" data-for-rendered='@fn'>{{el}}</li>
<ul>
```

fn为vm中的一个函数，用法与`ms-on-*`差不多，如果不传参，默认第一个参数为事件对象，类型type为`rendered`， target为当前循环区域的父节点，这里为｀ul`元素。并且回调中的this指向vm。

```javascript
{type: 'rendered', target: ULElement }
```

你也可以在回调里面传入其他东西，使用`$event`代表事件对象

```html
<ul>
   <li ms-for="el in @arr" data-for-rendered="@fn('xxx',$event)">{{el}}</li>
<ul>
```

如果你想截取数组的一部分出来单独循环,可以用limitBy过滤器, 使用as来引用新数组

```html
<ul>
    <li ms-for="el in @aaa | limitBy(10) as items">{{el}}</li>
</ul>
```

上例是显示数组的前10个元素, 并且将这10个元素存放在items数组中, 以保存过滤或排序结果

使用注释节点实现循环,解决同时循环多个元素的问题

```html
<!DOCTYPE html>
<html>
    <head>
        <title>TODO supply a title</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <script src="../dist/avalon.js"></script>
        <script>
            vm = avalon.define({
                $id: 'for4',
                arr: [1, 2, 3, 4]
            })
        </script>
    </head>
    <body>
        <div ms-controller='for4'  >
            <!--ms-for: el in @arr-->
            <p>{{el}}</p>
            <p>{{el}}</p>
            <!--ms-for-end:-->
        </div>
    </body>
</html>
```

avalon 不需要像angular那样要求用户指定trace by或像react 那样使用key属性来提高性能,内部帮你搞定一切

如果你只想循环输出数组的其中一部分,请使用filterBy,只想循环输出对象某一些键值并设置默认值,则用selectBy. 不要在同一个元素上使用ms-for与ms-if,因为这样做会在页面上生成大量的注释节点,影响页面性能

可用于ms-for中的过滤器有limitBy, filterBy, selectby, orderBy

> ms-for支持下面的元素节点继续使用ms-for,形成双重循环与多级循环, 但要求双重循环对应的二维数组.几维循环对应几维数组

```javascript
vm.array = [{arr: [111,222, 333]},{arr: [111,222, 333]},{arr: [111,222, 333]}]
<p>array的元素里面有子数组,形成2维数组</p>        
<ul>
    <li ms-for="el in @array"><div ms-for='elem in el.arr'>{{elem}}</div></li>
</ul>
```

**如何双向绑定ms-for中生成的变量?**

由于 循环生成的变量前面不带@, 因此就找不到其对应的属性,需要特别处理一下

```
<div ms-controller="test">
<div ms-for="(key,el) in @styles">
        <label>{{ key }}::{{ el }}</label>
        <input type="text" ms-duplex="@styles[key]" >
        <!--不能ms-duplex="el"-->
    </div>
</div>

<script type="text/javascript">
    var root = avalon.define({
        $id: "test",
        styles: {
            width: 200,
            height: 200,
            borderWidth: 1,
            borderColor: "red",
            borderStyle: "solid",
            backgroundColor: "gray"
        }
    })




</script>
```

**过滤器** 可以到[这里获取详情](http://avalonjs.coding.me/directives/filter.html#循环过滤器)