## redux



[TOC]



### redux 关键知识点?

1. flux思想（四个部分:dispatcher,stores,views,actions）--flux相当于mvc中的m和c， react相当于v层；
2. redux常用中间件（redux-logger 提供日志输出 ,redux-thunk处理异步操作,redux-promise处理异步操作-actionCreator返回值是promise）





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



