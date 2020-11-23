## 移动端



[TOC]



### 事件穿透的原理及解决方案？

原理 ：

- 300ms延迟问题，事件穿透了（可跨页面）

解决方案：

- 不要混用touch,click， touch300ms会触发click
- tap后延迟350ms再隐藏mask，消除掉touch之后的click （tap后延迟350毫秒再隐藏mask）
- 第三方插件fastclick





### 高清屏1px边框问题的解决方案？

- 伪类+transform（scale） 实现
- border-image， backgroud-image  实现
- viewport+rem

处理方法：

使用svg来设置border-image；

```
.example { border: 1px solid transparent; border-image: url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg' height='2px'%3E%3Crect fill='%2300b1ff' width='100%25' height='50%25'/%3E%3C/svg%3E") 2 2 stretch; }
```





### 浏览器有哪些兼容些问题，你是如何处理的？

1. html5的兼容性可以使用htmlshiv.js来处理（可以兼容到ie6-ie8）
2. css3 新特性，单独增加前缀（可以使用postcss的插件 Autoprefixer）;  IE条件注释
3. js兼容性：jquery





### 移动端兼容性问题？

- ios下软键盘弹出，fixed元素容易定位出错，影响fixed元素定位；

​        解决方案：可用iscroll插件解决这个问题；

- zepto点击穿透的bug：使用fastclick插件代替
- 移动端300s延迟处理方案？

1. 使用插件fastclick
2. 使用zepto的touch事件代替
3. 可以绑定ontouchstart事件



### 事件穿透的原理及解决方案？

原理 ：

- 300ms延迟问题，事件穿透了（可跨页面）

解决方案：

- 不要混用touch,click， touch300ms会触发click
- tap后延迟350ms再隐藏mask，消除掉touch之后的click （tap后延迟350毫秒再隐藏mask）
- 第三方插件fastclick





### H5与原生APP交互的原理？



**一、APP调用H5**

> 【不建议使用】app做为容器，尽量不要去调用H5的方法，这样会导致数据流混乱，APP  webview容器不能做到模块化

步骤

1. h5中暴露一些全局对象, 把变量，方法注入到window对象下

2. app中调用这些对象





**二、H5调用APP**

> 由于h5不能直接访问宿主APP，所以这种调用相对复杂一点



**1）APP向h5注入一个全局JS对象**

优点：简单

缺点：可能存在安全隐患,android系统



H5中直接调用：

```
window.appSdk.double(10);  //20
```



![img](https://segmentfault.com/img/bVbit5e?w=1006&h=353)



**2）H5发起一个自定义协议请求**

> scheme协议，app的一种页面跳转协议

步骤：

1. 【定协议】app自定义协议
   - ydl-user://[ceshi](haoshi://ceshi/home)/list   跳转到【原生】测评列表页
   - ydl-user://h5/test?params={"url":" [http://www.x.com/ceshi/1](http://www.x.com/ceshi/1)"}    //跳转到【H5】测评详情页，params 参数部分 使用UrlEncode编码 且不能有空格
2. 【定回调】h5定义好回调函数
   - window.bridge={getDouble:value=>{}, getTriple:value=>{}}
3. 【发请求】H5发起一个自定义协议请求，
   - location.href = 'sdk://double?value=10'
4. 【拦截并解析请求】app拦截这个请求，进行相应操作，获取返回值
5. 【调方法】由app调用h5中的回调函数，
   - window.bridge.getDouble(20)



![img](https://segmentfault.com/img/bVbit5f?w=977&h=338)





参考资料： [h5 与原生 app 交互的原理](https://segmentfault.com/a/1190000016759517)





### JSBridge 原理？

> 构建Native与非Native间消息通信的双向通道，使得H5与app可以互相调用各自的方法

案例：

- 微信sdk (微信内获取用户openID，调起微信支付功能，分享功能)



**实现原理：**



**一、注入API**

前端直接调用

```js
//android 4.2+系统劫持
window.nativeBridge.postMessage(message);  //android webview提供了 addJavascriptInterface 的方法

//ios 有新老两个容器
//UIWebview (过时了)，WKWebview(推荐)
window.webkit.messageHandlers.share.postMessage(xxx);  //WKWebViewConfiguration的调用方法
```



**二、拦截URL scheme**

```
ydl-user://[ceshi](haoshi://ceshi/home)/list 
```





参考资料:

 [JSBridge的原理](https://juejin.cn/post/6844903585268891662#heading-1)

[微信开放文档](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html#2)

[小白必看，JSBridge 初探](https://www.zoo.team/article/jsbridge)