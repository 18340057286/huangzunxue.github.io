---
title: 无缝滚动，兼容ie7
date: 2022-10-28 11:23:53
tags: js
---
# 无缝滚动，兼容ie7
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <body>
        <div id="container">
            <ul id="content">
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=2927586094,212838758&fm=253&fmt=auto&app=138&f=JPEG?w=800&h=500" /></a>
                </li>
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=712694738,3554631191&fm=253&fmt=auto&app=120&f=JPEG?w=500&h=353" /></a>3
                </li>
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=2318304628,3725588736&fm=253&fmt=auto&app=120&f=JPEG?w=787&h=500" /></a>4
                </li>
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=1686691636,3058220847&fm=253&fmt=auto&app=120&f=JPEG?w=889&h=500" /></a>6
                </li>
                <li><a href="#"><img
                            src="https://img0.baidu.com/it/u=3325784342,2976910751&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=281" /></a>5
                </li>
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=377961337,1258578817&fm=253&fmt=auto&app=120&f=JPEG?w=1422&h=800" /></a>5
                </li>
                <li><a href="#"><img
                            src="https://img2.baidu.com/it/u=4260665178,1669850930&fm=253&fmt=auto&app=138&f=JPEG?w=889&h=500" /></a>4
                </li>
            </ul>
        </div>
    </body>
</body>
<style>
    * {
        margin: 0;
        padding: 0;
    }

    img {
        border: 0;
    }

    #container {
        width: 800px;
        height: 130px;
        border: 3px solid blue;
        overflow: hidden;
        position: relative;
    }

    #container ul {
        list-style: none;
        /* 此处注意一定要给 ul非常大的宽度，要不然li浮动先是掉下去然后宽度够了再上来，效果受影响 */
        width: 10000px;
        position: absolute;
    }

    #container ul li {
        width: 120px;
        height: 130px;
        float: left;
        margin-right: 10px;

    }

    img {
        width: 120px;
        height: 130px;
    }
</style>
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script>

    /* window.onload比 $(function(){}) 加载的更晚一些，这样那些宽度的计算在Chrome中就可以准确计算了*/
    window.onload = function () {

        /*计算一个segment的宽度*/

        var segmentWidth = 0;
        // 速度 越大越慢
        var speed = 20000;
        $("#container #content li").each(function () {
            segmentWidth += $(this).outerWidth(true);
        });

        $("#container #content li").clone().appendTo($("#container #content"));

        run(speed);

        function run(interval) {
            $("#container #content").animate({ "left": -segmentWidth }, interval, "linear", function () {
                $("#container #content").css("left", 0);
                run(speed);
            });
        }

        $("#container").mouseenter(function () {
            $("#container #content").stop();
        }).mouseleave(function () {
            var passedCourse = -parseInt($("#container #content").css("left"));
            var time = speed * (1 - passedCourse / segmentWidth);
            run(time);
        });
    };

</script>

</script>

</html>
```
