## ES6+



[TOC]



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
- 【】



```js
//类不存在变量提升
new Person{}; //ReferenceError
class Person{}
```





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



### 介绍下promise?

Promise是es6引入的一个新的对象，用来解决js中异步回调地狱的写法，并不是什么突破性的api，只是封装了异步回调函数； 

使得异步写的更加优雅， 可读性更高，而且支持链式操作； 





### promise内部实现原理？





```js
//promise 应用

<script>
    function axiosAsync(){
        return new Promise((resolve,reject)=>{
            axios.get('https://testapi.ydl.com/api/test-category/all',{
                params:{},
                timeout:2000
            }).then(res=>{
                const {data}=res;
                resolve(data);
            }).catch(err=>{
                reject(err);
            })
        })
    }

    async function getCategoryData(){
        try {
            const result=await axiosAsync()
            console.log(result,1);
        }catch (e) {
            alert(e);
        }
    }


    getCategoryData()
    
    // axiosAsync().then(res=>{
    //   console.log(res,44);
    // });
    
</script>
```





参考资料：

[图解 Promise 实现原理（一）—— 基础实现](https://zhuanlan.zhihu.com/p/58428287)




### es6临时死区？

在 ES6 中，let 和const 跟 var、class和function一样也会被提升，只是在进入作用域和被声明之间有一段时间不能访问它们，这段时间是**临时死区(TDZ)**。

```js
//console.log(aLet)  // would throw ReferenceError

let aLet;

console.log(aLet); // undefined

aLet = 10;

console.log(aLet); // 10

```






### es6中的map和原生对象有什么区别？

原生对象的key只能是string类型的，map的key可以是任意值（object,array,function,undefind...）

map是一种更完善的hash结构实现；

map的适用场景，把不同的事件关联起来；、




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

