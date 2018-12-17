# js对undefined的处理

JavaScript 中有两个特殊数据类型：undefined 和 null，先看看 undefined 的判断，欢迎各位同仁交流一番：
第一次碰见undefined的时候，我用的是java那一套，我是这样处理的。

```js
if (obj== undefined){
　　/*逻辑*/
}
```

事实说明我是自作聪明了，查询之发现，大家通常正确的做法是这样的。

```js
if (typeof(obj) == "undefined") { 
   /*逻辑*/ 
}
```

为什么会这样的呢？js怎么会多出这样一种数据类型呢？undefined是怎样一种存在呢？接下来就走进科学吧，

大多数计算机语言，有且仅有一个表示"无"的值，比如，用过可知C语言的NULL，Java语言的null，查询可知Python语言的None，Ruby语言的nil，但是javascript是不一样的烟火，它有两个表示"无"的值：undefined和null。这是为什么？

**1、历史的行程**

1995年JavaScript诞生时如早一年的Java一样，用null作为表示"无"的值。根据C语言的传统，null被设计成可以自动转为0，设计Brendan Eich觉得这样做还不够，因为，null在Java里被当成一个对象。但是，JavaScript的数据类型分成原始类型（primitive）和合成类型（complex）两大类，Brendan Eich觉得表示"无"的值最好不是对象。其次，JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误。因此，Brendan Eich又设计了一个undefined。

**2.关于undefined**

undefined 表示一个未声明的变量，或已声明但没有赋值的变量，或一个并不存在的对象属性，函数没有返回值时，默认返回undefined。这是undefined的几种典型用法，而判断一个变量是不是undefined，用typeof函数，typeof函数主要是返回的是字符串，主要这么几种："number"、"string"、"boolean"、"object"、"function"、"undefined"