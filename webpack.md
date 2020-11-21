## webpack



[TOC]



### webpack打包过程？



一）初始化阶段：

二）编译阶段：

三）输出阶段：





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



### webpack有什么作用?

模块打包工具(提高前端工程化的能力);

- babel
- loader
- plugin





### webpack如何做性能优化？

一、缩小文件搜索范围

1. 排除：loader的时候排除不必要的文件 ，比如node,modules
2. 别名定位：设置文件别名，alias,避免文件层层解析，快速定位
3. 减少文件查找：合理配置 resolve.extensions（匹配尽量少，高频类型放前面，导入文件加后缀）

二、提取公共模块：使用dllPlugin提取共用公用模块，打包出vendor.js,避免重复编译

三、开进程转换：HappyPack开启多进程loader转换

四、开进程压缩：使用paralleUglifyPlugin开启多进程压缩JS文件



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

babel： 广泛的转码器,

- 只转换新的语法（箭头函数）
- 不转换新的API（promise,proxy,set,symbol） 需要另外安装poliy

loader：加载更类资源文件

- css 类 ----  less-loader,sass-loader, postcss-loader,autoprefixer-loader
- 文件类 ---- url-loader，file-loader
- 

plugin：

- webpack-dev-server (小型node express服务器)
- postcss-plugin-px2rem,





### 什么是babel？

babel是一个广泛的转码器，可以把es6/7等最新的语法转换成es5的语法，让浏览器能够兼容；

babel的配置文件 .babelrc 主要有两个配置：

- 转码规则 presets
- 插件 plugins





### babel是如何把 es6代码转换成 es5代码的？

babel将es6代码转换成es5，主要有三个阶段；

第一：把es6代理，通过babylon解析成，AST语法树 

第二：把AST语法树，plugin用babel-traverse对AST树进行遍历转译，得到新的AST树

第三：把新的AST树生成，用babel-generator，转成es5代码

注意事项：babel默认不转换的代码需要通过安装babelpolyfill来支持，比如 ：

- Array.from,generator,set,maps,proxy,symbol...



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





### webpack如何配置？

答：webpack是一个打包工具，最后生成一个bundle.js的文件；

通过访问index.html可以浏览打包的网页，通过如下方法可以实现自动打包的功能；

运行如何命令，webpack会自动监测页面上的内容，如果内容有变化就会把 自动打包，可以在命令行面板中看到打包的进度；

webpack --watch   





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