## ES6+



[toc]

 

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





### 介绍下promise?

Promise是es6引入的一个新的对象，用来解决js中异步回调地狱的写法，并不是什么突破性的api，只是封闭了异步回调函数； 

使得异步写的更加优雅， 可读性更高，而且支持链式操作； 




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
