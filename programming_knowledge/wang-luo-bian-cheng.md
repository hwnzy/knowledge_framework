# 网络编程

URL的英文全拼是\(Uniform Resoure Locator\)，意思是统一资源定位符，也就是网址

### URL的组成

[https://news.163.com/18/1122/10/E178J2O4000189FH.html](https://news.163.com/18/1122/10/E178J2O4000189FH.html)

1. **协议部分：** [https://、http://、ftp://](https://、http://、ftp://)
2. **域名部分：** news.163.com
3. **资源路径部分：** /18/1122/10/E178J2O4000189FH.html

域名就是**IP地址的别名**，它是用点进行分割使用英文字母和数字组成的名字，**使用域名目的就是方便的记住某台主机IP地址**

[https://news.163.com/hello.html?page=1&count=10](https://news.163.com/hello.html?page=1&count=10)

**查询参数部分：**?page=1&count=10

? 后面的 page 表示第一个参数，后面的参数都使用 & 进行连接



```python
import socket
import threading
import sys


# 定义web服务器类
class HttpWebServer(object):
    def __init__(self, port):
        # 创建tcp服务端套接字
        tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        # 设置端口号复用, 程序退出端口立即释放
        tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
        # 绑定端口号
        tcp_server_socket.bind(("", port))
        # 设置监听
        tcp_server_socket.listen(128)
        # 保存创建成功的服务器套接字
        self.tcp_server_socket = tcp_server_socket

    # 处理客户端的请求
    @staticmethod
    def handle_client_request(new_socket):
        # 代码执行到此，说明连接建立成功
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            print("关闭浏览器了")
            new_socket.close()
            return

        # 对二进制数据进行解码
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)
        # 根据指定字符串进行分割， 最大分割次数指定2
        request_list = recv_client_content.split(" ", maxsplit=2)

        # 获取请求资源路径
        request_path = request_list[1]
        print(request_path)

        # 判断请求的是否是根目录，如果条件成立，指定首页数据返回
        if request_path == "/":
            request_path = "/index.html"

        try:
            # 动态打开指定文件
            with open("static" + request_path, "rb") as file:
                # 读取文件数据
                file_data = file.read()
        except Exception as e:
            # 请求资源不存在，返回404数据
            # 响应行
            response_line = "HTTP/1.1 404 Not Found\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"
            with open("static/error.html", "rb") as file:
                file_data = file.read()
            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        else:
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"

            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        finally:
            # 关闭服务与客户端的套接字
            new_socket.close()

    # 启动web服务器进行工作
    def start(self):
        while True:
            # 等待接受客户端的连接请求
            new_socket, ip_port = self.tcp_server_socket.accept()
            # 当客户端和服务器建立连接程，创建子线程
            sub_thread = threading.Thread(target=self.handle_client_request, args=(new_socket,))
            # 设置守护主线程
            sub_thread.setDaemon(True)
            # 启动子线程执行对应的任务
            sub_thread.start()


# 程序入口函数
def main():

    print(sys.argv)
    # 判断命令行参数是否等于2,
    if len(sys.argv) != 2:
        print("执行命令如下: python3 xxx.py 8000")
        return

    # 判断字符串是否都是数字组成
    if not sys.argv[1].isdigit():
        print("执行命令如下: python3 xxx.py 8000")
        return

    # 获取终端命令行参数
    port = int(sys.argv[1])
    # 创建web服务器对象
    web_server = HttpWebServer(port)
    # 启动web服务器进行工作
    web_server.start()


if __name__ == '__main__':
    main()
```



**HTTP请求报文:**

1. GET 方式的请求报文
2. POST 方式的请求报文

![GET&#x548C;POST&#x8BF7;&#x6C42;&#x5BF9;&#x6BD4;&#x6548;&#x679C;&#x56FE;](../.gitbook/assets/image%20%2887%29.png)

#### HTTP响应报文 <a id="1-http&#x54CD;&#x5E94;&#x62A5;&#x6587;&#x5206;&#x6790;"></a>

![HTTP&#x54CD;&#x5E94;&#x62A5;&#x6587;](../.gitbook/assets/image%20%2813%29.png)

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

### HTTP/0.9

        HTTP协议的最初版本，功能简陋，仅支持请求方式GET，并且仅能请求访问HTML格式的资源。

### HTTP/1.0    

        在0.9版本上做了进步，增加了请求方式POST和HEAD；不再局限于0.9版本的HTML格式，根据Content-Type可以支持多种数据格式，即MIME多用途互联网邮件扩展，例如text/html、image/jpeg等；同时也开始支持cache，就是当客户端在规定时间内访问统一网站，直接访问cache即可。

        但是1.0版本的工作方式是每次TCP连接只能发送一个请求，当服务器响应后就会关闭这次连接，下一个请求需要再次建立TCP连接，就是不支持keepalive。

### HTTP/1.1    

        解决了1.0版本的keepalive问题，1.1版本加入了持久连接，一个TCP连接可以允许多个HTTP请求； 加入了管道机制，一个TCP连接同时允许多个请求同时发送，增加了并发性；新增了请求方式PUT、PATCH、DELETE等。

        但是还存在一些问题，服务端是按队列顺序处理请求的，假如一个请求处理时间很长，则会导致后边的请求无法处理，这样就造成了队头阻塞的问题；同时HTTP是无状态的连接，因此每次请求都需要添加重复的字段，降低了带宽的利用率。

### HTTP/2.0

        为了解决1.1版本利用率不高的问题，提出了HTTP/2.0版本。增加双工模式，即不仅客户端能够同时发送多个请求，服务端也能同时处理多个请求，解决了队头堵塞的问题；HTTP请求和响应中，状态行和请求/响应头都是些信息字段，并没有真正的数据，因此在2.0版本中将所有的信息字段建立一张表，为表中的每个字段建立索引，客户端和服务端共同使用这个表，他们之间就以索引号来表示信息字段，这样就避免了1.0旧版本的重复繁琐的字段，并以压缩的方式传输，提高利用率。

        另外也增加服务器推送的功能，即不经请求服务端主动向客户端发送数据。

当前主流的协议版本还是HTTP/1.1版本。



## 简述 TCP 和 UDP 的区别以及优缺点?

UDP 是面向无连接的通讯协议，UDP 数据包括目的端口号和源端口号信息。 

优点：UDP 速度快、操作简单、要求系统资源较少，由于通讯不需要连接，可以实现广播发送 

缺点：UDP 传送数据前并不与对方建立连接，对接收到的数据也不发送确认信号，发送端不知道数 据是否会正确接收，也不重复发送，不可靠。

TCP 是面向连接的通讯协议，通过三次握手建立连接，通讯完成时四次挥手 

优点：TCP 在数据传递时，有确认、窗口、重传、阻塞等控制机制，能保证数据正确性，较为可靠。 

缺点：TCP 相对于 UDP 速度慢一点，要求系统资源较多。

## TCP连接

![&#x5EFA;&#x7ACB;TCP&#x8FDE;&#x63A5;](../.gitbook/assets/image%20%2879%29.png)

| seq | ack | ACK | SYN | FIN |
| :--- | :--- | :--- | :--- | :--- |
| 序号字段 | 确认号字段 | 确认位 | 同步位 | 终止位 |

1. SYN=1, seq=x
2. SYN=1, ACK=1, seq=y, ack=x+1
3. ACK=1, seq=x+1, ack=y+1

![&#x91CA;&#x653E;TCP&#x8FDE;&#x63A5;](../.gitbook/assets/image%20%2880%29.png)

1. FIN=1, seq=u
2. ACK=1, seq=v, ack=u+1
3. FIN=1, ACK=1, seq=w, ack=u+1
4. ACK=1, seq=u+1, ack=w+1 

###  为什么不采用“两次握手”建立连接？

* 这主要是为了防止两次握手情况下已失效的连接请求报文突然又传送到服务端，而产生了错误。考虑下面情况：客户A向服务端B发送TCP连接请求，第一个连接请求报文在网络的某个结点长时间滞留，A超时后认为报文丢失，于是再重传一次连接请求，B收到后建立连接。数据传输完毕后双方断开连接，而此时前一个滞留在网络中的连接请求到达了服务器B端，而B认为A又发送了请求，如果采用的两次握手，则这种情况下B认为传输连接已经建立，并一直等待A传输数据，而A此时并无连接请求，因此不理睬，这样就造成B资源的浪费。
* 为了实现可靠数据传输， TCP 协议的通信双方， 都必须维护一个序列号， 以标识发送出去的数据包中， 哪些是已经被对方收到的。 三次握手的过程即是通信双方相互告知序列号起始值， 并确认对方已经收到了序列号起始值的必经步骤 如果只是两次握手， 至多只有连接发起方的起始序列号能被确认， 另一方选择的序列号则得不到确认 

### **为什么需要四次挥手？**为什么要不采用三次握手释放连接？

因为TCP是一个全双工协议，必须单独拆除每一条信道。4次挥手的目的是终止数据传输，并回收资源，此时两个端点两个方向的序列号已经没有了任何关系，必须等待两方向都没有数据传输时才能拆除虚链路，不像初始化时那么简单，发现SYN标志就初始化一个序列号并确认SYN的序列号。因此必须单独分别在一个方向上终止该方向的数据传输。

如果是三次挥手，会怎么样？三次的话，被动关闭端在收到FIN消息之后，需要同时回复ACK和Server端的FIN消息。如果Server端在该连接上面并没有Pending的消息要处理，那么是可以的，如果Server端还需要等待一段时间才可以关闭另外一个方向的连接，那么这样的三次挥手就不能满足条件。

### 为什么要不采用三次握手释放连接，且发送最后一次握手报文后要等待2MSL（最长报文段寿命, Maximum Segment Lifetime）的时间呢

1. 为了保证A发送的最后一个确认报文能够到达B，如果A不等待2MSL，若A返回的最后确认报文段丢失，则B不能进入正常关闭状态，而此时A已经关闭，也不可能再重传。
2. 为了防止出现“已经失效的连接请求报文”。A在发送完最后一个确认报文段后，再经过2MSL可保证本连接持续的时间内所产生的所有报文段从网络中消失。

