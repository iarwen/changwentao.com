title: VMware虚拟机关闭不掉的解决办法
date: 2016/03/22 18:50:25
categories:
- VMware
tags: [VMware]
---
杀进程是最终的解决方法，问题是怎么找到这个虚拟机进程

每个虚拟机启动之后，都会产生两个进程，一个主进程vmware-vmx，另一个是代理进程vprintproxy

打开任务管理器，显示出列“命令行”，vmware-vmx的命令行最后面参数就是每个虚拟机的唯一标示，然后去你要杀的虚拟机的文件夹找到vmware.log，找pipe然后找类似 \\.\pipe\vmxb6467e48d864ad84′ 然后在进程中找到这段类似的vmware-vmx进程，杀掉后vprintproxy会自己退出