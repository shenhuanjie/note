# 第6章 使用Redux

第5章介绍了Flux和Redux的基础知识和实现，本章将把Redux用到实战中。首先介绍Redux和React是怎样完美配合的，然后把第4章完成的Deskmark程序基于Redux加以改造，完成后读者可以对比一下两种方法实现的异同。

## 6.1 在React项目中使用Redux

Redux把自己定位成为一般的JavaScript应用提供状态的容器，通过极为有限的几个接口来提供强大的功能。

不过不难看出的是，Redux着眼于对状态整体的维护，而不会产生出具体变动的部分。React是一个由状态整体输出界面整体的view层实现，因此非常适合搭配Redux实现。那么在React项目中，如何使用Redux才是比较合适的做法？有哪些要注意的点呢？

### 6.1.1 如何在React项目中使用Redux

在Redux的使用中，创建store的方式总是相似的。当我们在说如何使用Redux的时候，说的其实是如何获取并使用store的内容（状态数据），以及创建并触发action的过程。

**1. 从store获取数据**

首先，来看如何获取并使用store中的状态数据

* 属性传递

Redux的特点是单一数据源，即整个应用的状态信息都保存在一个store中，因而需要由store将数据从React组件树的根节点传入。为了在数据变化时更新界面，还需要对store进行监听。

```js
function render() {
    const state = store.getState();
    React.render(
        <App state={state} />,
        document.getElementById('app')
    );
}

store.subscribe(render);
render;
```

这样，在组件APP中，可以通过this.props.state获取状态信息。组件App可以进一步将状态信息以props的形式传递给其子孙结点，像如下这样。

```js
class App extends React.Component {
    render() {
        return (
            <Header content={this.props.title} />
        );
    }
}
```

正如例子中的组件Header，每个组件都可以从上层节点获取它所需要的信息。

然而，在项目规模变大、组件树层级变多时，我们会发现一些弊端。为了说明这一点，首先来看一个例子。

这个例子是一个TodoList，应用到根节点是组件App，页面的底层部分被我们抽取为组件Footer。在页面底部提供了一些链接：“All”、“Active”及“Completed”，它们可作为筛选的条件对当前展示项进行切换。如单击“All”的时候，列表展示所有项；单击“Active”时，列表只展示当前进行中的项。我们把这部分功能单独做成一个组件TodoFilter。那么，现在组件是如下这样一个层级关系。

```js
App -> Footer -> TodoFilter
```

我们的数据如下。

```js
{
    "filter": "all",
    "list": [
        {
            "text": "Hey!",
            "completed": false
        },
        {
            "text": "Yo!",
            "completed": true
        }
    ]
}
```

组件App中引入组件Footer，组件Footer中包含了组件TodoFilter，组件TodoFilter中会包含我们所说的用于切换筛选条件的链接。当前生效的筛选条件所对应的链接需要被高亮，因此TodoFilter需要获取状态中的filter信息。按照前面总结的状态数据获取方式，这份数据需要由组件TodoFilter的父节点，即组件Footer传递过来，组件Footer的数据又由组件App提供。那么，组件Footer与TodoFilter一样，都要求有filter这样一个属性，如下。

```js
TodoFilter.propTypes = {
    filter: PropTypes.string.isRequired
};

Footer.propTypes = {
    filter: PropTypes.string.isRequired
};
```

问题开始浮现出来：当应用中的一个组件需要某份数据的时候，这个组件，以及它所有的祖先节点（除了根节点外），都需要添加一个对应的属性来为它层层传递这份数据。尤其是这里的组件Footer，它的定位只是展示页面底部信息，这样一个原意不涉及业务逻辑的组件，被要求通过属性获得一个filter的数据是不合理的。这不仅让组件Footer本身的接口变得令人费解，也造成了组件Footer与其内容的耦合。当增删Footer中与Footer本身行为无关的子元素的时候，需要额外地修改Footer的接口，以及插入Footer的代码。

这一问题在组件树层级变多时会急剧地恶化，因为如果某个组件处在组件树的第*n*级，那么夹在这个组件与提供数据的根节点之间的*n-1*个组件，都是像上述Footer那样的受影响者。每修改一个属性，或增删一个组件，都需要*n-1*处改动，这是无法想象的维护成本。

该如何解决这个问题呢？

* 组件自行获取状态数据

一个显而易见的思考方向是，如何像上例中的TodoFilter这样的组件自己去获取store中的数据，而不依赖父节点的传入，*n-1*的噩梦便不复存在。

对应的做法是，把createStore的结果通过一个独立的模块以module export的方式暴露出来，所以组件都可以直接去import这个模块获得store，然后对store进行subscribe。

```jsx
// store.js
import reducers from 'reducers';
export default createStore(reducers);
```

```jsx
//TodoFilter.js
import React from 'react';
import store from 'store';

export default class TodoFilter extends React.Component {
    constructor() {
        super();
        this.state = {
            filter: null
        };
    }
    componentDidMount() {
        store.subscribe(() => {
            this.setState({
                filter: store.getState().filter
            })
        })
    }
    render() {
        // 界面渲染
    }
}
```

为了将状态反映到界面，可以将这部分数据放到组件的state中。在合适的时机（在组件挂载完成后）对store进行监听，在store每次更新时，取出其中的最新数据，并通过组件本身的setState方法来更新组件的state信息，这个方法会自动触发组件的重新渲染，从而使界面得到同步。

