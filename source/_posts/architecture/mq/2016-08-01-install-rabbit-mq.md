title: 安装rabbitmq
date: 2016/08/01 13:00:25
categories:
- MQ
tags: [rabbitmq]
---

```
echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc |         sudo apt-key add -
apt-get install rabbitmq-server
rabbitmq-plugins enable rabbitmq_management
cd /etc/rabbitmq/
vim rabbitmq.config  <-<--[{rabbit, [{loopback_users, []}]}].
service rabbitmq-server restart
```