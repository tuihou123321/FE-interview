## html



[TOC]



### indexedDB介绍？

浏览器本地数据存储方案，容量大





### script标签中defer和async的区别？

**defer**：异步加载，延迟执行（文档解析完成后）

async：异步加载，立即执行



![img](https://xiaomuzhu-image.oss-cn-beijing.aliyuncs.com/c84fdc0e47268832fa8914ab4d125002.png)





### src与href的区别？

> 相同点：都可以引用外部资源

不同点：

**src**

- 【嵌入页面】指向的内容会嵌入到文档中，当前标签所在的位置
- 【阻塞页面】当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，所以一般js脚本建议放在页尾

**href**

- 【建链接】当前元素和文档之间的连接关系
- 【不阻塞】并行下载



```
//src
<img src='/img.jpg'/>
<script src='/jquery.js'></script>
<iframe src='https://www.baidu.com'></iframe>


//href
<link rel='icon' herf="/logo.png">
<link rel ="stylesheet" href="/public.css">
<a href="/"></a>

```





### doctype的作用是什么？

> DOCTYPE是html5标准网页声明，且必须声明在html文档第一行，来告知浏览器的解析器用什么文档标准解析这个文档，不同的渲染模式会影响到浏览器对于CSS代码甚至 js脚本的解析



文档解析类型：

- backCompat：怪异模式，浏览器使用自己的怪异模式渲染（未声明doctype默认就是这个）
- CSS1Compat：标准模式，浏览器使用W3C 标准解析渲染页面

```html
//html5的 申明方式
<!DOCTYPE html>   
```





### 什么是 data-属性？

> html的数据属性，用于将数据储存于html元素中作为额外信息，可以通过js访问并操作它，来达到操作数据的目的

```html
<ul>
	<li data-id='100'>1</li>
</ul>
```

> 前端框架出现后，此方法已不流行



### html语义化的理解?

优点：

- 对开发者友好：增强代码可读性
- 对机器友好
  - 搜索引擎：适合搜索引擎的爬虫抓取有效信息
  - 读屏软件：根据文章可以自动生成目录 







### html5有哪些新特性？

- 语义化的标签：(header,footer,nav,section,artice, aside)
- 连通性：(websockets)
- 离线&存储：(localStorage临时,sessionStorage永久,indexedDB)
- 多媒体：(video,audio)
- 2d/3d绘图效果：(canvas,svg,webGL)
- 样式设计：  
  - 背景：box-shadow  边框：border-radio,border-images  
  - 动画：transitions  
  - 排版：font-face字体图标  
  - 布局：flex
    (input-phone,number,search,date; border-radio,flex，font-face定义字体图标，transitions动画)
- 表单控件，calendar、date、time、email、url、search
- 地理(Geolocation) API
- 拖拽释放(Drag and drop) API