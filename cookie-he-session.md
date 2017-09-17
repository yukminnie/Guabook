# Cookie and Session

---

1. 服务器给登陆用户自动发送的，浏览器自动保存
2. 服务器在响应头中设置Set-Cookie字段，来产生cookie
3. 不会让你自己实现
4. cookie不能解决身份伪造，我们使用session产生随机字符串的方式来进行检验
5. 通过arp欺骗可以获取别人的cookie和session，比如公共wifi
6. https加密协议可以防止arp欺骗，对方可以获取你的请求，但是全部都是加密的
7. 但是https也可以通过伪造证书来进行破解
8. session没有强制保存与哪里的说法，session持久化可以通过下面的方式
   1. session增加新内容时可以进行save到文件，避免重启服务器后重复登陆
   2. 对称加密，指对cookie进行加密，收到cookie后服务器进行解密
9. session过期时间可以通过在session\_id中增加时间数据，来进行判定
10. session共享，通过session服务器进行实现
11. cookie失效可以通过，使服务器session失效来进行实现

12. 


