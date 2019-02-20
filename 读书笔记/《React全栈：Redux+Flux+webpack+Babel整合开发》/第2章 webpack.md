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

* 对CommonJS规范（Node.js模块所采用的规范）的模块代码进行的转换与包装。
* 对很多Node.js的标准package进行了浏览器端的适配，只要是遵循CommonJS规范的JavaScript模块，即使是纯前端代码，也可以使用它进行打包。

webpack则是一个为前端模块打包构建而生的工具。它既吸收了大量已有方案的优点与教训，也解决了很多前端开发过程中已存在的痛点，如代码的拆分与异步加载、对非JavaScript资源的支持等。强大的loader设计使得它更像是一个构建平台，而不只是一个打包工具。

### 2.1.2 模块规范

模块规范是模块打包的基础，我们首先对这三者所支持的模块化方案进行比较。

RequireJS项目本身是最流行的AMD规范实现的，格式如下。

```js
// hello.js
define(function (require) {
    module.exports = 'hello!';
});
```

AMD通过将模块的实现代码包在匿名函数（即AMD的工厂方法，factory）中实现作用域的隔离，通过文件路径作为天然的模块ID实现命名空间的控制，将模块的工厂方法作为参数传入全局的define（由模块加载器事先定义），使得工厂方法的执行时机可控，也就变相模拟出了同步的局部require，因而AMD的模块可以直接以原文件的形式在浏览器中加载并调试，这也称为RequireJS方案不多的优点之一。

browserify支持的则是符合CommonJS规范的JavaScript模块。不严格地说，CommonJS可以看成去掉了define及工厂方法外壳的AMD。上述hello.js对应CommonJS版本是如下这样的。

```js
// hello.js
module.exports = 'hello!';
```

正如我们在前面提到的define函数的作用，没有define函数的CommonJS模块是无法直接在浏览器中执行的——浏览器环境中无法实现同Node.js环境一样的同步require方法。同样也因为没有define与工厂方法，CommonJS模块书写起来要更简单、干净。在这个显而易见的好处下，越来越多的前端项目开始采用CommonJS规范的模块书写方式。

考虑到AMD规范与CommonJS规范各有各的优点，且都有着客观的使用率，webpack同时支持这两种模块格式，甚至支持二者混用。而且通过使用loader，webpack也可以支持ES6 module（这一特性在即将到来的webpack2中原生支持），可以说覆盖了现有的所有主流的JavaScript模块化方案。通过特定的插件实现shim后，在webpack中，甚至可以把以最传统变量形式暴露的库当作模块require进来。

2.1.3 非javascript模块支持

在现代的前端开发中，组件化开发成为越来越流行的趋势。将局部的逻辑进行封装，通过尽可能少的必要的接口与其他组件进行组装与交互，可以将大的项目逻辑拆分成一个个小的相对独立的部分，减少开发与维护的负担。在传统的前端开发中，页面的局部组成所依赖的各种资源（JavaScript、CSS、图片等）是分开维护的，一个常见的目录组织方式（以Less为例对样式代码进行组织）如下。

```bash
- static/
	- javascript/
		- main.js
		- part-A/
		- ...
    - less/
    	- main.less
    	- part-A/
    	- ...
    - ...
- ...
```

这意味着一个局部组件（如part A）的引入至少需要：

* 在main.js中引入（require）part A对应的JavaScript文件。
* 在main.less中引入（import）part A对应的Less文件。

如果part A需要用到特定的模版，可能还需要在页面HTML文件中插入特定ID的template标签。而引入组件的入口越多，意味着组件内部与外部需要的约定约定，耦合度也越高。因此减少组件的入口文件数，尽可能将其所有依赖进行内部声明，可以提高组件的内聚度，便于开发与维护，这也是模块打包工具支持多种前端资源的意义所在。如上例中，在打包工具支持Less资源依赖的引入与合并的情况下，目录结构可以改成：

```bash
- app/
	- main/
		- index.js
		- index.less
	- part-A/
		- index.js
		- index.less
		- ...
	- ...
```

part A的样式实现从JavaScript中直接引入。

```js
// part-A/index.js
require('./index.less');
```

这样，仅需在main/index.js里声明对part-A/index.js的依赖，即可实现对组件part A的引入。说了这么多，我们来看一下这里提到的3个打包方案对非JavaScript模块资源的支持情况。

很多人不知道的是，RequireJS是支持除AMD格式的JavaScript模块以外的其他类型的资源加载的，而且有着相当丰富的plugin，从纯文本到模版，从CSS到字体等都有覆盖。然而基于AMD规范的非JavaScript资源加载有着本质的如下缺陷。

* 加载与构建的分离导致plugin需要分别实现两套逻辑。
* 浏览器的安全策略决定了绝大多数需要读取文本内容进行解析的静态资源无法被跨域加载（即使是JavaScript模块本身，也要依靠define方法包裹，类似于JSONP原理实现的跨域加载）。

