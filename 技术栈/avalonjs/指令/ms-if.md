# if绑定

通过属性值决定是否渲染目标元素, 为否时原位置上变成一个注释节点

> `avalon1.*`中ms-if-loop指令已经被废掉,请使用limitBy, selectBy, filterBy过滤器代替相应功能

```html
<body :controller="test">
<script>
var vm = avalon.define({
  $id: "test",
  aaa: "这是被隐藏的内容"
  toggle: false
})
</script>
<p><button type="button" :click='@toggle = !@toggle'>点我</span></button></p>
<div :if="@toggle">{{@aaa}}</div>
</body>
```

> 注意,有许多人喜欢用ms-if做非空处理,这是不对的,因此ms-if只是决定它是否插入DOM树与否,ms-if里面的 **ms-**指令还是会执行.

```html
avalon.define({
  $id: 'test',
  aaa: {}
})
<div ms-controller="test">
   <div ms-if="@aaa.bbb">
     {{@aaa.bbb.ccc}}这里肯定会出错
   </div>
</div>
```

正确的做法是,当你知道这里面有非空判定,需要用方法包起来

```html
avalon.define({
  $id: 'test',
  aaa: {},
  show: function(aaa, bbb, ccc){
     var obj = aaa[bbb]
     if(obj){
        return obj[ccc]
     }
     return ''
  }
})
<div ms-controller="test">
   <div ms-if="@aaa.bbb">
     {{@show(@aaa, 'bbb', 'ccc')}}
   </div>
</div>
```

