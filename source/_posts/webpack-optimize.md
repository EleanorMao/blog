---
title: webpack打包速度优化
date: 2017-09-11 13:38:24
tags:
- javascript
- webpack
---

看了一些文章，还有实际使用中，总结一下

* webpack1的可以使用happypack
* 其他可以尽量升级webpack和打包用的依赖项升级到最高版本
* 删除废弃的引用包
* 不要为了小功能使用大号的依赖
* 不要同时使用 babel-runtime 和 babel-polyfill
* 慎用css-module
* DedupePlugin, OccurrenceOrderPlugin(npm2&webpack1)
<!-- * commonsChunkPlugin增加minChunks参数配置 -->
* external, exclude, resolve.modules
* dllPlugin(和commonPlugin二选一说)
* webpack-uglify-parallel多核并行压缩
* -json 分析打包状况后，再用PrefetchPlugin优化(但是效果一般)(webpack-visualizer-plugin分析用)