通过这种方式，我们顺利地解决了前面所遇到的问题，但是细想之下，我们的做法依然存在一些问题。

1. 组件私自与store建立联系，导致数据流难以追溯。
2. 拥有内部state的组件不便于测试。
3. 在每个需要访问store的组件中都实现一份subscribe&setState这样的逻辑略显烦琐。

幸而，相比先前，这些都可以算是小问题，这些问题有没有成熟的、通用的方案来避免呢？将在后面介绍。

**2. 创建与触发action**

在前面介绍过，在Flux架构中，store的状态改变必须由action引起，除了获取与使用状态数据外，另一个组件与store打交道的方式便是创建与触发action。在Redux中，这个过程包含两个部分。

* 创建action，即使用actionCreator。
* 触发action，即通过store.dispatch将上一步得到的action作用到特定的store上。

下面将分别讨论这两部分行为。

* 从actions获得actionCreator。
* 如何获得dispatch方法。

在获取创建与触发action的方法时，面临了与获取store中数据相似的问题。不难发现的是，获取触发action的方法dispatch与获取store中的数据是类似的逻辑，可以通过相似的方式来实现。下面将介绍针对这一问题的官方答案：react-redux。

### 6.1.2 react-redux

react-redux是Redux官方提供的React绑定，用于辅助在React项目中使用Redux，其特点是性能优异且灵活强大。

它的API相当简单，包括一个React Component(Provide)和一个高阶方法connect。

**1. Provider**

顾名思义，Provider的作用主要是“provide”。Provider的角色是store的提供者。一般情况下，把原有的组件树根节点包裹在Provider中，这样整个组件树上的节点都可以通过connect获取store。

```jsx
ReactDOM.render(
    <Provider store={store}>
        <MyRootComponent />
    </Provider>,
    rootEl
)
```

**2. connect**

connect是用来“连接”store与组件的方法，它常见的用法是如下这样的。

```jsx
import { add } from 'actions';

function mapStateToProps(state) {
    return {
        num: state.num
    };
}

function mapDispatchToProps(dispatch) {
    return {
        onBtnClick() {
            dispatch(add());
        }
    };
}

function Counter(props) {
    return (
        <p>
            {props.num}
            <button onClick={props.onBtnClick}>+1</button>
        </p>
    )
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

在这个示例中，我们通过connect让组件Counter得以连接store，从store中取得num信息并在按钮单击的时候出发store上的add方法（这里的add是个action creator，执行结果是一个action）。

* **enhancer**

为了便于理解上面的例子中发生了什么，首先介绍下connect(mapStateToProps,mapDispatchToProps)的执行结果——一个典型的“高阶组件”（HoC，higher-order components），这里我们称之为enhancer。

高阶组件是指符合以下条件的函数：接收一个已有的组件作为参数，并返回一个新的组件，后者将前者封装于内部。一般使用高阶组件都是为了对已有的组件进行某些能力上的增强，很像对一个类或ES5写法的React组件使用Mixin产生的效果，这也正是这里称之为enhancer的原因。那么作为connect方法的产物，这里的enhancer会对传入的组件进行怎样的增强呢？

答案是接触到store的能力。原有的组件，如上例中的Counter，不直接与store打交道，甚至不知道store与Redux的存在，而经过enhancer处理得到的组件，即上例中最终被export的内容，能够直到接触到store，监听、读取状态数据并触发action。这里的store，就是先前通过Provider引入的store。react-redux通过React在0.14版本引入的context特性实现了store内容的隐式传递：Provider作为整个组件树的根节点，通过实现getChildContext方法将store提供给它的子孙节点们，而enhancer通过组件的context属性获取store对象，从而可以调用其提供的subscribe、getState、dispatch等方法。针对分别作为参数与返回值的两个组件的特点，将作为参数传入enhancer的组件称为**展示组件**，而将其返回的结果组件称为**容器组件**。

那么这个enhancer是怎么制作出来的呢？它所产出的容器组件又是怎么具体地操作了store，从而将数据与行为交给了最初定义的展示组件Counter的呢？谜底就在connect方法中。

connect是一个高阶函数，接收3个参数mapStateToProps、mapDispatchToProps及mergeProps，并返回enhancer。enhancer的作用已经不必多说，它决定了被返回的容器组件的行为，而enhancer的行为又由connect方法决定。下面，首先简要说明下在connect被调用时，它的3个参数各自的作用。

* **mapStateToProps**

mapStateToProps要求是一个方法，接收参数state（即store.getState()的结果），返回一个普通的JavaScript对象，对象的内容会被合并到最终的展示组件上。简单地说，mapStateToProps就是从全局的状态数据中挑选、计算得到展示组件所需数据的过程，即从state到组件属性的映射，正如它的参数名所暗示的“map state to props”。这个方法会在最初state发生改变时，被调用并计算出结果，结果会被作为展示控件属性影响其行为。这部分控件属性被称为stateProps。

在我们的例子中，mapStateToProps返回了只有一个字段num的对象，字段num的值为state.num的值。这意味着展示组件Counter可以通过this.props.num获取state.num的值。

* **mapDispatchToProps**

mapDispatchToProps的命名风格与第一个参数类似，不难推断它的作用：“map dispatch to props”，即接收参数dispatch（正是store的dispatch方法），并返回一个简单的JavaScript对象，对象的内容也会被合并到最终的展示组件上。对应于mapStateProps，一般用于生成数据属性，mapDispatchToProps一般用于生成行为属性，即典型的onDoSth这样的回调，被称为dispatchProps