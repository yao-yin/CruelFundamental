# 对称性加密跟非对称性加密的比较，使用场景

### 对称加密

对称加密的加密和解密都用同一个密钥。安全性较低但速度更快，通常使用在对正文内容（大段）的加密中

### 非对称加密

会使用两组密钥，用任意一组里面的一个加密就必须使用另一组解密。通常设置其中一组只有一个密钥，这个密钥被所有端共享，称为公钥；另一组则每个端拥有的实例各异，称为私钥。  
安全性较高但速度较慢，通常使用在：
    1.数字签名：常见场景是客户端私钥加密+服务端公钥解密，这样服务端就能认证该请求来自于被信任的客户端。
    2.保护对称密钥：常见场景是公钥加密+私钥解密，两端传输用对称加密方法加密过的内容以及公钥加密过的对称密钥。接收端需要用自己的私钥才能解密对称密钥，进而把需读内容翻译成明文。