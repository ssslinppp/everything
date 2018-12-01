# Linux知识
[Linux运维跳槽必备的40道面试精华题](https://zhuanlan.zhihu.com/p/33967414)   

[2017年企业运维岗经典面试题（28题）](http://www.yunweipai.com/archives/18798.html)

---
## Mysql的调优参数（重要TODO）


---
## HTTP请求的全过程（重要TODO）
重点：
> HTTP协议需要建立TCP连接，从而引出TCP的3次握手和4次挥手（区别：SYN和ACK是否一起发送），以及TCP滑动窗口的概念；（面试的重点）


---

## TCP/IP的七层模型

- 应用层：HTTP FTP TFTP SMTP SNMP DNS TELNET HTTPS POP3 DHCP
- 表示层
- 会话层
- 传输层（IP+MAC）：TCP UDP
- 网络层（IP）：ICMP IGMP IP（IPV4 IPV6） ARP RARP
- 数据链路层（MAC）
- 物理层

---

## mysql的innodb如何定位锁问题，mysql如何减少主从复制延迟？
### 定位死锁
```
show engine innodb status
```

在5.5中，information_schema 库中增加了三个关于锁的表（MEMORY引擎）
innodb_trx         ## 当前运行的所有事务
innodb_locks       ## 当前出现的锁
innodb_lock_waits  ## 锁等待的对应关系

```
MariaDB [(none)]> show databases like '%schema%';
+---------------------+
| Database (%schema%) |
+---------------------+
| information_schema  |
| performance_schema  |
+---------------------+

MariaDB [information_schema]> show tables like '%INNODB_LOCK%';
+----------------------------------------------+
| Tables_in_information_schema (%INNODB_LOCK%) |
+----------------------------------------------+
| INNODB_LOCK_WAITS                            |锁等待的对应关系
| INNODB_LOCKS                                 |当前出现的锁
+----------------------------------------------+
2 rows in set (0.00 sec)

MariaDB [information_schema]> show tables like '%INNODB_TR%';
+--------------------------------------------+
| Tables_in_information_schema (%INNODB_TR%) |
+--------------------------------------------+
| INNODB_TRX                                 |当前运行的所有事务
+--------------------------------------------+

```

### 减少主从延时复制
TODO   
- 从库硬件差
- 网络延时
- 复制为单线程，可考虑多线程
- 慢查询多
- 主库负载大：读写压力大，导致复制延时，考虑加缓存
- 从库负载大：读压力大，可多台从库分担


---

## raid0 raid1 raid5 （TODO）
TODO   

---

## 讲一下Keepalived的工作原理、如何做到健康检查

> 虚拟路由器： 多个物理路由器组成一个路由器组，这个路由器组就是虚拟路由器（1个master+N个Backup）   
> VRRP：虚机路由冗余协议，可以认为是`实现路由器高可用`的协议；  
> VIP： 虚拟路由器组对外提供的虚拟IP   

### 原理
keepalived是以VRRP协议为实现基础的，VRRP全称Virtual Router Redundancy Protocol，即虚拟路由冗余协议。

虚拟路由冗余协议，可以认为是实现路由器高可用的协议，即将N台提供相同功能的路由器组成一个路由器组

这个组里面有一个master和多个backup，master上面有一个对外提供服务的vip（该路由器所在局域网内其他机器的默认路由为该vip），master会发组播，当backup收不到vrrp包时就认为master宕掉了

这时就需要根据VRRP的优先级来选举一个backup当master。这样就可以保证路由器的高可用了

keepalived主要有三个模块，分别是core、check和vrrp。core模块为keepalived的核心，负责主进程的启动、维护及全局配置文件的加载和解析。check负责健康检查，包括常见的各种检查方式，vrrp模块是来实现VRRP协议的。

当MASTER不可用时，BACKUP收不到通告信息，多台BACKUP中优先级最高的这台会被抢占为MASTER。这种抢占是非常快速的(<1s)，以保证服务的连续性。

于安全性考虑，VRRP包使用了加密协议进行加密。BACKUP不会发送通告信息，只会接收通告信息

### 健康检查
```
global_defs {
   router_id LVS_k8s
}

//定义监控脚本
vrrp_script CheckK8sLb {
    script "curl http://admin:admin@10.254.9.21:8082/admin"
    interval 3
    timeout 9
    fall 2
    rise 2
}

//具体实例
vrrp_instance VI_1 {
    state MASTER
    interface  bond1
    virtual_router_id 61
    priority 200
    advert_int 1
    mcast_src_ip 10.254.9.21
    nopreempt
    authentication {
        auth_type PASS
        auth_pass sqP05dQgMSlzrxHj
    }
    unicast_peer {
        10.254.9.21
        10.254.9.23
    }
    virtual_ipaddress {
        10.254.9.129       
    }
    track_script {
        CheckK8sLb
    }
}

```


---

## 简述DNS进行域名解析的过程
找： www.baidu.com

1. 会先找`本机host文件`;   
2. 再找`本地设置的DNS服务器`;   
3. 网络中找`根服务器`;   
4. 找一级域名：`.cn`
5. 找二级域名：`.com.cn`
6. 找三级域名：`baidu.com.cn`

---

## 现在给你三百台服务器，你怎么对他们进行管理
1）设定`跳板机(仅跳板机可以访问)`，使用`统一账号`登录，便于安全与登录的考量。   

2）使用salt、ansiable、puppet进行系统的`统一调度与配置的统一管理`。   

3）建立简单的服务器的系统、配置、应用的`cmdb信息管理`。便于查阅每台服务器上的各种信息记录。  

---

## FTP的主动模式和被动模式
[FTP主动模式和被动模式的区别](https://www.cnblogs.com/xiaohh/p/4789813.html)

> 要点： 命令链路 + 数据链路 （类似`监听socket`和`通信socket`）  
> 不同点： 数据链路的建立方式不同；   
> 主动模式： 传送数据是“服务器”连接到“客户端”的端口(可能被客户端的防火墙阻塞掉)；  
> 被动模式： 传送数据是“客户端”连接到“服务器”的端口(可能被服务端的防火墙阻塞掉)：

主动模式    
![主动模式](http://img7.ph.126.net/GGuwAMPXzA8-US1qbwas4Q==/6597331450424136847.jpg)

![主动模式](http://static.oschina.net/uploads/space/2011/0528/204156_vG81_97118.jpg)


被动模式    
![被动模式](http://img8.ph.126.net/ulgv5cwN-D_jPR454TGiTg==/6597473287424278948.jpg)

被动模式    
![被动模式](http://static.oschina.net/uploads/space/2011/0528/204218_G2gD_97118.jpg)


```
主动FTP：
 　　命令连接：客户端 >1023端口 -> 服务器 21端口
 　　数据连接：客户端 >1023端口 <- 服务器 20端口 

被动FTP：
 　　命令连接：客户端 >1023端口 -> 服务器 21端口
 　　数据连接：客户端 >1023端口 -> 服务器 >1023端口 
```

#### 优缺点
主动模式（服务器发起）：FTP对FTP服务器的管理有利，但对客户端的管理不利。因为FTP服务器企图与客户端的高位随机端口建立连接，而这个端口很有可能`被客户端的防火墙阻塞掉`。

被动模式（客户端发起）：FTP对FTP客户端的管理有利，但对服务器端的管理不利。因为客户端要与服务器端建立两个连接，其中一个连到一个高位随机端口，而这个端口很有可能`被服务器端的防火墙阻塞掉`。

#### 选择
主动模式需要客户端必须开放端口给服务器，很多客户端都是在防火墙内，开放端口给FTP服务器访问比较困难。

被动模式只需要服务器端开放端口给客户端连接就行了。（一般选择被动模式的比较多）


---

## Linux启动流程
![启动流程](https://images2015.cnblogs.com/blog/99941/201606/99941-20160604163710258-1247432254.jpg)   

[Linux 开机引导和启动过程详解](https://linux.cn/article-8807-1.html)


![MBR](https://www.ibm.com/developerworks/cn/linux/l-linuxboot/fig2.gif)


- BIOS： 上点自检，会查找启动介质（DISK、USB等）
- MBR：主引导记录，包含`主引导加载程序(第一阶段)`：目的是查找并加载`次引导加载程序（第二阶段）`；
- GRUB： 安装在MBR的BootLoader中，加载内核，加载虚机文件系统(initramfs)，加载根文件系统的驱动程序；
- 内核引导： 根据GRUB中的配置，来加载内核
- Init阶段：内核主动调用`/sbin/init`进程，同时设置运行级别，3：多用户模式， 5：支持界面；
- Init执行`/etc/rc.d/rc.sysinit`： 准备基础环境
- Init执行`/etc/rc.d/rc.local`: 支持用户自定义脚本,用户个性化设置
- 启动阶段：