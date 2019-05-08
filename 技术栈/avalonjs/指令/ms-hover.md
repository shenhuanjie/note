# hover绑定

用于类实现:hover伪类的效果, 当用户鼠标移动元素上方时添加几个类名, 鼠标移走时将刚才的 类名移除

用法类似于[类名绑定](http://avalonjs.coding.me/directives/ms-class.html)

```html
<body ms-controller="test">
    <script>
        avalon.define({
            $id: 'test',
            aaa: "aaa bbb ccc",
            bbb: 'ddd',
            ccc: ['xxx', 'yyy', 'zzz'],
            ddd: 'eee',
            toggle: true,
            toggle2: false
        })

    </script>

    <div :hover="@aaa">直接引用字符串</div>
    <div :hover="@ccc">直接引用数组</div>
    <div :hover="[@aaa, @bbb,'kkk']">使用对象字面量</div>
    <div :hover="['aaa', 'bbb',(@toggle? 'ccc':'ddd')]">选择性添加类名</div>
    <div :hover="[@toggle && 'aaa']">选择性添加类名</div>
    <div :hover="[ @ddd + 4]">动态生成类名</div>
</body>
```