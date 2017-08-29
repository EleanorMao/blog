---
title: 八月问题单总结
date: 2017-08-28 10:43:29
tags:
- javascript
- HTTP
---
最近都没在更博_(:з」∠)_ 但是来总结一下这段时间（我还记得的）发生过的问题

# throw errnoException(err, 'spawn'); Error: spawn EACCES
这是有个项目一直只在公司的win上面开发，虽然是用git管理的，但是最近一次有急需才clone到mac上来继续开发，
然后打包项目的时候就遇到了这个报错。<br>
总之，发现其实是权限的问题，<br>
**解决方案**<br>
在项目的root路劲下跑一下这个，把权限调整好就可以了
```
chmod -R u+x .
```
# HTTP code 206 Partial Content 
是的，那是一天的半夜……，后端的小组长跑来被自动化测试要求发环境，结果发了几次都是挂的，<br>
然后跑来问我是怎么回事。<br>
虽然乍一看静态资源是`200`了，但是仔细一看其实是`206`。<br>
然后仔细一看资源的`Content-Type`是1，然后居然有了个`Content-Range`，而且数字是个很微妙的东西<br>
{% asset_img content-range.jpg Content-Range %}
顺便一提↓<br>
{% blockquote  MDN web docs  https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Range  Content-Range%}
Content-Range 显示的是一个数据片段在整个文件中的位置。
{% codeblock Content-Range https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Range MDN web docs %}
Content-Range: <unit> <range-start>-<range-end>/<size>
Content-Range: <unit> <range-start>-<range-end>/*
Content-Range: <unit> */<size>
{% endcodeblock %}
{% endblockquote %}
而且控制台静态资源报错`ERR_CONTENT_LENGTH_MISMATCH`
然后在[Stack Overflow](https://stackoverflow.com/search?q=ERR_CONTENT_LENGTH_MISMATCH)上搜了一波，好多人说是nginx的权限问题。<br>
不过最后一查，是因为磁盘满了=。=

# try-catch
还有一个是因为最外层的try-catch把error都catch住了，导致报错没有被发现，造成了一个线上bug<br>
不过我还蛮佩服我自己的，因为这么写已经大半年了，这个缘故的bug还是第一次……原来我都一直没有写出过bug呜呜呜呜<br>
总之来说是这样的，在外层渲染dom的时候用了这样一段代码<br>
{% codeblock lang:javascript %}
try {
    ReactDOM.render(<Foo />, document.querySelector('.root'))
}catch(e){}
{% endcodeblock %}
结果我在Foo这个类里面调用了一个某引入组件没有的方法，这个error就被catch……我还找了好久为啥都不报错=L=<br>
不过小钻风说把source里面的`Pause on exceptions`打开就可以抓住这种奇奇怪怪的bug了

# 微信里的transitionEnd事件
在微信里面调用了`transitionEnd`事件，但是ios8里面并没有被触发。<br>
最后谷歌之后，发现说是因为只有`transition-property`是all的时候才能一定出发`transitionEnd`事件……<br>
吐血<br>
我确实`transition-property`写的是opacity，为了兼容只能忍辱负重了……抹眼泪<br>

# 微信定位
这又是一个神坑 =L=<br>
产品发现部分手机微信定位会蜜汁失败，然后跑来找我们，不过并没有说是什么环境，只有个小视频……而且小视频最后还被删掉了(吐血)<br>
结果最后又变成在盲找bug<br>
先说我们的流程，就是先由后端获取定位，如果500的话，再调用JS-SDK定位，如果再没有位置或失败的话就告诉用户定位失败了。<br>
已知的可能定位失败就是安卓如果在公众号内没有停留超过5s的话就会定位不到，因为微信定位5s推送一次，还有就是关掉或打开定位不会马上定位失败/成功，必须经过一段时间（目测是有缓存）<br>
然后我们架构师就表示怎么可能定不到位……非要叫我们搞定，心塞的要死😑最后是能一种种试过去<br>
经过各种试验，可以确定ios关了定位肯定就定不到了，安卓开了定位也有可能订不到，但是关了调用`navigator.geolocation`还能定到，完了这个定位看网络情况可能要好久才能定位到<br>
过程略，总之最后就是决定改成后端定位返回附近地址和最近收货地址，然后如果没有附近地址就先JS-SDK定位，如果失败再调用`navigator.geolocation`，如果也失败的话再使用收货地址，如果再没有再提示没有。

