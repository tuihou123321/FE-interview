## redux



[TOC]



### Redux与 MobX的区别？

- 【代码量】 redux>mobx
  - redux： 需要定义一堆的action, dispatch,reducer
  - mobx： store和改变的方法
  
- 【开发难度】 redux>mobx
  - mobx：使用面向对象的编程思维，相对简单
  - redux：比较复杂，函数式编程思维，同时需要借助一系列的中间件来处理异步和副使用
  
- 【调试难度】 mobx>redux 
	 - redux：状态不可变，返回一个新的状态，同时使用纯函数；,redux 提供时间回溯的开发工具，同时纯函数以及更少的抽象，让调试变得更加容易
	- mobx：中状态可变，可对其直接修改，mobx中有更多的抽象和封装，调试比较困难，同时结果难预测
	
- 【store数】

- - redux：单个store
   - mobx: 多个store

- 【功能性】redux> mobx

- - redux: 可回溯状态，时间旅行，适合：画板应用，表格应用，很多时候需要撤销，重做等操作
   - mobx: 直接修改源数据

   

适用场景总结：

- mobx： 简单项目，适合数据不复杂的应用
- redux：大型项目，有回溯需求







### redux的实现流程？

- 用户（通过view）发出Action,发出方式是调用dispatch方法；
- Store自动调用 Reducer ,传入两个参数，当前state,收到的Action，Reducer 返回新的 State
- state更新后，store就会调用监听函数， 根据state触发重新渲染，更新view

> 整个流程中数据都是单向流动的，这种方式保证了流程的清晰



几个核心概念：

- **Store** :数据中心，整个应用只能有一个store
- **State**: store对象包含的所有数据
- **Action**: 用户触发的行为名称（通过action再去触发state的改变，最终响应view的改变）
- Action Creator: 生成action的函数，可生成多种action
- **Reducer**: store收到action后，处理state的函数，叫到reducer，接收两个参数：action ，和当前state；返回值： 新的state
- Dispatch: view发出action的唯一方法







### redux中间件是什么，中间件的执行过程？

定义：redux提供了类似后端experss中间件的概念，本质目地是提供第三方插件的模式，自定义拦截 action-> reducer的过程；变成 action->middlewares->reducer. 这种机制可以让我们改变数据流，实现如异步action,action过滤，日志输出，异常报告等功能；

使用： redux提供的一个方法： applyMiddleware，可应用多个中间件；

![img](en-resource://database/11931:0)





### 什么是redux,和vuex有什么区别?

```
redux主要用来做程序状态管理js库，提供可预测的状态变化；
```

主要由三部分组成

store 总状态==等同于react中的state----- vuex中的 （state）vuex中多了一个getter计算属性

reducer 数据处理方法==等同于父级方法----- vuex中的(mutation同步,action异步)

action  参数==等于于回调参数；-----

```
多个vuex中引入module可以把store划分成多个单元 ；
```





### redux常用的中间件？

- redux-logger   ： 日志中间件，输出触发的action，和经过reducer处理前后的state值；
- **redux-thunk**：处理异步
  - 优点：
    - **体积小**：实现方式简单，只有不到20行代码
    - **使用简单**：没有引入像redux-sage 或者 redux-observable 额外范式，上手简单
  - 缺点：
    - **耦合严重**：异步操作与redux的action 偶合在一起，不方便管理
    - **功能弱**：常用的功能需要自己封装
- **redux-saga**：处理异步
  - **异步解耦**：异步操作被转移到单独的saga.js中，不再掺杂在action.js 或 component.js中
  - **异常处理**：受益于generator function的 saga实现，代码异常/请求失败 都可以直接通过 try/catch语法直接捕获处理
  - **功能强大**：提供了大量的saga辅助函数和 Effect 创建器 供开发者使用
  - **灵活**：可以将多个saga 串行/并行组合起来，形成一个非常实用的异步flow
  - **易测试**：提供了各种case的测试方案，包括 mock task ,分支覆盖等
- redux-promise：处理异步
- redux-observable
  - -优点
    - -功能最强：背靠 rxjs这个强大的响应式编程库，几乎做任何你能想到的异步处理
  - 缺点
    - -学习成功奇高：如果不会rxjs，则需要额外学习两个复杂的库







### redux中的connect有什么作用？

redux中的connect方法主要是把UI组件生成容器组件，connect的意思就是就是把两个连接起来；

```
import {connect} from "react-redux"

const VisibleTodoList=connet(

​    mapStateToProps,   //参数

​    mapDispatchToProps   //处理方法

)(TodoList)
```



### 解释一下 Flux思想？



![flux](https://user-gold-cdn.xitu.io/2019/3/25/169b42c4d3813a3e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Flux 是一种强制单向数据流的架构模式。它控制派生数据，并使用具有所有数据权限的中心 store 实现多个组件之间的通信。整个应用中的数据更新必须只能在此处进行。 Flux 为应用提供稳定性并减少运行时的错误。





### Redux遵循的特点？

- **单一数据来源**"："整个数据存储在store当中，方便调试和检查应用程序
- **状态只读**：改变状态的唯一方法是云触发一个动作。
- **使用纯函数更改**：纯函数是那些返回值仅取决于其参数的函数





### Redux与flux有何不同？

![image-20201111172155995](C:\Users\22064\AppData\Roaming\Typora\typora-user-images\image-20201111172155995.png)

**Flux** **Redux**   1. Store 包含状态和更改逻辑 1. Store 和更改逻辑是分开的  2. 有多个 Store 2. 只有一个 Store  3. 所有 Store 都互不影响且是平级的 3. 带有分层 reducer 的单一 Store  4. 有单一调度器 4. 没有调度器的概念  5. React 组件订阅 store 5. 容器组件是有联系的  6. 状态是可变的 6. 状态是不可改变的





### 在 React 中如何处理事件

为了解决跨浏览器的兼容性问题，`SyntheticEvent` 实例将被传递给你的事件处理函数，`SyntheticEvent`是 React 跨浏览器的浏览器原生事件包装器，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`。

比较有趣的是，React 实际上并不将事件附加到子节点本身。React 使用单个事件侦听器侦听顶层的所有事件。这对性能有好处，也意味着 React 在更新 DOM 时不需要跟踪事件监听器。





## react-redux



### react-redux的实现原理？

react-redux 提供两个api; 

- **Provider**: 从最外部封装整个应用，并向connect模块传递store;  (父子组件)
- **connect**: （高阶组件）负责连接react和redux
- - **包装原组件**：将state,action通过props的方式传入到原组件内部
  - **监听store tree变化**：使其包装的原组件可以响应state的变化



![img](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/710f0a9f0a8e6a320f55fa0ca795a3c7.png)





### react-redux的使用流程?

- 创建store：（定义state,reducer）  （使用Redux中的createStore api创建）
- 封装应用：通过ReactRedux中的Provider方法把store传递给connect模块
- connect









