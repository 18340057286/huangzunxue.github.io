---
title: localStorage定时缓存
date: 2023-03-20 09:06:33
tags: js
---
# 1.基本方法
```
var MyLocalStorage = {
    Cache: {
        /**
         * 总容量5M
         * 存入缓存，支持字符串类型、json对象的存储
         * 页面关闭后依然有效 ie7+都有效
         * @param key 缓存key
         * @param stringVal
         * @time 数字 缓存有效时间（秒） 默认60s
         * 注：localStorage 方法存储的数据没有时间限制。第二天、第二周或下一年之后，数据依然可用。不能控制缓存时间，故此扩展
         * */
        put: function (key, stringVal, time) {
            try {
                if (!localStorage) {
                    return false;
                }
                if (!time || isNaN(time)) {
                    time = 1800;
                }
                var cacheExpireDate = (new Date() - 1) + time * 1000;
                var cacheVal = {
                    val: stringVal,
                    exp: cacheExpireDate
                };
                localStorage.setItem(key, JSON.stringify(cacheVal)); //存入缓存值
            } catch (e) { }
        },
        /**获取缓存*/
        get: function (key) {
            try {
                if (!localStorage) {
                    return false;
                }
                var cacheVal = localStorage.getItem(key);
                var result = JSON.parse(cacheVal);
                var now = new Date() - 1;
                if (!result) {
                    return null;
                } //缓存不存在
                if (now > result.exp) { //缓存过期
                    this.remove(key);
                    return "";
                }
                return result.val;
            } catch (e) {
                this.remove(key);
                return null;
            }
        },
        /**移除缓存，一般情况不手动调用，缓存过期自动调用*/
        remove: function (key) {
            if (!localStorage) {
                return false;
            }
            localStorage.removeItem(key);
        },
        /**清空所有缓存*/
        clear: function () {
            if (!localStorage) {
                return false;
            }
            localStorage.clear();
        }
    } //end Cache
}

//调用缓存函数
function getHotList() {
    try {
        var cache = MyLocalStorage.Cache.get("cacheKey");
        if (cache == null) {
            // $.get("php/timeUpdata.php", function (data) {
            //     MyLocalStorage.Cache.put("cacheKey", data, 2 * 60);
            // });
            // 重新调取接口
        }
    } catch (e) { }
}
getHotList();
```
原文链接：[https://www.bbsmax.com/A/kjdwqOp2JN/](https://www.bbsmax.com/A/kjdwqOp2JN/)

# 2.简单运用
```
function dyajax{
    var url = "ajax请求地址"
    $.ajax({
        url: url,
        type: "post",
        data: {},
        dataType: "json",
        success: function (resultAll) {
            MyLocalStorage.Cache.put(datalist, resultAll.data, 60 * 10);
            //成功调取后将内容给localStorage进行存储 60 * 10 代表10分钟
            resultAll.data.list.sort(function (a, b) { return b.publishDate - a.publishDate });
            var listlength = resultAll.data.totalCount;
            if (listlength != 0) {

                $.each(resultAll.data.list, function (i, result) {
                   //  第一次调用 循环拼接
                });
            }
        }
    })
}



// 获取缓存
function ajaxfun {
    console.log(MyLocalStorage.Cache.get('localStorage中指定存储键值'))
    if (window.localStorage.getItem('localStorage中指定存储键值')) {
        // 没有缓存，重新调接口
        try {
            var cache = MyLocalStorage.Cache.get('localStorage中指定存储键值');
            console.log(cache)
            if (cache == null) {
                console.log('有缓存，但过期，重新调取接口')
                //缓存过期之后重新调用接口
                dyajax()
            }
        } catch (e) { console.log('发生异常' + e) }
        //将缓存中数据赋值给userinfo 并进行拼接
        var userinfo = cache.list;
        userinfo.sort(function (a, b) { return b.publishDate - a.publishDate });
        var listlength = cache.totalCount;
        // console.log(listlength)
        if (listlength != 0) {
            console.log('有缓存，未过期，正常拼接')
            $.each(userinfo, function (i, result) {
               
            });
        }
    }

    else {
        //再localStorage中不存在指定存储键值时，重新调用接口
        dyajax()
    }
}
```
