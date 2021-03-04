## webpack



[TOC]



### webpack里面的插件是怎么实现的？

**Compiler（编译器） 和 Compilation（编译方法）**

在开发 Plugin 时最常用的两个对象就是 Compiler 和 Compilation，它们是 Plugin 和 Webpack 之间的桥梁。
Compiler 和 Compilation 的含义如下：

- **Compiler：** 对象包含了 Webpack 环境所有的的配置信息，包含 options，loaders，plugins 这些信息，这个对象在 Webpack 启动时候被实例化，它是全局唯一的，可以简单地把它理解为 Webpack 实例；
- **Compilation：** 对象包含了当前的模块资源、编译生成资源、变化的文件等。当 Webpack 以开发模式运行时，每当检测到一个文件变化，一次新的 Compilation 将被创建。Compilation 对象也提供了很多事件回调供插件做扩展。通过 Compilation 也能读取到 Compiler 对象。
- 

Compiler 和 Compilation 的区别在于：

Compiler 代表了整个 Webpack 从启动到关闭的生命周期，而 Compilation 只是代表了一次新的编译。



插件运行机制：



![image-20210304113253309](https://i.loli.net/2021/03/04/CFXlwh4yNZVa63f.png)







参考资料：

[Webpack原理-编写Plugin](https://juejin.cn/post/6844903550623940621#heading-0)







### webpack打包过程？

一）初始化阶段：

二）编译阶段：

三）输出阶段：



webpack的运行流利是一个串行过程，过程如下： 

* 始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；

* 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；

* 确定入口：根据配置中的 entry 找出所有的入口文件；

* 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；

* 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；

* 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；

* 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。



> 同时我们也了解了webpack中比较核心的几个概念compiler、compilation、tapable。



参考资料：[webpack编译流程](https://juejin.cn/post/6844903935828819981)





### 组件库按需加载如何实现？



**一）实现方式**

**1) 通过安装  babel-plugin-import (推荐)**

.babelrc or  babel-loader option

```
{
 "plugins":[
 	["import":{
 	  {
 	  libraryName:"antd-mobile",style:"css"   //style:true会加载less文件
 	  }
 	}]
 ]
}
```



**2) 手动引入**

```
import DatePicker from "antd-mobile/lib/date-picket";
import "antd-mobile/lib/date-picker/style/css"
//import "antd-mobile/lib/date-picker/style"    //加载less
```



**二）支持的库**

- antd
- antd-mobile 
- loadsh
- material-ui



**三) 实现原理**

>  动态引入css,和模块，最终编译的效果就是转义成 手动引入的效果

配置项：{ "libraryName": "antd", style: "css" }   

style参数说明：

- css：直接引入经过打包后的antd样式文件
- true：在项目编译阶段，对引入的antd样式文件进行编译，从而可压缩打包尺寸

```
import { Button } from 'antd';
ReactDOM.render(<Button>xxxx</Button>);

      ↓ ↓ ↓ ↓ ↓ ↓

var _button = require('antd/lib/button');
require('antd/lib/button/style/css');
ReactDOM.render(<_button>xxxx</_button>);

```



