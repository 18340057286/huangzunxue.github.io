---
title: js获取url中指定参数
date: 2023-03-03 09:03:53
tags: js
---

#1.js中获取路径基本方法
```
例如路径为:huangzunxue998.top/2023/3/3/index.html?wd=dada
1. Window Location href  返回当前页面完整路径
   str = Window.Location.href
   结果为：www.huangzunxue998.top/2023/3/3/index.html?wd=dada
2.Window Location pathname  返回url路径名（域名之后到问号之前）
   str2 = Window.Location.pathname
   结果为：/2023/3/3/index.html
3.window.location.assign(url) ： 加载 URL 指定的新的 HTML 文档。 就相当于一个链接，跳转到指定的url，当前页面会转为新页面内容，可以点击后退返回上一个页面。
window.location.replace(url) ： 通过加载 URL 指定的文档来替换当前文档 ，这个方法是替换当前窗口页面，前后两个页面共用一个窗口，所以是没有后退返回上一页的
4.location.hostname 返回 web 主机的域名
  location.port 返回 web 主机的端口 （80 或 443）
  location.protocol 返回所使用的 web 协议（http: 或 https:）
5.Window Location search 返回URL的查询部分（问号之后的内容）
  str3 = Window.Location.search
  结果为：?wd=dada
```

#2.获取路径中?之后的指定内容：
```
如：https://www.baidu.com/s?wd=大大
获取wd的值

function getUrlParam(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)"); //构造一个含有目标参数的正则表达式对象
    var r = window.location.search.substr(1).match(reg); //匹配目标参数
    if (r != null) return unescape(r[2]);
    return null; //返回参数值
}

var str = 'https://www.baidu.com/s?wd=大大'

str_wd = str.getUrlParam('wd')
结果为：大大
```

#3.获取路径中的某个值
```
例如路径为：https://huangzunxue998.top/tags/js001/index.html
var getUrlUrlname = function (name) {
    var str = window.location.pathname;
    var jsid = str.substr(str.lastIndexOf(name, str.lastIndexOf(name) - 1) + 1);
    jsid = jsid.replace('js', '').replace('/index.html', '').trim()
    return jsid; //返回参数值
}
想要获取路径中js后的值
js_id = getUrlUrlname('/')
```
