## node





### koa 洋葱模型？



**特点 ：**

- next函数把中间件分成了两部分，导致中间件执行了两次
- 打印执行的顺序 a1,b1,b2,a2,   理解成镜向翻转（洋葱进入/出去的对称感）



**洋葱模型执行流程：**

- 外层中间件进行前期处理（next前的逻辑）
- 调用next，将控制流交给下个中间件，并await其完成，直到后面没有中间件或者没有Next函数执行为止
- 完成后一层层回溯执行各个中间件的后期处理（next后的逻辑）



![img](https://upload-images.jianshu.io/upload_images/15804534-c24dcae3d47774bf.png?imageMogr2/auto-orient/strip|imageView2/2/w/900/format/webp)



![img](https://pic1.zhimg.com/80/v2-b9070a7555568d9310c6cd11b157da58_720w.jpg)





```javascript

const Koa=require('koa');

//应用程序
const app=new Koa();

//中间件1
app.use((ctx,next)=>{
    console.log(a1);
    next();
    console.log(a2);
})

//中间件2
app.use((ctx,next)=>{
    console.log(b1);
    next();
    console.log(b2);
})

app.listen(9000,'127.0.0.1',()=>{
 console.log('server is starting');
})

//最后执行结果： a1,b1,b2,a2
```



参考资料： [Koa洋葱模型 从理解到实现](https://zhuanlan.zhihu.com/p/279391637)





### express 、koa的区别？



express就是组合了很多功能的koa;  koa唯一多了的一个功能就是中间件可以像洋葱一样嵌套（express完全可以实现这样一个功能）；异步处理如：promise,generator,callback, async/await thunk 都是浮云，完全可以用任何一种处理异步的方式写任何框架



一、**相同点：**

都是web Framework 网站框架



**二、不同点：**

**express**

特点：

- 更贴合web Framework的概念，自带Router、路由规则；

- 事件处理方式使用的回调函数；

优点：历史更久，文档更完善，资料更多

缺点：不能忍的callback

 

**koa**

特点：

- 更多是一个中间件框架，提供一个架子，几乎所有功能都是需要第三方中间件完成（功能更简单，更轻量）
- 事件处理方式利用生成器函数（Generator Function）来作为响应器

优点：借助promise和generator的能力，丢掉了callback 

（koa2.x和1.x最大的区别就是使用ES7中的Async/Await的特性，代替了co的Generator Function语法）

缺点：Connect/Express中间件基本不能重用，基本要重写