# skip绑定

让avalon的扫描引擎跳过某一部分区域, 方便能原样输出

合理使用ms-skip能大大提高性能

```html
<body :controller="test">
<script>
var vm = avalon.define({
  $id: "test",
  aaa: "XXXX"
  toggle: false
})
</script>
<div ms-skip='true' >{{@aaa}}</div>
<div>{{@aaa}}</div>
</body>
```

