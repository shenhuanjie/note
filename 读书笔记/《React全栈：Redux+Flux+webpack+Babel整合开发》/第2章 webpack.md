# 第2章 webpack

如今，前端项目日益复杂，构建系统已经成为开发过程中不可或缺的一个部分，而模块打包（module bundler）正是前端构建系统的核心。

正如前面介绍到，前端的模块系统经理了长久的演变，对应的模块打包方案也几经变迁。从最初简单的文件合并，到AMD的模块具名化并合并，再到browserify将CommonJS模块转换成为浏览器端可运行的代码，打包器做的事情越来越复杂，角色也越来越重要。

在这样一个竞争激烈的细分领域中，webpack以极快的速度风靡全球，成为当下最流行的打包解决方案，并不是偶然。它功能强大、配置灵活，特有的code spliting方案正戳中了大规模复杂Web应用的痛点，简单的loader/plugin开发使它很快拥有了丰富的配套工具与生态。

对多种模块方案的支持与视一切资源为可管理模块的思路让它天然地适合React项目的开发，成为React官方推荐的打包工具。在本章中将着重介绍webpack这个工具的特点与使用，作为接下来使用webpack辅助开发React项目的准备。

## 2.1 webpack的特点与优势

正如前面提到的，打包工具也有不同的开源方案，那么相比其他的流行打包工具，webpack有着怎样的特点与优势呢？本节将对RequireJS、browserify及webpack这三者做一个全面的比较。

### 2.1.1 webpack与RequireJS、browserify

首先对三者做一下简要的介绍。

RequireJS是一个JavaScript模块加载器，基于AMD规范实现。它同时也提供了对模块进行打包与构建的工具r.js，通过将开发时单独的匿名模块具名化并进行合并，实现线上页面资源加载的性能优化。这里拿来对比的是由RequireJS与r.js等一起提供的一个模块化构建方案。开发时的RequireJS模块往往是一个个单独的文件，RequireJS从入口文件开始，递归地进行静态分析，找出所有直接或间接被依赖（require）的模块，然后进行转换与合并，结果大致如下（未压缩）。

```js
// bundle.js
define('hello', [], function (require) {
    module.exports = 'hello';
});
define('say', ['require', 'hello'], function (require) {
    var hello = require('./hello');
    alert(hello);
});
```

browserify是一个以在浏览器中使用Node.js模块为出发点的工具。它最大的特点在于以下两点。

* 对CommonJS规范（Node.