因此在RequireJS的方案中，非JavaScript模块的资源虽然得到了支持，但支持得并不完善。

browserify可以通过各种transform插件实现不同类型资源的引入与打包。

在webpack中，与browserify的transform相对应的是loader，但是功能更加丰富。

### 2.1.4 构建产物

另外一个三者较大的区别在于构建产物。r.js构建的结果是上述define(function(){…});的集合。其结果文件的执行以来页面上事先引入一个AMD模块加载器（如RequireJS自身），所以常见的AMD项目线上页面往往存在两个JavaScript文件：loader.js及bundle.js，而browserify与webpack的构建结果都是可以直接执行的JavaScript代码。它们也都支持通过配置生成符合特定格式的结果文件，如以UMD的形式暴露库的exports，以便其他页面代码调用。后者的这种形式更加适用于JavaScript库（library）的构建。

### 2.1.5 使用

在使用上，三者也是有较大差异的。

作为npm包的RequireJS提供了一个可执行的r.js工具，通过命令执行，使用方式如下。

```bash
npm install -g requirejs
r.js -o app.build.js
```

RequireJS包也可以作为一个本地的Node.js依赖被安装，然后通过函数调用的形式执行打包。

```js
var requirejs = require('requirejs');
requirejs.optimize({
    baseUrl:'.../appDir/scripts',
    name:'main',
    out:'../build/main-built.js'
},function(buildResponse){
    // success callback
},function(err){
    // err callback
});
```

显然，前者使用更简单，而后者更适合需要进行复杂配置的场合。不过r.js的可配置项相当有限，其功能也比较简单，仅仅是实现了AMD模块的合并，并输出为字符串。如果需要如监视等功能，则需要自己编码实现。

browserify提供的命令行工具，用法与r.js很像，相当简洁。

```bash
npm install -g browserify
browserify main.js -o bundle.js
```

不过，它通过对大量配置项的支持，使得仅仅通过命令行工具也可以进行较复杂的任务。通过browserify --help及browserify --help advanced可以查看所有的配置项，覆盖了从输入/输出位置、格式到使用插件等各个方面。

browserify同样支持直接调用其Node.js的API。

```js
var browserify = require('browserify');
var b = browserify();
b.add('./browser/main.js');
b.bundle().pipe(process.stdout);
```

通过调用browserify提供的方法，手工实现脚本构建，可以进行更为灵活的配置及精细的流程控制。

webpack的使用与前两者大同小异，主要也支持命令行工具及Node.js的API两种使用方式，前者更常用一点，最简单的形式如下。

```bash
npm install webpack -g
webpack main.js bundle.js
```

不过它的特点是，虽然它会支持部分命令行参数形式的配置项，但是其主要配置信息需要通过额外的文件（默认是webpack.config.js）进行配置。这个文件只需要是一个Node.js模块，且export一个JavaScript对象作为配置信息。相比命令行参数式配置，这种配置方式更为灵活强大，因为配置文件会在Node.js环境中运行，甚至可以在其中require其他模块，这样对复杂项目中不同任务的配置信息进行组织变得更容易。例如，可以实现一个webpack.config.common.js，然后分别实现webpack.config.dev.js与webpack.config.prod.js，用于开发环境与生产环境的构建（通过命令行参数指定配置文件），后两者可以直接通过require使用webpack.config.common.js中的公共配置信息，并在此基础上添加或修改以实现各自特有的部分。

得益于webpack众多的配置项、强大的配置方式以及丰富的插件体系，大多数时候，我们仅仅书写配置文件，然后通过命令行工具就可以完成项目的构建工作。不过，webpack也提供了Node.js的API，使用也很简单。

```js
var webpack = require("webpack");

// 返回一个Compiler实例
webpack({
    // webpack配置
},function（err,stats){
    // ...
});
```

### 2.1.6 webpack的特色

在经过多方面的对比之后，我们能发现，在吸取了各前辈优点的基础上，webpack几乎在每个方面都做到了优秀。不过除此之外，webpack还有一些特色功能也是不得不提的。

**1. 代码拆分（code splitting）方案**

对于较大规模的Web应用（特别是单页应用），把所有代码合并到单个文件是比较低效的做法，单个文件体积过大会导致应用初始加载缓慢。尤其如果其中很多逻辑只在特定情况下需要执行，每次都完整地加载所有模块就变得很浪费。webpack提供了代码拆分的方案，可以将应用代码拆分为多个块（chunk），每个块包含一个或多个模块，块可以按需被异步加载。这一特性最早并不是由webpack提出的，但webpack直接使用模块规范中定义的异步加载语法作为拆分点，将这一特性实现得极为简单易用，下面以CommonJS规范为例。

```js
require.ensure(["module-a"],function(require){
    var a = require("module-a");
});
```

