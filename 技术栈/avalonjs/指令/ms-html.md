# HTML绑定

HTML绑定类似于文本绑定,能将一个元素清空,填上你需要的内容

它要求VM对应的属性的类型为字符串

```html
<span ms-html="@aaa">不使用过滤器</span>
<span ms-html="@aaa | uppercase">使用过滤器</span>
```

我们可以通过ms-html异步加载大片内容。

```html
<body :controller="test">
<script>
var vm = avalon.define({
  $id: "test",
  aaa: "loading..."
})
jQuery.ajax({
   url:'action.do',
   success: function(data){
      vm.aaa = data.html
   }
})
</script>
<div ms-html="@aaa"></div>
</body>
```

