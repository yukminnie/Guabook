1.网址组成

协议+主机+端口+路径

2.电脑通信靠什么

ip地址,ip地址记不住了,就发明了dns服务器

3.端口是什么

* ip相当于公司前台,端口是收信人姓名
* 程序在发送数据之前,需要向操作系统索要一个端口
* 端口是16位的,范围是0-65535

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

#其中
1. GET 是请求方法（还有POST等，这就是个标志字符串而已）
2./ 是请求的路径（这代表根路径）
3.HTTP/1.1  中，1.1是版本号，通用了20年


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



