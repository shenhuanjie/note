# 类名绑定

属性绑定用于为元素节点添加几个类名, 因此要求属性值为字符串或字符串数组.

字符串形式下,可以使用空格隔开多个类名

字符串数组形下, 可以在里面使用三元运算符或与或号

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

    <div :class="@aaa">直接引用字符串</div>
    <div :class="@ccc">直接引用数组</div>
    <div :class="[@aaa, @bbb]">使用数组字面量</div>
    <div :class="['aaa', 'bbb',(@toggle? 'ccc':'ddd')]">选择性添加类名</div>
    <div :class="[@toggle && 'aaa']">选择性添加类名</div>
    <div :class="[ @ddd + 4]">动态生成类名</div>
</body>
```