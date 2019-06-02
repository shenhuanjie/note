# [ms-on](http://avalonjs.coding.me/)

# 事件绑定

此绑定为元素添加交互功能，对用户行为作出响应. `ms-on-*="xxx"`是其使用形式, `*`代表click, mouseover, touchstart等事件名，只能与小写形式定义， xxx是事件回调本身，可以是方法名，或表达式。 默认,事件回调的第一个参数是事件对象,并进行标准化处理. 如果你是用`ms-on-click="@fn(el, 1)"`这样的传参方式，第一个传参被你占用， 而你又想用事件对象，可以使用**$event**标识符，即`ms-on-click="@fn(el, 1, $event)"` 那么第三个参数就是事件对象。

如果你想绑定多个点击事件,可以用`ms-on-click-1="@fn(el)", ms-on-click-2="@fn2(el)",ms-on-click-3="@fn3(el)"`来添加。

并且,avalon对常用的事件,还做了快捷处理,你可以省掉中间的on。

avalon默认对以下事件做快捷处理:

```
animationend、 blur、 change、 input、 click、 dblclick、 focus、 keydown、 keypress、 keyup、 mousedown、 mouseenter、 mouseleave、 mousemove、 mouseout、 mouseover、 mouseup、 scroll、 submit
```

此外,avalon2相对avalon1，还做了以下强化:

以前`ms-on-*`的值只能是vm中的一个函数名`ms-on-click="fnName"`, 现在其值可以是表达式,如`ms-on-click="el.open = !el.open"`, 与原生的onclick定义方式更相近. 以前`ms-on-*`的函数,this是指向绑定事件的元素本身,现在this是指向vm, 元素本身可以直接从e.target中取得.

`ms-on-*`会优先考虑使用事件代理方式绑定事件,将事件绑在根节点上!这会带来极大的性能优化! `ms-on-*`的值转换为函数后,如果发现其内部不存在ms-for动态生成的变量,框架会将它们缓存起来! 添加了一系列针对事件的过滤器 对按键进行限制的过滤器esc，tab，enter，space，del，up，left，right，down 对事件方法stopPropagation, preventDefault进行简化的过滤器stop, prevent

```html
var vm = avalon.define({
    $id: "test",
    firstName: "司徒",
    array: ["aaa", "bbb", "ccc"],
    argsClick: function(e, a, b) {
        alert([].slice.call(arguments).join(" "))
    },
    loopClick: function(a, e) {
        alert(a + "  " + e.type)
    },
    status: "",
    callback: function(e) {
        vm.status = e.type
    },
    field: "",
    check: function(e) {
        vm.field = e.target.value + "  " + e.type
    },
    submit: function() {
        var data = vm.$model
        if (window.JSON) {
            setTimeout(function() {
                alert(JSON.stringify(data))
            })
        }
    }
})
<fieldset ms-controller="test">
    <legend>有关事件回调传参</legend>
    <div ms-mouseenter="@callback" ms-mouseleave="@callback">{{@status}}<br/>
        <input ms-on-input="@check"/>{{@field}}
    </div>
    <div ms-click="@argsClick($event, 100, @firstName)">点我</div>
    <div ms-for="el in @array" >
        <p ms-click="@loopClick(el, $event)">{{el}}</p>
    </div>
    <button ms-click="@submit" type="button">点我</button>
</fieldset>
```

绑定多个同种事件的例子：

```html
var count = 0
var model = avalon.define({
    $id: "multi-click",
    str1: "1",
    str2: "2",
    str3: "3",
    click0: function() {
        model.str1 = "xxxxxxxxx" + (count++)
    },
    click1: function() {
        model.str2 = "xxxxxxxxx" + (count++)
    },
    click2: function() {
        model.str3 = "xxxxxxxxx" + (count++)
    }
})
<fieldset>
    <legend>一个元素绑定多个同种事件的回调</legend>
    <div ms-controller="multi-click">
        <div ms-click="@click0" ms-click-1="@click1" ms-click-2="@click2" >请点我</div>
        <div>{{@str1}}</div>
        <div>{{@str2}}</div>
        <div>{{@str3}}</div>
    </div>
</fieldset>
```

回调执行顺序的例子：

```html
 avalon.define({
      $id: "xxx",
      fn: function() {
          console.log("11111111")
      },
      fn1: function() {
          console.log("2222222")
      },
      fn2: function() {
          console.log("3333333")
      }
  })
<div ms-controller="xxx" 
    ms-on-mouseenter-3="@fn"
    ms-on-mouseenter-2="@fn1"
    ms-on-mouseenter-1="@fn2"
    style="width:100px;height:100px;background: red;"
    >
</div>
```

avalon已经对ms-mouseenter, ms-mouseleave进行修复，可以在这里与这里了解这两个事件。 到chrome30时，所有浏览器都原生支持这两个事件。

```html
avalon.define({
    $id: "test",
    text: "",
    fn1: function (e) {
       this.text = e.target.className + " "+ e.type
    },
    fn2: function (e) {
       this.text = e.target.className + " "+ e.type
    }
})
.bbb{
    background: #1ba9ba;
    width:200px;
    height: 200px;
    padding:20px;
    box-sizing:content-box;
}
.ccc{
    background: #168795;
    width:160px;
    text-align: center;
    line-height: 160px;
    height: 160px;
    margin:20px;
    box-sizing:content-box;
}
    <div class="aaa" ms-controller="test">
      <div class="bbb" ms-mouseenter="@fn1" ms-mouseleave="@fn2">
          <div class="ccc" >
              {{@text}}
          </div>
      </div>
    </div>
```

最后是mousewheel事件的修改，主要问题是出现firefox上， 它死活也不愿意支持mousewheel，在avalon里是用DOMMouseScroll或wheel实现模拟的。 我们在事件对象通过wheelDelta属性是否为正数判定它在向上滚动。

```html
 avalon.define({
      $id: "event4",
      text: "",
      callback: function(e) {
          this.text = e.wheelDelta + "  " + e.type
      }
  })
<div ms-controller="event4">
    <div ms-on-mousewheel="@callback" id="aaa" style="background: #1ba9ba;width:200px;height: 200px;">
        {{@text}}
    </div>
</div>
```

此外avalon还对input，animationend事件进行修复，大家也可以直接用avalon.bind, avalon.fn.bind来绑定这些事件。但建议都用ms-on绑定来处理。