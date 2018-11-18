# 安装
直接下载
```
wget http://server-ip:15672/cli/rabbitmqadmin

mv rabbitmqadmin /usr/local/bin
chmod +x /usr/local/bin/rabbitmqadmin
```

## rabbitmqadmin界面
```
http://192.168.35.138:15672/cli/
```

## 命令
```
[root@rmq-node1 opt]# rabbitmqadmin help subcommands
Usage
=====
  rabbitmqadmin [options] subcommand

  where subcommand is one of:

Display
=======

  list users [<column>...]
  list vhosts [<column>...]
  list connections [<column>...]
  list exchanges [<column>...]
  list bindings [<column>...]
  list permissions [<column>...]
  list channels [<column>...]
  list parameters [<column>...]
  list consumers [<column>...]
  list queues [<column>...]
  list policies [<column>...]
  list nodes [<column>...]
  show overview [<column>...]

Object Manipulation
===================

  declare queue name=... [node=... auto_delete=... durable=... arguments=...]
  declare vhost name=... [tracing=...]
  declare user name=... password=... tags=...
  declare exchange name=... type=... [auto_delete=... internal=... durable=... arguments=...]
  declare policy name=... pattern=... definition=... [priority=... apply-to=...]
  declare parameter component=... name=... value=...
  declare permission vhost=... user=... configure=... write=... read=...
  declare binding source=... destination=... [arguments=... routing_key=... destination_type=...]
  delete queue name=...
  delete vhost name=...
  delete user name=...
  delete exchange name=...
  delete policy name=...
  delete parameter component=... name=...
  delete permission vhost=... user=...
  delete binding source=... destination_type=... destination=... properties_key=...
  close connection name=...
  purge queue name=...

Broker Definitions
==================

  export <file>
  import <file>

Publishing and Consuming
========================

  publish routing_key=... [exchange=... payload=... payload_encoding=... properties=...]
  get queue=... [count=... requeue=... payload_file=... encoding=...]

  * If payload is not specified on publish, standard input is used

  * If payload_file is not specified on get, the payload will be shown on
    standard output along with the message metadata

  * If payload_file is specified on get, count must not be set
```
