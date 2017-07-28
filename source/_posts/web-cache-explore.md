---
title: WEB缓存探究
date: 2017-07-26 13:06:44
tags:
- javascript
- 前端优化
- HTTP
- 缓存
---

## 前言
由于项目越来越大，即使了使用代码压缩工具减少文件大小，js文件还是不可避免的越变越大。
而对于用户来说每次重新下载都有可能会消耗大量时间，让我们的首屏展示有较长时间的空白。
为了提升网站性能，有效利用缓存能够提升用户体验，提高访问效率。

## 浏览器缓存
### HTML中的Meta标签
http-equiv属性，相当于http的文件头中的参数，而content的内容则是对应参数的值
```html
<!-- 告诉浏览器不缓存当前页面 -->
<meta http-equiv="pragma" content="no-cache">
```
然而设置`pragma: no-cache`并不能应用于HTTP1.1及以上规范，
而且因为这个方法太老了，如果你不需要估计那些史前客户的感受，完全可以不加👆

当然可以不用太方，还有其他的参数可以选择使用
```html
<meta http-equiv="Cache-Control" content="no-cache" /> <!-- HTTP1.1 在1.1中优先于expires-->
<meta http-equiv="pragma" content="no-cache" /> <!-- HTTP1.0 -->
<meta http-equiv="Expires" content="0" /> <!-- 示意到期时间 HTTP1.0 & 1.1 -->
```

但是使用meta标签设置的参数优先级低于http请求中声明的，如果你同时设置了http头，那么就没有必要加上meta标签了。

**当然，最后还有一个重要的一点，就是根据叉烧包的实验，meta制定这些内容可以说基本没有什么卵用:)**
**悲伤的故事……当然可能你的浏览器还可以用哦**

### Header参数
最保险的显然是配置Header参数来保证资源的缓存

1. **Cache-Control**
    Cache-Control 标头是在 HTTP/1.1 规范中定义的，取代了之前用来定义响应缓存策略的标头例如 Expires。
    所有现代浏览器都支持 Cache-Control。
    * **max-age**     指从请求的时间开始，允许缓存有效的最长时间(单位是s)
    * **public**      无条件缓存。它不是必须的，因为明确的缓存信息已表示响应是可以缓存的
    * **private**     通常只为单个用户缓存，不允许任何中间缓存对其进行缓存
    * **no-cache**    表示必须先与服务器确认返回的响应是否发生了变化。
    * **no-store**    禁止浏览器以及所有中间缓存存储任何版本的返回响应，每次请求必须重新下载

    借用谷歌爸爸的一张图来展示一下Cache-Control的选择策略
    {% asset_img http-cache-decision-tree.png 最佳Cache-Control策略树 %}
2. **Expires**
   它代表一个缓存过期的绝对时间，在HTTP/1.0中实现，在HTTP/1.1中优先级低于Cache-Control。
   它的缺点就是如果服务器与客户端误差较大，那么它的误差也会变大
3. **Last-Modified**
   标记的是资源的最后修改时间，需要配合Cache-Control使用。只能精确到秒级，如果某些文件在1秒内修改多次，则无法及时更新
4. **ETag**
   相当于验证令牌。通过它可以可实现高效的资源更新检查：资源未发生变化时不会传送任何数据。
   ETag通常是服务器生成的文件内容的哈希值或某个其他指纹。如果请求时指纹仍然相同，则表示资源未发生变化，则可跳过下载。

#### 参数弃用小指南
* 如果你不考虑ie6和`HTTP 1.0`客户端，那么你可以无视`Pragma`
```http
Cache-Control: no-store, must-revalidate
Expires: 0
```
* 如果你也不打算管`HTTP 1.0`代理，那么你可以无视`Expires`
```http
Cache-Control: no-store, must-revalidate
```
* 如果服务器自动包含有效的`Date`标头，则理论上也可以省略`Cache-Control`，并仅依赖于`Expires`。不过如果客户端和服务端时间有差别，就可能会失败哦
```http
Date: Wed, 24 Aug 2016 18:32:02 GMT
Expires: 0
```
* 总的来说还是使用`Cache-Control`最妥妥的(如果不打算考虑`HTTP 1.0`)

## 项目实践
### 更新文件&弃用缓存
在项目中，当我们使用本地缓存后又会遇到另一个问题——如何更新文件、弃用缓存。
通常，我们通过对文件名加入指纹来实现。

以webpack为例，
写配置文件时
```javascript
{
    output: {
        filename: "bundle.[hash].js"
    }
}
```
为打包后的文件名加上hash，使文件更新之后会生成新的hash，以达到弃用原来缓存的效果。

### 定制缓存策略
可以为不同类型的文件定义不同的缓存策略，以达到最高效的结果
1. 将HTML被标记为“no-cache”，使浏览器在每次请求时都始终会重新验证文档，并在内容变化时能够及时获取最新版本，即使下载新资源。
2. 允许浏览器和中间缓存（如CDN）缓存CSS，并将CSS设置为1年后到期，超长的缓存时间可以让用户避免每次都从服务端获取响应。同时不要忘记给文件名加上指纹，以便及时更新改动
3. JavaScript同样设置为1年后到期，但标记为private，因为它可能会包含某些用户私人数据，这是CDN不应缓存的。
4. 图像缓存时不包含版本或唯一指纹，并设置为1天后到期。

### 其他技巧
1. 减少对Cookie的依赖，因为每次HTTP请求都会带上Cookie，这回增大传输流量（当然将静态资源挂载在其他域名下，也可以达到cookie free的效果）

