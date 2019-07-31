# 面试题

## 说说 HTTP 和 HTTPS 区别？

HTTP 协议传输的数据都是未加密的，也就是明文的，因此使用 HTTP 协议传输隐私信息非常不安 全，为了保证这些隐私数据能加密传输，于是网景公司设计了 SSL（Secure Sockets Layer）协议用于 对 HTTP 协议传输的数据进行加密，从而就诞生了 HTTPS。简单来说，HTTPS 协议是由 SSL+HTTP 协 议构建的可进行加密传输、身份认证的网络协议，要比 http 协议安全。

HTTPS 和 HTTP 的区别主要如下： 

1、 https 协议需要到 ca 申请证书，一般免费证书较少，因而需要一定费用。 

2、 http 是超文本传输协议，信息是明文传输，https 则是具有安全性的 ssl 加密传输协议。 3、 http 和 https 使用的是完全不同的连接方式，用的端口也不一样，前者是 80，后者是 443。

4、 http 的连接很简单，是无状态的；HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、 身份认证的网络协议，比 http 协议安全。

## 谈一下 HTTP 协议以及协议头部中表示数据类型的字段？

HTTP 协议是 Hyper Text Transfer Protocol（超文本传输协议）的缩写，是用于从万维网 （WWW:World Wide Web）服务器传输超文本到本地浏览器的传送协议。

 HTTP 是一个基于 TCP/IP 通信协议来传递数据（HTML 文件， 图片文件， 查询结果等）。 

HTTP 是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体 信息系统。它于 1990 年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在 WWW 中 使用的是 HTTP/1.0 的第六版，HTTP/1.1 的规范化工作正在进行之中，而且 HTTP-NG\(Next Generation of HTTP\)的建议已经提出。 

HTTP 协议工作于客户端-服务端架构为上。浏览器作为 HTTP 客户端通过 URL 向 HTTP 服 务端即 WEB 服务器发送所有请求。 Web 服务器根据接收到的请求后，向客户端发送响应信息。 表示数据类型字段： Content-Type

## HTTP 请求方法都有什么？

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。 

HTTP1.0 定义了三种请求方法： GET， POST 和 HEAD 方法。 

HTTP1.1 新增了五种请求方法：OPTIONS， PUT， DELETE， TRACE 和 CONNECT 方法。 

1、 GET 请求指定的页面信息，并返回实体主体。 

2、 HEAD 类似于 get 请求，只不过返回的响应中没有具体的内容，用于获取报头

3、 POST 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在 请求体中。 POST 请求可能会导致新的资源的建立和/或已有资源的修改。 

4、 PUT 从客户端向服务器传送的数据取代指定的文档的内容。 

5、 DELETE 请求服务器删除指定的页面。 

6、 CONNECT HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。 

7、 OPTIONS 允许客户端查看服务器的性能。 

8、 TRACE 回显服务器收到的请求，主要用于测试或诊断。

## HTTP 常见请求头？

1. Host \(主机和端口号\)
2. Connection \(链接类型\)
3. Upgrade-Insecure-Requests \(升级为 HTTPS 请求\)
4. User-Agent \(浏览器名称\)
5. Accept \(传输文件类型\)
6. Referer \(页面跳转处\)
7. Accept-Encoding（文件编解码格式）
8. Cookie （Cookie）
9. x-requested-with :XMLHttpRequest \(是 Ajax 异步请求\)



