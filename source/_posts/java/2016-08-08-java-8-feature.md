title: Java8的新特性
date: 2016/08/08 15:05:25
categories:
- Java
tags: [lambda]
---
之前对于Java的版本概念不是很强烈，从Java5中学习到的那些语法足以满足日常开发中的需要，最近迷恋上了`IDEA`，对于其提示出来的语法建议深感自身知识的落伍。

在这篇文章中，将汇集再发现到新语法或者特性时将会补充。

## 语法
### lambda
lambda也称为`闭包`或`匿名方法`，方法的参数为方法。
它可以自动推导出参数类型，也就是说编译器根据参数类型可以推导出用哪个类，用哪个方法去执行表达式里要执行的方法
lambda最先出现在其他语言中，从Java8才进入此语法，此语法确实带来了更好的阅读以及代码量的减少。
以下已一些常用类加以说明[BEFORE：之前的语法   AFTER：使用lambda的语法]。
<!--more-->
List:

```
List<String> list = new ArrayList<>();
list.add("1");
list.add("2");
list.add("3");
-- 输出
//BEFORE：
for(String i:list){
	System.out.println(i);
}
--- 或者
list.forEach(new Consumer<String>() {
	@Override
	public void accept(String s) {
		System.out.println(s);
	}
});

//AFTER：
--- style1:expression lambda
list.forEach((val) -> System.out.println(val));
--- style2:method reference
list.forEach(System.out::println);

-- 排序
//BEFORE：
list.sort(new Comparator<String>() {
        @Override
        public int compare(String v1, String v2) {
           return v1.compareTo(v2);
       }
});

//AFTER：
list.sort((v1, v2) -> v1.compareTo(v2));
--- 或者
list.sort(String::compareTo);
```
Map：
```
Map<String,String> map = new HashMap<>();
map.put("k1","v1");``
map.put("k2","v2");
map.put("k3","v3");

//BEFORE：
map.forEach(new BiConsumer<String, String>() {
    @Override
   	public void accept(String key, String val) {
          System.out.println(key+"-"+val);
    }
});

//AFTER：
map.forEach((key, val) -> System.out.println(key + "-" + val));
```

参考资料:
1.[IDEA官网对于语法提示的一些解释](https://blog.jetbrains.com/idea/2014/09/the-inspection-connection-issue-1/)

