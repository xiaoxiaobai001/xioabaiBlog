# https #
1. 什么是 https
HTTPS（全称： Hypertext Transfer Protocol over Secure Socket Layer），是以安
全为目标的 HTTP 通道，简单讲是 HTTP 的安全版。即 HTTP 下加入 SSL 层， HTTPS 的安全基础是 SSL，因此加密的详细内容请看 SSL。
见下图：
![](https://i.imgur.com/dNH1WDR.png)
https 所用的端口号是 443。

2. https 的实现原理
有两种基本的加解密算法类型：
1） 对称加密：密钥只有一个，加密解密为同一个密码，且加解密速度快，典型的对称
加密算法有 DES、 AES 等；
2）非对称加密：密钥成对出现（且根据公钥无法推知私钥，根据私钥也无法推知公钥），
加密解密使用不同密钥（公钥加密需要私钥解密，私钥加密需要公钥解密），相对对称加密
速度较慢，典型的非对称加密算法有 RSA、 DSA 等。
下面看一下 https 的通信过程：
![](https://i.imgur.com/LaVAY8R.png)

3. https 通信的优点：
1）客户端产生的密钥只有客户端和服务器端能得到；
2）加密的数据只有客户端和服务器端才能得到明文；
3）客户端到服务端的通信是安全的。