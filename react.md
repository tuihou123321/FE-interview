# react


[TOC]





### cli工具？

- commander  //解析node 参数
- inquirer   //交互式命令
- download-git-repo   //下载远程代码 



参考：

[仿 vue-cli 搭建属于自己的脚手架]( https://juejin.cn/post/6844903807919325192#heading-14)



### React如何进行组件/逻辑复用？

- HOC（高阶组件）
- - 属性代理
  - 反向继承
- 渲染属性
- react-hooks
- Mixin (已废弃，不讨论)





### PureComponent组件介绍？

> 当props/states改变时，PureComponent会对它们进行浅比较，起到性能优化的作用；
>
> 相当于在component组件的shouldComponentUpdate方法中进行了比较才渲染



特别说明 ：

- 引用对象的数据建议不要使用PureComponnet组件，否则会有坑





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

- 父子组件：**props**，props回调
- 兄弟组件：**共同父级**，再由父节点转发props，props回调
- 跨级组件：**context对象**，注入全局变量：getChildContext;   获取全局变量：this.context.color;
- 非嵌套组件：使用**事件订阅**，向事件对象添加监听器，和触发事件来实现组件之间的通讯，通过引入event模块实现
- 全局状态管理工具：Redux,Mobox维护全局store





### react UI组件和容器组件的区别与应用？

容器组件：拥有自己的状态，生命周期

UI组件：只负责页面UI渲染，不具备任何逻辑，功能单一，通常是无状态组件，没有自己的state,生命周期。



![1604622713442](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1604622713442.png)








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
- 初始阶段 mount： componentWillMount,**componentDidMount**;
- 更新阶段 update：componetWillReceiveProps ,**shouldComponetUpdate**  ,componentWillUpdate,
- 销毁阶段 unmount：**componetWillUnmount**;

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





### React的请求放在componentWillMount有什么问题？

错误观念：componentWillMount中可以提前进行异步请求，避免白屏时间；

分析：componentWillMount比 componentDidMount相差不了多少微秒；



问题

- 在SSR（服务端渲染）中，componentWillMount生命周期会执行两次，导致多余请求
- 在react16进行fiber重写后，componentWillMount 可能在一次渲染中多次调用
- react17版本后要删除componentWillMount生命周期

目前官方推荐异步请求在 **componentDidMount**中





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

> jsx以js为中心来写html代码

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





### 什么是高阶函数？

> 如果一个函数，**接受一个或多个函数作为参数或者返回一个函数**，就可称之为**高阶函数**

特点：

- 是函数
- 参数是函数
- or 返回是函数

eg:  array  对象中的 map,filter,sort方法都是高阶函数

```js
  function add(x,y,f){
      return f(x)+f(y)
  }

  let num=add(2,-2,Math.abs)
  console.log(num);
```





### 什么是高阶组件？

>  高阶组件就是一个函数（react函数组件），接收一个组件，处理后返回的新组件
>
> 高阶组件是高阶函数的衍生



它的函数签名可以用类似hashell的伪代码表示

- W（WrappedComponent）被包裹的组件，当传参数传入hoc函数中

- E（EnhancedComponent）返回的新组件

```
hocFactory:: W: React.Component => E: React.Component
```



高阶组件，不是真正意义上的组件，其实是一种模式；

可以对逻辑代码进行抽离，或者添加某个共用方法；





**主要用途：**

- 代码重用，逻辑和引导抽象
- 渲染劫持
- 状态抽象和控制
- Props 控制



参考资料：[React 中的高阶组件及其应用场景](https://zhuanlan.zhihu.com/p/61711492)





### hoc存在的问题？

**一、静态方法丢失**





**二、refs属性不能透传**





**三、反向继承不能保证完整的子组件树被解析**









### hoc高阶组件使用场景？

react-redux   ：connect就是一个高阶组件,接收一个component,并返回一个新的componet,处理了监听store和后续的处理 ；

react-router  ：withrouter 为一个组件注入 history对象；





### 高阶组件和父组件的区别？

高阶组件可以重写传入组件的state,function,props;可以对代码逻辑进行抽离，重写 ；

父组件只是控制子组件的view层；





## hook



### hooks原理分析，如何实现？





参考：

[[译] 深入 React Hook 系统的原理](https://juejin.cn/post/6844903807269208072)





### react hook优缺点？



react hook是v16.8的新特性；

传统的纯函数组件，

react hooks设计目的，加强版的函数组件，完全不使用‘类’，就能写出一个全功能的组件，不能包含状态，也不支持生命周期）， hook在无需修改组件结构的情况下复用状态逻辑；

优势：

-  简洁：react hooks解决了hoc和render props的嵌套问题，更加简洁 （在不使用class的情况下，使用state及react其他特性（省的把纯函数组件/其他组件改来改去））
-  解耦：react hooks可以更方便地把UI和状态分离，做到更彻底的解耦
-  组合：hooks 中可以引用另外的hooks 形成新的hooks， 组合千变万化
-  函数友好：react hooks为函数组件而生，解决了类组件的几大问题
   - 处理了this的指向问题
   
   - 让组件更好的复用（老的class组件冗长、包含自身的状态state）
   
   - 让react编程风格更取向函数式编程风格，使用function代替class
   
     

缺点：

- 响应式的useEffect： 当逻辑较复杂时，可触发多次
- 状态不同步：函数的运行是独立的，每个函数都有一份独立的作用域。函数的变量是保存在运行时的作用域里面，当我们有异步操作的时候，经常会碰到异步回调的变量引用是之前的，也就是旧的（这里也可以理解成闭包场景可能引用到旧的state、props值）
- 破坏了pureComponent、react.memo 浅比较的性能优化效果（为了取最新的props和state,每次render都要重新创建事件处函数）
- react.memo 并不能完全替代 shouldComponentUpdate （因为拿不到 state change ,只针对 props change）





### react hooks API有哪些？



hooks（本质是一类特殊的函数，可以为函数式注入一些特殊的功能）的主要api:

**基础Hook:**

- **useState** : 声明状态变量
- **useEffect** ：提供了类似于componentDidMount等生命周期钩子的功能
- **useContext** ：提供上、下文的功能

**额外的Hook:**

- **useReducer**: useState的替代方案，使用useReducer 会让触发深新的组件做性能优化，因为你可以向子组件传递dispatch而不是回调函数
- **useCallBack**：把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的memoized版本，该回调函数仅在某个依赖项改变时才会更新
- **useMemo**：把""创建""函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时重新计算， 可以作为性能优化的手段。
- **useRef**：返回一个可变的ref对象，返回的ref对象在组件的整个生命周期内保持不变
- **useLayoutEffect**： 它会在所有DOM变更后同步调用effect,
- **useDebugValue**



demo:

```js
//useMemo 
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

//useReducer
const [state, dispatch] = useReducer(reducer, initialArg, init);

```





参考资料： [Hook API 索引](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)





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

> effect返回一个函数，在清除时调用 （相当于class中componentWillUnmount生命周期）

由于添加/删除订阅代码的紧密性，所以useEffect设计在同一个地方，如果effect返回一个函数，react将会在执行清除时调用它

使用场景：

- 订阅外部数据源（防止数据泄露）
- 设置页面标题，返回重置原标题





### useCallBack介绍?







## Fiber



### 什么是Fiber？是为了解决什么问题？

>  react fiber 是一种基于浏览器的**单线程调度算法**



react算法的改变（Fiber实现的原理）：

- 16版本之前： reconcilation 算法实际上是递归，想要中断递归是很困难的
- 16版本之后：开始使用循环来代替之前的递归



**fiber**： 一种将 **recocilation**（递归diff），拆分成无数据个小任务的算法；它随时能够停止，恢复。停止恢复的时机取决于当前的一帧（16ms）内，还有没有足够的时间允许计算



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





## Virtual DOM



### 什么是 Virtual DOM，它的优点？



> Virtual DOM 是对 DOM的抽象，本质是js对象，这个对象就是更加轻量级对DOM的描述



![img](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/1b6e845647c60b0ce00ec91f679ec6cf.png)



**优点：**

- **【性能优化】**操作真实DOM慢，频繁变动DOM会造成浏览器回流/重绘，虚拟DOM抽象的这一层，在patch（batching批处理）过程中尽可能地一次性将差异更新到DOM中，降低更新DOM的频率
- **【数据驱动程序】**使用数据驱动页面，而不是操作DOM的形式
- **【跨平台】**：node层没有DOM,但是react却可以在node层（SSR）运行

可以通过chrome的console面板中的



参考资料：

[虚拟DOM原理](http://www.cxymsg.com/guide/virtualDom.html#%E4%BB%80%E4%B9%88%E6%98%AFvirtual-dom)





### Virtual DOM 的创建，更新，diff 过程？



**虚拟DOM的创建**

虚拟DOM是对真实DOM的抽象，根据不同的需求，可以做出不同的抽象，比较 snabbdom.js 的抽象方式



基本结构

```js
  /*
    * 虚拟DOM 本质是一个JS对象，这个对象是更加轻量级的对DOM的描述
    * */
    
    //tagName,props,children
    const element={
        tagName:'ul',
        props:{
            id:'list'
        },
        children: [{
            tagName:'li',
            props:{
                class:'item'
            },
            children:['item1']  //item1 只是没有包裹的子节点
        }]
    }
```





### react diff 介绍？

传统的页面更新，是直接操作dom来实现的，比如原生js或者jquery，但是这种方式性能开销比较大；

react 在初始化的时候会生成一个虚拟dom,每次更新视图会比较前后虚拟dom的区别；

这个比较方法就是diff算法，diff算法很早就已经出现了；但是react的diff算法有一个很大区别；





**react diff 算法优势：**

**传递diff算法**：

- 遍历模式：**循环递规**对节点进行依次对比
- 时间算法复杂度： o(n^3)次方，n代表节点数



**react diff 算法**：    

- 遍历模式：采用**逐层比较**的方式（DOM的特点，一般很少出现跨层级的DOM变更） 
- 时间算法复杂度：O(n)次方；



目前社区里的两个算法库：

- snabbdom.js（双端比较算法）：v2.0借鉴
- inferno.js（速度更快）： vue3.0借鉴
  - 核心思想：利用LIS（最长递增序列）做动态规划，找到最小的移动次数



react 算法 PK inferno.js

- react diff: 
  1. 把 a,b,c移动到他们相应的位置
  2. 再+1共三步操作
- inferno: 直接把d移动到最前面，一步到位

```
A:[a,b,c,d]
B:[d,a,b,c]
```





同层比较

![img](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/63e704710ceda7b3e14c586eb51b36ae.png)



参考资料：

[diff 算法原理概述](https://github.com/NervJS/nerv/issues/3)

