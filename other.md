### 其他





[TOC]

### 项目中如何处理安全问题？

- CSRF（跨站请求伪造）攻击，token认证
- 对用户上传的内容进行转义，防止XSS 跨站脚本攻击
- 开启https，避免数据劫持/监听
- cookis设置path来限制防问的域名范围，开启secure



比如对抗 XSS/CSRF 的策略，账号风控策略、图片安全策略、恶意内容对抗策略、等等

跨站请求伪造：通过get请求进行数据操作，



### pm2有什么用？

pm2是一个进程管理工具；

带有负载均衡功能的node应用进程管理器；

pm2管理node服务的优点：

- 自带负载均衡
- 程序崩溃自动重启
- 支持性能监测
- 程序崩溃自动重启



### 文件如何做断点续传？

主要分为三步：

1. 保存文件唯一性标识（通过let md5code=md5(file)）
2. 切割文件；html5的新特性,使用slice方法分割；
3. 分段上传，每次上传一段，根据唯一性标识判断上传进度；

主要思路是把一个大的文件拆分成几个小的文件，分批上传；

如果中断了，从本地存储里面判断已经上传到第几个了，

如果所有文件都上传完了，后台再把文件合并存入数据库中；



### 什么是观察者模式？

通常也被称为 发布-订阅者模式 或者 消息机制，它定义了对象间的一种一对多的今依赖关系；

只要当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新，解决一个对象状态改变给其他对象通知的问题；

使用案例：

- 对DOM的事件绑定就是一个非常典型的 发布-订阅者模式
- vue的双向数据绑定（）

$('body').on('click',function(){

 alert(1);

})





### 通过什么做到并发请求？

event loop 事件轮询，setTimeout, 异步请求（ajax,资源加载）





### 什么service worker？

sevice worker 本质是充当web应用程序和浏览器之间的代理，也可在网络可用的情况下作为浏览器和网络间的代理；

用户在离线的情况下，也能通过缓存访问网页

拦截并修改访问的资源和请求，细粒度的缓存资源 ；

主要有以下特性：

1. 只支持https服务，在firefox隐藏模式下 service worker不可用；
2. 消息推送；
3. 后台同步，启动一个serive worker 即使没有用户访问特定站点，也可以更缓存 ；
4. 性能增强，预读取资源





### cookies和session有什么区别？

cookies和session都是做本地存储的；

区别如下：

- cookies存放在客户端，session存放在服务端；
- session默认存在服务器的一个文件中，而不是内存中；
- session一般需要一个session_id,来查找相关数据，session_id一般通地客户端中的cookies和url中取值；



### 你对AST了解多少？

AST是抽象语法树，用来描述源代理的树形结构，类似html中dom;

展现形式看起来像一个对象； 

使用场景：

1. webpack的babel代码转换：es6/7 => es5；

浏览器会把js代码转换成抽象语法树，再转换成字节码或者直接生成机器码；



### webview和原生是如何通讯的？

js调起原生的事件方法，是通过jsBridrage