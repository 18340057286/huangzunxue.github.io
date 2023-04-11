---
title: replace替换以及配合正则表达式实现部分功能
date: 2023-02-08 17:26:25
tags: js
---
# 1.简单替换，只能执行一次
```
var str = "税务政策类资讯可直接拨打税务资讯电话12366"
#要求是将资讯错别字替换为咨询
var new_str=str.replace("资讯","咨询"); 
alert(new_str);
```
# 2.多次替换（全局替换）
```
var str = "税务政策类资讯可直接拨打税务资讯电话12366"
var new_str=str.replace(/资讯/g,"咨询"); 
#或者是定义一个正则
var str_reg=new RegExp("资讯","g");
var new_str2=str.replace(str_reg,"咨询"); 
```
# 3.配合检索查询使需要检索的内容高亮展示
```
var str="税务政策类咨询可直接拨打税务咨询电话12366";
var str_reg = new RegExp("咨询","g");
var new_str=str.replace(str_reg,"<font color=red>$1</font>");
document.write(new_str);
#当然，咨询两字也可以用任何变量代替，这几可以实现input输入框输入字，检索到后通过重新拼接展示，同时对搜索到的内容高亮,如下
var title = $('.jiansuo').val()
var title_reg = new RegExp("("+title+")","g");
var new_str2=str.replace(title_reg,"<font color=red>$1</font>");
document.write(new_str2);
```
# 4.扩展 
```
#在node.js对文本文档进行读取写入时可能会遇到要获取html中script中的变量,首先需要在node.js引入cheerio依赖，
它可以使我们使用jquery的方法进行操作，如获取指定节点内容
#如下是一个script内容
<script>
    var id = "00056";
    var name = "大大大";
</script>
#则获取方法为：$('script').html()
var script_text = $('script').html()
#得到的数据会有\n 换行，以及空格，但是要将信息处理为正确的data数据格式，在进行转化，才能最后通过data.id获得想要的值
script_text = script_text.replace(/var/, '"{').replace(/;/, '}"').replace(/=/g, ':').replace(/"/g, '').replace(/\n/g, '').replace(/&nbsp;/, '')
script_text = script_text.replace(/}/, ',').replace(/var title :/, 'title :"').replace(/;/, '"}').replace(/\s*/g,"");
script_text = eval("(" + script_text + ")");
var text_id = script_text.id
var text_name = script_text.name
#经过多次replace得到所需数据
```

# 5.去除空格
```
去除字符串内所有的空格：str = str.replace(/\s*/g,"");
去除字符串内两头的空格：str = str.replace(/^\s*|\s*$/g,"");
去除字符串内左侧的空格：str = str.replace(/^\s*/,"");
去除字符串内右侧的空格：str = str.replace(/(\s*$)/g,"");
去除html代码 str = str.replace(/<.*?>/g, "")
```