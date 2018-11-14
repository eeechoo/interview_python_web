# 网络 socket

## 1. socket 
### 1.1. 一个端口能够绑定多个 socket
关于一个端口能不能绑定多个socket,
事实证明一个端口可以绑定多个socket。

socket是由 <client ip, client port, server ip, server port> 这个四元组唯一标示的。
``` python
import socket

s = socket.socket()
s.bind(('localhost', 8080))
s.listen(5)
while True:
    conn, address = s.accept()
    print('client address is\n' + str(address))

    # 将两个 socket 打印， 可以看到这两个socket fd 不同，不是同一个socket
    print(conn)
    print(s)

    print('server listen socket is \n' + str(s.getsockname()))
    # 这句话不能要，要了会报错
    # 因为server listen socket 只是监听 server 端口，并不和 client 端口进行链接，
    # 所以 getpeername 会出错
    # print('server listen socket connected to client socket \n' + str(s.getpeername()))
    print('server conn socket is \n' + str(conn.getsockname()))
    print('server conn socket connected to client socket \n' + str(conn.getpeername()))
    conn.close()
    print('************************************************')
```
``` python
# 客户端代码也是可以通过代码模拟的
import socket

s = socket.socket()

# 下面这句放在这里就不行了
#s.getsockname()

s.connect(('localhost', 8080))
print('client socket address is \n' + str(s.getsockname()))

s.close()
```


运行上述代码 server 端打印
```shell
client address is
('127.0.0.1', 49400)
<socket.socket fd=128, family=AddressFamily.AF_INET, type=SocketKind.SOCK_STREAM, proto=0, laddr=('127.0.0.1', 8082), raddr=('127.0.0.1', 49400)>
<socket.socket fd=12, family=AddressFamily.AF_INET, type=SocketKind.SOCK_STREAM, proto=0, laddr=('127.0.0.1', 8082)>
server listen socket is 
('127.0.0.1', 8082)
server conn socket is 
('127.0.0.1', 8082)
server conn socket connected to client socket 
('127.0.0.1', 49400)
************************************************
client address is
('127.0.0.1', 56542)
<socket.socket fd=568, family=AddressFamily.AF_INET, type=SocketKind.SOCK_STREAM, proto=0, laddr=('127.0.0.1', 8082), raddr=('127.0.0.1', 56542)>
<socket.socket fd=12, family=AddressFamily.AF_INET, type=SocketKind.SOCK_STREAM, proto=0, laddr=('127.0.0.1', 8082)>
server listen socket is 
('127.0.0.1', 8082)
server conn socket is 
('127.0.0.1', 8082)
server conn socket connected to client socket 
('127.0.0.1', 56542)
************************************************
```

运行上述代码 client 端 打印
```shell
client socket address is 
('127.0.0.1', 49400)

client socket address is 
('127.0.0.1', 56542)
```

### 1.2. web 服务器原理

- 单进程阻塞型服务器
[一起写一个 Web 服务器（1）](http://python.jobbole.com/81524/)
- 手撸 符合 WSGI 规范的 socket 服务器
[一起写一个 Web 服务器（2）](http://python.jobbole.com/81523/)
    
    WSGI服务器核心
    
    1. 接收到 HTTP 请求
        
        将解析后的信息 连同 WSGI 服务器本身的一些信息 构成 environ对象
    2. 调用 application(environ, start_response)

        start_response 会 构建 HTTP response 中的 status code 和 reponse headers
    3. response_body = application(environ, start_response)

        response_body 会 构成 HTTP response 中的 body 部分
- 手撸 简单的 多进程 Web 服务器
[一起写一个Web服务器（3）](http://python.jobbole.com/81820)
    
    神奇的 fork 函数(windows 下没有 fork 函数)
        
        - fork 会 调用一次，返回两次
        - 父进程 返回 子进程 id
        - 子进程 返回 0

    神奇的 file descriptor

    神奇的 僵尸进程 & SIGCHILD & os.wait & os.waitpid
        - os.wait()
            Wait for completion of a child process
        - os.waitpid()
            Wait for the completion of one or more child processes

## 3. 常用工具比较
- cURL wget 下载工具
[What is the difference between curl and wget](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget)

- API 测试工具（图形化工具）
    
    **Postman** is the only complete API development environment, for API developers, used by more than 5 million developers and 100000 companies worldwide

-  HTTP接口测试（命令行工具）

    HTTPie vs cURL vs telnet
    
    HTTPie 更人性化点


