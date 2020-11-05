## js原生



[TOC]



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



### null,undefined区别？分别会在什么情况下出现？

null:代表无的状态，此处不应该有值  ---(之前没定义这个变量，就是null)---作用对象原形的终点

undefined:代表未找到，缺少值---（之前定义了，却没有赋值就是undefined）---变量赋值，函数传参



### 图片懒加载的实现原理？

原理：img src中放入一张很小的图片，把真实的图片放在img的属性中，data-src, 通过监听页面滚动过程中，图片距离顶部的高度来设置页面是否显示；

>  <img src='default.jpg' data-src='https://img.ydl.com/images/1.jpg' />

注意事项：

- 使用节流函数进行性能优化





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







### AMD和CMD的区别？

常用的模块化方法：



| **模块名**                                                   | 导出模块       |      | 引入模块                           | 加载方式 | 说明                                           |
| ------------------------------------------------------------ | -------------- | ---- | ---------------------------------- | -------- | ---------------------------------------------- |
| **commonjs**                                                 | module.exports |      | require                            | 同步     | nodejs中的标准                                 |
| AMD(requirejs)                                               | define         |      | require                            | 异步     | 依赖前置，提前执行（在模块定义的时候就要引入） |
| CMD(sea.js)                                                  | define         |      | require(["jquery","math"],,()=>{}) | 异步     | 依赖就近，延迟执行（用到的时候才引入）         |
| **ES6**                                                      | export         |      | import                             | 异步     |                                                |
| commonjs 与 ES6的区别：commonjs 输出的是一个值的拷贝，ES6输出的是一个值 的引用 |                |      |                                    |          |                                                |



/** AMD写法 **/

define(["a", "b", "c", "d", "e", "f"], function(a, b, c, d, e, f) {

​     // 等于在最前面声明并初始化了要用到的所有模块

​    a.doSomething();

​    if (false) {

​        // 即便没用到某个模块 b，但 b 还是提前执行了

​        b.doSomething()

​    }

});





/** CMD写法 **/

define(function(require, exports, module) {

​    var a = require('./a'); //在需要时申明

​    a.doSomething();

​    if (false) {

​        var b = require('./b');

​        b.doSomething();

​    }

});





### 什么是事件节流和函数防抖的区别，和实际运用？

相同点：为了限制函数执行的次数 ，导致的性能浪费；

不同点：

 事件节流throttle：指定时间周期内，只能执行一次；

运用

- 滚动到底部触发事件
- 

 函数防抖 debounce：指定时间周期后，才能执行此函数， 如果这周期内重复触发，就会重新开始计算

- 输入框实时搜索
-  window resize





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





### 什么是深拷贝和浅拷贝，如何实现深拷贝？

js中数据类型分为2类，引用数据类型（array,object,function），和其他数据类型(string,number,boolean,null,undefind);

深拷贝指拷贝后的对象与原来的对象相互独立，修改任何一方，其他一方都不受影响；

深拷贝主要有

1. es6的 spread展开，可以深拷贝数组，对象；
2. for循环依次读取复制
3. 先通过JSON.stringify把对象转化成字符串，再通过 JSON.parse把字符串转换成对象  （如果对象中包含函数，会丢失）
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







### es6的箭头函数和普通函数区别？

不能绑定自己的this;'





### 什么是重排什么是重绘？

DOM的结构属性改变就会触发重排或者重绘；

只改变DOM节点对象的css的颜色，不改变大小，其他排版属性就属于重绘；

visibility:hidden，隐藏dom节点只触发重绘，display:none隐藏dom节点触发重排和重绘；

改变padding会触发:重排+重绘





### 什么是闭包，如何使用，闭包有什么问题？

定义：闭包是由函数创建的一个词法作用域，里面的变量被引用后（外部函数引用，前提得先return出来），

可以在这个词法环境之外使用；

函数嵌套函数，能够读取【其他函数】内部变量的【函数】；

闭包一定得return才能读取内部的值；

\##用途

1. 创建内部变量，使得这些变量不能随意被外部修改，但是又可以通过指定的函数接口修改；
2. 减少全局变量
3. 让这些这是的值始终保持在内存中；

闭包的问题：

耗性能：闭包会使函数内的变量一直保持在内存当中，

-----解决：在退出函数之前，将不使用的函数变量全部删除；





### 如何获取闭包函数内的变量？

1. 函数嵌套，闭包
2. 函数返回值，return



### js有哪几种数据类型？

 js一共七种类型；

基本类型（6）：String,Boolean,Number,Undefined,Null,Symbol（创建唯一的值）

引用类型（1）：Object（Function，,Array,Date,RegExp）





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





### get和post有什么区别？

区别：

第一：get参数是在url上面，post 是放在request body里面；

- get参数长度有限制，参数没有暴露在url上面；
- get请求的数据可以缓存

相同点：

底层都是TCP/IP

本质区别：

Get产生一个TCP包，post产生两个；



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



