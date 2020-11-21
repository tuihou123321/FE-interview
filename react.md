# react


[TOC]



### JSX本质是什么？

> JSX 使用js的形式来写html代码

jsx本身是语法糖，无法直接被浏览器解析，需要通过babel或者typescript来转换成 js。

许多包含预配置的工具，例如：create- react app 和 next.js 在其内部也引入了jsx转换。

旧的 JSX转换会把jsx 转换为 React.createElement调用。

jsx调用js本身的特性来动态创建UI，与于传统模式下的模板语法不同





### React 中的 `useState()` 是什么？

`useState` 是一个内置的 React Hook。`useState(0)` 返回一个元组，其中第一个参数`count`是计数器的当前状态，`setCounter` 提供更新计数器状态的方法。


```
...
const [count, setCounter] = useState(0);
const [moreStuff, setMoreStuff] = useState(...);
...

const setCount = () => {
    setCounter(count + 1);
    setMoreStuff(...);
    ...
};

```





### react中组件通信的几种方式？

react组件之前通讯主要要四种方式

1. 通过共用父组件传值, 适合，层级简单的； props来实现，子调父回调函数；
2. redux，全局可调用，从store中取；
3. context，注入全局变量：getChildContext;   获取全局变量：this.context.color;
4. 自定义事件,向事件对象添加监听器，和触发事件来实现组件之间的通讯；



- 父子组件：props，props回调
- 兄弟组件：共同父级，再由父节点转发props，props回调
- 跨级组件：context对象
- 非嵌套组件：使用事件订阅
- 全局状态管理工具：Redux,Mobox维护全局store





### react UI组件和容器组件的区别与应用？

容器组件：拥有自己的状态，生命周期

UI组件：只负责页面UI渲染，不具备任何逻辑，功能单一，通常是无状态组件，没有自己的state,生命周期。



![1604622713442](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1604622713442.png)





### react diff算法是如何提高性能的？

传统的页面更新，是直接操作dom来实现的，比如原生js或者jquery，但是这种方式性能开销比较大；

react 在初始化的时候会生成一个虚拟dom,每次更新视图会比较前后虚拟dom的区别；

这个比较方法就是diff算法，diff算法很早就已经出现了；但是react的diff算法有一个很大区别；

传递diff算法：通过循环递规对节点进行依次对比，时间算法复杂度是 o(n^3)，n代表节点数；

react diff 算法：    采用逐层比较的方式；把时间算法复杂度从O（n^3）次方，降低到O(n)次方；





### 什么是react虚拟dom，为什么虚拟dom可以提高性能，如何监测性能提升？

虚拟dom是真实dom的一份映射表，react中我们只要改变state,

react就会调用batching（批处理）、diff算法自动更新虚拟dom；

虚拟dom再来操作真实dom,从而改变视图；

当页面参数修改的时候不进行重绘，只修改改变的内容。

可以通过chrome的console面板中的




### 为什么setState不设计成同步的？

- 保持内部的一致性，和状态的安全性

保持state,props.refs一致性；

-  性能优化

react会对依据不同的调用源，给不同的 setState调用分配不同的优先级；

调用源包括：事件处理、网络请求、动画 ；

- 更多可能性

异步获取数据后，统一渲染页面；保持一致性，



### setState到底是同步还是异步？

>  结论：有时表现出同步，有时表现出“异步“

表现场景：

- **同步**：setTimeout，原生事件；
- **异步**：合成事件，钩子函数( 生命周期 ); 

react异步说明：

>  setState 异步并不是说内部代码由异步代码实现，其实本身执行过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前；在异步更新中，多次setState后面的值会覆盖前面的；





### react生命周期？

react 生命周期主流的主要有2个大的版本；

一个是 v16.3之前的：

一个是v16.3之后的；

v16.3之前 的生命周期主要分为4个阶段,8个生命周期：

- 初始化值阶段 initialization： getDefaultProps,getInitialState;
- 初始阶段 mount： componentWillMount,componentDidMount;
- 更新阶段 update：componetWillReceiveProps ,shouldComponetUpdate  ,componentWillUpdate,
    - 销毁阶段 unmount：componetWillMount;

v16.3之后的生命周期:

    新引入了两个生命周期：

- getDerivedStateFromProps
- getSnapshotBeforeUpdate

提示3个不安全的生命周期（在v17中要被删除）

- componentWillMount
- componentWillReceiveProps
- componetWillUpdate



