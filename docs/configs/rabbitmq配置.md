# rabbitmq配置

[RabbitMQ Configuration](https://www.rabbitmq.com/configure.html)  

- 环境变量
- 配置文件
- 运行时参数和策略

---

## 环境变量
- `RABBITMQ_`前缀（SHELL变量）: ；
- `rabbitmq-env.conf`配置文件：`$RABBITMQ_HOME/etc/rabbitmq/目录`；
- 优先级：`RABBITMQ_` > `rabbitmq-env.conf` > `默认配置`

[Customise RabbitMQ Environment](https://www.rabbitmq.com/configure.html#customise-environment)     
[RabbitMQ Environment Variables](https://www.rabbitmq.com/configure.html#define-environment-variables)    

默认配置位置：`$RABBITMQ_HOME/sbin/rabbitmq-defaults`
```
[root@rmq-node1 rabbitmq]# rpm -ql rabbitmq-server |grep defaults
/usr/lib/rabbitmq/bin/rabbitmq-defaults
/usr/lib/rabbitmq/lib/rabbitmq_server-3.6.12/sbin/rabbitmq-defaults
```
默认配置文件：
```
#!/bin/sh -e
##  The contents of this file are subject to the Mozilla Public License
##  Version 1.1 (the "License"); you may not use this file except in
##  compliance with the License. You may obtain a copy of the License
##  at http://www.mozilla.org/MPL/
##
##  Software distributed under the License is distributed on an "AS IS"
##  basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
##  the License for the specific language governing rights and
##  limitations under the License.
##
##  The Original Code is RabbitMQ.
##
##  The Initial Developer of the Original Code is GoPivotal, Inc.
##  Copyright (c) 2012-20
15 Pivotal Software, Inc.  All rights reserved.
##

### next line potentially updated in package install steps
SYS_PREFIX=

### next line will be updated when generating a standalone release
ERL_DIR=

CLEAN_BOOT_FILE=start_clean
SASL_BOOT_FILE=start_sasl

if [ -f "${RABBITMQ_HOME}/erlang.mk" ]; then
    # RabbitMQ is executed from its source directory. The plugins
    # directory and ERL_LIBS are tuned based on this.
    RABBITMQ_DEV_ENV=1
fi

## Set default values

BOOT_MODULE="rabbit"

CONFIG_FILE=${SYS_PREFIX}/etc/rabbitmq/rabbitmq
LOG_BASE=${SYS_PREFIX}/var/log/rabbitmq
MNESIA_BASE=${SYS_PREFIX}/var/lib/rabbitmq/mnesia
ENABLED_PLUGINS_FILE=${SYS_PREFIX}/etc/rabbitmq/enabled_plugins

PLUGINS_DIR="${RABBITMQ_HOME}/plugins"

# RABBIT_HOME can contain a version number, so default plugins
# directory can be hard to find if we want to package some plugin
# separately. When RABBITMQ_HOME points to a standard location where
# it's usually being installed by package managers, we add
# "/usr/lib/rabbitmq/plugins" to plugin search path.
case "$RABBITMQ_HOME" in
    /usr/lib/rabbitmq/*)
        PLUGINS_DIR="/usr/lib/rabbitmq/plugins:$PLUGINS_DIR"
        ;;
esac

CONF_ENV_FILE=${SYS_PREFIX}/etc/rabbitmq/rabbitmq-env.conf  # 指定了配置文件的位置

```

生产环境中不建议修改rabbitmq的环境变量，下面的几个可能会用到：
- RABBITMQ_CONFIG_FILE: 配置文件地址
- RABBITMQ_CONF_ENV_FILE： 环境变量的配置文件的地址


## 配置文件
- [Configuration File(s)](https://www.rabbitmq.com/configure.html#configuration-files)     
- [rabbitmq.conf.example](https://github.com/rabbitmq/rabbitmq-server/blob/master/docs/rabbitmq.conf.example)      

- 在终端查看config.example
```
[root@rmq-node1 ~]# rpm -ql rabbitmq-server |grep config
/usr/share/doc/rabbitmq-server-3.6.12/rabbitmq.config.example
```

#### 配置文件位置
1. 通过rabbitmq的日志：`config files`
2. 通过`ps aux|grep rabbit | grep config`

#### 网络配置(抛砖引玉)
##### tcp_listeners
设置监听的端口

##### tcp_listen_options
设置tcp的缓存大小（默认192K），缓存大，则吞吐量高，占用内存也会变高，但是并发量会变少。     
一般取值在`88KB`~`128KB`之间。
```
tcp_listen_options.buffer = 196608
tcp_listen_options.sndbuf = 196608
tcp_listen_options.recbuf = 196608
```

当连接数量到达数万或更多时，重要的是确保rabbit服务器能够接受入站连接。未接受的TCP连接将会放在有`长度限制`的队列中，可通过如下参数设置
```
# 队列中`未接受连接`的最大数目。当达到此值时，新连接会被拒绝
# 如果有成千上万的并发连接环境或者存在大量客户重新连接的场景，可以设置为 4096 或更高
tcp_listen_options.backlog = 128 
```
当挂起的连接队列的长度超过此值时，连接将被操作系统拒绝。

##### Nagle算法的enable/disable
禁用Nagle算法：用于减少延时
```
tcp_listen_options.nodelay = true  # true:禁用Nagle算法
```

##### 设置Erlang运行时线程池的大小
`RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS="+A 128"`

##### 设置文件句柄数
1. 大部分操作系统都限制了同一时间可以打开的文件句柄数；
2. TCP连接会占用文件句柄，例如，如果需要支撑10万个TCP连接，则可以设置文件句柄数为15万。

##### 操作系统相关参数
文件位置：`/etc/sysctl.conf`(注意不是rabbitmq.config)       

```
fs.file-max # 内核分配的最大文件句柄数，`cat /proc/sys/fs/file-nr `可以查看系统的极限值和当前值
net.ipv4.ip_local_port_range: 本地IP端口范围
net.ipv4.tcp_tw_reuse
...
```


---

## 运行时参数和策略
rabbitmq有两种类型的参数：
1. vhost级别的Parameter
2. global级别的Parameter

#### vhost级别参数
```
rabbitmqctl:
set_parameter [-p <vhost>] <component_name> <name> <value>
clear_parameter [-p <vhost>] <component_name> <key>
list_parameters [-p <vhost>]
```

#### global级别参数
```
rabbitmqctl:
set_global_parameter <name> <value>
clear_global_parameter <name>
list_global_parameters
```

### 策略（Policy）
策略存在的意义：    
通过Client设定的一些属性，一旦设置成功`就不能在修改`，除非删除原来的Exchange或Queue，然后重新创建。

policy是`vhost级别的Param`。      
一个Policy可以匹配1个或多个Queue、Exchange，便于批量管理。     

Policy支持动态的修改一些属性。常用来配置Federation、镜像、备份交换机、死信队列等。


```
rabbitmqctl: 
set_policy [-p <vhost>] [--priority <priority>] [--apply-to <apply-to>] 
clear_policy [-p <vhost>] <name>
list_policies [-p <vhost>]
```

man 
```
set_policy [-p vhost] [--priority priority] [--apply-to apply-to] {name} {pattern} {definition}
           Sets a policy.

           name
               The name of the policy.

           pattern ：用来匹配相关的Exchange或Queue
               The regular expression, which when matches on a given resources causes the policy to apply.

           definition: 为匹配的Exchange或Queue附加相应的功能
               The definition of the policy, as a JSON term. In most shells you are very likely to need to quote this.

           priority：
               The priority of the policy as an integer. Higher numbers indicate greater precedence. The default is 0.

           apply-to：用来指定当前Policy作用于哪一方。
               Which types of object this policy should apply to - "queues", "exchanges" or "all". The default is "all".

           For example: 设置默认的vhost中所有以"^amp."开头的Exchange为联邦交换器

           rabbitmqctl set_policy --apply-to exchanges federate-me "^amq." '{"federation-upstream-set":"all"}'

           This command sets the policy federate-me in the default virtual host so that built-in exchanges are federated.

```



