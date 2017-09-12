---
title: 用canvas绘制视频(然而并没有什么卵用)
date: 2017-09-12 10:25:38
pages: demo
---
[参考](https://developer.mozilla.org/en-US/Apps/Fundamentals/Audio_and_video_delivery/Cross-browser_audio_basics)<br>
亲测在ip6上会全屏播放，并没有什么卵用<br>
据说在更之前的浏览器(大概13年及之前那波)一定要把video渲染在dom里面才能用<br>
如果想获取更高帧频，应考虑用raf方法替换timeupdate方法<br>

<div>
    <button id="button">播放</button>
    <br/>
    <canvas id="player" width="500" height="300" style="background: #000">canvas</canvas>
</div>
<script>
function playVideo() {
    if( video.ended || video.paused ) return;
    context.drawImage(video, 0, 0, player.width, player.height);
    console.log('requestAnimationFrame: '+ video.currentTime);
    requestAnimationFrame(playVideo);
};
var player = document.querySelector("#player");
var context = player.getContext("2d");
var button = document.querySelector("#button");
var video = document.createElement("video");
video.addEventListener("loadeddata", function(){
    console.log("loaded video data!");
    //player.width = video.videoWidth;
    //player.height = video.videoHeight;
});
video.addEventListener("timeupdate", function(event){
    console.log("timeupdate: " + video.currentTime);
    button.innerText = "播放";    
});
video.preload = "auto";
video.src = "http://gslb.miaopai.com/stream/acqw7E1g2UQz3vvfwTkwPUH1xpI07LAbzgm-Iw__.mp4?ssig=fc558e33c84a0723f0e3000b21287c8a&time_stamp=1505187251951&cookie_id=&vend=1&os=3&partner=1&platform=2&cookie_id=&refer=miaopai&scid=acqw7E1g2UQz3vvfwTkwPUH1xpI07LAbzgm-Iw__";
button.addEventListener("click", function(){
    if ( video.paused ) {
        button.innerHTML = "暂停";
        video.play();
        requestAnimationFrame(playVideo);
    } else {
        video.pause();
        button.innerHTML = "播放";
    }
});
</script>

```html
<button id="button">播放</button>
<br/>
<canvas id="player" width="500" height="300" style="background: #000">canvas</canvas>
```

```javascript
var player = document.querySelector("#player");
var context = player.getContext("2d");
var button = document.querySelector("#button");
var video = document.createElement("video");
video.addEventListener("loadeddata", function(){
    console.log("loaded video data!");
    //player.width = video.videoWidth;
    //player.height = video.videoHeight;
});
video.addEventListener("timeupdate", function(event){
    console.log("timeupdate: " + video.currentTime);
    button.innerText = "播放";    
});
video.preload = "auto";
video.src = "http://gslb.miaopai.com/stream/acqw7E1g2UQz3vvfwTkwPUH1xpI07LAbzgm-Iw__.mp4?ssig=fc558e33c84a0723f0e3000b21287c8a&time_stamp=1505187251951&cookie_id=&vend=1&os=3&partner=1&platform=2&cookie_id=&refer=miaopai&scid=acqw7E1g2UQz3vvfwTkwPUH1xpI07LAbzgm-Iw__";
button.addEventListener("click", function(){
    if ( video.paused ) {
        button.innerHTML = "暂停";
        video.play();
        requestAnimationFrame(playVideo);
    } else {
        video.pause();
        button.innerHTML = "播放";
    }
});

function playVideo() {
    if( video.ended || video.paused ) return;
    context.drawImage(video, 0, 0, player.width, player.height);
    console.log('requestAnimationFrame: '+ video.currentTime);
    requestAnimationFrame(playVideo);
}
```

