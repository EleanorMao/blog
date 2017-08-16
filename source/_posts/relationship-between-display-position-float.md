---
title: display、position和float的关系
date: 2017-07-31 14:56:45
tags:
- html
- css
---
【翻译自[W3C](https://www.w3.org/TR/CSS2/visuren.html#dis-pos-flo)】

`display`，`positon`和`float`这三个属性都会影响盒子模型和布局，
他们的关系影响如下：

* 如果`display`的值设为`none`，那么`position`和`float`不会被应用。这种情况下，元素不会生成盒子。
* 如果`position`如果值设为`absolute`或`fixed`，被定位后的盒子会将`float`计算为`none`，display会按照下表表现。
  如果该盒子定义了`top`、`right`、`bottom`和`left`属性，那么该盒子则是块级的。
* 如果`float`定义为`none`以外的值，那么这个盒子的`display`则如下表表现。
* 如果该元素是根元素，`display`则如下表表现。除了在CSS2.1中因`list-item`会被计算为`block`还是`list-item`是未定义的。
* 其余`display`属性会按所指定属性表现。

| 属性 | 计算为|
| ---- | ---- |
| inline-table | table |
| inline, table-row-group, table-column, table-column-group, table-header-group, table-footer-group, table-row, table-cell, table-caption, inline-block | block |
| others| 如所设置的 |