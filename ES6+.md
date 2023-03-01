## ES6+



[TOC]



### 介绍下Generator?

> Generator函数是ES6提供的一种 异步编程解决方案,Generator函数是分段执行的，**yield**表达式是**暂停执行**的标记，而**next方法**可以**恢复执行**

Generator函数是一个状态机，封装了多个内部状态。执行Generator 函数会返回一个遍历器对象，可以依次遍历Generator函数内部的每个状态。



Generator与普通函数的区别：

- 【不执行】调用Generator函数后，该函数并不执行
- 【返回指针】返回的是一个指向内部状态的指针对象，而不是函数运行结果（遍历器对象 Iterator Object）
- 【继续运行】必须调用遍历器对象的next方法，使得指针移向下一个状态，每次调用next方法，就继续执行，直到遇到下一个yield表达 式(或return语句）为止。




Generator API：

- next():  返回一个由yield 表达式生成的值  
- return():  返回给定的值并结束生成器 
- throw():  向生成器抛出一个错误



```js
function* helloWorldGenerator(){
	yield 'hello';   //遇到yield 暂停执行
	yield 'world';
	return 'ending';
}

let hw = helloWorldGenerator();  //调用不执行函数，只是返回一个遍历器对象
hw.next();  //{value:'hello',done:false}  调用next方法继续执行，直到遇到yield或return表达式
hw.next(); //{value:'world',done:false}
hw.next(); //{value:'ending',done:true}
hw.next(); //{value:undefind,donw:true}  

```



参考资料：

[Generator 函数的语法](https://es6.ruanyifeng.com/#docs/generator)





### ES6 Set 和 Array 的区别？

区别：

- 【重复性】
  - set: value不重复  //通过此特性，可用来去重
  - array: value 可重复




### ES6 Map和Objects的区别？

> Map对象保存键值对，任务值（对象或原始值）都可以做为一个键或一个值



区别（Maps的优点）：

- **【key值类型】**
  - Map：key可以是对象
  - Objects: key只能是一个string 或是 symbol
- **【键的顺序】**
  - Map: 有序
  - Objects: 无序
- **【size】**
  - -Map: 增加size属性，直接获取
  - Objects: 只能依靠手动计算
- **【键名冲突】**
  - Map: 默认不包含任何键，只包含显式插入的键
  - Object: object都有自己的原型，原型链上的键名有可能和你自己对象上的设置的键名产生冲突 （ES5开始可用Object.create(null)来创建一个没原原型的对象，但这种用法不常见）
- **【性能】**
  - Map: 在频繁增删键值对的场景下表现更好
  - Object: 在频繁添加/删除键值对的场景下未作出优化

![img](https://www.runoob.com/wp-content/uploads/2018/12/1_HmXTMmVps1oJ7MU-odCpUA.jpeg)





### 箭头函数的this 指向哪里?

- 默认绑定外层this
- 不能使用call方法修改里面的this
  - -原因：函数的this可以用call方法来手动指定，而为了减少this的复杂性，箭头函数无法用call方法来指定this



参考资料：

[JS中的箭头函数与this](https://juejin.cn/post/6844903573428371464#heading-8)





### 箭头函数与普通函数区别？

箭头函数的优点 

- 语法更简洁
  - 不用写function 更简洁  // let sum=()=>{console.log(11);}
  - 只有一个参数的情况下，不用括号，直接写参数  // let sum = num1= > num1*2
  - 返回体只有一句的情况下，可以省去大括号     // let sum=(num1,num2)=> num1+num2
  - 如果箭头函数的函数体只有一条语句，并且 需要返回值，可以给这条语句前加一个 void  // let fn=()=> void donotReturn()
- 箭头函数不会创建自己的this （它只会从自己的作用域上一层继承this）
  - 定义时所处外层如执行环境的上下文，并继承这个 this，之后永远不会修改



```js
var id = 'Global';

function fun1() {
    // setTimeout中使用普通函数
    setTimeout(function(){
        console.log(this.id);
    }, 2000);
}

function fun2() {
    // setTimeout中使用箭头函数
    setTimeout(() => {
        console.log(this.id);
    }, 2000)
}

fun1.call({id: 'Obj'});     // 'Global'

fun2.call({id: 'Obj'});     // 'Obj'

```





参考资料：

[ES6 - 箭头函数、箭头函数与普通函数的区别](https://juejin.cn/post/6844903805960585224)





### 什么是严格模式(use strict)？

特点：

- 【严谨】对语法要求更规范，消除js语法的一些不合理、不严谨之处，减少一些怪异行为
- 【安全】消除代码运行的一些不安全之处
- 【高效】提高编译器效率，增加运行速度
- 【扩展性】为未来新版本js做好铺垫



调用方式：在代码最前面添加一行代码（"use strice";）

- 针对整个脚本文件
- 针对单个函数



改变：

- 全局变量显式声明：不能省略 var/const/let 关键词
- 静态绑定：让属性和方法在编译阶段就确定指向哪个对象
  - 禁止使用with语句
  - 创设eval作用域
- 增强字全措施：
  - 禁止this 指向全局对象
  - 禁止在函数内部遍历调用栈
- 禁止删除变量（除非configurable 属性为true）
- 显式报错：
  - 只读属性赋值
  - 对禁止扩展的对象添加新属性: 
  - 删除一个不可删除的属性： delete  object.prototype
- 重名错误
- 对象不能重名的属性（之前是覆盖）
- 函数不能有重名的参数
- 禁止八进制表示法
- arguments对象的限制：arguments是函数的参数，对它的使用做了限制
  - 不允许对arguments赋值
  - arguments 不再追踪参数的变化
  - 禁止使用arguments.callee:  无法在匿名函数内部调用自身
- 函数必须声明在顶层：不允许在非函数的代码块内声明函数（比如if,for），只允许在全局作用域或函数作用域的顶层声明函数
- 保留字：新增了一些保留字，向将来js新版本过渡
  - let ,private,public,static ,package,yield,interface,protected,implements



js语法不规范的几个地方：

- 【变量定义】可以不使用 var/const/let 关键词，直接定义



参考资料：

[Javascript 严格模式详解](https://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)





### 介绍下js的装饰器（decorators）?

> Decorator 就是一种动态地往一个类中添加新的行为的设计模式，它可以在类时，扩展一个类的功能，并且 去修改类本身的属性和方法，使其可以在不同类之间更灵活的共用一些属性和方法；
>
> 修饰模式（Decortaor），是面向对象编程领域中，一种动态地往一个类中添加新的行为的设计模式。修饰模式相比生成子类更加灵活，这样可以给某个对象而不是整个类添加一些功能



用法：

1) 装饰类

```js
@FooDecorator
class Foo {

}

function FooDecorator(target){
    // target 就是这个 class 的原型对象
}
```



2) 装饰属性

```js
//给MyClass 所有实例的  getType 属性设置为仅为可读不可更改的属性 readonly
class MyClass {
	constructor(){
		this.type="myClass"
	}
	@readonly
	getType(){
		return this.type
	}
}

function readonly(target, key, discriptor){
	discriptor.writable = false
	return discriptor
}
```



3）多个装饰器

```js
class MyClass {
	constructor(){
		this.type="myClass"
	}
	@readonly
	@logHello
	getType(){
		return this.type
	}
}

//装饰器让类的getType方法不可更改
function readonly(target, key, discriptor){
	discriptor.writable = false
	return discriptor
}

//让每次调用类中的getType方法会在控制台输出 hello
function logHello(target, key, discriptor){
	const oldFn = target[key]
    target[key] = function(...rest){
        console.log('Hello')
        return oldFn.call(this,...rest)
    }
    return target
}
```



参考资料：

[JS 装饰器，一篇就够](https://segmentfault.com/a/1190000014495089)
[学习 ES7 语法 decorator 装饰器]()







### 创建对象的三种方式？

- 字面量 
- 构造函数
- Object.create

参考资料：

[Object.create()、new Object()和{}的区别]()







### 箭头函数？





### ES6常用 API有哪些？

let,const： 变量，常量
变量的解构赋值：
promise：处理异步
字符串扩展： for..of字符串遍历接口，repeat将一个新字符串重量N次，模板字符串$(baseUrl)
数值扩展：math.trunc()去掉小数部分，sign判断一个数是正/负/零，指数运算符**，
函数扩展：箭头函数（不绑定自己的this）
数组扩展：填充数组(fill),复制数据const a2=[...a1]
对象扩展：
正则的扩展：
增加async函数： 对异步的处理



### AMD和CMD的区别，ES6模块与CommonJs 模块有什么区别？



常用的模块化方法：

| **模块名**     | 导出模块       | 引入模块                           | 加载方式 | 说明                                                   |
| -------------- | -------------- | ---------------------------------- | -------- | ------------------------------------------------------ |
| **ES6**        | export         | import                             | 静态加载 | 输出的引用，静态引用，只读属性                         |
| **commonjs**   | module.exports | require                            | 动态引用 | 输出值的浅拷贝对象，动态加载，可读可写。nodejs中的标准 |
| AMD(requirejs) | define         | require                            | 异步     | 依赖前置，提前执行（在模块定义的时候就要引入）         |
| CMD(sea.js)    | define         | require(["jquery","math"],,()=>{}) | 异步     | 依赖就近，延迟执行（用到的时候才引入）                 |



CommonJs（require） 与 ES6(import)的区别：

- require: 输出的是一个值的浅拷贝对象，import输出的是一个值 的引用（即es6 module 只存只读，不能改变其值，具体点就是指针指向不能变，类似const）
- require是动态引入，import是静态加载了；动态引入的方式，引入的对象可以是一个变量，或者能通过计算出来的地址
- require是同步加载模块，import命令是异步加载，require有一个独立的模块依赖的解析阶段



 参考： https://es6.ruanyifeng.com/#docs/module-loader#ES6-%E6%A8%A1%E5%9D%97%E4%B8%8E-CommonJS-%E6%A8%A1%E5%9D%97%E7%9A%84%E5%B7%AE%E5%BC%82



/** AMD写法 **/

```
define(["a", "b", "c", "d", "e", "f"], function(a, b, c, d, e, f) {

​     // 等于在最前面声明并初始化了要用到的所有模块

​    a.doSomething();

​    if (false) {

​        // 即便没用到某个模块 b，但 b 还是提前执行了

​        b.doSomething()

​    }

});


```



/** CMD写法 **/

```
define(function(require, exports, module) {

​    var a = require('./a'); //在需要时申明

​    a.doSomething();

​    if (false) {

​        var b = require('./b');

​        b.doSomething();

​    }

});
```





### js中let ,var 区别，let有什么优点？

>  相同点：都是定义这是的，都可以修改

let 优点：

-  **不能重复声明**：var可以重复声明
- **不存在变量提升**：let 不存在变量提升，未定义的变量会报 ： 未捕获的引用错误
- **暂时性死区**：  只要块级作用域内存在let命令，它所声明的变量变绑定这个区域，不再受外部影响
- **拥有块级作用域**：ES5中只有全局和函数作用域，没有块级作用域，let会产生块级作用域。
- - var没有块级作用域的一些问题
  - - 1. 内层变量会覆盖外层变量； 
    - 2. 用来计数的循环变量泄露为全局变量；





### es6/7有什么新特性？

箭头函数（不绑定自己的this），解构赋值，spread展开，块级作用域(let,const)，promise异步解决方案，set（没有重复的值）/map数据结构，proxy拦截器

重要属性：class类(类继承extends,constructor,super)

es7: async（async,await）异步操作,返回promise对象，可以接着用.then方法来连接；





### es6临时死区？

在 ES6 中，let 和const 跟 var、class和function一样也会被提升，只是在进入作用域和被声明之间有一段时间不能访问它们，这段时间是**临时死区(TDZ)**。

```js
//console.log(aLet)  // would throw ReferenceError

let aLet;

console.log(aLet); // undefined

aLet = 10;

console.log(aLet); // 10

```





### ES6中的map和原生对象有什么区别？

原生对象的key只能是string类型的，map的key可以是任意值（object,array,function,undefind...）

map是一种更完善的hash结构实现；

map的适用场景，把不同的事件关联起来；、



### 介绍下ES6中的Symbol，它有哪些运用场景？

symbol是ES6中新增的一种基本数据类型，通过typeof symbol()可以发现，输出的是symbol。

symbol最大的特点返回值的唯一性，即使传入相同的参数（参数主要有来在控制来做区分，备注用的）。

所以基于这个特性，Symbol 可以用在以下几个场景。

- 作为对象的唯一值
- switch作为唯一的枚举



Symbol需要注意的点

- Symbol()函数不能使用new命令，否则会报错。 //这个和其他基本类型都不一样，比始：String, Number, Boolean等
- Symbol作为属性名时，不能通过以前的api直接获取，比如：Object.keys(), Object.getOwnPropertyNames()。甚至是JSON.stringify()。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象所有的Symbol属性名。



### 介绍下ES6中的Proxy对象，它有哪些使用场景？

Proxy的字面意思是代理器，任何对对象（对象，数组，函数等都可以）的操作（包括：访问，写入等操作）都会进入这一层的拦截。可以在这一层添加一些自定义的操作。

比始拦截后的一些操作，方法复写与增强

- 增加日志记录
- 提供友好提示和阻止特定操作  // 读取不存在的属性时，自定义报错信息。不能删除特定的属性等。
- 读取负索引的值
- 设置真正的私有属性，私有的定义：不能访问，不能修改，不能遍历。   // 一般约定以 _ 开头key为私有属性



参考资料

- https://weread.qq.com/web/reader/2ba32920720a57e92ba5389ka4a32da02aba4a042cf4e81





## class类



### class类的基本介绍？

> 生成对象的模板

主要组成：

- constructor：构造方法
  - 默认属性，也可手动添加
  - 直接指向“类”的本身，和es5的行为一致  //Person.prototype.constructor === Person
- 实例方法
- 静态方法
- 私有方法、私有属性
- new.target属性



特点：

- 类内部定义的方法，都是不可枚举的（non-enumeerable），和构造函数不一样



```js
//es6的类，可看作是构造函数的另一种写法
class Person{}
typeof Person  //"function"
Person===Person.prototype.constructor //true
```



参考资料：

[Class 的基本语法](https://es6.ruanyifeng.com/?search=class&x=0&y=0#docs/class#new-target-%E5%B1%9E%E6%80%A7)





### class与普通构造函数有什么区别？

class本质使用prototype的原型链，只是一种语法糖；

在语法上更加贴合面向对象的语法，在实现继承上更新易读、易理解；

区别：

- 【变量提升】
  - 类：没有
  - 构造函数：有
- 【调用】
  - 类：必须使用new调用，否则报错
  - 构造函数：可直接调用
- 【严格模式】
  - 类：类和模块的内部，默认是严格模式，无需使用 use strict 关键词
  - 构造函数：无强制要求



```js
//类不存在变量提升
new Person{}; //ReferenceError
class Person{}
```





### 在构造函数中调用super(props)的目的是什么？

> es6语法 中，super指代父类的构造函数; 
>
> react里面就是指代 React.Component 的构造函数



在调用super() 之前，无法在构造函数中使用this；在es2015中，子类必须在 constructor中调用super(),传递props给super() 的原因是便于能在constructor访问this.props;



参考资料：

[React构造函数中为什么要写 super(props)](https://blog.csdn.net/huangpb123/article/details/85009024)







## Async/Await





### 对async await的理解，内部原理？

本质：async是Generator的语法糖；

async改变有如下几个方法

- 更好的语义化：async相当于*,await 相当于yield;
- 返回值是promise，generator返回的是Iterator对象，更方便的使用then来操作
- 内置执行器：async自带执行器，Generator需要依靠执行器（每次都是执行g.next()方法）；
- 更广的适用性：async函数的await后台可以是promise对象，也可以是原始类型的值；



### async/await相比 promise的优势？

- **同步写法优雅**：使摆脱了then的链式调用带来的阅读负担
- **获取返回值方便**：promise传递中间传非常麻烦，而async/await几乎是同步的写法，更优雅
- **错误处理友好**：async/await可使用成熟的 try/catch，promise的错误捕获非常冗余
- **调试友好**：promise中的then使用调试器的步进(step-over)功能，调试器并不会进入后续的then代码块，因为调试器只能跟踪同步代码的【每一步】









## promise





### 介绍下promise?

Promise是es6引入的一个新的对象，用来解决js中异步回调地狱的写法，并不是什么突破性的api，只是封装了异步回调函数； 

使得异步写的更加优雅， 可读性更高，而且支持链式操作； 





### promise有几个状态？

Promise一共有三种状态

1.初始化，状态：pending

2.当调用resolve(成功)，状态：pengding=>fulfilled

3.当调用reject(失败)，状态：pending=>rejected



![img](https://user-gold-cdn.xitu.io/2019/12/2/16ec4a7d3d2b1086?imageslim)







### promise内部实现原理？

 **Promise/A+规范**

1. pending:表示初始状态，可以转移到 fullfilled 或者 rejected 状态
2. fulfilled:表示操作成功，不可转移状态
3. rejected:表示操作失败，不可转移状态
4. 必须有一个 then 异步执行方法，then 接受两个参数且必须返回一个promise

**实现思路**

我们定义Promise1对象，在对象内部创建status、reason、fullfilledCallbacks、rejectedCallbacks这四个属性，这些属性分别表示的意义为:

1. reason:保存当前promise实例状态	
2. value:保存fullfilled之后的值
3. reason:保存rejected后的原因
4. fullfilledCallbacks: fullfilled回调队列
5. rejectedCallbacks:rejected回调队列

我们定义resolve和reject方法用于处理传进来的executor函数。在当前实例调用then方法时候去返回新的Promise1实例，并判断当前实例状态是否pendding，如果pendding，将传入的成功和失败回调函数加入队列，在外部调用resolve或者reject时候，再次判断当前状态是否pendding，如果是，则修改当前实例状态为fullfilled或者rejected，并批量执行回调队列中的回调函数。



**promise 状态流转过程**

![img](https://camo.githubusercontent.com/5bdcdb90caf703499d6b3d2db0504f6858a0b54928dcc50222247948500808cf/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323031392f706e672f3134393834362f313535333834313231323335352d64633438316261642d323530652d346231362d393230372d6132323132333063366662612e706e6723616c69676e3d6c65667426646973706c61793d696e6c696e65266865696768743d323737266f726967696e4865696768743d323937266f726967696e57696474683d3830312673697a653d30267374617475733d646f6e652677696474683d373436)





参考资料：

[今日头条: 介绍下Promise，内部实现(一面)](https://github.com/frontend9/fe9-interview/issues/14) 

[图解 Promise 实现原理（一）—— 基础实现](https://zhuanlan.zhihu.com/p/58428287)





### promise和async处理失败有什么区别?

```js
## promise

function asyncTast(url){

   return new Promise(resolve,reject) => {

	}
}


## async

try{

}catch(e){

console.log(e);

}
```





### 如何设计promise.all？

总结 `promise.all` 的特点

1、接收一个 `Promise` 实例的数组或具有 `Iterator` 接口的对象，

2、如果元素不是 `Promise` 对象，则使用 `Promise.resolve` 转成 `Promise` 对象

3、如果全部成功，状态变为 `resolved`，返回值将组成一个数组传给回调

4、只要有一个失败，状态就变为 `rejected`，返回值将直接传递给回调
`all()` 的返回值也是新的 `Promise` 对象



实现思路：

- 

```js
function promiseAll(promises) {
  return new Promise(function(resolve, reject) {
    if (!isArray(promises)) {
      return reject(new TypeError('arguments must be an array'));
    }
    var resolvedCounter = 0;
    var promiseNum = promises.length;
    var resolvedValues = new Array(promiseNum);
    for (var i = 0; i < promiseNum; i++) {
      (function(i) {
        Promise.resolve(promises[i]).then(function(value) {
          resolvedCounter++
          resolvedValues[i] = value
          if (resolvedCounter == promiseNum) {
            return resolve(resolvedValues)
          }
        }, function(reason) {
          return reject(reason)
        })
      })(i)
    }
  })
}
```







### Promise 构造函数是同步执行还是异步执行，那么 then 方法呢？



promise构造函数是同步执行的，then方法是异步执行的

```js
const promise = new Promise((resolve, reject) => {
  console.log(1)
  resolve()
  console.log(2)
})

promise.then(() => {
  console.log(3)
})

console.log(4)
```



参考资料：

[Promise 构造函数是同步执行还是异步执行，那么 then 方法呢？](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/19)





## API



### Map api？

> Ｍap 保存键值对，并且能够记住键的原始插入顺序。**任何值**都可以作为一个**键**或一个**值**；

- **Map的属性**
  - size
- **Map的方法**
- - 增：set({key:value})  //增加对象
  - 删：
    - -clear() //清空对象
    - delete(key) //删除对象
  - 改：
  - 查：
    - -get(key)    //获取对象
    - has(key)  //返回一个布尔值，判断Map实例是否包含键对应的值
  - 遍历：
    - forEach(callbackFn[,thisArg]) //删除对象
  - 其他：
    - keys()  //返回类型：Iterator对象，获取keys列表
    - values() //返回类型：Iterator对象 ,返回一个新的Iteratror对象，它按插入顺序包含了Map对象中每个元素的值





### Set api?

> set对象允许你存储**任何类型**的**唯一值**，无论是原始值或者是对象引用

**属性:**

- 长度：size



**实例方法：（Set.prototype）**

- 创建： new Set();   new Set([1,2]);
- 增(只支持添加在尾部)：
  - 尾：只接受一个参数，但可链式调用， mySet.add(1).add(2)
- 删：
  - 指定item : delete(value)    //Array(value=item)，Object (value=object)
  - 清空： clear()
- 查:
  - 判断值是否存在： has(value)   //Array(value=item)，Object (value=object)
- 遍历：
  - for循环：for(const item of  set1){console.log(item);}
  - forEach: 无返回值 

