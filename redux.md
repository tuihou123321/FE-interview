## redux



[TOC]



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



### react-redux的实现原理？

react-redux 提供两个api; 

- **Provider**: 从最外部封装整个应用，并向connect模块传递store;  (父子组件)

- **connect**: （高阶组件）
- - 包装原组件，将state,action通过props的方式传入到原组件内部
  - 监听store tree变化，使其包装的原组件可以响应state的变化



### react-redux的使用流程?

- 创建store：（定义state,reducer）  （使用Redux中的createStore api创建）
- 封装应用：通过ReactRedux中的Provider方法把store传递给connect模块
- connect



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

- redux-logger
- redux-thunk
- redux-promise
- redux-saga
- redux-observable





### redux中的connect有什么作用？

redux中的connect方法主要是把UI组件生成容器组件，connect的意思就是就是把两个连接起来；

```
import {connect} from "react-redux"

const VisibleTodoList=connet(

​    mapStateToProps,   //参数

​    mapDispatchToProps   //处理方法

)(TodoList)
```



