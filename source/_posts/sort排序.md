---
title: sort排序
date: 2023-01-16 11:00:03
tags: js
---
#1.按一定规则，序号或者时间
```
var data = data
data.sort(function (a, b) { return b.time - a.time });
```
#2.按指定方式进行排序
```
var order = ['天气','日期', '地址']
var data = data
如果data数据中的城市属性（cssx）字段有排序中的值，可按如下进行排序
data = data.sort((a, b) => {
return order.indexOf(a.cssx) - order.indexOf(b.cssx)
})
```