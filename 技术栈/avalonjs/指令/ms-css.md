# 样式绑定

CSS绑定用于为元素节点添加一组样式, 因此要求属性值为对象或数组形式. 数组最后也会合并成一个对象.然后取此对象的键名为样式名, 键值为样式值为元素添加样式

> 如果键名为表示长宽,字体大小这样的样式, 那么键值不需要加单位,会自动加上px
>
> 如果键名如果为float,请务必在两边加上引号
>
> 如果键名如果为font-size,请务必转换为驼峰风格或两边加上引号

```html
<body ms-controller="test">
    <script>
        avalon.define({
            $id: 'test',
            obj: {backgroundColor: '#3bb0d0',width:300, height:50, 'text-align': 'center'},//属性名带-,必须用引号括起
            active: {color: 'red'},
            width:  300,
            height: 60,
            toggle: true,
            array: [{width:100},{height:50},{border: '1px solid #5cb85c'}]
        })

    </script>
  <div ms-css="@obj">直接引用对象</div>
  <div :css="{width: @width, height: @height,background: 'pink'}">使用对象字面量</div>
  <div :css="@array">直接引用数组</div>
  <div :css="[@obj, @toggle && @active ]" :click="@toggle = !@toggle">选择性添加多余属性或重写已有属性</div>
</body>
```

需要注意的是 设置背景图片是比较复杂

```html
<span :css="{background: 'url('+@imageUrl + ') no-repeat center center;'}">图片</span>
<span :css="{backgroundImage: 'url('+@imageUrl + ')'}">图片</span>
```