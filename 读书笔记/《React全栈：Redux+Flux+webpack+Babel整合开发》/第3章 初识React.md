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

如果使用React把这个例子重新实现一遍的话，它可能是如下这样的（为了清晰简单，这里把list也放在组件的state中，在实际的实现中，通过props将list传入组件会更合适）。

```jsx
class List extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            list: [1, 2, 3],
            activeIndex: -1
        };
    }
    activate(index) {
        this.setState({
            activeIndex: index
        });
    }
    render() {
        const { list, activeIndex } = this.state;
        const lis = list.map(
            (item, index) => {
                const cls = index === activeIndex ? 'active' : '';
                return (
                    <li
                        key={index}
                        className={cls}
                        onClick={() => this.activate(index)} >
                    </li >
                )
            }
        )
        return (
            <ul> {lis}</ul >
        );
    }
}
```

不难看出，除了React特有的Component接口、字段及JSX语法等，在基于React Component的实现中，整体逻辑组成与第二个例子是类似的：首次选了通过render得到界面，每个li的点击出发activate方法，activate方法调用setState更新状态信息，setState会出发重新render（React提供了这一机制），从而使界面得到更新。

那么刚才的两个缺点是怎么解决的呢？

在具体分析之前，首先要说明一点：JSX内容的渲染结果其实不是真实的DOM节点，本质上是JavaScript对象的虚拟DOM节点，它记录了这个节点的所有信息，可以依据一定的规则生成对应的真实DOM节点。

**1. 性能问题**

我们知道前端的性能瓶颈大多数时候都在于操作DOM，所以如果避开操作DOM，只是重新生成虚拟的DOM节点（JavaScript对象），本身是很快的。在将虚拟的DOM对应生成真实DOM节点之前，React会将虚拟的DOM树与先前的进行比较（Diff），计算出变化的部分，再将变化的部分作用到真实DOM上，实现最终界面的更新。得益于Diff算法的高效，整个过程的代价大致接近于最终操作真实DOM的代价，即与最初的实例很接近了。二者的区别在于，React以额外的计算量换取了对于更新点的自动定位，以框架本身复杂的代码实现换取了业务代码逻辑的清晰简单。

**2. DOM事件与引用失效**

在React的哲学里，直接操作DOM是典型的反模式。React对DOM事件进行了封装并提供了相应的接口。值得注意的是，React提供的事件绑定接口与其界面声明方式是一脉相承的，事件绑定表现为，值为回调函数的组件属性（props）。这样的好处是，绑定事件的过程自然地变成了界面渲染（render）的一部分，无须特别处理。

在事件绑定与读/写操作都被React通过抽象层屏蔽后，业务代码基本无须接触真实DOM，需要持有引用的场景自然也不复存在，引用失效也就无从说起。

### 3.1.4 小结

最后总结一下，React的出现允许我们以简单粗暴的方式构建我们的界面：仅仅声明数据到视图的转换逻辑，然后维护数据的变动，自动更新视图。它看起来很像每次状态更新时，都需要整体地更新一次视图，但React的抽象层避免了这一做法带来的弊端，让这一开发方式变得可行。

## 3.2 JSX

### 3.2.1 来历

下面这一段是官方文档中的引用，它可以解释JSX这种写法诞生的初衷。

We strongly believe that components are the right way to sparate concerns rather than “templates” and “display logic.” We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

多年依赖，在传统的开发中，把模板和功能分离看作是最佳实践的完美例子，翻阅形形色色的框架文档，总有一个模板文件夹里面放置了对应的模板文件，然后通过模板引擎处理这些字符串，来生成把数据和模板结合起来的字符。而React认为世界是基于组件的，组件自然而然和模板相连，把逻辑和模板分开放置是一种笨重的思路。所以，React创造了一种名为JSX的语法格式来架起他们之间的桥梁。

### 3.2.2 语法

**1. JSX不是必需的**

JSX编译器把类似HTML的写法转换成原生的JavaScript方法，并且会将传入的属性转化为对应的对象。它就类似于一种语法糖，把标签类型的写法转换成React提供的一个用来创建ReactElement的方法。

```jsx
const MyComponent;
//input JSX,在JS中直接写类似HTML的内容，前所未有的感觉。其实它返回的是一个
//ReactElement
let app = <h1 title="my title">this is my title</h1>;
//JSX转换后的结果
let app = React.createElement('h1',{title:'my title'},'this is my title');
```

**2. HTML标签与React组件**

React可以直接渲染HTML类型的标签，也可以渲染React的组件。

React组件会在下面几章详细介绍，这里读者可以把它看作一个特殊对象。

HTML类型的标签第一个字母用小写来表示。

```jsx
import React from 'react';
// 当一个标签里面为空的时候，可以直接使用自闭和标签
// 注意class是一个JavaScript保留字，所以如果要写class应该替换成className
let divElement = <div className="foo" />
// 等同于
let divElement = React.createElement('div', { className: 'foo' });
```

React组件标签第一个字母用大写来表示。

```jsx
import React from 'react';
class Headline extends React.component {

    render() {
        // 直接return JSX语法
        return <h1>Hello React</h1>
    }
}
let headline = <Headline />
// 等同于
let headline = React.createElement(Headline);
```

