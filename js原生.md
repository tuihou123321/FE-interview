## js原生



[TOC]





### 什么是事件委托?

> 事件委托就是利用**事件冒泡**，只指定一个事件处理程序，就可以管理某一类型的所有事件，利用父级去触发子级的事件；

优点：

- 节省内存占用，减少事件注册
- 新增子对象时，无需再次对其绑定事件，适合动态添加元素

局限性：

- focus,blur 事件本身没有事件冒泡机制，所以无法委托
- mousemove,mouseout，需要不断通过位置去计算定位，对性能消耗高，不适合事件委托

```
<script>
     // jquery当中的事件委托，支持动态添加的元素
     
     $('#ui').on('click','li',function (event){
        console.log(event.target.innerHTML);
    })
</script>
```





### 解释下变量提升？

js引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行行地运行。这造成了所有变量的声明语句，都会被提升到代码头部，这就叫做变量提升（hoisting）.






### js的执行过程？

> js特点：异步，单线程；

**一、语法分析**
js脚本代码块加载完毕后，进入语法分析，判断 语法是不中正确，如果不正确，向外抛出 **语法错误(syntaxError)**，　停止该ｊｓ代码块的执行，然后继续查找并加载下一个代码块；如果语法正确，则进入预编译阶段

**二、预编译阶段** 

前置知识：
ｊｓ运行环境：
－　**全局环境**：ｊｓ代码加载完毕后，代码进入全局环境）
－　**函数环境**：函数调用执行时，进入该函数环境，不同函数则函数环境不同
－　**ｅｖａｌ**　：不建议使用，有安全、性能问题



预编译阶段过程如下：

**函数调用栈**：

- 形成执行上下文：进入不同环境创建相应的执行上下，可创建多个
- 形成函数调用栈：js引擎以栈的形式处理执行上下文 （栈底永远全局执行上下文，栈顶永远是当前执行上下文）

**创建执行上下文**：

- 创建变量对象

- 建立作用域链

- 确定this指向

  

**三、执行阶段：**

>  可参考：  宏任务与微任务,事件循环event loop介绍





推荐文章：

- [js引擎的执行过程](https://heyingye.github.io/2018/03/26/js%E5%BC%95%E6%93%8E%E7%9A%84%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B%EF%BC%88%E4%BA%8C%EF%BC%89/#%E5%AE%8F%E4%BB%BB%E5%8A%A1)



### js中的宏任务与微任务，事件循环event loop？

**宏任务（macro-task):  **

同步任务   ：js引擎按主线程按顺序执行的任务，一个一个任务执行，形成一个执行栈（函数调用栈）

> eg：console.log(11);   let a=10;

异步任务  :不直接进入js引擎主线程，而是满足条件时触发，相关的线程将该任务推进 **任务队列**，等待js引擎主线程上的任务执行完毕，再读取执行中的任务

> eg：ajax, dom事件，setTimeout



**微任务（micro- task） **

微任务是在es6和node环境中出现的一个任务类型

> eg：promise，process.nextTick

js引擎执行过程： 宏任务（同步任务） ---> 微任务 --> 宏任务（异步任务） 



