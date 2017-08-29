---
title: 关于inline-block和line-height
date: 2017-08-29 15:10:46
tags:
- css
- diary
---
# 前言
最近在看关于inline-block布局产生间隙的原因，顺下去就慢慢看到line-height相关的东西，下面就整理了一下看的东西

# 整理
## line-height
* `line-height`设置为百分比或明确的像素(如100%或20px)，行高会被继承下来，继承的是计算值，与之后设置的字号无关(如body设置了font-size:12px,line-height:200%，则继承的行高都是24px)
* `line-height`设置为数字时候(如1.2)时，行高会与根据之后的字号计算
* （`line-height`-`font-size`)/2 这段距离被称为半行间距
* 基线，指的是一行字横排时下沿的基础线，基线并不是汉字的下端沿，而是英文字母x的下端沿
* 行高具有垂直居中的特性

### boxes？？
* 行内元素会生成一个行内框(inline box)，没有特别标签的叫匿名inline box
* 一行的又叫行框(line box)，以一行里面最高的inline box为准
* 内容区(content area)，有时inline box会小于content area，如果line-height小于font-size，那inline-box会优于行高

### 参考资料
* [行号：line-height属性1](http://www.ddcat.net/blog/?p=227)
* [行号：line-height属性2——行高的计算与继承](http://www.ddcat.net/blog/?p=228)
* [行号：line-height属性3](http://www.ddcat.net/blog/?p=232)
* [深入了解css的行高Line Height属性](http://www.cnblogs.com/fengzheng126/archive/2012/05/18/2507632.html)
* [css行高line-height的一些深入理解及应用](http://www.zhangxinxu.com/wordpress/2009/11/css%E8%A1%8C%E9%AB%98line-height%E7%9A%84%E4%B8%80%E4%BA%9B%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%8F%8A%E5%BA%94%E7%94%A8/)

## inline-block layout
* inline-block布局的空格产生于标签(HTML)间的空格(写的时候换行有关)，使用letter-space或margin负值会因为不同浏览器和字体间的不同间隙不一样所以最好别用，可以用letter-spacing:-3px;font-size:0来处理
* inline-block布局不会和float一样错来错去是因为line box的关系

### 参考资料
* [去除inline-block元素间间距的N种方法](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)
* [拜拜了,浮动布局-基于display:inline-block的列表布局](http://www.zhangxinxu.com/wordpress/2010/11/%E6%8B%9C%E6%8B%9C%E4%BA%86%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%80-%E5%9F%BA%E4%BA%8Edisplayinline-block%E7%9A%84%E5%88%97%E8%A1%A8%E5%B8%83%E5%B1%80/)

