# 网站用户密码该如何保存（加密方式等等）

1.使用对称加密算法来保存，比如3DES、AES等算法，任何一方只要获得密钥即可解密出原文。
2.使用非对称加密算法

    2.1 普通单向hash，要验证passwd0是否等于passwd，只需判断hash(passwd0)是否等于hash(passwd)，有hash碰撞的缺点，难以抵御彩虹表攻击
    2.2 加盐hash，明文加上一段固定字段“盐”之后再hash，需要确保“盐”不泄露，否者和普通单向hash没有区别。
    2.3 使用例如PBKDF2、bcrypt、scrypt等多次hash和加盐混合的加密算法，破解难度大大提高。