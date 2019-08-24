# TCP 客户端程序

![TCP &#x5BA2;&#x6237;&#x7AEF;&#x7A0B;&#x5E8F;&#x5F00;&#x53D1;&#x6D41;&#x7A0B;&#x7684;&#x4ECB;&#x7ECD;](../../../.gitbook/assets/image%20%2824%29.png)

1. 创建客户端套接字对象
2. 和服务端套接字建立连接
3. 发送数据
4. 接收数据
5. 关闭客户端套接字

```python
import socket

if __name__ == '__main__':
    # 创建tcp客户端套接字
    # 1. AF_INET：表示ipv4
    # 2. SOCK_STREAM: tcp传输协议
    tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 和服务端应用程序建立连接
    tcp_client_socket.connect(("192.168.131.62", 8080))
    # 代码执行到此，说明连接建立成功
    # 准备发送的数据
    send_data = "你好服务端，我是客户端".encode("gbk")
    # 发送数据
    tcp_client_socket.send(send_data)
    # 接收数据, 这次接收的数据最大字节数是1024
    recv_data = tcp_client_socket.recv(1024)
    # 返回的直接是服务端程序发送的二进制数据
    print(recv_data)
    # 对数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收服务端的数据为:", recv_content)
    # 关闭套接字
    tcp_client_socket.close()
```

