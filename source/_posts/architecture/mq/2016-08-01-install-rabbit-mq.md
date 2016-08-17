title: 安装rabbitmq
date: 2016/08/01 13:00:25
categories:
- MQ
tags: [rabbitmq]
---
AMQP当中有四个概念非常重要
virtual host：虚拟主机
exchange：交换机
queue：队列
binding：绑定

## 持久化

当你将消息发布到交换机的时候，可以指定一个标志“Delivery Mode”（投递模式）。根据你使用的AMQP的库不同，指定这个标志的方法可能不太一样。简单的说，就是将Delivery Mode设置成2，也就是持久的（persistent）即可。一般的AMQP库都是将Delivery Mode设置成1，也就是非持久的。所以要持久化消息的步骤如下：
1、将交换机设成 durable。
2、将队列设成 durable。
3、将消息的 Delivery Mode 设置成2 。

```
echo 'deb http://www.rabbitmq.com/debian/ testing main' | \
		sudo tee /etc/apt/sources.list.d/rabbitmq.list
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | \
		sudo apt-key add -
apt-get install rabbitmq-server
rabbitmq-plugins enable rabbitmq_management
cd /etc/rabbitmq/
vim rabbitmq.config  <-<--[{rabbit, [{loopback_users, []}]}].
service rabbitmq-server restart
```