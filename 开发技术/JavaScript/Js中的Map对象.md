# Js中的Map对象

## 定义

键/值对的集合。

## 语法

```js
mapObj = new Map()
```

## 备注

集合中的键和值可以是任何类型。如果使用现有密钥向集合添加值，则新值会替换旧值。

## 属性

下表列出了 Map 对象的属性和描述。

| 属性             | 描述                   |
| :--------------- | ---------------------- |
| 构造函数         | 指定创建映射的函数     |
| Prototype — 原型 | 为映射返回对原型的引用 |
| size             | 返回映射中的元素数。   |

## 方法

下表列出了 Map 对象的方法和描述。

| 方法     | 描述                                |
| -------- | ----------------------------------- |
| clear    | 从映射中移除所有元素。              |
| delete   | 从映射中移除指定的元素。            |
| forEach  | 对映射中的每个元素执行指定操作。    |
| get      | 返回映射中的指定元素。              |
| has      | 如果映射包含指定元素，则返回 true。 |
| set      | 添加一个新建元素到映射。            |
| toString | 返回映射的字符串表示形式。          |
| valueOf  | 返回指定对象的原始值。              |

下面的示例演示如何将成员添加到 Map，然后检索它们。

```js
var m = new Map();
m.set(1, "black");
m.set(2, "red");
m.set("colors", 2);
m.set({x:1}, 3);

m.forEach(function (item, key, mapObj) {
    document.write(item.toString() + "<br />");
});

document.write("<br />");
document.write(m.get(2));

// Output:
// black
// red
// 2
// 3
//
// red
```

