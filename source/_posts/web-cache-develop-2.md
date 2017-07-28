---
title: WEB缓存探究第二弹——实战
date: 2017-07-27 10:44:59
tags:
- javascript
- 前端优化
- HTTP
- 缓存
---

## 前言
{% post_link web-cache-explore WEB缓存探究第一弹%}中我们讲了一些WEB缓存的基础知识和策略。
第二弹我们来讲讲如何实际在项目中配置。

## 实战
鉴于叉烧包本包是个前端，所以我们就以HTML和Node为例开始{% asset_img heywego.jpg hey-we-go %}

### HTML——在header中加入meta标签
***当然根据我的测试发现这种方式好像并没有什么卵用***
这段代码代表的是不需要浏览器缓存
```html
<header>
    <meta http-equiv="Cache-Control" content="no-cache" /> <!-- HTTP 1.1 -->
    <meta http-equiv="Pragma" content="no-cache" /> <!-- 兼容HTTP1.0 -->
    <meta http-equiv="Expires" content="0" /> <!-- 资源到期时间设为0 -->
</header>
```

### Node.js——Express
鉴于Express2.x和3.x已经是deprecated，所以此处以Express4.x为例。
#### HTML
在{% post_link web-cache-explore WEB缓存探究第一弹%}定制缓存策略中已经提到对于HTML最好标记为不缓存，以便及时获取最新的静态资源版本。
通常我们在Express中渲染HTML会用到以下类似的代码👇
``` javascript res.render http://expressjs.com/en/4x/api.html#res.render Express
//当访问/index时，渲染模板index到页面
router.get('index', (req, res)=>{
    res.render('index');
});
```

在这时我们可以使用[`res.set(field[,value])`](http://expressjs.com/en/4x/api.html#res.set)或者它的别名`res.header(field [, value])`为HTML设置Header。
此时代码如下：
``` javascript res.set http://expressjs.com/en/4x/api.html#res.set Express
router.get('index', (req, res)=>{
    res.set('Cache-Control', 'no-cache;max-age:0').render('index');
    /*
        或者  res.header('Cache-Control', 'no-cache;max-age:0').render('index');
        或者  res.set({'Cache-Control':'no-cache', 'max-age':0}).render('index');
    */
});
```
也可以使用中间件的方式批量为请求加上需要的头：
```javascript
app.use((req, res, next) => {
  res.set('Cache-Control', 'no-cache;max-age:0');
  next();
})
```
效果如下：
{% asset_img express1.png Express %}

不过细心的小伙伴应该已经发现了，
{% asset_img express2.png Express %}
没错机智的Express已经为我们加上了`ETag`😂

让我们来复习一下第一弹的知识点，`Etag`资源的验证令牌，如果指纹变化请求时则会重新下载资源，否则则不会。

可能有的人就问了，那我还需要给HTML加上`Cache-Control`吗？

当然仅用`ETag`来控制资源是否缓存和更新是合理的，不过我的意见是，如果明确不缓存该资源，最好还是要加上`Cache-Control`。

#### 静态资源
Express发布静态资源通过的是[`express.static(root, [options])`](http://expressjs.com/en/4x/api.html#express.static)方法。
```javascript
app.use(express.static(path.join(__dirname, 'public')));
```
它的options参数可以配置header参数
{% asset_img express3.png Express %}
静态资源我们最好是为他加上一个超长的过期时间，像这样
```javascript express.static http://expressjs.com/en/4x/api.html#express.static Express
//作为Exprss参数的maxAge的单位是毫秒，但是在header中单位是秒！不要搞错哦
app.use(express.static(path.join(__dirname, 'public'), {
  maxAge: 3153600000,
  setHeaders: (res, path, stat) => {
    res.set({'Expires': new Date('2100-12-12')})
  }
}));
//如果需要分别为资源设置头，可以使用多个`express.static`管理
//或者在`setHeaders`函数中通过判断`path`后缀分别加上不同的头
//当然有更靠谱的方法欢迎联系我 >w< 
```
效果如下：
{% asset_img express4.png Express %}

**不过不要忘记给静态资源文件名加上指纹哦**


### Nginx

同理，就不在重复叙述了，只写一下配置

不过同时设置`expires`和`add_header Cache-Control`会在请求中出现复数的`Cache-Control`，但HTTP1.1能够识别它。
```nginx
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
    etag off;   #关闭etag
    expires 1d; #有效期一天 
    # expires的单位可以使用
    # ms: 毫秒
    #  s: 秒
    #  m: 分钟
    #  h: 小时
    #  d: 天
    #  w: 星期
    #  M: 月 (30 天)
    #  y: 年 (365 天)
}

location ~ .*\.css$ {
    expires 1y; #有效期一年
    add_header Cache-Control public; #cache-control设为public
}


location ~ .*\.js$ {
    expires 1y; #有效期一年
    add_header Cache-Control private; #cache-control设为private
}
```