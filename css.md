## CSS



[TOC]



### mouseover/mouseout 与 mouseenter/mouseleave区别与联系？

|              | mouseover/mouseout                                 | mouseenter/mouseleave |
| ------------ | -------------------------------------------------- | --------------------- |
| 浏览器兼容性 | 所有浏览器都支持                                   | 现代标注浏览器才支持  |
| 是否冒泡     | 是（多个元素监听事件推荐使用，事件代理，提高性能） | 否                    |



### 垂直居中一个元素有哪几种方法？

- 行高法： line-height    //只指对行间元素

- transfrom

- 内边距法

- css3的box方法

- 绝对定位法   position

- 模拟表格法

- flex (align-items:center)       //pc端兼容不好

  



### link 与 @import的区别？

相同点：都可以引入外部css



不同点：

|          | link         | @import                                  |
| -------- | ------------ | ---------------------------------------- |
| 引入文件 | html         | css                                      |
| 下载方式 | 最大限度并行 | （性能差）过多嵌套导致串行下载，出现FOUC |



```
//在html文件中使用
<link rel="stylesheet" href="style.css" />

//在css文件中使用
@import "style.css";
@import url("style.css");
<style type="text/css">
  .hd{
    color: orange;
  }
  @import "import.css";
</style>
```



参考资料：

[高性能网站设计：不要使用@import](https://www.qianduan.net/high-performance-web-site-do-not-use-import/)





### css hack原理及常用hack?

> 利用不同浏览器对CSS的支持和解析结果不一样 编写针对特定浏览器样式。

常见的hack：

- 属性hack
- 选择器hack
- IE条件注释



### html 脱离文档流的方法？

- float
- sposition
  - absolute
  - fixed





### 使用translate来改变位置而不是用定位为原因是什么？

transform,opacity不会触发浏览器，重排/重绘，只会触发复合，性能好。

transform 使用GPU图层，更高效，可以缩短平滑动画的时间





### sass,less的优点，如何使用?

sass,less是一门CSS预处理语言，扩展了CSS语言，增加了

- **变量**：可以引用变量
- **混合（mixins）**：继承
- **嵌套(nesting)**：不用写父级，类似与作用域
- **函数**：less内置多种函数用于转换颜色，处理字符串，算数运算等；
- **作用域(scope)**：变量先在本地本找，如果找不到，编译器将查找父范围，依此类推；
- **注释** （支持块注释和行注释//）
- **导入**(import,如果导入的是less文件，后缀可去掉)