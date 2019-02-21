# 第3章 初识React

React是Facebook推出的一个JavaScript库，它的口号就是”**用来创建用户界面的JavaScript库**“，所以它只是和用户的界面打交道，你可以把它看成MVC的V（视图）这一层。现在前端领域各种框架和库层出不穷，那么是什么原因让React如此流行呢？在本章中，我们会探索React非同寻常的特色。

简单来说，它有三大颠覆性的特点。

**1. 组件**

**React的一切都是基于组件的。**Web世界的构成是基于各种HTML标签的组合，这些标签天生就是语义化组件的表现，还有一些内容是这些标签的组合，比如说一组幻灯片、一个人物简介界面、一组侧边栏导航等，可以称之为自定义组件。React最重要的特性是基于组件的设计流程。使用React，你唯一要关心的就是构建组件。组件 有着良好的封装性，组件让代码的复用、测试和分离都变得更加简单。各个组件都有各自的状态，当状态变更时，便会重新渲染整个组件。组件特性不仅仅是React的专利，也是未来Web的发展趋势。React顺应了时代发展的方向，所以它如此流行也就变得顺其自然。

一个组件的例子如下。

```jsx
//Profile.jsx
import React from 'react';
export default Class Profile extends React.Component{
    render(){
        return (
            <div className="profile-component">
                <h2>Hi, I am {this.props.name}</h2>
            </div>
        )
    }
}
```

用这种方式，就实现了一个React的组件，在其他的组件中，可以像HTML标签一样引用它。

```js
import Profile from './profile';
export default function (props) {
    return {
        <Profile />
    }
}
```

**2. JSX**

通过上面的例子可以看出，在render方法中有一种直接把HTML嵌套在JS中的写法，它被称为JSX。这是一种类似XML的写法，它可以定义类似HTML一样简洁的树状结构。这种语法结合了JavaScript和HTML的优点，既可以像平常一样使用HTML，也可以在里面嵌套JavaScript语法。这种友好的格式，让开发者易于阅读和开发。而且，对于组件来说，直接使用类似HTML的格式，也是非常合理的。但是，需要注意的是。JSX和HTML完全不是一回事，JSX只是作为编译器，把类似HTML的结构编译成JavaScript。

当然，在浏览器中不能直接使用这种格式，需要添加JSX编译器来完成这项工作。

**3. Virtual DOM**

在React的设计中，开发者不太需要操作真正的DOM结点，每个React组件都是用Virtual DOM渲染的，它是一种对于HTML DOM结点的抽象描述，你可以把它看成是一种用JavaScript实现的结构，它不需要浏览器的DOM API支持，所以它在Node.js中也可以使用。它和DOM的一大区别就是它采用了更高效的渲染方式，组件的DOM结构映射到Virtual DOM上，当需要重新渲染组件时，React在Virtual DOM上实现了一个Diff算法，通过这个算法寻找需要变更的结点，再把里面的修改更新到实际需要修改的DOM结点上，这样就避免了整个渲染DOM带来的巨大成本。

React改变了传统强队开发的固定模式，当然还有React Native，把它的魔力扩展到Web应用以外。在下面的章节里，会根据这三大特性做详细的讲解。

## 3.1 使用React与传统前端开发的比较

凡事都有动机，在开始使用React之前，首先要搞清楚一件事：React到底给前端开发带来了什么改变，是什么支撑我们接受这样一个新事物的学习成本，把它的方案引入到工作中。

为了回答这些问题，我们决定从一个前端工程师的日常触发。

首先，以一个可选列表（单选）为例，列表由多个列表项组成，每个列表项有选中状态和普通状态，单击某项选中该项。最多可选中一项，即选中某项后先前的选中项变回普通状态。

### 3.1.1 传统做法

下面使用传统的方式来实现这个列表的逻辑。

