# Cookie and Session

---

1. 服务器给登陆用户自动发送的，浏览器自动保存
2. 服务器在响应头中设置Set-Cookie字段，来产生cookie
3. 不会让你自己实现
4. cookie不能解决身份伪造，我们使用session产生随机字符串的方式来进行检验
5. 通过arp欺骗可以获取别人的cookie和session，比如公共wifi
6. https加密协议可以防止arp欺骗，对方可以获取你的请求，但是全部都是加密的
7. 但是https也可以通过伪造证书来进行破解
   1. 