参考资料[：babel-plugin-import的配置项](https://www.jianshu.com/p/87efabb6a333)







### webpack 核心概念？



**Entry**: 指定webpack开始构建的入口模块，从该模块开始构建并计算出直接或间接依赖的模块或者库。

**Output**：告诉webpack如何命名输出的文件以及输出的目录

**Module**: 模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。

**Chunk**：`coding split`的产物，我们可以对一些代码打包成一个单独的`chunk`，比如某些公共模块，去重，更好的利用缓存。或者按需加载某些功能模块，优化加载时间。在`webpack3`及以前我们都利用`CommonsChunkPlugin`将一些公共代码分割成一个`chunk`，实现单独加载。在`webpack4` 中`CommonsChunkPlugin`被废弃，使用`SplitChunksPlugin`

**Loader**：模块转换器，用于把模块原内容按照需求转换成新内容。

**Plugin**：扩展插件，在 Webpack 构建流程中的特定时机会广播出对应的事件，插件可以监听这些事件的发生，在特定时机做对应的事情。



参考资料：

[webpack编译流程](https://juejin.cn/post/6844903935828819981)





### webpack如何提高打包速度？

一、缩小文件搜索范围

1. 排除：loader的时候排除不必要的文件 ，比如node,modules
2. 别名定位：设置文件别名，alias,避免文件层层解析，快速定位
3. 减少文件查找：合理配置 resolve.extensions（匹配尽量少，高频类型放前面，导入文件加后缀）

二、提取公共模块：使用dllPlugin提取共用公用模块，打包出vendor.js,避免重复编译

三、开进程转换：HappyPack开启多进程loader转换

四、开进程压缩：使用paralleUglifyPlugin开启多进程压缩JS文件

五、利用缓存：`webpack.cache`、babel-loader.cacheDirectory、`HappyPack.cache`都可以利用缓存提高rebuild效率



参考资料：

[使用webpack4提升180%编译速度](https://louiszhai.github.io/2019/01/04/webpack4/)



### webpack 与 rollup  打包工具对比区别？

相同点：webpack与rollup都是用来打包项目的；

实际使用：开发应用时用webpack,开发库时使用rollup;

区别：

**webpack**：主要用来打包工程项目，主要它是通过将模块分别封闭进函数中，构建出来的代码体积较大；

功能特点：

- 代码分割
- 静态资源导入
- 模块热更新HMR；

**rollup**： 下一代ES6模块化工具，主要用来打包类库（library）,用标准化的格式es6来写代码，构建出来的代码体积更小，更纯净；react，vue目前就是用rollup打包的；



### webpack常用的babel,loader，plugin有哪些？

**babel： 广泛的转码器,（es6/ts代码转成es5代码）**

- 只转换新的语法（箭头函数）
- 不转换新的API（promise,proxy,set,symbol） 需要另外安装poliy

**loader：加载更类资源文件**

- css 类 ----  less-loader,sass-loader, postcss-loader,autoprefixer-loader
- 文件类 ---- url-loader，file-loader

**plugin：**插件

- webpack-dev-server (小型node express服务器)
- postcss-plugin-px2rem




### 静态页面如何使用，webpack来实现自动更新？

1.创建npm配置文件 package.json

```js
{

  "name": "demo",

  "version": "1.0.0",

  "description": "",

  "main": "index.js",

  "scripts": {

​    "test": "echo \"Error: no test specified\" && exit 1"

  },

  "author": "",

  "license": "ISC"

}
```



2.安装webpack

3.安装webpack-dev-server







### webpack-dev-server有什么用，如何配置？

webpack-dev-server是一个小型的express服务器，使用它可以为webpack生成的资源文件提供web服务；

主要功能：

1. 为静态文件提供http服务
2. 自动刷新新热替换（HMR）



安装

先安装好

npm i  webpack-dev-server



第一：webpack-dev--server可以用来生成localhost:8080在线地址的网站，生成站点域名；

直接运行命令

webpack-dev-server  //通过这个地址可以直接访问（但是地址地址栏有尾巴，并且内容区域顶部有内容）

或者通过这个来指定路径，去掉尾部和url不优雅的地方

--contentbase src  指定路径（监控src内容的变化，src下放的是bundle.js）

--inline  把webpack-dev-server之前默认的iframe默认修改成inlinie模式，这样页面就不会有多余的内容了；

webpack-dev-server  --contentbase src  --inline

第二：webpack热更新，功能是在页面内容发生变化后会自动刷新页面；

webpack-dev-server  --contentbase src  --inline --hot





### 你对webpack做过哪些配置?

除了常规我们引用的各种file loader,plugin,babel配置主要还有三方面的优化：

一、加快打包速度方面（见专题回答）

二、优化开发体验

- 自动刷新：启用webpack-dev-server自动刷新文件；（轮训时间，排除的文件夹，构建延迟时间）
- 热替换：开启模块替换HMR，不刷新页面替换内容；

三、优化输出质量

- 压缩文件体积
- 使用cdn加速静态资源
- 分割代码以按需加载
- 多页面应用提取页面间公共组件
- 区分生产/测试环境



参考资料：

[webpack 做过哪些优化，开发效率方面、打包策略方面等等](https://github.com/lgwebdream/FE-Interview/issues/25)





## Babel



### 什么是babel？

babel是一个广泛的转码器，可以把es6/7等最新的语法转换成es5的语法，让浏览器能够兼容；

babel的配置文件 .babelrc 主要有两个配置：

- 转码规则 presets
- 插件 plugins





### babel是如何把 es6代码转换成 es5代码的？

babel将es6代码转换成es5，主要有三个阶段；

1. 【解析Parse】把es6代码，通过babylon解析成，AST语法树，即词法分析与语法分析的过程 
2.	【转换Transform】通过babel-traverse plugin 对AST树进行遍历转译，在此过程中进行添加/更新/删除等操作，生成新的AST
3. 【生成Generate】通过 babel-generator，将变换后的AST再转成es5代码

注意事项：babel默认不转换的代码需要通过安装babelpolyfill来支持，比如 ：

- Array.from,generator,set,maps,proxy,symbol...



![img](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/566a1ac865cfc0b1bf5511f377ff6828.png)



### 如何写一个babel插件?

实现流程

1. babel解析成AST
2. 然后插件更改AST （Babel的插件的模块需要暴露一个function,在function内返回visitor）
3. 最后Babel输出代码







