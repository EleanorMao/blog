---
title: require别名(alias)HACK解析
date: 2017-09-03 22:49:47
header-img: "modulejs.png"
tags:
- javascript
- node
---
# 前言
上一篇中我们已经探讨了一下如何在node之中使用别名require模块，里面提到了几种HACK方法，这里来解释一下是通过什么原理hack的。

# module模块
其实吧，读过module的源码就懂了…因为require是靠module模块实现的，所以几种hack方式都是依靠hack module模块来搞的。
因为很多人都有分享过module源码的解析，所以下面就不讲累赘的内容了
（以下用的代码来自于node6.5.0，包包只看了下6.x及以上的代码）

## _initPaths
可以看到源码中，首先Module模块首先运行了`_initPaths`方法
{% asset_img initPaths.png node.js %}
{% codeblock module.js lang:javascript https://github.com/nodejs/node/blob/master/lib/module.js%}
Module._initPaths = function() {
  const isWindows = process.platform === 'win32';

  var homeDir;
  if (isWindows) {
    homeDir = process.env.USERPROFILE;
  } else {
    homeDir = process.env.HOME;
  }

  var paths = [path.resolve(process.execPath, '..', '..', 'lib', 'node')];

  if (homeDir) {
    paths.unshift(path.resolve(homeDir, '.node_libraries'));
    paths.unshift(path.resolve(homeDir, '.node_modules'));
  }

  var nodePath = process.env['NODE_PATH'];
  if (nodePath) {
    paths = nodePath.split(path.delimiter).filter(function(path) {
      return !!path;
    }).concat(paths);
  }

  modulePaths = paths;

  // clone as a read-only copy, for introspection.
  Module.globalPaths = modulePaths.slice(0);
};
{% endcodeblock %}
这段代码中，把全局node路径和环境变量中的`NODE_PATH`和当前app的node路径收集在了一起，
在后来查找文件位置的时候会从这些路径查找。

知道了它会使用`NODE_PATH`，所以可以设定`NODE_PATH`来缩短一大堆`../../`

（其实我觉得如果可以修改`modulePaths`变量就更好了，可惜是个变量，而且`Module.globalPaths`这个变量根本没在用，摔！）

## require
require模块用的方法就是：
{% codeblock module.js lang:javascript https://github.com/nodejs/node/blob/master/lib/module.js%}
Module.prototype.require = function(path) {
  assert(path, 'missing path');
  assert(typeof path === 'string', 'path must be a string');
  return Module._load(path, this, /* isMain */ false);
};
{% endcodeblock %}
因此可以，拦截这个方法，然后修改传入的path
如：
```javascript
// 假设我们想要这样简写 require('@/abc.js')
var nodePath = require('path')
var _old_require = Module.prototype.require

Module.prototype.require = function(path) {
    if(path.startsWith('@')){
        path = nodePath.join(__dirname, path.replace(/^@/, ''))
    }
    return _old_require(path)
}
```

## _findPath
因为`Module.prototype.require`调用了`Module._load`方法，`Module._load`方法调用了`Module._resolveFilename`方法，
然后通过`Module._findPath`方法获取模块地址，如果没有地址就会报错，所以可以hack`_findPath`方法（就是node6.x&以上才有这个方法）
{% asset_img findPath.png node.js%}
可以看到如果request变量(其实就是你写的require的路径)是绝对路径，paths就是`['']`，<br>
也就是说直接指定了模块了路径，可以省去下面一大堆和预设好的path拼来拼去的过程了，不过给paths增加可选的路径也是个方法
不过我比较怕懒，所以我直接就把路径拼成绝对路径：
```javascript
// 假设我们想要这样简写 require('@/abc.js')
var nodePath = require('path')
var _old_findPath = Module._findPath

Module._findPath = function(request, paths, isMain){
    if(request.startsWith('@')){
        request = nodePath.join(__dirname, path.replace(/^@/, ''))
    }
    return _old_findPath(request, paths, isMain)    
}
```

## _nodeModulePaths + _resolveFilename
是的还有这种组合技，不过我觉得实在比较麻烦 = =
代码可以看这里[module-alias](https://github.com/ilearnio/module-alias/blob/master/index.js)
因为`Module._nodeModulePaths`只会在`Module._resolveFilename`成功之后和没有parent的时候调用，关改这个还不管用，

修改`Module._resolveFilename`其实单用也可以，可以把传入的路径修改为绝对路径，然后`Module._findPath`跑的时候就可以直接用了，其实和修改`Module._findPath`一样


# 最后
我用的方法是修改`_findPath`方法做到的，其中还添加了缓存什么的<br>
这里安利一下自己的插件[`node-require-alias`](https://www.npmjs.com/package/node-require-alias)<br>
支持node6.0以上<br>
使用方法：
```javascript
const path = require("path")
require('node-require-alias').setAlias({
    "@": path.join(__dirname, "this/is/a/path")
})
// or require('node-require-alias').setAlias("@", path.join(__dirname, "this/is/a/path"))
```    
require模块时候↓
```javascript
require('@/abc.js')
```

欢迎来PR(づ￣3￣)づ╭❤～

