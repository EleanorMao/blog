---
title: 使用node时为require设置别名(alias)
date: 2017-09-03 15:35:33
tags:
- node 
- javascript
- 翻译
---
# 前言
由于本包包是个很懒惰的人，然后我们有些个项目设计的不是很好，所以导致写代码的时候有很多这样的代码:
```javascript
require('../../../../../../foo.js');
```
写的时候数那个小点点感觉人都要死了😤<br>
这种时候如果写node能像用了`webpack`(and so on)那样能够`require`别名就好了。
比如这样:
```javascript
require('modules/foo.js');
```
于是我搜寻了几种方法。

# 来自[branneman](https://gist.github.com/branneman)总结的方法
**以下内容来自于github上一名叫做branneman的同志的总结，我为他的内容进行了随性的翻译**

原文可查看[这里](https://gist.github.com/branneman/8048520)

## 1. Symlink
"偷"自[focusaurus / express_code_structure # the-app-symlink-trick](https://github.com/focusaurus/express_code_structure#the-app-symlink-trick)

1. 在应用的`node_modules`文件夹下面创建一个`symlink`
* Linux: `ln -nsf node_modules app`
* Windows: `mklink /D app node_modules`
（耍的时候请把app换成你要复制的模块的路径）

然后你就可以
```javascript
var Article = require('app/article');
```
**小贴士**：由于git不能处理跨平台的symlinks，所以你不能再git repo里面用这样的文件。不过如果你是在克隆后、git-hook或者是由开发人员手动创建一个symlink，那就没啥问题

另外，你可以在npm里面里面增加一条postinstall钩子,这个方法由[scharf](https://gist.github.com/branneman/8048520#comment-1412502)提出。可以把命令加进`package.json`里面
```json
"scripts": {
    "postinstall" : "node -e \"var s='../src',d='node_modules/src',fs=require('fs');fs.exists(d,function(e){e||fs.symlinkSync(s,d,'dir')});\""
  }
```
## 2. 全局变量
在你的app里面增加
```javascript
global.__base = __dirname + '/';
```
这样你就可以这样用了
```javascript
var Article = require(__base + 'app/models/article');
```

## 3. 使用别人开发的库
（这里就不翻了，npm搜一搜）

## 4. 环境变量
设置环境变量`NODE_PATH`成一个指向你app的绝对路径，用你想用的模块的相对路径结尾（作者的情况下是.）。
然后就可以
```javascript
var Article = require('app/models/article');
```
### 4.1 提前设置
在启动app前线确保已经设置好了环境变量
* Linux: `export NODE_PATH=.`
* Windows: `set NODE_PATH=.`
使用`export`和`set`是仅对当前shell有效的，如果你需要让他全局、永久的改变，需要修改你的配置文件

### 4.2 只在执行node时设置
这个方法不会影响你的环境，除非node运行。他需要你改变应用启用命令。

像这样启动你的app
* Linux: `NODE_PATH=. node app`
* Windows: `cmd.exe /C "set NODE_PATH=.&& node app"`
（在win下面如果你在path和&&之间加空格的话就启动不了）

## 5. 启用脚本
其实和4.2差不多<br>
其实就是写成一个脚本来运行，不过这样比较方便加各种参数

例：
* Linux: `./app` (Windows PowerShell可)
* Windows: `app`

### 5.1 Node.js
看[这里](https://gist.github.com/branneman/8775568)
代码具体是这样
{% blockquote branneman https://gist.github.com/branneman/8775568 %}

{% codeblock lang:javascript %}
#!/usr/bin/env node

'use strict';
hquote
var spawn = require('child_process').spawn;

var args = [
  '--harmony',
  'app/bootstrap.js'
];

var opt = {
  cwd: __dirname,
  env: (function() {
    process.env.NODE_PATH = '.'; // Enables require() calls relative to the cwd :)
    return process.env;
  }()),
  stdio: [process.stdin, process.stdout, process.stderr]
};

var app = spawn(process.execPath, args, opt);
Raw
{% endcodeblock %}

{% endblockquote %}

### 5.2 操作系统特定的启动脚本
* Linux
可以创建一个app.sh    
```bash
#!/bin/sh
NODE_PATH=. node app.js
```

* Windows
可以创建一个app.bat
```bat
@echo off
cmd.exe /C "set NODE_PATH=.&& node app.js"
```

# 6. Hack
谢谢[@joelabair](https://github.com/joelabair)和4.2差不多，但不需要在app之外指定NODE_PATH。但是，由于这依赖于一个专用的Node.js核心方法。╮(╯_╰)╭

你需要在`require`之前运行这个方法。
```javascript
process.env.NODE_PATH = __dirname;
require('module').Module._initPaths();
```

# 7. 包裹
感谢[@a-ignatov-parc](https://github.com/a-ignatov-parc)
简单来说你可以在你的app最开始运行这样的代码。
```javascript
global.rootRequire = function(name) {
    return require(__dirname + '/' + name);
}
```
然后就可以这样
```javascript
var Article = rootRequire('app/models/article');
```

# 叉烧包总结
基于我的原则一向来就是越快越好，打字越少越好，所以个人认为
* symlink的方式由于各种限制而且你增加一点你的包就得新生产一个，感觉比较的不爽，pass
* 增加全局变量，但是还需要自己拼接一下，难受，pass
* 使用别人开发的库就见仁见智啦~因为我比较作喜欢自己写，所以pass
* 环境变量，我比较能接受写一份脚本的方法 0-0
* Hack方法，这个我比较的喜欢，然后我看了`_initPaths`的代码，我觉得启动的时候也可以这样`NODE_PATH=. node app`
* 包裹方法在require时比较难受，我喜欢能直接`require`…so

# 包子的方法
## 1. 修改启用命令
在启用时这样操作`NODE_PATH=. node app`。
不过得考虑到不懂操作系统下面path分割符的问题

## 2. HACK
我hack了`_findPath()`方法，这个就是想设成啥别名就啥别名w，只不过只能兼容到node6
具体可以看[这里](https://github.com/EleanorMao/require-alias/blob/master/index.js)

# 其他人的方法
* hack了`require`方法，这个和我想的hack差不多，但是这样兼容性比较好[code](https://github.com/sultan99/sexy-require/blob/master/index.js)
* 还有hack了`export`方法的，通过`Object.defineProperty`，在get方法时给模块的添加必要的属性
* [babel-plugin-resolver](https://github.com/jshanson7/babel-plugin-resolver)

# 最后的总结
我还是比较喜欢HACK！不过我觉得加环境变量也很快，也不用管兼容b(￣▽￣)d