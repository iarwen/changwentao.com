title: Ubuntu基础设置
date: 2016/08/01 14:10:25
categories:
- Ubuntu Linux
tags: [Linux,Ubuntu]
---

## ssh
1.检查是否启动
```
sudo ps -e |grep ssh
```
2.修改配置允许root登陆：
```
vim /etc/ssh/sshd_confg
```
找到：修改PermitRootLogin 为 PermitRootLogin yes
```
service ssh stop
service ssh start
```

## 固定IP
```
vim /etc/network/interfaces
```
添加如下信息
```
# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.8.150
netmask 255.255.255.0
gateway 192.168.8.1
```
DNS
```
vim /etc/resolv.conf  && vim /etc/resolvconf/resolv.conf.d/base
```
写入相同的内容:
```
nameserver 192.168.8.1
nameserver 8.8.8.8
```

## 设置主机名
```
echo nginx.master >/etc/hostname
```

## 设置时区
```
tzselect
ntpdate time.windows.com
```

