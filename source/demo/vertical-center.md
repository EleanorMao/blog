---
title: 垂直居中demo
date: 2017-05-15 13:06:19
pages: demo
---
<div style="height: 200px; line-height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px">
    <span>文字的垂直居中</span>
</div>
```html
<div style="height: 200px; line-height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px">
    <span>文字的垂直居中</span>
</div>
```
<div style="height: 200px; line-height: 200px; text-align: center;font-size: 0; border: 1px solid #ddd; margin: 10px">
    <span style="font-size: 12px; display: inline-block; line-height: 1.5; vertical-align: middle">
        多行文字垂直居中多行文字垂直居中多行文字垂直居中<br>
        多行文字垂直居中多行文字垂直居中多行文字垂直居中<br>
        多行文字垂直居中多行文字垂直居中多行文字垂直居中多行文字垂直居中
    </span>
    <span style="display: inline-block; vertical-align: middle"></span>
</div>
```html
<div style="height: 200px; line-height: 200px; text-align: center;font-size: 0; border: 1px solid #ddd; margin: 10px">
    <span style="font-size: 12px; display: inline-block; line-height: 1.5; vertical-align: middle">
        多行文字垂直居中多行文字垂直居中多行文字垂直居中<br>
        多行文字垂直居中多行文字垂直居中多行文字垂直居中<br>
        多行文字垂直居中多行文字垂直居中多行文字垂直居中多行文字垂直居中
    </span>
    <span style="display: inline-block; vertical-align: middle"></span>
</div>
```
<div style="height: 200px; width: 500px; max-width: 500px; text-align: center; display: table-cell; vertical-align: middle; border: 1px solid #ddd;">
    <p>多行文字</p>
    <p>多行文字</p>
    <p>多行文字</p>
    <p>多行文字</p>
</div>
```html
<div style="height: 200px; width: 500px; max-width: 500px; text-align: center; display: table-cell; vertical-align: middle; border: 1px solid #ddd;">
    <p>多行文字</p>
    <p>多行文字</p>
    <p>多行文字</p>
    <p>多行文字</p>
</div>
```
<div style="height: 200px; width: 100%; display: table;border: 1px solid #ddd; margin: 10px">
    <div style="display:table-cell;vertical-align:middle;text-align:center">
        <div style="margin-left:auto;margin-right:auto; width: 50px; height: auto; background: pink">
            不定高不定宽垂直居中
        </div>
    </div>
</div>
```html
<div style="height: 200px; width: 100%; display: table;border: 1px solid #ddd; margin: 10px">
    <div style="display:table-cell;vertical-align:middle;text-align:center">
        <div style="margin-left:auto;margin-right:auto; width: 50px; height: auto; background: pink">
            不定高不定宽垂直居中
        </div>
    </div>
</div>
```
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="margin-left: -25px; margin-top: -25px; position: absolute; top: 50%; left: 50%; width: 50px; height: 50px; background: pink">
        定高定宽垂直居中
    </div>
</div>
```html
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="margin-left: -25px; margin-top: -25px; position: absolute; top: 50%; left: 50%; width: 50px; height: 50px; background: pink">
        定高定宽垂直居中
    </div>
</div>
```
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="margin: auto; position: absolute; top: 0; left: 0; right: 0; bottom: 0;width: 50px; height: 50px; background: pink">
        定高定宽垂直居中
    </div>
</div>
```html
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="margin: auto; position: absolute; top: 0; left: 0; right: 0; bottom: 0;width: 50px; height: 50px; background: pink">
        定高定宽垂直居中
    </div>
</div>
```
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="width: 50px; background: pink; position: absolute; top: 50%;  transform: translate3d(-50%, -50%, 0); left: 50%">
        不定高定宽垂直居中
    </div>
</div>
```html
<div style="height: 200px; text-align: center;border: 1px solid #ddd; margin: 10px; position: relative">
    <div style="width: 50px; background: pink; position: absolute; top: 50%;  transform: translate3d(-50%, -50%, 0); left: 50%">
        不定高定宽垂直居中
    </div>
</div>
```
<div style="display:flex;justify-content:center;align-items:center;width:100%;height:200px;border: 1px solid #ddd; margin: 10px;">
    <div>flex垂直居中</div>
</div>
```html
<div style="display:flex;justify-content:center;align-items:center;width:100%;height:200px;border: 1px solid #ddd; margin: 10px;">
    <div>flex垂直居中</div>
</div>
```