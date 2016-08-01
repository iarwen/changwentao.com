title: ubuntu将字符集设置为英文
date: 2016/06/13 10:00:25
categories:
- Ubuntu Linux
tags: [ubuntu,charset]
---

ubuntu安装时选择了中文安装环境，安装结束后字符集变为乱码，以下设置将会将终端字符集设置为英文
```
#sudo vim /etc/default/locale
中文设置为：
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"
修改为：
LANG="en_US.UTF-8"
LANGUAGE="en_US:en"
LANG="en_US.UTF-8"
LANGUAGE="en_US:en"
```

