##  网络请求，协议



[TOC]




### ajax 过程？

(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.

(2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.

(3)设置响应HTTP请求状态变化的函数.

(4)发送HTTP请求.

(5)获取异步调用返回的数据.

(6)使用JavaScript和DOM实现局部刷新.




### 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？

- 查找浏览器缓存 
- DNS解析、查找该域名对应的IP地址、重定向（301）、发出第二个GET请求
- 进行HTTP协议会话
- 客户端发送报头(请求报头)
- 服务器回馈报头(响应报头)
- html文档开始下载
- 文档树建立，根据标记请求所需指定MIME类型的文件
- 文件显示
      

浏览器这边做的工作大致分为以下几步：

- 加载：根据请求的URL进行域名解析，向服务器发起请求，接收文件（HTML、JS、CSS、图象等）。

- 解析：对加载到的资源（HTML、JS、CSS等）进行语法解析，建议相应的内部数据结构（比如HTML的DOM树，JS的（对象）属性表，CSS的样式规则等等）








### get和post有什么区别？

区别：

第一：get参数是在url上面，post 是放在request body里面；

- get参数长度有限制，参数没有暴露在url上面；
- get请求的数据可以缓存

相同点：

底层都是TCP/IP

本质区别：

Get产生一个TCP包，post产生两个；



### fetch如何处理跨域？

服务端要支付cors 才能得到数据，如果服务端不支持网络请求的整个过程？

1. 浏览器解析URL，解析url的协议/网络地址/资源路径（端口号这一步用不到）
2. dns解析（host读取，dns服务器）
3. 找到了服务器，浏览器获取端口号，http协议默认80端口
4. 找到服务器IP后，开始建立TCP连接（三次握手）
5. 发送http请求（请求报文）
6. 服务器处理请求, web 服务器（apache,ngnix,IIS）解析用户请求，知道调用哪些资源，调用数据库，最后将通过web服务器将结果返回给浏览器客户端；
7. 返回响应结果 （http状态码，）
8. 关闭TCP连接 （四次挥手）
9. 浏览器加载解析渲染




### 浏览器加载解析渲染过程 ？
【自上而下】进行加载，并在加载过程中进行解析渲染；
解析过程

1. html解析成一个dom树，dom树的构建过程是一个深度遍历过程 ；当前节点所有子节点渲染后才会去构建当前节点的下一个子节点 ；
2. css解析成css rule tree;
3. 根据dom树和CSSOM(css对象模型)来构造 rendering tree;

![1603798073301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1603798073301.png)

![1603797913912](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1603797913912.png)




### http请求报文解剖？
http请求报文主要由三部分组成，
http报文行，报文头，报文体；
请求行：定义请求协议post,get;  请求url，http协议及版本
报文头：缓存控制，cookies设置，accept告诉服务端接收什么类型参数，referer发送请求的地址来源；
报文体：请求的参数


* 接收的格式，来源，语言编码，浏览器信息，是不使用缓存，超时时长和最大请求数, cookies（keep-alive:timeout=5,max=1000）
* http/2协议中， connection和 keep-alive是被忽略的，在其中采用其他机制来进行管理



### 响应头部的信息？
1. 请求头部信息（request header）

- 接收的格式，来源，语言编码，浏览器信息，是不使用缓存，超时时长和最大请求数, cookies（keep-alive:timeout=5,max=1000）
- http/2协议中， connection和 keep-alive是被忽略的，在其中采用其他机制来进行管理


2.响应头部信息（response  headers）

- 内容压缩格式/长度/类型：gzip/1000/text、html/日期/服务器/




### dns查询层级？
* 从浏览器缓存中查询（一般是2-30分钟）
* 从操作系统缓存中查询
* 从路由器中查询DNS缓存
* ISP中查询DNS缓存；
* 域名服务器迭代查询，根据返回的地址逐级向上查询。www.google.com->本地dns服务器查询的结果 101.1.1.1->101.2.2.2->查询的结果返回到本地dns服务器->发送给客户端

![1603798095909](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1603798095909.png)

说明 ：客户端到本地服务器属于递归查询，DNS服务器属于迭代查询





### http状态码有哪些，分别代表什么意思 ？

* 1** 信息类：表示接收到请求并且继续处理；
* 2**  响应成功：动作被成功接收、理解、接受
* 3**  重定向：为了完成指定的动作，必须接收进一步处理
* 4**  请求包含错误语法或不能正确执行；
* 5**  服务器错误：服务器不能正确执行一个正确的请求






### 为什么axios会发两次？
这个和跨域有关系，第一次预请求，不携带数据，判断服务器能否跨域；在预检请求的返回中，服务器端也通知客户端，是否需带身份凭证（cookies,http认证相关数据）第二次携带真实的数据，发送请求；



### 浏览器缓存技术？

浏览器缓存机制主要分为两种

http强缓存（本地缓存）,协商缓存，

强缓存  状态码304，直接从本地读取，不会发起请求到服务器；

* 有效期

* 协商缓存  状态码200，  从服务器端缓存中读取，会发起请求到服务器；

  通过返回的header

* cache-control: 

  expires: 控制缓存有效期

* etag:

  last-modified: 最后更新时间





## http&https





###  http与https有什么区别？

1. https增加了数据加密，比http更安全
2. https 默认使用443端口，http是80端口
3. https相对比http慢一些，因为为加/解密这个过程
4. https需要向CA机构申请证书；



### https加密过程？

主要分为三个阶段

1. 验证证书，生成秘钥阶段
2. 使用公钥，加密数据阶段
3. 使用私钥，解密数据阶段

参考：https://cloud.tencent.com/developer/article/1643914





### http三次握手四次挥手?

三次握手：是指在建立一个TCP连接时，需客户端和服务器总共发送3个数据包；

参考：https://cloud.tencent.com/developer/article/1643914



![图片1](https://oscimg.oschina.net/oscnet/5f28460ed8cc8d5c6d225f0e1514c85a65e.png)





![img](https://oscimg.oschina.net/oscnet/1c0dc08b250b64aacc1b5d9acbf7c6e40a6.png)