```js
// 列表容器
const wrapper=${'#list-wrapper'};

// 列表所需数据
const data:{
    list:[1,2,3],
    activeIndex:-1
};

// 初始化行为
function init(){
    wrapper.on('click','li',function(){
        activate($(this).data('index'));
    });
    wrapper.html(template.render(data))
)

// 选中某项的行为
function activate(index){
    wrapper.find('li').removeClass('active');
    wrapper.find('li[data-index='+index+']').addClass('active');
    data.activeIndex = index;
}
```

下面是模版的内容。

```html
<ul>
    {{ each list item index }}
    	{{ if index === activeIndex }}
    		<li data-index="{{index}}" class="active">{{item}}</li>
    	{{ else }}
    		<li data-index="{{index}}">{{item}}</li>
    	{{ /if }}
    {{ /each }}
</ul>
```

列表组件的数据包含两部分：list及activeIndex。list是列表内容，activeIndex是当前选中项的索引。初始化（init）的时候，使用data渲染模版来产生原始DOM。在后续的交互中，每次单击列表项都触发一次选中（activate），activate方法会移除其他项的active状态，并给本次选中的项添加active状态，它同时也会更新activeIndex信息，使其与DOM元素的表现相符。

这是一种很常见的做法，尤其在许多比本例复杂得多的组件实现中很常见。我们习惯于以这种方式实现UI组件，而忽略了它严重的缺陷：需要同时维护数据及视图。模版引擎帮助我们解决了初始状态下二者的对应关系，即初始化时给出一个合理的数据就行了，视图会自动（通过模版渲染）生成。然而，在组件状态变化时，它们依然是需要被各自维护的。

对此，一般的MVVM框架的做法是，通过对模版的分析获取数据与视图元素（DOM节点）细致而具体的对应关系，然后对数据进行监控，在数据变化时更新对应的视图元素。然而，传统的流行模版基本都是通用模版，渲染仅仅是文本替换的过程，在语法上不足以支持MVVM的需求，因此一般的MVVM框架都会在HTML的基础上扩展得到一套独特的模版语法，使用自定义的指令（Directive）进行逻辑的描述，从而使得这部分逻辑可以被解析并复用。

独特的语法和丰富的指令同时也成为MVVM的门槛之一，我们可不可以不学习这些，也能得到复用“数据生成视图”逻辑的效果呢？其实有一个简单（然而问题很大）的做法。

### 3.1.2 全量更新

下面对先前的实现稍作改动。

```js
const wrapper = $('#list-wrapper');

const data: {
    list: [1, 2, 3],
    activeIndex: -1
};

function render() {
    wrapper.html(template.render(data));
}

function init() {
    wrapper.on('click', 'li', function () {
        activate($(this).data('index'));
    });
    render();
}

function activate(index) {
    data.activeIndex = index;
    render();
}
```

可以看到，把初始化时渲染的逻辑单独抽出为render方法，除在init时调用外，在选中某项（或任意的状态变更）时、更新数据后，同样可以通过直接调用render方法进行视图的更新。

模版本身就是一份“数据生成视图”的逻辑。这份逻辑得到复用的时候，只需要维护数据，每次数据发生变化时，渲染一下就能更新视图。如果对数据本身进行监听，这个行为甚至可以自动化完成。对于开发用户界面应用，这实在是一个再自然不过的做法，逻辑清晰，更容易被理解与维护。

然而，这只是一个再简单不过的Demo，在真实的业务中，这么做的缺陷比第一种做法更突出：①每次数据变动都要整体重新渲染，性能会非常差，尤其在数据变动频繁、界面复杂时；②每次渲染都重新生成所有的DOM节点，那么在这些DOM节点上绑定的事件及外部持有的对这些DOM节点的引用都将失效。

那么可不可以既享受第二种做法带来的逻辑实现上的优势，又避开它的缺陷呢？答案是React。

### 3.1.3 使用React

如果使用React把这个例子重新实现一遍的话，它可能是如下这样的（为了清晰简单，这里把list也放在组件的state中，在实际