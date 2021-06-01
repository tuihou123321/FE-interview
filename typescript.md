# typescript



[TOC]



### typescript优点？ 

- 强类型
  - 【可选的】不要求100%覆盖
    - 更方便迁移老代码
    - 降低入门门槛
  - 【js的超集】支持js(包含es6,及es6+)所有语法
    - 方便快速入手
  - 【静态检查】编译时就报错，而不像js在运行时报错
    - 静态检查就知道错误，效率更高
    - 避免低级错误
  - 【支持模块】方便类型的导入导出，复用组合
  - 【更好维护】类型就是最好的注释，减少查文档的时间
- 支持面向对象编程，类型重用率更高，代码可读性更强，相比react的propTypes功能更强大
  - 接口 interface
  - 继承： extend
  - 类： class
  - 泛型 <T>

![img](https://upload-images.jianshu.io/upload_images/16021827-9f2935d0f3da0cf7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

参考资料：https://www.zhihu.com/question/273619114/answer/369126175



### typescript缺点？

- 【编译耗时】需要长时间的来编译代码：deno内部将删除ts代码
- 【第三方定义文件】在使用第三方库时，需要有三方库的定义文件，并不是所有三方库都提供了定义文件，提供的定义文件是否准确也值得商榷





### typescirpt有哪些基础类型?

**js的类型**

- number
- string
- boolean
- object
- array
- function
- null
- undefined
- symbol



**ts新增类型**

- any: 任意值类型

- void：函数没有返回值时用void

- Tuple(元组)：规定数组成员的数量，各位置的类型

- enum(枚举)：定义数值集合

- never：（实际上不常用) never是其他类型的子类型，代表不会把出现的值

  





### 介绍下ts中的泛型？

泛型代表的是泛指某一类型，更像是一个类型变量。由尖括号包裹<T>。

主要作用是创建逻辑可复用的组件。

泛型可以作用在函数、类、接口上。

```js
函数：

function greet<T>(name: T) {}

类：

class createObj<T> {

​    name: T

}

接口：

interface IF<T> {

​    name: T

}

泛型还可以被约束，这样就是任意类型了。

interface TIF {

​    length: number

}

function test<T extends TIF>(params: T) {

​    console.log("=========>>>", params.length);

}

泛型约束之类型参数

function getPropoty<T, K extends keyof T>(obj: T, key: K) {

​    return obj[key];

}

```







## 强烈推荐

像更系统的学习typescript，可查看  [typeScript-demo](https://github.com/tuihou123321/typeScript-demo)





参考资料：

https://www.runoob.com/typescript/ts-type.html

[Typescript面试题](https://www.jianshu.com/p/c8aaba6e8ce0)