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

