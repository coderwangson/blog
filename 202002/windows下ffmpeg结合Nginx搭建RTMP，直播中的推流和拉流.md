# windows下ffmpeg结合Nginx搭建RTMP，直播中的推流和拉流  

## 推流和拉流的概念 以及RTMP协议  

在直播中，一般需要三个角色：主播，服务器，用户。主播通过推流将数据推到服务器上，而用户可以通过拉流的方式将视频流拉去下来，这个过程如下所示： 

![](https://notecdn.yiban.io/cloud_res/159578458/imgs/20-2-11_11:46:24.941_40455.png)  

而推流和拉流的过程中需要将视频转为视频流，并且进行同步控制，这里就需要使用RTMP协议。RTMP 协议 Real Time Message Protocol（实时信息传输协议）的首字母缩写，是由 Adobe 公司开发的一种用于解决多媒体数据传输流多路复用和分包的网络协议。  

我们做应用的话，可以使用`Nginx-Rtmp`，这个就相当于图片中间的部分。  

## 结合Nginx-Rtmp的直播  

像上面说的，如果进行直播，我们需要三个方面：主播，服务器，用户。就像斗鱼有直播伴侣，斗鱼的服务器，以及提供一个web界面共用户访问。这里主播我们采用ffmpeg进行视频推流，而服务器则使用`Nginx-Rtmp`进行视频流处理，并提供一个用户拉流的接口。  

### 软件安装  

ffmpeg这里不详细讲了，Nginx-rtmp可以把 https://github.com/illuspas/nginx-rtmp-win32 这个项目clone下来，直接运行`nginx.exe` 即可。  

### ffmpeg推流  

我们运行`nginx.exe` 之后，其实是启动了一个服务（类似于tomcat一样），所以你可以在 http://localhost:8080/ 下访问。注意要开启flash插件。  

![2020-02-11_195934.jpg](http://ww1.sinaimg.cn/large/005Dd0fOly1gbsppkyj19j30s90iywkd.jpg)  

在这个服务运行之后，我们就可以利用ffmpeg向上面推流了，其实有很多专业的软件可以推流的，比如OBS等。不过这里为了编程需要，就使用了ffmpeg。在命令行输入

`ffmpeg -re -i a.mp4 -vcodec libx264 -acodec aac -f flv rtmp://localhost:1935/live/room`  

我们就可以将a.mp4推送到nginx上面了，这个命令里面`rtmp://localhost:1935/live/room`  就是nginx服务的地址，其中`rtmp://localhost:1935/live/` 中端口什么的都可以设置，具体参考nginx-Rtmp的相关文档，而room可以随便设，主播推流到什么位置，则客户需要从哪个位置拉流即可。  

如果推送成功的话，这个窗口会一直在不停的更新frame，代表持续推送帧到服务器。  


![2020-02-11_200732.jpg](http://ww1.sinaimg.cn/large/005Dd0fOly1gbspw6bv70j30lk08y7al.jpg)  

当我们推流结束，我们就可以拉流了，可以通过KMPlayer或者VLC这类支持流播放的播放器播放。输入推流的那个地址`rtmp://localhost:1935/live/room`，即可播放。  

如果你需要嵌入到网页里面，提供一个web界面，则可以使用video.js进行播放，代码如下：  

```
<!DOCTYPE html>
<html lang="en">
<head>

    <title>Video.js | HTML5 Video Player</title>
    <!-- <link href="video-js-6.2.0/video-js.css" rel="stylesheet">
    <script src="video-js-6.2.0/videojs-ie8.min.js"></script> -->

    <link href="http://vjs.zencdn.net/5.5.3/video-js.css" rel="stylesheet">
    <script src="http://vjs.zencdn.net/ie8/1.1.1/videojs-ie8.min.js"></script>

</head>
<body>

  <video id="example_video_1" class="video-js vjs-default-skin" controls preload="auto" width="1280" height="720" poster="http://vjs.zencdn.net/v/oceans.png" data-setup="{}">
    <!-- <source src="1.mp4" type="video/mp4"> -->
    <source src="rtmp://localhost:1935/live/room" type="rtmp/flv">

    <p class="vjs-no-js">To view this video please enable JavaScript, and consider upgrading to a web browser that <a href="http://videojs.com/html5-video-support/" target="_blank">supports HTML5 video</a></p>
  </video>

  <script src="http://vjs.zencdn.net/5.5.3/video.js"></script>
</body>

</html>

```

