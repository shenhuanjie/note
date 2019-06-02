# 可见性绑定

这是通过修改元素的style.display值改变元素的可见性, 要求属性值对应一个布尔，如果不是布尔， avalon会自动转换值为布尔。

```html
<body :controller="test">
    <script>
        var vm = avalon.define({
            $id: "test",
            aaa: "这是被隐藏的内容",
            toggle: false
        })
    </script>
    <button type="button" :click='@toggle = !@toggle'>点我</button>
    <div :visible="@toggle">{{@aaa}}</div>
</body>
```

