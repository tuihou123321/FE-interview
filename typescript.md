# typescript



[TOC]

参考资料：

[Typescript面试题](https://www.jianshu.com/p/c8aaba6e8ce0)



### typescript优点？ 

1：快速简单，易于学习。

2：编译时提供错误检查， 在代码运行前就会进行错误提示。

3：支持所有的JS库。

4：支持ES6，提供了ES6所有优点和更高的生产力。

5：使用继承提供可重用性。

6：有助于代码结构。

7：通过定义模块来定义命名空间。



![img](https://upload-images.jianshu.io/upload_images/16021827-9f2935d0f3da0cf7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



### typescript缺点？

1：需要长时间的来编译代码。

2：在使用第三方库时，需要有三方库的定义文件，并不是所有三方库都提供了定义文件，提供的定义文件是否准确也值得商榷。





### typescirpt有哪些基础类型?

1：number

2：string

3：boolean

4：Symbol

5：Array

6：Tuple(元组)

7：enum(枚举)

8：object

9：never

表示那些永不存在的值类型。如总是抛出异常或者根本不会有返回值的函数的返回值类型。

10：void

与any相反表示没有任何类型。函数没有返回值时用void。

11：null和undefined

它们是所有类型的子类型。当你指定structNullChecks时，它们只能赋值给void或者它们自己本身。

12：any





### 泛型？

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





### 元组？





### 枚举？



### 类型断言？

