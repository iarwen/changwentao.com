title: Maven学习
date: 2016/08/01 11:10:25
categories:
- Maven
tags: [Maven]
---

# 基础命令
### 拷贝依赖的jar
拷贝依赖的jar包到`target-dependencies`  可指定目录`-DoutputDirectory=lib`
```
#scope 
#runtime : scope gives runtime and compile dependencies,
#compile : scope gives compile, provided, and system dependencies,
#test (default) : scope gives all dependencies,
#provided : scope just gives provided dependencies,
#system : 	scope just gives system dependencies.

mvn dependency:copy-dependencies   -DincludeScope=compile -DoutputDirectory=lib
```

