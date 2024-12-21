## 1 搭建python自带静态web服务器

### 1.1 静态web服务器是什么

静态web服务器是指可以为出发请求的浏览器提供静态文档的程序。

平时我们浏览百度新闻数据的时候，每天的新闻数据都会发生变化，那访问的这个页面就是动态的。

而静态的Web服务器，页面的数据不会发生变化。

### 1.2 如何搭建python自带的静态web服务器

搭建python自带的静态web服务器使用命令：

`python -m http.server`

+ -m：表示运行包里面的模块，执行这个命令时，需要进入指定静态文件的目录，然后通过浏览器就能访问对应的html文件

+ 端口号不指定默认是8000

+ 搭建步骤

  + 在终端首先进入到指定静态文件的目录

  + 输入命令：python -m http.server [端口号]，不指定端口号的话默认是8000


### 3.3 访问本地的静态文件

在浏览器中访问本地的html文件

首先在命令行进入该文件所在目录，然后启动静态服务器：

![1677141228241](imgs\1677141228241.png)

最后可以在浏览器进行访问

![1677141276464](imgs\1677141276464.png)

### 3.4 查看浏览器和搭建的web静态服务器的通信过程

![1677141611914](imgs\1677141611914.png)

![1677141699550](imgs\1677141699550.png)

## 2 静态web服务器-返回固定页面数据

### 2.1 开发自己的静态web服务器

+ 实现步骤
  + 编写一个TCP服务端程序。
  + 获取浏览器发送的http请求报文数据。
  + 读取固定页面数据，把页面数据组装成HTTP响应数据发送给浏览器。
  + HTTP响应报文数据发送完成以后，关闭服务于客户端的套接字。

### 2.2 返回固定页面数据的代码示例

```python
import socket


if __name__ == '__main__':
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用, 程序退出端口立即释放
    # SOL_SOCKET：基本套接口
    # SO_REUSEADDR：打开或关闭地址复用功能。
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    # 绑定端口号
    tcp_server_socket.bind(("", 9000))
    # 设置监听
    tcp_server_socket.listen(128)
    while True:
        # 等待接受客户端的连接请求
        new_socket, ip_port = tcp_server_socket.accept()
        # 代码执行到此，说明连接建立成功
        recv_client_data = new_socket.recv(4096)
        # 对二进制数据进行解码
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)

        with open("static/index.html", "rb") as file:
            # 读取文件数据
            file_data = file.read()


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

        # 关闭服务与客户端的套接字
        new_socket.close()
```

## 3 静态web服务器-返回指定页面数据

### 3.1 静态web服务器的问题

目前的web服务器，不管用户访问什么页面，返回的都是固定页面的数据，接下来需要根据用户的请求返回指定页面的数据

返回指定页面数据的实现步骤：

1. 获取用户请求资源的路径。
2. 根据请求资源的路劲，读取指定文件的数据。
3. 组装指定文件数据的响应报文，发送给浏览器。
4. 判断文件在服务端不存在，组装404状态的响应报文，发送给浏览器。

### 3.2 返回指定页面数据的代码示例

```python
import socket


def main():
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用, 程序退出端口立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    # 绑定端口号
    tcp_server_socket.bind(("", 9000))
    # 设置监听
    tcp_server_socket.listen(128)
    while True:
        # 等待接受客户端的连接请求
        new_socket, ip_port = tcp_server_socket.accept()
        # 代码执行到此，说明连接建立成功
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            print("关闭浏览器了")
            new_socket.close()
            return

        # 对二进制数据进行解码
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)
        # 根据指定字符串进行分割，最大分割次数指定2
        request_list = recv_client_content.split(" ", maxsplit=2)
        # print("===")
        # print(request_list)
        # print("===")

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


if __name__ == '__main__':
    main()
```

