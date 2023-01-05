---
title: swiper插件添加鼠标悬停停止滚动
date: 2023-01-05 14:02:32
tags: js
---

```
$('.swiper-container').hover(function() {
      swiper.stopAutoplay();
},function() {
     swiper.startAutoplay();
});
```