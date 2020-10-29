##  前端面试题 ---react



## react 关键知识点 ：
1. 生命周期 （16.3之前后之后的区别）
2. 性能优化（组件类型，key, shouldComponentUpdate减少组件的更新，异步加载组件，代码分割）；
3. 高阶组件
4. refs
5. diff算法
6. fiber架构
7. setState （类异步，优点，参数）
8. 代码分割(基于)
9. 生态 （使用过哪些库）
10. 组件设计  （如何设计 一个好组件）



##  redux 关键知识点：
1. flux思想（四个部分:dispatcher,stores,views,actions）--flux相当于mvc中的m和c， react相当于v层；
2. redux常用中间件（redux-logger 提供日志输出 ,redux-thunk处理异步操作,redux-promise处理异步操作-actionCreator返回值是promise）


## react-router知识点：
1. history 路由监听
2. history 类型，配置服务器要注意哪些地方


## react类

高阶组件相关
什么是高阶组件，它有哪些运用？
高阶组件就是一个函数，接收一个组件，经过处理后返回后的新的组件；
高阶组件，不是真正意义上的组件，其实是一种模式；
可以对逻辑代码进行抽离，或者添加某个共用方法；
eg：
react-redux   ：connect就是一个高阶组件,接收一个component,并返回一个新的componet,处理了监听store和后续的处理 ；
react-router  ：withrouter 为一个组件注入 history对象；


高阶组件和父组件的区别？
高阶组件可以重写传入组件的state,function,props;可以对代码逻辑进行抽离，重写 ；
父组件只是控制子组件的view层；


react diff算法是如何提高性能的？
传统的页面更新，是直接操作dom来实现的，比如原生js或者jquery，但是这种方式性能开销比较大；
react 在初始化的时候会生成一个虚拟dom,每次更新视图会比较前后虚拟dom的区别；
这个比较方法就是diff算法，diff算法很早就已经出现了；但是react的diff算法有一个很大区别；
传递diff算法：通过循环递规对节点进行依次对比，时间算法复杂度是 o(n^3)，n代表节点数；
react diff 算法：    采用逐层比较的方式；把时间算法复杂度从O（n^3）次方，降低到O(n)次方；



什么是react虚拟dom，为什么虚拟dom可以提高性能？
虚拟dom是真实dom的一份映射表，react中我们只要改变state,
react就会调用batching（批处理）、diff算法自动更新虚拟dom；
虚拟dom再来操作真实dom,从而改变视图；
               

什么是Fiber？是为了解决什么问题？
fiber是react16中新发布的特性；
解决的问题：
react在渲染过程时，从setState开始到渲染完成，中间过程是同步；
如果渲染的组件比较大，js执行会长时间占有主线程，会导致页面响应度变差，使得react在动画，手势等应用中效果比较差；
实现过程及原理（核心理念就是：time slicing）： 
1. 拆分：把渲染过程进行拆分成多个小任务
2. 检查：每次执行完一个小任务，就去对列中检查是否有新的响应需要处理
3. 继续执行：如果有就执行优化及更高的响应事件，如果没有继续执行后续任务



为什么setState不设计成同步的？
## 保持内部的一致性，和状态的安全性
保持state,props.refs一致性；
## 性能优化
react会对依据不同的调用源，给不同的 setState调用分配不同的优先级；
调用源包括：事件处理、网络请求、动画 ；
## 更多可能性
异步获取数据后，统一渲染页面；保持一致性，



react生命周期？
react 生命周期主流的主要有2个大的版本；
一个是 v16.3之前的：
一个是v16.3之后的；
v16.3之前 的生命周期主要分为4个阶段,8个生命周期：
* 初始化值阶段 initialization： getDefaultProps,getInitialState;
* 初始阶段 mount： componentWillMount,componentDidMount;
* 更新阶段 update：componetWillReceiveProps ,shouldComponetUpdate  ,componentWillUpdate,
* 销毁阶段 unmount：componetWillMount;
v16.3之后的生命周期:
新引入了两个生命周期：
* getDerivedStateFromProps
* getSnapshotBeforeUpdate
提示3个不安全的生命周期（在v17中要被删除）
* componentWillMount
* componentWillReceiveProps
* componetWillUpdate


