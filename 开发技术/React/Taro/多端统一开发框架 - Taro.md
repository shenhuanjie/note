# Taro 是什么？Taro - 多端开发框架

Taro 是由京东 - 凹凸实验室打造的一套遵循 React 语法规范的多端统一开发框架。

现如今市面上端的形态多种多样，Web、App 端(React Native)、微信小程序等各种端大行其道，当业务要求同时在不同的端都要求有所表现的时候，针对不同的端去编写多套代码的成本显然非常高，这时候只编写一套代码就能够适配到多端的能力就显得极为需要。

使用 Taro，我们可以只书写一套代码，再通过 Taro 的编译工具，将源代码分别编译出可以在不同端（微信小程序、H5、App 端等）运行的代码。同时 Taro 还提供开箱即用的语法检测和自动补全等功能，有效地提升了开发体验和开发效率。

![t](assets/163d9f93825ae795)

## Taro 能提供什么？

### 一次编写，多端运行

既然是一个多端解决方案，Taro 最重要的能力当然是写一套代码输出多端皆可运行的代码。目前 Taro 已经支持一套代码同时生成 H5 和小程序，App端(React Native)端也即将支持，同时诸如快应用等端也将得到支持。

同时 Taro 也已经投入到了生产环境使用，目前已经支撑了一个 3 万行代码小程序 [TOPLIFE](https://link.juejin.im/?target=https%3A%2F%2Fwww.toplife.com%2F) 的开发和部分京东购物小程序，未来也将会支撑更多的京东核心业务小程序。

![qrcode.png](assets/163d9f9382448ad5)

### 现代前端开发流程

和微信自带的小程序框架不一样，Taro 积极拥抱社区现有的现代开发流程，包括但不限于：

- NPM 包管理系统
- ES6+ 语法
- 自由的资源引用
- CSS 预处理器和后处理器（SCSS、Less、PostCSS）

对于微信小程序的编译流程，我们从 [Parcel](https://link.juejin.im/?target=https%3A%2F%2Fparceljs.org%2F) 得到灵感，自研了一套打包机制将 AST 不断传递，因此代码分析的速度得到了很大的提高。一台 2015 年 的 15寸 RMBP 在编译上百个组件时仅需要大约 15 秒左右。

### 和 React 完全一致的 API 和组件化系统

在 Taro 中，你不用像小程序一样区分什么是 `App` 组件，什么是 `Page` 组件，什么是 `Component`组件，Taro 全都是 `Component` 组件，并且和 React 的生命周期完全一致。可以说，一旦你掌握了 React，那就几乎掌握了 Taro。而学习 React 的资源也几乎是汗牛充栋，完全不用担心学不会。

Taro 和 React 一样，同样使用声明式的 JSX 语法。相比起字符串的模板语法，JSX 在处理精细复杂需求的时候会更得心应手。

```js
// 一个典型的 Taro 组件
import Taro, { Component } from '@tarojs/taro'
import { View, Button } from '@tarojs/components'
export default class Home extends Component {
  constructor (props) {
    super(props)
    this.state = {
      title: '首页',
      list: [1, 2, 3]
    }
  }
  
  componentWillMount () {}
  
  componentDidMount () {}
  
  componentWillUpdate (nextProps, nextState) {}
  
  componentDidUpdate (prevProps, prevState) {}
 
  shouldComponentUpdate (nextProps, nextState) {
    return true
  }
  add = (e) => {
    // dosth
  }
  render () {
    const { list, title } = this.state
    return (
      <View className='index'>
        <View className='title'>{title}</View>
        <View className='content'>
          {list.map(item => {
            return (
              <View className='item'>{item}</View>
            )
          })}
          <Button className='add' onClick={this.add}>添加</Button>
        </View>
      </View>
    )
  }
}
```

### 良好的开发效率和体验

鉴于 Taro 的语法和 React 完全一样，因此编辑器/IDE 能够对 Taro 的支持和 React 是几乎一样的。现代的编辑器默认都对 JSX 进行了支持，如果没有，找一个插件也是非常容易的事情。但毕竟我们做 Taro 就是为了提升开发效率和开发体验，而真正使用 Taro 的人就是我们自己或正坐在我们旁边的同事。因此在此基础上，我们又对 Taro 开发体验进行了进一步加强。

#### 自定义 ESLint 规则

我们之前提到过，当学会了 React，其实也差不多会 Taro 了。其中很重要的一个原因就是我们对 Taro 不支持的语法和特性单独写了 ESLint 规则：开发者只管写代码，写到不支持的语法/特性编辑器会报错，并给出报错信息和一个文档地址描述。

![Taro eslint](assets/163d9f9382322ca3)

#### 类型安全和运行时检测

JSX 的本质就是 JavaScript 的语法增强，所以例如没有 `import` 组件等语法错误在编译期就能发现。开发者也可以使用 `TypeScript` 或 `Flow` 来对代码的可靠性进一步增强，或使用 `PropsType`在运行时进一步保障代码的鲁棒性。

![typings.gif](assets/163d9f93823960ad)

#### 高效的自动补全和 ES6+ 语法

Taro 的所有 API（包括微信小程序等端能力接口）都有智能的提醒和自动补全，包括接口的参数和返回值。

![Taro èªå¨è¡¥å¨](assets/163d9f93a74314b7)

## Taro 的设计思路

我们的初心就是做一款能够适配多端的解决方案，结合业务场景、技术选型和前端历史发展进程，我们的解决方案必须满足下述要求：

- 代码多端复用，不仅能运行在时下最热门的 H5、微信小程序、React Native，对其他可能会流行的端也留有余地和可能性。
- 完善和强大的组件化机制，这是开发复杂应用的基石。
- 与目前团队技术栈有机结合，有效提高效率。
- 学习成本足够低
- 背后的生态强大

同时满足这几个需求并不容易，在我们经过充分地调研和思考之后发现只有 React 体系能够满足我们的需求。而对于微信小程序而言，使用 React 完全没有办法进行开发——直到我们从 [codemod](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Ffacebook%2Fcodemod)得到灵感：

> 在一个优秀且严格的规范限制下，从更高抽象的视角（语法树）来看，每个人写的代码都差不多。

也就是说，对于微信小程序这样不开放不开源的端，我们可以先把 React 代码分析成一颗抽象语法树，根据这颗树生成小程序支持的模板代码，再做一个小程序运行时框架处理事件和生命周期与小程序框架兼容，然后把业务代码跑在运行时框架就完成了小程序端的适配。

![Taro](assets/163d9f93a840481b)

对于 React 已经支持的端，例如 Web、React Native 甚至未来的 React VR，我们只要包一层组件库再做些许样式支持即可。鉴于时下小程序的热度和我们团队本身的业务侧重程度，组件库的 API 是以小程序为标准，其他端的组件库的 API 都会和小程序端的组件保持一致。

## 技术选型与权衡

在我们前面社区已经有多个优秀的框架以小程序为核心对多端适配进行了探索，我们将各个开发框架的主要特点和特性进行了对比并制成图表。大家可以结合团队技术栈、技术需求以及框架特点、特性进行选型和权衡。

![Taro](assets/163d9f93ae6055b1)

## 结语

经过数个月的开发，Taro 从第一次 commit 到发展成包括 16 个包，十多位同学共同参与的大型项目。与此同时，Taro 也在生产环境支撑了数个复杂业务线上项目的开发，将来也会支撑更多的京东业务。

Taro 的技术方案和实现也根植于社区，我们也希望为技术社区的发展壮大贡献一份自己的力量。秉持着京东凹凸实验室长久以来开源、开放、共享的优良传统，我们今天将 Taro 全部代码开源，为广大开发者快速开发多端项目提供一整套技术解决方案。未来，我们也将继续拓展 Taro 现有能力，支持更多端能力，继续完善开发者体验，提高开发者效率，帮助更多开发者，同时也从社区中汲取养分，让 Taro 变得更加强大。

官网：[taro.aotu.io/](https://link.juejin.im/?target=http%3A%2F%2Ftaro.aotu.io%2F)

GitHub: [github.com/nervjs/taro](https://link.juejin.im/?target=http%3A%2F%2Fgithub.com%2Fnervjs%2Ftaro)

如果你还没听过 Nerv，可以来这里看看：[nerv.aotu.io/](https://link.juejin.im/?target=https%3A%2F%2Fnerv.aotu.io%2F)

感谢您的阅读，本文由 [凹凸实验室](https://link.juejin.im/?target=%2F%2Faotu.io) 版权所有。如若转载，请注明出处：凹凸实验室（[aotu.io/notes/2018/…](https://link.juejin.im/?target=https%3A%2F%2Faotu.io%2Fnotes%2F2018%2F06%2F07%2FTaro%2F)）