---
title: WEB缓存探究第三弹——为什么关闭ETag
date: 2017-07-28 13:48:11
tags:
- javascript
- 前端优化
- HTTP
- 缓存
---
## 前言
在{% post_link web-cache-explore 前面的文章%}里我们已经知道了ETag作为文件的指纹，
在浏览器缓存中发挥着重要作用。但今天我们就来说说为什么有时要关闭Etag。

## WHY？
以Apache为例，它其中中内建的ETag机制是用inode、文档大小和最后修改时间来产生的。
{% blockquote 阮一峰 http://www.ruanyifeng.com/blog/2011/12/inode.html 理解inode %}
**什么是inode？**
inode就是`索引节点`。
inode包含文件的元信息，具体来说有以下内容：
* 文件的字节数
* 文件拥有者的User ID
* 文件的Group ID
* 文件的读、写、执行权限
* 文件的时间戳，共有三个：ctime指inode上一次变动的时间，mtime指文件内容上一次变动的时间，atime指文件上一次打开的时间。
* 链接数，即有多少文件名指向这个inode
* 文件数据block的位置
{% endblockquote %}

### 会带来什么影响
这导致在分布式下不同机器可能会产生不同的inode，那会让每台机子上生成的ETag都有可能不同，
那么Etag反而失去了原来的意义，反过来让每次请求都可能去重新下载一次资源啦

## HOW
如果是分布式，使用ETag先了解好如何产生的，充分测试，来决定要不要关闭ETag。
当然也可以完全不用ETag，依靠cache-control来控制缓存更新时间。