![react老的生命周期](https://techstudyblog.top/2019/09/28/react-life-cycle/react.png)



新的命令周期：

![react新生命周期](https://pic1.zhimg.com/v2-4bc3a7a23ed8047eac25a62ef22cf205_1440w.jpg?source=172ae18b)







### create-react-app有什么优点和缺点？

优点：

快速生成架手架

缺点：

1. 默认不引入polyfill,需要在入口引入babel-polyfill
2. 默认只支持css热加载，不支持html,js热加载（推荐使用：react-hot-loader）





### 介绍下Immutable?

    Immutable是一种不同变的数据类型，

数据一旦被创建，就不能更改的数据，每当对它进行修改，就会返回新的immutable对象；

在做对象比较时，能提升性能；

immutable实现原理是持久化数据结构，结构共享，避免对数据对象进行深拷贝；





### react、vue有什么区别？

react优势:

    - 可以使用shouldComponetUpdate控制是否render，当应用比较大的时候，这些优化点比较好
- react组件是函数，vue组件 高度封装的函数; react的发挥控件更大；所以react流行hoc，

vue优势：

- 兼容老项目
- 上手简单 ,vue模板语法，react ,jsx





- 如果你能够改进React的一样功能，那会是哪一个功能？
- React 的事务？
- 为什么说react是一个ui框架？





### 对react的看法，它的优缺点？

优点：

1. 提供了声明式的编程思想
2. 提供了组件化的开发思想，大大提高前端开发的效率
3. 引入了虚拟dom的概念，使得react可以跨端进行类各界面开发，react native,react vr,ssr;
4. 引入了diff算法，提高了渲染效率

不足：

1. 侵入性强，对老代码只能重做，不能部分集成（比如vue）；









### jsx语法有什么特点相比js?

    答：jsx以js为中心来渲染html,
    
    jsx语法特点：

1. 支持js+html混写；
2. jsx编译更快比html

优点：jsx类型安全的，在编译过程中就能发现错误；





### create-react-app 如何实现，包含哪些内容，如何自定义一个脚手架？

定义：create-react-app是一个快速生成react项目的脚手架；

优点：

- 无需设置webpack配置

缺点：

- 隐藏了webpack,babel presets的设置，修改起来比较麻烦



## HOC



### 什么是高阶组件，它有哪些运用？

高阶组件就是一个函数，接收一个组件，经过处理后返回后的新的组件；

高阶组件，不是真正意义上的组件，其实是一种模式；

可以对逻辑代码进行抽离，或者添加某个共用方法；

主要用途：

- 代码重用，逻辑和引导抽象
- 渲染劫持
- 状态抽象和控制
- Props 控制

eg：

react-redux   ：connect就是一个高阶组件,接收一个component,并返回一个新的componet,处理了监听store和后续的处理 ；

react-router  ：withrouter 为一个组件注入 history对象；



### 高阶组件和父组件的区别？

高阶组件可以重写传入组件的state,function,props;可以对代码逻辑进行抽离，重写 ；

父组件只是控制子组件的view层；





## hook



参考资料： [Hook API 索引](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)

### react hook优点？

react hook是v16.8的新特性；

传统的纯函数组件，

react hooks设计目的，加强版的函数组件，完全不使用‘类’，就能写出一个全功能的组，不能包含状态，也不支持生命周期）；

优势：

1. 在不使用class的情况下，使用state及react其他特性（省的把纯函数组件/其他组件改来改去）
2. 让组件更好的复用（老的class组件冗长、包含自身的状态state）
3. 让react编程风格更取向函数式编程风格，使用function代替class
4. 处理了this的指向问题

缺点：

- 响应式的useEffect： 当逻辑较复杂时，可触发多次
- 状态不同步：函数的运行是独立的，每个函数都有一份独立的作用域。函数的变量是保存在运行时的作用域里面，当我们有异步操作的时候，经常会碰到异步回调的变量引用是之前的，也就是旧的（这里也可以理解成闭包）



hooks（本质是一类特殊的函数，可以为函数式注入一些特殊的功能）的主要api:

基础Hook:

- useState : 声明状态变量
- useEffect ：提供了类似于componentDidMount等生命周期钩子的功能
- useContext ：提供上、下文的功能

额外的Hook:

- useReducer
- useCallBack
- useMemo
- useRef
- useLayoutEffect
- useDebugValue



### useEffect 介绍？

> useEffect可以让你在函数组件中执行副使用（数据获取，设置订阅，手动更改React组件中的DOM）操作

默认情况下每次函数加载完，都会执行（不要在此修改state，避免循环调用），useEffect相当于是class组件中的componentDidMount，componentDidUpdate,componentWillUnMount三个函数的组合

**参数说明：**

- 参数一：
- 参数二（可选）：数组，监听值
  - 【只执行一次】如果只想初始化时加载一次，可以输入第二个参数[]，这就告诉react你的effect不依赖于props或 state中任何值，所以它永远不需要重复执行。
  - 【当监听值改变才执行】如果值没有改变，就不用执行effect函数，可以传入监听的值



知识点：

- useEffect相比componentDidMount/componentDidUpdate不同之处在于，使用useEffect调度的effect不会阻塞浏览器更新屏幕，这让应用响应更快，大多数据情况下，effect不需要同步地执行，个别情况下（例如测量布局），有单独的useLayoutEffect hook可使用，其API与useEffect相同



**分类：**

**一）不需要清除**

> react更新DOM之后运行的一些额外代码，操作完成即可忽略

使用场景:

- 手动变更DOM（修改title）
- 记录日志
- 发送埋点请求



**二）需要清除**

> effect返回一个函数，在清除时调用

由于添加/删除订阅代码的紧密性，所以useEffect设计在同一个地方，如果effect返回一个函数，react将会在执行清除（相当于class中componentWIllMount生命周期）时调用它

使用场景：

- 订阅外部数据源（防止数据泄露）



### useCallBack介绍?







## Fiber



### 什么是Fiber？是为了解决什么问题？

fiber是react16中新发布的特性；

解决的问题：

react在渲染过程时，从setState开始到渲染完成，中间过程是同步；

如果渲染的组件比较大，js执行会长时间占有主线程，会导致页面响应度变差，使得react在动画，手势等应用中效果比较差；

实现过程及原理（核心理念就是：time slicing）：

1. 拆分：把渲染过程进行拆分成多个小任务
2. 检查：每次执行完一个小任务，就去对列中检查是否有新的响应需要处理
3. 继续执行：如果有就执行优化及更高的响应事件，如果没有继续执行后续任务



## refs



### react的refs有什么用，使用场景？

react 主要提供了一种标准数据流的方式来更新视图；

但是页面某些场景是脱离数据流的，这个时候就可以使用 refs;

react refs 是用来获组件引用的,取后可以调用dom的方法；

使用场景

- 管理焦点、选择文本或者媒体播放时，获取焦点  this,refs.inputPhone.focus();
- 触发式动画
- 与第三方DOM库整合

refs 注意事项：

不能在无状态组件中使用refs











