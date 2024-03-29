# react


[TOC]



### react 如何实现keep-alive?

> Keep-alive是缓存路由使用的，保留之前路由的状态

实现方法：

1. 使用npm库：
   1. [react-router-cache-router](https://github.com/CJY0208/react-router-cache-route/blob/HEAD/README_CN.md)
   2. [React-activation](https://github.com/CJY0208/react-activation)







### react错误处理？


主要的api(生命周期):

- componentDidCatch(error,errorInfo):    同样可以将错误日志上报给服务器
- getDerivedStateFromError(error)：更新state使下一次渲染能够显示降级后的UI

注意事项：

- 仅可捕获其子组件的错误，无法捕获其自身的错误





### 你有使用过suspense组件吗？

动态加载（异步组件）加载时会有延迟，在延迟期间可以将一些内容展示给用户，比如：loading

(react16.6新增的API)

```js
const resource = fetchProfileData();

function ProfilePage() {
  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails />git pull
        
      <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline />
      </Suspense>
    </Suspense>
  );
}

function ProfileDetails() {
  // 尝试读取用户信息，尽管该数据可能尚未加载
  const user = resource.user.read();
  return <h1>{user.name}</h1>;
}

function ProfileTimeline() {
  // 尝试读取博文信息，尽管该部分数据可能尚未加载
  const posts = resource.posts.read();
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.text}</li>
      ))}
    </ul>
  );
}
```





参考资料：

[何为 Suspense？](https://zh-hans.reactjs.org/docs/concurrent-mode-suspense.html)







### 怎么动态导入组件，按需加载，代码分割？

> 只有当组件被加载时，对应的资源才会导入

- react-loadable:  npm 库 按需加载
- react.lazy:  原生支持（新版本16.6），配合suspense一起使用，还要webpack code splitting
- require(component) : 在特定条件下，动态引入







### react Context介绍？

主要api:

- react.createContext :　创建store
- [store].Provider: 包裹组件，并通过value 字段传递参数
- [store].Consumer:  获取包裹组件内容



创建过程：

1. 【创建store】：通过 React.createContext 创建 AppContext 实例

2. 【包裹整个组件】：使用AppContext.Provider组件

3. 【注入全局变量】，在AppContext.provider组件上

4. 【引入全局变量】： 通过 AppContext.Consumer组件 ，子组件的回调，获取store中的内容和方法





### 为什么react并不推荐我们优先考虑使用context?

- 【实验性】context目前还处于实验阶段，可能会在后期有大改变，避免给未来升级带来麻烦
- 【稳定性】context的更新需要通过 setState触发，但是这并不是可靠的。context劫持跨组件访问，但是，如果中间子组件通过一些方法不响应更新，比如 shouldComponentUpdate返回false，那么不能保证 context的更新一定达使用context的子组件。因此，context的可靠性需要关注 。不过是更新的问题，在新版的APP中得以解决

> 只要你能确保 context是可控的，合理使用，可以给react组件开发带来强大体验





### render函数中return如果没用使用()会用什么问题吗？

结论：如果换行就有问题

原因：babel会将jsx语法编译成js，同时会在每行自动添加分号（;）, 如果 return 换行了，那么就变成了return;  就会导致报错

```
  //这样会报错，react
  class App extends React.Component{
        render(){
            return 
            <div>111</div>
        }
    }
```





### 介绍下渲染属性render props？

> 功能：给纯函数组件加上state,响应react的生命周期

优点：ｈｏｃ的缺点render prop 都可以解决

- 扩展性限制：hoc无法从外部访问子组件的state,因此无法通过shouldComponentUpdate 过滤掉不必要的更新，react在支持es6 class之后提供了react.PureComponnet来解决这个问题
- ref传递问题：ref被隔断，后来的react.forwardRef来解决这个问题
- Wrapper Hell(多层包裹)：多层包裹，抽象增加了复杂度和理解成本
- 命名冲突：多次嵌套，容易覆盖老属性
- 不可见性：hoc相当于黑盒

缺点：

- 使用繁琐：hoc复用只需借助装饰器语法（decorator）一行代码进行复用，render props无法做到如此简单
- 嵌套过深：render props 虽然摆脱了组件多层嵌套的问题，但是转化为了函数回调的嵌



参考资料：

[React 中的 Render Props](https://zhuanlan.zhihu.com/p/31267131)



### React如何进行组件/逻辑复用？

- HOC（高阶组件）
- - 属性代理
  - 反向继承
- 渲染属性(render props)
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



![image-20210304115509312](https://i.loli.net/2021/03/04/FjGvlQWsEeaNdiB.png)







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

> Immutable是一种不同变的数据类型，数据一旦被创建，就不能更改的数据，每当对它进行修改，就会返回新的immutable对象，在做对象比较时，能提升性能；

实现原理：

immutable实现原理是持久化数据结构，结构共享，避免对数据对象进行深拷贝；





### react、vue有什么区别？



| 区别                   | react                                                    | vue                                                          |
| ---------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 模板引擎               | JSX，更多灵活，纯js语法（可以通过babel插件实现模板引擎） | vue template,指令，更加简单 （vue也可以使用jsx语法）         |
| 复用                   | Mixin->Hoc->render props->hook                           | Mixin-> slot->Hoc(比较用，模板套模板，奇怪做法)-> vue3.0 function baseed APi |
| 性能优化方式           | 手动优化：PureComponent/shouldComponentUpdate            | 自动优化：精确监听数据变化                                   |
| 监听数据变化的实现原理 | 手动：通过比较引用的方式（diff）                         | 自动：getter/setter以及一些函数的劫持（当state特别多的时候，当watcher也会很多，导致卡顿） |
| 数据流                 | 数据不可变，单向数据流，函数式编程思想                   | 数据可变，双向数据绑定，（组件和DOM之间的），响应式设计      |
| 设计思想               | all in js (html->jsx,  css->style-component/jss)         | html,,css,js写在一个文件中，使用各自的方式                   |
| 功能内置               | 少（交给社区，reactDom,propType）                        | 多（使用方便）                                               |
| 优点                   | 大型项目更合适                                           | 兼容老项目，上手简单                                         |



数据流对比：

![img](https://user-gold-cdn.xitu.io/2018/9/2/165984beb2c823d1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

react 数据更改逻辑：

![img](https://ask.qcloudimg.com/http-save/yehe-1260054/bdof6i6pdv.jpeg?imageView2/2/w/1620)



vue数据更改逻辑：

![img](https://ask.qcloudimg.com/http-save/yehe-1260054/wz8yf1jeu2.jpeg?imageView2/2/w/1620)



参考资料：

[Vue 和 React 的优点分别是什么？](https://www.zhihu.com/question/301860721)

[个人理解Vue和React区别](https://juejin.cn/post/6844903668446134286)

[Vue与React的区别之我见](https://cloud.tencent.com/developer/article/1518142)







### 让react支持vue的template语法如何实现?

基于抽象语法树AST，实现解析模板指令的插件（应该是实现一个babel插件，因为jsx解析成js语法，是通过babel解析的）

![img](https://pic3.zhimg.com/80/v2-c186c225f687bb7e5e77eeac200acda3_720w.jpg?source=1940ef5c)







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
>  高阶组件是高阶函数的衍生
>
>  核心功能：实现抽象和可重用性



它的函数签名可以用类似hashell的伪代码表示

- W（WrappedComponent）被包裹的组件，当传参数传入hoc函数中

- E（EnhancedComponent）返回的新组件

```
hocFactory:: W: React.Component => E: React.Component
```



高阶组件，不是真正意义上的组件，其实是一种模式；

可以对逻辑代码进行抽离，或者添加某个共用方法；

高阶组件是装饰器模式在react中的实现 





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





### 你在项目中怎么使用的高阶组件？

> 不谈场景的技术都是在耍流氓

- **权限控制**：//也可以在history路由切换的时候，加个监听方法，在顶层做监听处理，不过页面会闪一下
  - 页面级别：withAdminAuth(Page)，withVIPAuth(Page)
  - 页面元素级别：
- **组件渲染性能追踪**
- **页面复用**
- 全局常量（通过接口请求），能过hoc抽离  //也可以通过全局状态管理来获取
- 对当前页面的一些事件的默认执行做阻止（比如：阻止app的默认下拉事件）



参考资料：

[React 中的高阶组件及其应用场景](https://juejin.cn/post/6844903782355042312#heading-23)





### 高阶组件和父组件的区别？

高阶组件可以重写传入组件的state,function,props;可以对代码逻辑进行抽离，重写 ；

父组件只是控制子组件的view层；





## hook



### hooks原理分析，如何实现？







参考：

[[译] 深入 React Hook 系统的原理](https://juejin.cn/post/6844903807269208072)





### hooks与 react 生命周期的关系？

hooks组件有生命周期，函数组件没有生命周期

hooks的生命周期其实就是：

- useState
- useEffect
- useLayoutEffect



//hooks模拟生命周期函数，与class的生命周期有什么区别

```

```





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
   
     

缺点（坑）：

- 【useState数组修改】使用useState修改array的值时，不要使用push/pop/splice等直接更改数据对象的方法，否则无法修改，应该使用解构或其他变量代替
- 【hook执行位置】不要在循环、条件 、嵌套中调有hook，必须始终在react函数顶层使用Hook，这是因为react需要利用调用顺序来正确更新相应的状态，以及调用相应的钩子函数，否则会导致调用顺序不一致性，从而产生难以预料到的后果
- 响应式的useEffect： 当逻辑较复杂时，可触发多次
- 状态不同步：函数的运行是独立的，每个函数都有一份独立的作用域。函数的变量是保存在运行时的作用域里面，当我们有异步操作的时候，经常会碰到异步回调的变量引用是之前的，也就是旧的（这里也可以理解成闭包场景可能引用到旧的state、props值），希望输出最新内容的话，可以使用useRef来保存state
- 破坏了pureComponent、react.memo 浅比较的性能优化效果（为了取最新的props和state,每次render都要重新创建事件处函数）
- react.memo 并不能完全替代 shouldComponentUpdate （因为拿不到 state change ,只针对 props change）



参考资料：
 [hooks中的坑，以及为什么？](https://blog.csdn.net/kellywong/article/details/106430977)





### hooks注意事项？

- 不同hook中，不要使用判断（if）

react hook底层是基于链表（Array）实现，每次组件被render时，会按顺序执行所有hooks,因为底层是链表，每个hook的next指向下一个hook，所有不能在不同hooks调用中使用判断条件，因为if会导致顺序不正确，从而导致报错



```js
//错误示例
function App(){
const [name,setName]=useState('xz');
//这里不能使用if判断
if(configState){
	const [age,setAage]=useState('0')
}
}

```







### react hooks异步操作注意事项？

**一、如何在组件加载时发起异步任务**

**二、如何在组件交互时发起异步任务**

**三、其他陷阱**



参考资料：

[React Hooks 异步操作踩坑记](https://zhuanlan.zhihu.com/p/87713171)





### react hooks API有哪些？



hooks（本质是一类特殊的函数，可以为函数式注入一些特殊的功能）的主要api:

**基础Hook:**

- **useState** : 状态钩子，为函数组件提供内部状态
- **useEffect** ：副作用钩子，提供了类似于componentDidMount等生命周期钩子的功能
- **useContext** ：共享钩子，在组件之间共享状态，可以解决react逐层通过props传递数据；

**额外的Hook:**

- **useReducer**: action钩子，提供了状态管理，其基本原理是通过用户在页面上发起的action，从而通过reduce方法来改变state，从而实现页面和状态的通信，使用很像redux
- **useCallBack**：把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的memoized版本，该回调函数仅在某个依赖项改变时才会更新
- **useMemo**：把""创建""函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时重新计算， 可以作为性能优化的手段。
- **useRef**：获取组件的实例，返回一个可变的ref对象，返回的ref对象在组件的整个生命周期内保持不变
- **useLayoutEffect**： 它会在所有DOM变更后同步调用effect
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
  - type:function
  - 执行的函数
- 参数二（可选）：监听值
  - type:array
  - 要监听的值（当监听值改变才执行，如果只想执行一次可以传一个[]）：如果值没有改变，就不用执行effect函数，可以传入监听的值
    - return callBack: 清理函数，执行有两种情况
      - componentWillUnmount
      - 在每次userEffect执行前(第二次开始)

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





### useEffect和useLayoutEffect区别？

- useEffect相比componentDidMount/componentDidUpdate不同之处在于，使用useEffect调度的effect不会阻塞浏览器更新屏幕，这让应用响应更快，大多数据情况下，effect不需要同步地执行，个别情况下（例如测量布局），有单独的useLayoutEffect hook可使用，其API与useEffect相同
- useEffect在副使用结束之后，会延迟一段时间执行，并非同步执行。遇到dom操作，最好使用userLayoutEffect
- userLayoutEffect 里面的callback函数会在DOM更新完成后立即执行，但是会在浏览器进行任何绘制前运行完成，阻塞了浏览器的绘制



### useContext介绍?

> 共享状态钩子，在组件之间共享状态，可以解决react 逐层通过props传递数据的问题



使用流程（使用流程和react-redux差不多）：

1. 创建store：通过 createContext Api

2. 包裹整个组件：通过store中的Provider方法

3. 注入全局变量，provider组件中

4. 引入全局变量： 通过 useContext,传入store的名字，返回一个store对象内容


```js
    const { useState, createContext, useContext } = React;

    function App(){
        //创建store
        const AppContext=createContext({});

        function A(){
            //从store中取值
            const {name}=useContext(AppContext);
            return <div>A组件Name:{name}</div>
        }

        function B(){
            //从store中取值
            const {name}=useContext(AppContext);
            return <div>B组件Name:{name}</div>
        }

        //在顶层包裹所有元素，注入到每个子组件中
        return (
            <AppContext.Provider value={{name:'xz'}}>
                <A/>
                <B/>
            </AppContext.Provider>
        )
    }

    ReactDOM.render(
        <App />,
        document.getElementById('app')
    );
```



参考资料：

[React Hooks 常用钩子及基本原理](https://blog.csdn.net/chenzhizhuo/article/details/104159910)





### useReducer介绍？

> action 钩子，提供了状态管理



实现过程(和redux差不多，但无法提供中间件等功能 )：

1. 用户在页面上**发起action** （通过dispath方法）
2. 从而通过**reducer方法**来改变state,从而实现 页面和状态的通信





### 如何创建自己的hooks?

实现步骤：

1. 定义一个 hook函数，并返回一个数组（内部可以调用react其他hooks）

2. 从自定义的hook函数中取出对象的数据，做业务逻辑处理即可



### useCallBack介绍?

useCallBack 返回一个[memoized](https://en.wikipedia.org/wiki/Memoization) （记忆组件）回调函数。

主要用来做性能优化的。接收一个函数，和函数依赖的数据项。

只有当依赖项变化时，才会返回一个新的值。这样可以避免在传递给子组件时的重复渲染。



参考资料：

- https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback



###  useState介绍？

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





### 介绍下useMemo，它和memo有什么区别，分别有哪些适用场景？

useMemo是一个函数，接收两个参数，第一个是一个函数，第二个参数是依赖项。返回一个被memoized 记忆数据对象。



memo是一个高阶组件。接收一个组件，并返回一个组件。

memo接收两个参数，第一个参数是一个组件，第二个参数（可选参数）是一个函数，此函数会返回一个布尔值，用来判断该组件是否需要重新渲染。

```js
// 简单数据（非引用对象，包括string, number, boolean）,会做浅比较
const NewMyComponent = memo(MyComponent);

// 复杂对象的比较需要使用第二个参数来判断，通过返回的布尔值来判断是否重新渲染
const NewMyComponent = memo(MyComponent,(prevProps, nextProps)=>{
	// 返回true代表相同，则不渲染。返回false代表参数改变，会重新渲染。
	return true;
})
```



相同点：

- 都是利用缓存 来做性能优化的



不同点：

- 类型不同
  - memo: 是一个高阶组件
  - useMemo：是一个hook
- 缓存的类型不一样
  - memo: 缓存组件,避免组件重复渲染
  - useMemo: 缓存组件中的数据，避免计算属性，重复计算



参考资料：

- [React.memo() 和 useMemo() 的用法和区别](https://juejin.cn/post/6991837003537088542#heading-2)





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

**1、refs是什么**

refs在计算机中叫做弹性文件系统（Resilient File System）。

通过refs可以访问组件实例，或者是dom节点。



**2、 为什么react要提供ref来返回组件实例和dom节点**

react 主要提供了一种标准数据流的方式来更新视图；

但是页面某些场景是脱离数据流的，这个时候就可以使用 refs;

react refs 是用来获组件引用的，读取后可以访问dom的属性和方法；



**3、使用场景**

- 管理焦点、选择文本或者媒体播放时，获取焦点  this,refs.inputPhone.focus();
- 触发式动画
- 与第三方DOM库整合

refs 注意事项：

不能在无状态组件中使用refs



参考资料：

- https://vue3js.cn/interview/React/React%20refs.html#%E4%B8%80%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88



### 创建refs有哪几种形式？

1、**传入字符串 （不推荐使用）**

```js
<div ref="myRef">元素</div>
myRef.innerHTML = "hello";

```



**2、传入对象:** 

```js
const myRef = React.createRef();
// 设置元素的的内容
myRef.innerHTML="hello"
<div ref={myRef}>元素</div>
```



**3、传入函数**

该函数会在Dom被挂载时进行回调，获取到元素信息后自己保存即可

```js
const myRef = React.createRef();
// 设置元素的的内容
myRef.innerHTML="hello"
<div ref={(element)=>{
	myRef=ref;
}}>元素</div>
```



**4、传入hooks**

```js
const myRef = React.useRef();
// 设置元素的内容
myRef.innerHTML="hello"
<div ref={myRef}>元素</div>
```



参考资料：

- https://vue3js.cn/interview/React/React%20refs.html#%E4%BA%8C%E3%80%81%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8





### react中useRef和createRef有什么区别?

 **相同点**

- 功能一致，都是获取元素dom的方法。
- 都是一个函数，返回一个元素的dom指引



**不同点**

- 使用场景范围不一样
  - useRef： 只支持函数组件(functionComponet)  // 原因：因为它是hooks, 所以不能用在类组件
  - createRef：使用范围更广泛，支持类组件（classComponent），和函数组件。
- 返回对象不一样
  - useRef：每次返回的引用相同 
  - createRef: 每次返回一个新的引用



参考资料：

- [useRef、createRef的区别及使用，及useRef妙用](https://juejin.cn/post/6950845509137334309)





### 介绍下forwardRef，它有哪些运用场景？

功能介绍：

forwardRef用在函数组件中，用来转发父组件的ref对象给子组件，让父组件可以操作子组件的dom节点。



使用场景：

- 类组件当中（直接通过ref获取到实例）：父组件获取子组件的值和触发父组件的方法
- 函数组件当中（需要配合forwardRef使用）：父组件获取子组件dom的节点，从而触发子组件当中，dom的原生事件，比如：焦点操作，属性设置



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





## setState





### setState之后发生了什么？

1. 【数据合并】多个setState会进行数据合拼，准备批量更新
2. 【数据合并到组件的当前状态】生成新的 react tree
3. 【更新UI】比较使用diff算法，比较新旧 virtual dom,，最小化DOM操作
4. 【执行回调函数】setState第二个参数





### setState到底是同步还是异步？

> 结论：有时表现出同步，有时表现出“异步“

表现场景：

- **同步**：setTimeout，原生事件；
- **异步**：合成事件，钩子函数( 生命周期 ); 

react异步说明：

> setState 异步并不是说内部代码由异步代码实现，其实本身执行过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前；在异步更新中，多次setState后面的值会覆盖前面的；





### 为什么setState不设计成同步的？

- 保持内部的一致性，和状态的安全性

保持state,props.refs一致性；

- 性能优化

react会对依据不同的调用源，给不同的 setState调用分配不同的优先级；

调用源包括：事件处理、网络请求、动画 ；

- 更多可能性

异步获取数据后，统一渲染页面；保持一致性，





## react事件



### react事件机制？



**一、事件注册**

- 【组件装载】装载/更新
- 【事件注册与卸载】能过**lastProps**，**nextProps**判断是否有新增、删除事件分别调用事件注册、卸载方法
- 【事件存储】能过eventPluginHub, enqueuePutListener 进行事件存储
- 【获取document对象】
- 【事件冒泡/捕获】根据事件名称（onClick,onCaptureClick）判断
- 【事件监听】：addEventListener,addachEvent(兼容IE)
- 【注册原生事件】给document注册原生事件回调为dispatchEvent (统一的事件分发机制)



**二、事件存储**

```js
{
onClick:{
 fn1:()=>{}
 fn2:()=>{}
},
onChange:{
 fn3:()=>{}
 fn4:()=>{}
}
}
```



**三、事件触发/执行**

 	事件执行顺序（原生事件与合成事件）

1. dom child

       2. dom parent
    3. react child
    4. react parent
    5. dom document



**四、合成事件**

1. 【调用EventPluginHub】 的 extractEvents 方法
2. 【遍历所有EventPlugin】 用来处理不同事的工具方法
3. 【返回事件池】在每个 EventPlugin 中根据不同的事件类型返回
4. 【取出合成事件】从事件池中取出，如为空，则创建
5. 【取出回调函数】根据元素nodeid(唯一标识key) 和事件类型 从listenerBink 中取出 回调函数
6. 【返回合成事件】返回带有合成事件参数的回调函数



![img](https://lsqimg-1257917459.cos-website.ap-beijing.myqcloud.com/blog/%E5%90%88%E6%88%90%E4%BA%8B%E4%BB%B6.png)





参考资料：

[【React深入】React事件机制](https://juejin.cn/post/6844903790198571021#heading-1)





### react事件与原生事件的区别？

**语法区别：**

- 【事件名小驼峰】react事件命令采用小驼峰式，而不是纯小写
- 【事件方法函数】使用JSX语法时，你需要传入一个函数作为事件处理函数，而不是一个字符串

**react事件的优点**

- 【兼容性更强】合成事件：react事件对生成事件进行了包装，处理了浏览器兼容性问题（阻止浏览器默认行为，阻止冒泡）





### react事件与原生事件的执行顺序？

1. 原生事件，依次冒泡执行

2. react合成事件，依次冒泡执行

3. document 事件执行

```js
  /*
     * 执行结果：
     * 1. dom child
     * 2. dom parent
     * 3. react child
     * 4. react parent
     * 5. dom document
     * */
```





### react事件与原生事件可以混用吗？

> react事件与原生事件最好不要混用

原因：

- 原生事件如果执行 `stopProagation` 方法，则会导致其他 react 事件失效，因为所有元素的事件将无法冒泡到 document上







## react-router



### React-Router怎么设置重定向？

使用 重定向 Api    : Redirect



![img](https://img-blog.csdnimg.cn/20190323185801166.png)