![img](https://heyingye.github.io/2018/03/26/js%E5%BC%95%E6%93%8E%E7%9A%84%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B%EF%BC%88%E4%BA%8C%EF%BC%89/img/Event%20Loop.jpg)






### 柯里化函数的例子，并说明柯里化的好处？

柯里化是一种模式，其中一个具有多个参数的函数被分解成多个函数，当被串联调用时，这些函数将一次累加一个所需的所有参数。这种技术有助于使用函数式编写的代码更容易阅读和编写。需要注意的是，要实现一个函数，它需要从一个函数开始，然后分解成一系列函数，每个函数接受一个参数。

使用场景：

- 使代码便于理解 //react-redux,connect方法， 将component要用到的state切面和action注入到它的property中，达到ui组件，和容器组件分离的目的。

  let Container=connect(mapStateToProps,mapDispatchToProps)(Component);  





![image-20201105114140755](C:\Users\22064\AppData\Roaming\Typora\typora-user-images\image-20201105114140755.png)



```
  let { Map }=Immutable;

    let obj={
        a:1,
        b:2
    }

    //克隆出一个
    let map=Map(obj);

    //调用immutable中的map对象下的set方法来修改指定key的value
    let map1=map.set('a',10);
    let map2=map1.set('a',20);
    
    //两个对象修改互不影响
    console.log(map1.get('a'),map2.get('a'));  //10,20
```





### 判断一个变量是不是数组的有哪几种方法？

-  Object.prototype.toString.call(arg) === '[object Array]'    //目前最准确的方法  

- Array.isArray(arg)   //有兼容性问题

- arg  instanceof Array    //不能跨iframe使用

- arg.constructor===Array    //不能跨iframe使用，constructor可以改写， a.constructor=Object;  会导致判断不准确

  



### 0.1+0.2 为什么不等于0.3?

浮点数据误差问题，JS中整数与小数都只有一种类型：Number;

它的实现遵循IEEE 754标准，使用64位固定长度表示，就是标准的double双精度浮点数。

这样存储的存储结构优点：归一化处理整数和小数，节省存储空间。

64位比特可分为三个部分：

- 符号位S： 第1位正负数符号位（sign），0代表正数，1代表负数；
- 指数位E：中间的11位存储指数（exponent）,用来表示次方数；
- 尾数位M：最后52位是尾数（mantissa）,超出的部分自动进一舍零；

![image-20201104095505544](C:\Users\22064\AppData\Roaming\Typora\typora-user-images\image-20201104095505544.png)

![image-20201104095517270](C:\Users\22064\AppData\Roaming\Typora\typora-user-images\image-20201104095517270.png)

数学工式如上图：

4.5 转换过程
- 比较10进制的4.5转换成2进制就是 100.1
- 科学计数法表示注是（二进制）  1.001*2^2
- 舍去1后，M=001；
- E=1025







### 如何解决js 精度计算问题?

类库：

- Math.js





### 哪些操作会造成内存泄漏？

>  内存泄漏原因：不再用到的内存，没有及时释放，就叫做内存泄漏；

- 全局变量缓存数据  （无法被垃圾回收机制收集）
- 定时器没有清除  
- 闭包的循环引用 
- js错误引用dom元素 （dom虽然删除，但是引用还在内存中）



### js垃圾回收机制？

- 引用计数 （如果没有引用指向该对象，对象将被垃圾回收机制回收，缺点：在循环引用的情况下，存在局限性）
- 标记清除 （处理循环引用的问题，和引用计数本质相同，可达内存被标记，其余的被当作垃圾回收）







### null,undefined区别？分别会在什么情况下出现？

null:代表无的状态，此处不应该有值  ---(之前没定义这个变量，就是null)---作用对象原形的终点

undefined:代表未找到，缺少值---（之前定义了，却没有赋值就是undefined）---变量赋值，函数传参





### js语言的特点？

答：异步，单线程，事件驱动，解释性语言，弱类型；



### bind,call,apply的区别?

共用点：

- 改变函数执行时的上下文，再具体点就是改变函数运行时this的指向
- 三者第一个参数都是this要指向的对象，如果未传入，默认指向全局window
- 三者都可以传入参数

不同点：写法不一样

bind:     fn.bind(thisArg,队列or数组)()

call:   fn.call(thisArg, arg1, arg2,...)

apply:  fn.apply(thisArg, [argsArray])

可以做的几个事：

- 求数组中的最大和最小值；
- 利用call和apply做继承

const arr=[1,2,3]

Max.max.apply(null ,arr);







### class与普通构造函数有什么区别？

class本质使用prototype的原型链，只是一种语法糖；

在语法上更加贴合面向对象的语法，在实现继承上更新易读、易理解；







### canvas与svg区别？

1. 历史：svg有十多年历史，并不是html5专用标签；
2. 展示效果：svg导出后的效果是矢量图形，而canvas显示效果是位图（所以canvas可以引入图片）
3. 技术原理：svg（XML文档）是通过html绘制--可以使用DOM操作，canvas通过js绘制；
4. canvas支持颜色较svg多；
5. 使用案例：svg（百度地图），canvas（图表）





### js中的this指向问题？

this指向调用函数的那个对象；

this只有在调用的时候才能判断是哪个对象；





### 什么是作用域链？

作用域链就是【变量】和【函数】和可访问范围，

控制着变量和函数的可见性与生命周期，在js中变量的作用域有全局作用域和局部作用域 ；







### 什么是闭包，如何使用，闭包有什么问题？

定义：闭包是由函数创建的一个词法作用域，里面的变量被引用后（外部函数引用，前提得先return出来），

可以在这个词法环境之外使用；

函数嵌套函数，能够读取【其他函数】内部变量的【函数】；

闭包一定得return才能读取内部的值；

用途

1. 创建内部变量，使得这些变量不能随意被外部修改，但是又可以通过指定的函数接口修改
2. 减少全局变量
3. 让这些值始终保持在内存中

闭包的问题：

- 耗性能：闭包会使函数内的变量一直保持在内存当中，

解决：在退出函数之前，将不使用的函数变量全部删除；



//使用自执行函数写法

````
 //定义一个闭包
    let secretFn=(function (){
        //变量secrent只有secretFn内的getSecret, setSecret 才可以访问，外部无法访问
        let secrent='100';
        let getSecret=function (){
            return secrent;
        }
        let setSecret=function (newSecrent) {
            secrent=newSecrent;
        }
        return {
            getSecret,
            setSecret
        }
    })()


    console.log(secretFn.getSecret());
    secretFn.setSecret(200);
    console.log(secretFn.getSecret());
    console.log(secretFn.secrent);   //Type error 无法访问
````

//使用new写法

```
    //定义一个闭包
    let SecretFn=function (){
        //变量secrent只有secretFn内的getSecret, setSecret 才可以访问，外部无法访问
        let secrent='100';
        this.getSecret=function (){
            return secrent;
        }
        this.setSecret=function (newSecrent) {
            secrent=newSecrent;
        }
    }

    const secretOne=new SecretFn();
    console.log(secretOne.getSecret());
    secretOne.setSecret(200);
    console.log(secretOne.getSecret());
```







### 如何获取闭包函数内的变量？

1. 函数嵌套，闭包
2. 函数返回值，return



### js有哪几种数据类型？

 js一共七种类型；

基本类型（6）：String,Boolean,Number,Undefined,Null,Symbol（创建唯一的值）

引用类型（1）：Object（Function，Array, Date, RegExp）





### js如何判断数据类型？

1. typeof ：不能判断array,object,function；
2. instanceof ：可以使用*可以用来判断数组；  [1]  instanceof  Array  返回 true*
3. *toSring*   *Object.prototype.toString.call([])  返回true*
4. *constructor*   *[].constructor===Array 返回true*





### js有哪些内置对象？

object是js中所有对象的父对象；

数据封闭类对象：object,array,boolean,number,string

其他对象：function,arguments,math,date,RegExp,error







### js如何实现继承？ 

1. 通过原型链（prototype）来实现---
2. 通过构造函数继承-（把子函数中使用）
3. es6的class,配合extend来实现继承
4. 拷贝继承（创建一个extendFun的方法）





### apply,call区别？

apply,call作用都是改变this的指向，只是参数写入的不同

apply把参数放入一个数组当中 ；

call把参数依次写入；

```
let params={

  a:20

}

function fun(value1,value2){

​     console.log(this.a,value1,value2);

}

//apply,call功能一样，都是为了改变this的指向，参数的写入方式有不同

fun.apply(params,[11,12]);

fun.call(params,11,12);
```







### 前端如何实现路由？

前端实现路由有两种方式：

第一：history模式

主要借助html5的两个api,  window.history.pushState("/detail"),  window.history.replaceState("/detail")

第二：hash模式





### form表单可以跨域吗？

可以；

跨域的唯一标准，就是你不能通过请求的方式拿到别人内容；

form表单只是发请求，而不是获取数据；

ajax一直在等待别人的done or fail 这就涉及到拿别人数据；




### 前端路由的原理

**什么是路由？**简单的说，路由是根据不同的 url 地址展示不同的内容或页面

**使用场景？**前端路由更多用在单页应用上, 也就是SPA, 因为单页应用, 基本上都是前后端分离的, 后端自然也就不会给前端提供路由。

前端的路由和后端的路由在实现技术上不一样，但是原理都是一样的。在 HTML5 的 history API 出现之前，前端的路由都是通过 hash 来实现的，hash 能兼容低版本的浏览器。

**两种实现前端路由的方式**

**HTML5 History**两个新增的API：history.pushState 和 history.replaceState，两个 API 都会操作浏览器的历史记录，而不会引起页面的刷新。

**Hash**就是url 中看到 # ,我们需要一个根据监听哈希变化触发的事件( hashchange) 事件。我们用 window.location 处理哈希的改变时不会重新渲染页面，而是当作新页面加到历史记录中，这样我们跳转页面就可以在 hashchange 事件中注册 ajax 从而改变页面内容。

**优点**

从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。



## 性能优化：



### 如何进行性能优化？

减少网络请求（大小，数量）---网络会有延迟,大部分浏览器一性能获取6-8个请求：

- 资源合并/代码压缩：js,图片（webP,base64），css
- 降低CPU消耗
  - 减少DOM操作：缓存DOM查询，合拼DOM插入，事件节流
- 缓存：保持名字不变，如果要改变可以使用hash来改变
- 加速服务端资源返回的速度：
  - cdn(千牛，bootcdn)
- 懒加载（主要处理图片，默认图片代码真实图片）
- SSR后端渲染



### 图片懒加载的实现原理？

原理：img src中放入一张很小的图片，把真实的图片放在img的属性中，data-src, 通过监听页面滚动过程中，图片距离顶部的高度来设置页面是否显示；

> <img src="E:/web/myProject/FE-interview/default.jpg" data-src='https://img.ydl.com/images/1.jpg' />

一般可以使用

- jquery.lazyload.js    //到了可视区才加载
- lazysizes.js   //没有到可视区才显示功能，不依赖jquery

注意事项：

- 使用节流函数进行性能优化



### 如何在js中实现不可变对象?

- 深拷贝，性能非常差，不适合大规模使用
- immutable.js，自成一体的一套数据结构，性能良好，但是学习额外的api
- immer,利用proxy特性，无需学习额外的api，性能良好





### immutable实现原理？

> 原理：持久化数据结构，结构共享，只修改对象树中变化的节点和受它影响的父节点，其它节点共享；

immutable data就是一旦创建，就不能再被更改的数据。对immutable对象的任何修改或添加/删除操作都会返回一个新的immutable对象。

优点：

- 降低mutable 可变对象的复杂度
- 节省内存空间 （不用深拷贝对象，共享结构，没有被引用的对象会被垃圾回收）
- 数据回退，任意穿越（每次数据都是不一样的，只要把这些数据放在一个数组中储存起来，可任意回退）
- 拥抱函数式编程，关心数据的映射，命令式编程关心解决问题的步骤，函数式编程比命令式编程更适用于前端开发。因为只要输入一致，输出必然一致，这样开的组件更易于调试和组装。

缺点

- 容易与原生对象混



### 什么是重排什么是重绘？

DOM的结构属性改变就会触发重排或者重绘；

只改变DOM节点对象的css的颜色，不改变大小，其他排版属性就属于重绘；

visibility:hidden，隐藏dom节点只触发重绘，display:none隐藏dom节点触发重排和重绘；

改变padding会触发:重排+重绘



### 什么是深拷贝和浅拷贝，如何实现深拷贝？

js中数据类型分为2类，引用数据类型（array,object,function），和其他数据类型(string,number,boolean,null,undefind);

深拷贝指拷贝后的对象与原来的对象相互独立，修改任何一方，其他一方都不受影响；

深拷贝主要有

1. es6的 spread展开，可以深拷贝数组，对象；
2. for循环依次读取复制
3. 先通过JSON.stringify把对象转化成字符串，再通过 JSON.parse把字符串转换成对象  （如果对象中包含函数，正则表达式会丢失；对象有循环引用会报错；）
4. 外部库：jquery extend， 使用标准库 lodash的 cloneDeep

注意事项：

Object.assign 只能深拷贝第一层，深层的还是浅拷贝；





### 深拷贝方法：jquery的extend

```
$.extend([deep],target,object1,object2....)  //deep不写就是浅拷贝

let phone={

​    from:"Apple"   

}

let phone1={

​    product:"iphone",

​    price:5000

}

let phone2={

​    product:"ipad"

​    price:4000,

}

$.extend(true,obj1,obj2);  //进行深拷贝，把obj2合拼到obj1中

$.extend(obj1,obj2);  //不进行深拷贝
```



### 什么是事件节流和函数防抖的区别，和实际运用？

相同点：为了限制函数执行的次数 ，导致的性能浪费；

不同点：

 事件节流throttle：指定时间周期内，只能执行一次；

运用

- 滚动到底部触发事件
- 

 函数防抖 debounce：指定时间周期后，才能执行此函数， 如果这周期内重复触发，就会重新开始计算

- 输入框实时搜索
- window resize

