
## 为什么要有这个项目?

在安卓手机上，使用video播放视频有个问题，video控件层级会永远在顶层，不利于视频互动H5开发，而IOS手机上不会有此问题。

腾讯给出了如下解决方案：
```html
  <video src="http://xxx.mp4" x5-video-player-type="h5"/>
```
但使用如上属性，视频播放时会全屏并将top bar挤掉（没试过的可以自行测试下），且只适用于微信
[点此跳到腾讯相关开发说明](https://x5.tencent.com/tbs/guide/video.html)

官方的方法走不通，其实可利用jsmpeg插件解决,
jsmpeg是一个能将视频文件转为canvas播放的插件，详情可[点击jsmpeg传送门](https://github.com/phoboslab/jsmpeg)了解
于是有了这个项目

## 如何使用

使用jsmpeg插件需要将视频mp4文件用ffmpeg转成.ts文件，ffmpeg可自行在[ffmpeg官网下载](http://ffmpeg.org/)
转换代码为（命令行）：
```html
    ffmpeg -i video.mp4 -f mpegts -codec:v mpeg1video -codec:a mp2 out.ts
```
或自行加入并设置参数（参数设置可查看ffmpeg教程）：
```html
    ffmpeg -i video.mp4 -f mpegts -codec:v mpeg1video -b:v 1000k -r 30 -bf 0 -codec:a mp2 -ar 44100 -ac 1 -b:a 128k out.ts
```
然后js中调用JSMpeg插件
```javascript
  var player = new JSMpeg.Player('video.ts', {canvas: canvas, loop: true, autoplay: true});  
```

## 在线演示DOMO

http://wx.karlew.com/tongceng/

## 注意

此项目中安卓上使用jsmpeg插件渲染canvas，ios上正常使用video并加入隐藏控制条等设置。

要注意的是，不要替换demo上的jsmpeg.js，因为demo的jsmpeg.js加入了视频开始播放、结束和播放进度的监听回调，而原作者的没有回调设置
