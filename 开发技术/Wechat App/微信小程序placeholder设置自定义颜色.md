# 微信小程序placeholder设置自定义颜色

```html
<view class='inp'>
	<input placeholder-class="phcolor" class="input-text" name="username" placeholder="测试placeholder" />
</view>
```

```css
.phcolor{
    color:#18acff;
}
```

placeholder设置自定义颜色，在input中添加`placeholder-class=""`标签，在css样式中添加，用到的样式即可。