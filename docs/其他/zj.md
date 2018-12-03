# zj

后端：
```
算法一个先递增再递减的数组，给一个数找到它的位置索引
给你一段文本，怎么建立一套索引
这几年做的最有成绩的一项工作或者一个点
说说你这几年的成长
说说你的自我学习的方式
你说自学了点机器学习的，学的怎么样
```

前端：
```
Python的一个名字里带io的插件，python异步编程
nodejs做过些什么东西，对nodejs做服务端和Java的差异说一下
nodejs的io说下
带过团队吗
做没做过端到端测试
对于线性代数的掌握？
列举对于性能优化的几种使用
说下web（客户端和服务器端）的缓存机制
http和https的不同
一个请求从发出到结束的全过程
```

## 一段文本，怎么建立一套索引(倒排索引)

https://blog.csdn.net/njpjsoftdev/article/details/54015485   

https://segmentfault.com/a/1190000014480912   

http://www.nosqlnotes.com/technotes/searchengine/lucene-invertedindex/

![Lucene原理图](https://img-blog.csdn.net/20170103091620316?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcm9uYWxvZA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 倒排索引示例
```
文章1的内容为：Tom lives in Guangzhou,I live in Guangzhou too. 　　

文章2的内容为：He once lived in Shanghai.
```

分词后

```
文章1的所有关键词为：[tom] [live] [guangzhou] [i] [live] [guangzhou] 　 　

文章2的所有关键词为：[he] [live] [shanghai]
```

建立倒排索引
```
关键词          文章号 　　
guangzhou        1 　　
he               2 　　
i                1 　　
live             1,2 　　
shanghai         2 　　
tom              1 
```

加上“出现频率”和“出现位置”信息后，我们的索引结构变为：
```
词典文件（Term Dictionary）： 关键词
频率文件(frequencies)： 文章号[出现频率]
位置文件 (positions)：  出现位置

关键词            文章号[出现频率]              出现位置 　　
guangzhou           1[2]                      3，6 　　
he                  2[1]                      1 　　
i                   1[1]                      4 　　
live                1[2]                      2，5, 
                    2[1]                      2 　　
shanghai            2[1]                      3 　　
tom                 1[1]                      1 
```

![xxx](https://images0.cnblogs.com/blog/522490/201411/242157396061709.png)


### FST ： 存储词典信息（即：关键字）
词典文件： 使用 FST(Finite State Transducers: 有限状态机) :这是一个有向无环图  

> 示例   

插入“cat”     
![插入“cat”](https://images0.cnblogs.com/blog/522490/201411/242224195125261.png)

插入“deep”    
![插入“deep”](https://images0.cnblogs.com/blog/522490/201411/242225053099870.png)

插入“do”   
![插入“do”](https://images0.cnblogs.com/blog/522490/201411/242225449029041.png)

插入“dog”     
![插入“dog”](https://images0.cnblogs.com/blog/522490/201411/242226206684914.png)

插入“dogs”    
![插入“dogs”](https://images0.cnblogs.com/blog/522490/201411/242226566064202.png)



---


DocID: SkipList（使用`跳表`而不是B+Tree）     
![xx](https://segmentfault.com/img/bV8VfX?w=2064&h=492)    


FST和SkipList       
![xxx](https://segmentfault.com/img/bV8Vgi?w=1744&h=1116)   

---

## 性能优化

---

## Http和Https的区别

https://segmentfault.com/a/1190000011185129

> HTTP+加密+认证+完整性保护 = HTTPS.
> 
> 认证： 确保服务器是真实可信，而不是伪装的，使用数字证书解决；
> 
> 加密： 非对称加密（用于传输对称加密的密钥） + 对称加密
> 
> 完整性保护：  


通俗的说：
> 客户端 -> 服务器： 问服务器要数字证书（该数字证书会包含非对称加密的公钥）
> 
> 客户端： 认证数字证书是否可信，如果可信，会将`对称加密的密钥`使用公钥加密后传给服务器；
> 
> 服务器 <-->客户端： 使用约定的`对称加密-密钥`进行数据传输；
> 

![xx](https://pic1.zhimg.com/80/v2-5e2241fae8b593ff7f3b3a308ef81c10_hd.png)  

HTTPS： `HTTP` + `SSL/TLS 协议` (HTTP over SSL)   

![xx](https://pic1.zhimg.com/80/v2-54ff04e1b0cc698f08f76d6356f59fac_hd.png)   

- SSL协议（Secure Sockets Layer）：SSL运行在TCP/IP层之上、应用层之下
- TLS（Transport Layer Security）：传输层安全协议

### 对称加密 vs 非对称加密

对称加密：  加解密使用相同的密钥；

非对称加密： 加密和解码使用不同的密钥；

### HTTP 和 TCP 之间的关系

> HTTP 协议需要依靠 TCP 协议来传输数据

HTTP对TCP的使用：   

- 短连接   
- 长连接（Keep-alive）   

### SSL/TLS协议的基本运行过程

- 采用公钥加密法   
- 客户端： 先向服务器端索要公钥，然后用公钥加密信息（重点：`客户端持有公钥`）      
- 服务器：收到密文后，用自己的私钥解密；（重点：`服务器端持有私钥`）   

主要解决两个难题：    
1. 验证服务器的可信性；（使用数字证书）；    
2. 合理使用对称和非对称加密，提高性能；   


解决服务器可信问题： 数字证书    
> 如何保证公钥不被篡改：将`公钥放在数字证书`中，只要数字证书可信，公钥就可信；

解决性能问题：    
非对称加密性能低，对称加密性能高，如何保证性能？    
解决方式：   
- 非对称加密： 只用于传输`对称加密的密钥`；    
- 对称加密： 用于传输具体的数据；   

具体过程：    
1. 客户端向服务器端索要并验证公钥；   
2. 双方协商生成“对话密钥”（这个密钥用于`对称加密`）；    
3. 双方采用“对话密钥”进行加密通信(对称加密)。    

### 区别总结
1. https协议需要到ca申请证书，一般免费证书很少，需要交费。   

2. http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。  

3.http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。   

4.http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全


----

## web服务器的缓存机制（client和server）

## HTTP请求的全过程

## 线性代数

