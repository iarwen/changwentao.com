title: js控制checkbox选中
date: 2016/03/08 14:49:25
categories:
- javascript
tags: [javascript,html,checkbox]
---
jquery使用attr('checked','checked') 设置`checkbox`后，第一次是成功的，后面清空掉再使用，就会出现有checked属性，但是勾没有显示
checkbox设置应换成 prop('checked',true)
jquery检查一个checkbox是否是选中状态
$("input[type='checkbox']").is(":checked")