如上例，通过require.ensure声明依赖module-a，module-a的实现及其依赖会被合并为一个单独的块，对应一个结果文件。当执行到require.ensure时才去加载module-a所在的结果文件，并在module-a加载就绪后再执行传入的回调函数。其中的加载行为及回调函数的执行时机控制都由webpack实现，这对业务代码的侵入性极小。在真实使用中，需要被拆分出来的可能是某个体积较大的第三方库（延后加载并使用），也可能是一个点击触发浮层的内部逻辑（除非触发条件得到满足，否则不需要加载执行），将这些内容按需地异步加载可以让我们以较小的代价，来极大地提升大规模单页应用的初始化加载速度。

**2. 智能的静态分析**

熟悉AMD规范的都知道，在AMD模块中使用模块内的require方法声明依赖的时候，传入的moduleId必须是字符串常量，而不可以是含变量的表达式。原因在于模块打包工具在打包前需要通过静态分析获取整个应用的依赖关系，如果传入require方法的moduleId是含变量的表达式，其值需要在执行期才能确定，那么静态分析就无法确定依赖的到底是哪个模块，自然也就没办法把这个模块的代码事先打包进来。如果依赖模块没有被事先打包进来，在执行期再去加载，那么由于网络请求的时间不可忽视，请求时阻塞JavaScript的执行也不可行，模块内的同步require也就无从实现。

在Node.js中，模块文件都是直接从本地文件系统读取，其加载与执行是同步的，因此require一个表达式成为可能，在执行到require方法时再根据当前传入的moduleId进行实时查找、加载并执行依赖模块。然而当CommonJS规范被用于浏览器端，如通过browserify进行打包，出于与AMD模块构建类似的考虑，这一特性也无法被支持。

虽然未能从根本上解决这个问题，webpack在这个问题上还是尽可能地为开发者提供了便利。首先，webpack支持简单的不含变量的表达式，如下。

```js
require(expr ? "a":"b");
require("a"+"b");
require("not a".substr(4).replace("a","b"));
```

其次，webpack还支持含变量的简单表达式，如下。

```js
require("./template/"+name+".jade");
```

对于这种情况，webpack会从表达式“.template/”+name+“.jade”中提取出以下信息。

* 目录./template下。
* 相对路径符合正则表达式：`/^.*\.jade$/`。

然后将符合以上条件的所有模块都打包进来，在执行期，依据当前传入的实际值决定最终使用哪个模块。

这样的特性平时并不常用，但在一些特殊的情况下会让代码变得更简洁清晰，如下。

```js
function render (tplName,data){
    const render = require('./tpls/'+tplName)；
    return render(data);
}
```

作为对比，如果不依赖这样的特性，可能要像下面这样实现。

```js
const tpls={
    'a.tpl':require('./tpls.a.tpl'),
    'b.tpl':require('./tpls.b.tpl'),
    'c.tpl':require('./tpls.c.tpl'),
};

function render(tplName,data){
    const render=tpls(tplName);
    return render(data);
}
```

一方面，代码变得冗长了；另一方面，当添加新的tpl时，不仅需要向./tpls目录添加新的模版文件，还需要手动维护这里的tpls表，这增加了编码时的心理负担。

**3. 模块热替换（Hot Module Replacement）**

在传统的前端开发中，每次修改完代码都需要刷新页面才能让修改生效，并验证改动是否正确。虽然像LiveReload这样的功能可以帮助我们自动刷新页面，但当项目变大时，刷新页面往往要耗费好几秒，只有等待页面刷新完成才能验证改动。而且有些功能需要经过特定的操作、应用处于特定状态时才能验证，刷新完页面后还需要手动操作并恢复状态，较为烦琐。针对这一问题，webpack提供了模块热替换的能力，它使得在修改完某一个模块后无须刷新页面，即可动态将受影响的模块替换为新的模块，在后续的执行中使用新的模块逻辑。

这一功能需要配合修改module本身，但一些第三方工具已经帮我们做了这些工作。如配合style-loader，样式模块可以被热替换；配合react-hot-loader，可以对React class模块进行热替换。

配置webpack启用这一功能也相当简单，通过参数--hot 启动 webpack-dev-server即可。

```bash
webpack-dev-server --hot
```

为了准确起见，需要说明的是，虽然这里说模块热替换是webpack的特色功能，但是有人借鉴webpack的方案，实现了插件browserify-hmr，这让browserify也支持了模块热替换这一特性。

### 2.1.7 小结

除了上面介绍过的，业界还有一些其他的打包方案，如rollup、jspm提供的bundle工具等，不过它们或者还不够成熟，或者缺乏特点，所以没有在这里介绍。本节主要选取了3个相对成熟、主流的模块打包工具进行了比较，webpack在功能、使用等方面均有一定的优势，且提供了一些很有用的特色功能，说它是目前最好的前端模块打包工具并不为过，这也真是越来越多的前端项目选择使用webpack的原因。此外，考虑到React官方也推荐使用webpack，本书中介绍的React开发项目将全部使用webpack进行构建。