# Spring Frame
## 核心
- IOC
- AOP

## [Spring框架中的单例Beans是线程安全的么](http://www.codeceo.com/article/spring-top-25-interview.html#singleton_bean_threadsafe)

非线程安全

最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”。   

其他解决方式： 在类中使用ThreadLocal


## [Spring Bean的作用域之间有什么区别](http://www.codeceo.com/article/spring-top-25-interview.html#bean_scopes)

- singleton：这种bean范围是默认的，这种范围确保不管接受到多少个请求，每个容器中只有一个bean的实例，单例的模式由bean factory自身来维护。    
- prototype：原形范围与单例范围相反，为每一个bean请求提供一个实例。    
- request：在请求bean范围内会每一个来自客户端的网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收。   
- Session：与请求范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效。   
- global-session：global-session和Portlet应用相关。当你的应用部署在Portlet容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那么这全局变量需要存储在global-session中。