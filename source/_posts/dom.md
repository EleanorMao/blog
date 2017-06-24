---
title: DOM
date: 2017-05-23 16:40:50
tags:
- html
---

DOM描绘了一个层次化的节点树，可以将HTML或XML文档描绘成一个由多节点构成的结构。

## 文档节点
是每个文档的根节点。通常在HTML页面中文档节点只有一个子节点，且始终是`<html>`元素，称之为文档元素。
文档元素是文档的最外层元素，每个文档只能有一个文档元素。（XML任何元素都可以成为文档元素）

## 节点关系
每个节点都有一个`childNodes`属性，其中保存着一个`NodeList`对象。
`NodeList`是类数组对象（不是快照的那种）。`NodeList`可以用方括号或者`item()`取节点。

每个节点有一个`parentNode`属性，指向父节点。
还有兄弟节点`previousSibling`和`nextSibling`。
所有节点都有一个ownerDocument，指向整个文档的文档节点。
(...没意思不想写惹_(:з」∠)_)

### 操作节点

Function | Description
---|---
appendChild | 插入末尾
insertBefore(要插入的, 参照的节点) | 插入最前
replaceChild(要插入的，要替换的)| 替换
removeChild | 移除
cloneNode | 复制（因为ie下的bug，所以福之前最好移除事件）
normalize | 移除不包含文本的文本节点，合并文本节点

## Node类型
因为IE没有公开Node的构造函数，所以最好还是用数字作比较

Constant | Value | Description
---|---|---
Node.ELEMENT_NODE | 1 | nodeName为标签名，nodeValue是null
Node. ATTRIBUTE_ NODE | 2 | nodeName是属性名，nodeValue是属性值名，不支持子节点
Node.TEXT_NODE | 3 | nodeName是"#text"，nodeValue是包含的文本，不支持子节点
Node.CDATA_SECTION_NODE | 4 | nodeName是"#cdata-section"，nodeValue是CDATA区域内容，不支持子节点
Node.ENTITY_REFERENCE_NODE | 5
Node.ENTITY_NODE | 6
Node.PROCESSING_INSTRUCTION_NODE | 7
Node.COMMENT_NODE | 8 | nodeName是"#comment"， nodeValue是注释内容，不支持子节点
Node.DOCUMENT_NODE | 9 | 是HTMLDocument的一个实例，nodeName是"#document"，nodeValue是null，parentNode是null，ownerDocument是null
Node.DOCUMENT_TYPE_NODE | 10 | nodeName是doctype的名称，nodeValue是null，不支持子节点
Node.DOCUMENT_FRAGMENT_NODE | 11 | nodeName是"#document-fragment"，nodeValue是null，parentNode是null
Node.NOTATION_NODE | 12


## Document

### 文档子节点
Document节点的子节点可以是DocumentType、Element、ProcessingInstruction或Comment。
几个快捷属性：
1. `document.documentElement`属性始终指向<html>元素。
2. `document.body`属性始终指向<body>元素。
3. `document.doctype`指向<!doctype>元素。（ie8及以前返回null）
如果子节点包含Comment，在Chrome中测试，仅会为第一条Comment创建节点。据说ie9及更高会为所有Comment创建节点，FF和Safari3.1之前会忽略。

### 文档信息
1. document.title
2. document.URL
3. document.domain
4. document.referrer

### 特殊集合
1. document.anchors
2. document.applets
3. document.forms
4. document.images
5. document.links

### 一致性检查
1. document.implementation.hasFeature(功能, 版本号) //返回true不代表实现与规范一致

## Element

### 标准特性
1. id
2. title
3. lang
4. dir
5. className
6. attributes
...

一般来说所有特性都是属性，不过如果为DOM元素自定义一个属性，不会自动成为元素的特性
```
div.myColor = "red";
console.log(div.getAttribute("myColor")) //null (除ie)
```

### attributes
该属性是Element类型唯一一个DOM类型节点。
方法：
1. getNamedItem(name)
2. removeNamedItem(name) //删除后返回被删除节点
3. setNamedItem(name)
4. item(position)

attributes对象中的特性不同浏览器返回顺序不同
ie7及更早会返回HTML元素中所有可能的特性，但指定特性specified属性为true

## Attr
虽然也是节点，但是不被认为是DOM文档树的一部分

### 属性
1. name 特性名
2. value 特性值
3. specified 是否是指定的

## Text

### 方法
1. appendData(text) 添加文本到结尾
2. deleteData(offset, count) 从offset开始删除count个字符
3. insertData(offset, text) 从offset开始插入text
4. replaceData(offset, count, text) 从offset开始把count个字符替换成text
5. splitText(offset) 从offset分割文本
6. substringData(offset, count) 从offset提取count个字符
7. createTextNode 创建文本节点

可以通过data属性获取文本内容

## Comment
与Text继承自相同基类
因此他拥有除splitText外所有的操作方法

## DocumentType
不常用，并不是所有浏览器都支持他
不能动态创建

### 属性
1. name //代表文档类型名称
2. entities //由文档类型描述的实体的NamedNodeMap对象，chrome试了没这个属性
3. notations //是由文档类型描述的符号的NamedNodeMap对象，chrome试了没这个属性

## DocumentFragment
是一种轻量级的文档，可以通过createDocumentFragment()创建文档片段