react组件通讯有哪几种方式？
react组件之前通讯主要要四种方式
1. 通过共用父组件传值, 适合，层级简单的； props来实现，子调父回调函数；
2. redux，全局可调用，从store中取；
3. context，注入全局变量：getChildContext;   获取全局变量：this.context.color;
4. 自定义事件,向事件对象添加监听器，和触发事件来实现组件之间的通讯；


react的refs有什么用？
react 主要提供了一种标准数据流的方式来更新视图；
但是页面某些场景是脱离数据流的，这个时候就可以使用 refs; 
react refs 是用来获组件引用的,取后可以调用dom的方法；
* 使用场景，获取焦点  this,refs.inputPhone.focus(); 
* 与第三方dom库整合
## refs 注意事项：
不能在无状态组件中使用refs




什么是redux,和vuex有什么区别?
redux主要用来做程序状态管理js库，提供可预测的状态变化；
主要由三部分组成
store 总状态==等同于react中的state----- vuex中的 （state）vuex中多了一个getter计算属性
reducer 数据处理方法==等同于父级方法----- vuex中的(mutation同步,action异步)
action  参数==等于于回调参数；----- 
多个vuex中引入module可以把store划分成多个单元 ；


redux常用的中间件？

* redux-logger
* redux-thunk
* redux-promise
* redux-saga
* redux-observable



redux中的connect有什么作用？
redux中的connect方法主要是把UI组件生成容器组件，connect的意思就是就是把两个连接起来；

import {connect} from "react-redux"

const VisibleTodoList=connet(
    mapStateToProps,   //参数
    mapDispatchToProps   //处理方法
)(TodoList)



create-react-app有什么优点和缺点？
优点：
快速生成架手架
缺点：
1. 默认不引入polyfill,需要在入口引入babel-polyfill
2. 默认只支持css热加载，不支持html,js热加载（推荐使用：react-hot-loader）


介绍一下JSX?
JSX 使用js的形式来写html代码；
JSX是一种语法糖，是调用js本身的特性来动态创建UI，与于传统模式下的模板语法不同；


介绍下Immutable?
Immutable是一种不同变的数据类型，
数据一旦被创建，就不能更改的数据，每当对它进行修改，就会返回新的immutable对象；
在做对象比较时，能提升性能；
immutable实现原理是持久化数据结构，结构共享，避免对数据对象进行深拷贝；


react hook简介？
react hook是v16.8的新特性；
优势：
1. 在不使用class的情况下，使用state及react其他特性（省的把纯函数组件其他组件改来改去）
2. 让组件更好的复用（老的class组件冗长、包含自身的状态state）
3. 让react编程风格更取向函数式编程风格，使用function代替class
4. 处理了this的指向问题
组件复用的几个方法：
hoc高阶组件
缺点：增加代码层级

hooks（本质是一类特殊的函数，可以为函数式张爱玲 年注入一些特殊的功能）的主要api:
* useState : 声明状态变量
* useEffect ：提供了类似于componentDidMount等生命周期钩子的功能
* useContext ：提供上、下文的功能
* useRef
* useReducer
* useCallBack
* useLayoutEffect



react、vue有什么区别？
react优势:
* 可以使用shouldComponetUpdate控制是否render，当应用比较大的时候，这些优化点比较好
* react组件是函数，vue组件 高度封装的函数; react的发挥控件更大；所以react流行hoc，


vue优势：
* 兼容老项目
* 上手简单 ,vue模板语法，react ,jsx




Setstate 被调用会发生什么？


* 如果你能够改进React的一样功能，那会是哪一个功能？
* React 的事务？
* 为什么说react是一个ui框架？


对react的看法，它的优缺点？
优点：
1. 提供了声明式的编程思想
2. 提供了组件化的开发思想，大大提高前端开发的效率
3. 引入了虚拟dom的概念，使得react可以跨端进行类各界面开发，react native,react vr,ssr;
4. 引入了diff算法，提高了渲染效率

不足：
1. 侵入性强，对老代码只能重做，不能部分集成（比如vue）；
