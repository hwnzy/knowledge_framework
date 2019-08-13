# HTTP报文

**HTTP请求报文:**

1. GET 方式的请求报文
2. POST 方式的请求报文

![GET&#x548C;POST&#x8BF7;&#x6C42;&#x5BF9;&#x6BD4;&#x6548;&#x679C;&#x56FE;](../../../.gitbook/assets/image%20%2882%29.png)

#### HTTP响应报文 <a id="1-http&#x54CD;&#x5E94;&#x62A5;&#x6587;&#x5206;&#x6790;"></a>

![HTTP&#x54CD;&#x5E94;&#x62A5;&#x6587;](../../../.gitbook/assets/image%20%2811%29.png)

| 状态码 | 说明 |
| :--- | :--- |
| 1XX | Informational（信息性状态码）接收的请求正在处理 |
| 2XX | Success（成功状态码）请求正常处理完毕 |
| 200 | OK 请求已正常处理 |
| 204 | No Content 请求处理成功但没有资源可返回 |
| 206 | Partial Content 对资源一部分的请求 |
| 3XX | Redirection（重定向状态码）需要进行附加操作以完成请求 |
| 301 | Moved Permanently 永久重定向 |
| 302 | Found 临时重定向 |
| 303 | See Other 与302相同但明确表示客户端应用 GET 方法获取资源 |
| 304 | Not Modified 资源已找到，但未符合条件请求 |
| 4XX | Client Error（客户端错误状态码）服务器无法处理请求 |
| 401 | Unauthorized 未认证，第二次响应请求则表示认证失败 |
| 403 | Forbidden 不允许访问资源 |
| 404 | Not Found 服务器没有请求的资源 |
| 5XX | Server Error（服务器错误状态码）服务器处理请求出错 |
| 500 | Internal Server Error 内部资源故障，bug 或某些临时的故障 |
| 503 | Service Unavailable 服务器暂时处于超负载或正在进行停机维护 |

