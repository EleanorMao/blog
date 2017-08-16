---
title: 解决input[type=file]打开时慢、卡顿问题
date: 2017-07-28 15:40:17
tags:
- html
- 优化
---

昨天临下班测试给我问我为什么图片上传插件打开文件夹的速度这么慢，让我想办法优化一下
然后我就努力的搞了起来_(:з」∠)_

由于我们内部系统不兼容ie，所以我就没有管ie，在浏览器里面玩了起来

经过测试发现，在mac里面safari、Firefox、Chrome(opera不知道为啥老闪退)都没有卡顿问题

在windows里面，Firefox不卡顿，只有Chrome卡顿。

然而，这个插件是从另一个项目里面借用过来，再加上了限定图片类型的功能而已。
原组件并没有这个卡顿问题，那么问题只可能是在限定图片类型这点上了。

先贴上我的代码
```javascript
<input
    accpet="image/*"
    style={inputStyle}
    ref={c=> this._imgFile = c}
    onChange={this.handleChange.bind(this)}
    type="file" name="image" disabled={disabled}
/>
```
于是我决定先去掉`accpet`试试……
果然就没有了卡顿的问题。
那么本包在试试`accpet="image/jpg"`果然也不卡卡的了！！
看来问题的所在就是`"image/*"`

但是写`accpet`的原意是要想要筛选出所有图片_(:з」∠)_
那么为了实现这个需求，同时提高用户体验，只能采取枚举了

修改后的代码
```javascript
<input
    style={inputStyle}
    ref={c=> this._imgFile = c}
    onChange={this.handleChange.bind(this)}
    type="file" name="image" disabled={disabled}
    accpet="image/gif,image/png,image/jpeg,image/jpg,image/bmp"
/>
```
再试试，果然妥妥的了！

但是到底是为什么会这么卡呢？？我查了查万能的Stack Overflow→_→

原来是因为Chrome的{% link SafeBrowsing http://safebrowsing.google.com/  SafeBrowsing %}功能会在上传或保存时检查文件，
如果网络连接到google的速度比较快呢，就没有什么问题。
但是如果连接比较慢，或者干脆跪掉了，那SafeBrowsing就会让Chrome挂起一段时间，直到文件检查结束或者超时

使用`accept="image/png, image/jpeg, image/gif"`就可以解决这个问题，因为这些MIME类型在SafeBrowsing的白名单里面，不需要检查。
但是如果用像是`accept="image/*"`这样的呢，就不行了，就有可能变得卡卡的。


