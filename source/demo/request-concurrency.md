---
title: 请求并发笔记
date: 2017-09-13 11:06:41
pages: demo
---
[参考](https://www.zhihu.com/question/20474326)

# 整理总结

## 浏览器的并发请求数目限制是针对同一域名的。
意即，同一时间针对同一域名下的请求有一定数量限制。超过限制数目的请求会被阻塞。
*另外，如果请求静态资源与主站在同一域名下，会把不需要的cookie信息带过去，造成资源浪费*[详见]
(http://webmasters.stackexchange.com/questions/26753/why-do-big-sites-host-their-images-css-on-external-domains）

## 不同浏览器的并发数

|Browser|HTTP/1.1|HTTP/1.0|
|-------|--------|--------|
|IE 6,7	|2|4|
|IE 8   |6|6|
|Firefox 2|2|8|
|Firefox 3|6|6|
|Safari 3,4|4|4|
|Chrome 1,2|6|?|
|Chrome 3|4|4|
|Chrome 4+|6|?|
|iPhone 2|4|?|
|iPhone 3|6|?|
|iPhone 4|4|?|
|Opera 9.63,10.00alpha|4|4|
|Opera 10.51+|8|?|






