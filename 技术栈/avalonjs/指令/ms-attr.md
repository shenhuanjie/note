# 属性绑定

属性绑定用于为元素节点添加一组属性, 因此要求属性值为对象或数组形式. 数组最后也会合并成一个对象.然后取此对象的键名为属性名, 键值为属性值为元素添加属性

如果键名如果为for, char这样的关键字,请务必在两边加上引号

如果键名如果带横杠,请务必转换为驼峰风格或两边加上引号

注意,不能在ms-attr中设置style属性

```html
<p ms-attr="{style:'width:20px'}">这样写是错的,需要用ms-css指令!!</p>
```

示例:

```html
<body ms-controller="test">
    <script>
        avalon.define({
            $id: 'test',
            obj: {title: '普通 ', algin: 'left'},
            active: {title: '激活'},
            width: 111,
            height: 222,
            arr: [{img: 'aaa'}, {img: 'bbb'}, {img: 'ccc'}],
            path: '../aaa/image.jpg',
            toggle: false,
            array: [{width: 1}, {height: 2}]
        })

    </script>
    <span ms-attr="@obj">直接引用对象</span>
    <img ms-attr="{src: @path}" />
    <ul>
        <li ms-for="el in @arr"><a ms-attr="{href:'http://www.ccc.xxx/ddd/'+ el.img}">下载</a></li>
    </ul>
    <span :attr="{width: @width, height: @height}">使用对象字面量</span><br/>
    <span :attr="@array">直接引用数组</span><br/>
    <span :attr="[@obj, @toggle && @active ]" :click="@toggle = !@toggle">选择性添加多余属性或重写已有属性</span>
</body>
```