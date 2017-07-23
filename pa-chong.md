# 爬虫笔记



---



> 多线程？非对称？分布式？自动化交易？ 

```
1. 抓包工具
    1.1 协议层
        1.1.1 tcp 传奇外挂 解包工具 抓包工具 wireshark
        1.1.2 udp
        1.1.3 http fiddler
    1.2 作用
        1. 分别前后端 谁的问题
        2. 学习目的
        3. 限速 代理 -> pc网络 fiddler
    1.3 https本身安全 证书 中间人
2. 爬虫
    1. 用途 搜索引擎 query -> page rank spider -> click model， ltl回归分析
    2. 数据分析 量化分析
    3. 种类
        3.1 裸请求
        3.2 反爬虫 ua 限制 IO复用
        3.3 SPA js代码 -> webkit
    4. robots.txt
    5. 做法
        1. 可变不可变分离
        2. 业务拆分解耦

        1. Downloader 下载器        requests(urllib2 urllib urllib3)
        2. html页面 -> 结构化的页面 Html Parser  pyquery(lxml)
        3. DataModel

        1. io复用 select poll epoll/kqueue asyncore.py
        2. 消息队列
            download -> queue
            parser ->queue -> datamodel

            celery
            sinatra

            RabbitMQ

3.x 编码
    数字 字符串 类型实例 - 二进制
    字符串 - 二进制 （编码）
    unicode - utf8 utf16 utf32

    字符串 -> encode() -> 二进制
    二进制 -> decode(utf8) -> 字符串

    终端
    file
    editor

3. json RESTful 分摊负载
    json -> 服务器渲染
            SPA 客户端渲染 ajax -> 数据 -> 最后去生成页面
            json 1. 跨框架 2. 跨语言
            ajax -> json -> 生成数据改变页面

    RESTful api url就是资源，就是访问资源形式。
    GET    /users
    GET    /user/1
    PATCH  /users
    PUT    /users
    DELETE /user/1
    GET    /user/1/items
    GET    /user/1/item/1

    restful
    rpc
```


