# 文本绑定

文本绑定是最简单的绑定,它其实是双花括号插值表达式的一种形式

它要求VM对应的属性的类型为字符串, 数值及布尔, 如果是null, undefined将会被转换为空字符串

```html
<span ms-text="@aaa">不使用过滤器</span>
<span ms-text="@aaa | uppercase">使用过滤器</span>
```

