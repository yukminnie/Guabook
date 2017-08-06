1.网址组成

协议+主机+端口+路径

2.电脑通信靠什么

ip地址,ip地址记不住了,就发明了dns服务器

3.端口是什么

* ip相当于公司前台,端口是收信人姓名
* 程序在发送数据之前,需要向操作系统索要一个端口
* 端口是16位的,范围是0-65535
* 1024以下的端口是系统保留的

4.http协议

* 协议就是双方共同遵守的规范

  * 浏览器（客户端）按照规定的格式发送文本数据（请求）到服务器
  * 服务器解析请求，按照规定的格式返回文本数据到浏览器
  * 浏览器解析得到的数据，并做相应处理

* 请求和相应是一样的数据格式

  * 请求行或者响应行
  * 请求头header\(host字段是必须的\)
  * 换行符\(/r/n/r/n\)
  * 请求体\(可选\)

* 请求格式和响应格式

```
GET / HTTP/1.1
Host: g.cn
Connection: keep-alive       减少页面的请求次数

#其中
1. GET 是请求方法（还有POST等，这就是个标志字符串而已）
2./ 是请求的路径（这代表根路径）
3.HTTP/1.1  中，1.1是版本号，通用了20年


---------------------------------------------------------------------------------
HTTP/1.1 301 Moved Permanently
Alternate-Protocol: 80:quic,p=0,80:quic,p=0
Cache-Control: private, max-age=2592000
Content-Length: 218
Content-Type: text/html; charset=UTF-8
Date: Tue, 07 Jul 2015 02:57:59 GMT
Expires: Tue, 07 Jul 2015 02:57:59 GMT
Location: http://www.google.cn/
Server: gws
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block

#Body部分太长，先不贴了
其中响应行（第一行）：
1.HTTP/1.1 是版本
2.301 是「状态码」，参见文末链接
3.Moved Permanently 是状态码的描述

浏览器会自己解析Header部分，然后将Body显示成网页
```

5.socket实现服务端

```
# 这个程序就是一个套路程序, 套路程序没必要思考为什么会是这样
# 记住套路, 能用, 就够了
# 运行这个程序后, 浏览器打开 localhost:2000 就能访问了
#
# 服务器的 host 为空字符串, 表示接受任意 ip 地址的连接
# post 是端口, 这里设置为 2000, 随便选的一个数字
host = ''
port = 2000

# s 是一个 socket 实例
s = socket.socket()
# s.bind 用于绑定,因为服务器是固定的
# 注意 bind 函数的参数是一个 tuple
s.bind((host, port))


# 用一个无限循环来处理请求
while True:
    # 套路, 先要 s.listen 开始监听
    # 注意 参数 5 的含义不必关心
    s.listen(5)
    # 当有客户端过来连接的时候, s.accept 函数就会返回 2 个值
    # 分别是 连接 和 客户端 ip 地址, address是一个type类型
    connection, address = s.accept()

    # recv 可以接收客户端发送过来的数据
    # 参数是要接收的字节数
    # 返回值是一个 bytes 类型
    # 以后会用while循环全部接收
    request = connection.recv(1024)

    # bytes 类型调用 decode('utf-8') 来转成一个字符串(str)
    print('ip and request, {}\n{}'.format(address, request.decode('utf-8')))

    # b'' 表示这是一个 bytes 对象
    response = b'<h1>Hello World!</h1>'
    # 用 sendall 发送给客户端
    connection.sendall(response)
    # 发送完毕后, 关闭本次连接
    connection.close()


#拓展,循环取数据

buffer_size = 1000
r = b''
while True:
    request = connection.recv(buffer_size)
    r += request
    if len(request) < buffer_size
        break
```

6.socket实现客户端

```
# coding: utf-8

import socket


# socket 是操作系统用来进行网络通信的底层方案
# 简而言之, 就是发送 / 接收数据

# 创建一个 socket 对象
# 参数 socket.AF_INET 表示是 ipv4 协议
# 参数 socket.SOCK_STREAM 表示是 tcp 协议
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 这两个其实是默认值, 所以你可以不写, 如下
# s = socket.socket()
# s = ssl.wrap_socket(socket.socket())

# 主机(域名或者ip)和端口
host = 'g.cn'
port = 80
# 用 connect 函数连接上主机, 参数是一个 tuple
s.connect((host, port))

# 连接上后, 可以通过这个函数得到本机的 ip 和端口
ip, port = s.getsockname()
print('本机 ip 和 port {} {}'.format(ip, port))

# 构造一个 HTTP 请求
http_request = 'GET / HTTP/1.1\r\nhost:{}\r\n\r\n'.format(host)
# 发送 HTTP 请求给服务器
# send 函数只接受 bytes 作为参数
# str.encode 把 str 转换为 bytes, 编码是 utf-8
request = http_request.encode('utf-8')
print('请求', request)
s.send(request)

# 接受服务器的响应数据
# 参数是长度, 这里为 1023 字节
# 所以这里如果服务器返回的数据中超过 1023 的部分你就得不到了
response = s.recv(1023)

# 输出响应的数据, bytes 类型
print('响应', response)
# 转成 str 再输出
print('响应的 str 格式', response.decode('utf-8'))
```

7.编码

```
ASCII 编码 

gb2312   gbk(windows简体中文版本)    gb18030  中文编码

unicode   utf-8(安卓,苹果)           通用编码
```



