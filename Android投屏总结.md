#android手机投屏
####导语
>至于手机投屏的实现方法可谓五花八门，今天小袁就说下以开发人员的角度来说下当今手机的主流投屏方法。目前这种将终端信号经由WiFi传输到电视、电视盒的技术有三种：DLNA、AirPlay、Miracast、Google Cast。



##手机投屏智能电视的几种方法
###DLNA
专业术语：DLNA的全称是DIGITAL LIVING NETWORK ALLIANCE(数字生活网络联盟)。
DLNA(Digital Living Network Alliance) 由索尼、英特尔、微软等发起成立、旨在解决个人PC，消费电器，移动设备在内的无线网络和有线网络的互联互通。
DLNA 2003年诞生是为了能够把当时相互独立的电视机及其相关设备（功放、音响、DVD/蓝光播放机）、PC及其相关设备（数码相机、音乐播放器）和移动电话在家庭中有机地整合起来，使得多媒体内容能够在各个设备上便捷地播放。

通俗术语：
DLNA（数字生活网络联盟） 就是大佬们一起搞事情 商讨“我们要将物联网设备连接起来，怎么搞啊？” 搞着搞着 就搞出了这套设备的工业标准。 然后让其他设备具备 DLNA 这种工业标准，这样这些设备就可以互联互通了。
####UPnp协议 
[*DLNA 标准包括多项协议及标准，其中最重要的部分是 UPnP 协议*](https://zh.wikipedia.org/wiki/UPnP)
UPnP是由“通用即插即用论坛”（UPnP Forum）推广的一套网络协议，它的目标是实现设备间网络互联。

* UPnP协议定义了设备之间，设备和控制点，控制点之间通信的协议
* 完整的UPnP协议栈由设备寻址、设备发现、设备描述、设备控制、事件通知以及基于Html的描述等几部分构成


![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-5d771cd8cc9aaa9b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
看到这个图有没有想到 [*OSI 七层模型*](https://blog.csdn.net/yaopeng_2005/article/details/7064869)？
我从下至上来介绍一下：


![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-037012393b0474f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/687)
ip网络协议 大哥哥
IP：就是ip网络协议。
为什么需要它？
因为用到网络层数据的传输，IP层用于数据的发送与接收。
想象一下两个设备互联，其实就是在网络互联，所以需要网络层协议。

![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-e805a99ec55b2b9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/656)
TCP-UDP

这里包含了UDP和TCP两块协议。
这里图应该这样表示：
![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-8360f1050e90e182.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/667)
UDP协议跟IP协议组合成：UDP/IP协议
而 HHTPMU 和 HTPU 这两个协议是基于 UDP/IP 协议之上的。
TCP协议跟IP协议组合成：TCP/IP 协议
HTTP 则是基于 TCP/IP 协议上。

TCP/IP 协议大家应该都了解，但为何出现一个 UDP/IP 呢？
使用UDP，可以通过多点传送（Multicast）向 LAN 上所有支持UPnP的设备发送新设备接入的通知。这里流媒体 也会使用 UDP 来传输，因为速度快。

HHTPMU 和 HHTPU 是什么？
它是构建在 TCP/IP 协议之上的是 HTTP 协议及其变种，所有 UPnP 消息都被封装在 HTTP 协议及其变种中。HTTP 协议的变种是 HTTPU 和 HTTPMU，这些协议的格式沿袭了 HTTP 协议，只不过与 HTTP 不同的是他们通过 UDP 而非 TCP 来承载的，并且可用于组播进行通信。
可见 它之所以存在就是替换 TCP 为 UDP。

接下来就是 设备发现 | 设备控制 | 设备事件 协议了：
1、设备发现 SSDP 协议
简单服务发现协议（Simple Service Discovery Protocol：SSDP）
具体包括控制点如何发现网络上有哪些服务，以及这些服务的资讯，还有控制点本身宣告他提供哪些服务。

2、控制设备 SOAP 协议
简单对象访问协议（Simple Object Access Protocol：SOAP）
它定义如何使用 XML 与 HTTP 来执行远程过程调用。包括控制点如何发送命令消息给设备，设备收到命令消息后如何发送响应消息给控制点。

3、设备事件 GENA 协议
通用事件通知架构（Generic Event Notification Architecture：GENA）
定义在控制点想要监听设备的某个服务状态变量的状况时，控制点如何传送订阅信息并如何接收这些信息。

体系定义
![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-eafd21da73d1ab80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/693)
这一层仅仅是一个抽象的、公用的设备模型。就是一个规范 定义，所有的 UPnP 设备都必须使用这层。
![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-e03be348eab9f829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/695)
UPnP论坛
UPnP 论坛的各个专业委员会的设备定义层，在这个论坛中，不同电器设备由不同的专业委员会定义，例如：电视委员会只负责定义网络电视设备部分，空调器委员会只负责定义网络空调设备部分，依此类推。
![Mou icon](https://upload-images.jianshu.io/upload_images/1677180-6c0ed95b68951aef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/687)
UPnP设备制造厂商定义
也就是应用层，由 UPnP 设备制造厂商定义的部分。
###Miracast
Miracast是由Wi-Fi联盟于2012年所制定，以Wi-Fi直连为基础的无线显示标准。支持此标准的设备可通过无线方式分享视频画面，例如手机可通过Miracast将影片或照片直接在电视或其他装置播放而无需受到连接线缆长度的影响。与DLNA不同的是，Miracast 也有类似于AirPlay 的镜像功能，可以将手机中屏幕内容直接投放到高清电视屏幕里，这样你也可以通过电视屏幕来玩游戏了。Android4.2版本以后系统标配此功能（在设置或显示菜单中可以找到，应用名称诸如：Wlan display、Wifi display、Miracast、Allshare cast、Mirroring screen、无线显示等，只是各手机厂家命名不同而已）。可以将手机屏幕通过无线显示接收器将画面无线传输到其他较大屏幕上，如客厅电视，会议室投影仪。画面传输延时150ms以下，几乎同时。点击此功能，但没有任何反应，因为此项功能需要配件支持。

###AirPlay
AirPlay 是苹果开发的一种无线技术，可以通过WiFi将iPhone 、iPad、iPodtouch 等iOS 设备上的包括图片、音频、视频通过无线的方式传输到支持AirPlay 设备。售价99美元的Apple TV就具备这样的功能，现在一些传统的家庭影院和HIFI如马兰士和天龙的新品功放和网络播放器已经支持AirPlay功能。

AirPlay具备与DLNA所没有的镜像功能，这一功能叫AirPlay镜像，可将iPhone 或iPad 上的画面无线传输到电视上，也就是说你设备显示的是什么，电视屏幕显示就就是什么，而不仅限于图片和视频。你可以拿着iPad 来当做方向盘，然后看着大屏玩游戏。

另外AirPlay镜像最牛地方是它可以实现双屏游戏，让你的游戏有更多的交互。比如，电视里显示的是游戏画面，而iPad上显示的是比赛的路线图。

###Google Cast
[*谷歌官方的文档*](https://developers.google.com/cast/)

[*谷歌官方的demo*](https://github.com/googlecast/CastVideos-android)

因为国内的手机基本都是阉割版本的。因为不需要使用Google Play，所以国内很多手机需要下载好谷歌的三件套才能使用，然后就是谷歌官方的硬件Chromecast,在国内基本很难用，因为要用访问谷歌的服务器，所以需要外网才行，
我在集成Chromecast中遇到的坑

#####申请id
*使用Styled Media Receiver类型注册，获取id，然后替换官方demo的id就不会有sample这个水印了，我开始用的Custom Receiver类型注册，水印还在。
#####Android代码加入
```java
   	compile 'com.google.android.gms:play-services-cast-framework:10.0.1'//这里要用低版本，困扰两天
    compile 'com.googlecode.android-query:android-query:0.25.9'
    compile 'com.android.support:mediarouter-v7:25+'  
```
#####投屏关键代码
```java
    private MediaInfo buildMediaInfo() {
        MediaMetadata movieMetadata = new MediaMetadata(MediaMetadata.MEDIA_TYPE_MOVIE);
        movieMetadata.putString(MediaMetadata.KEY_SUBTITLE, mTitle);
        movieMetadata.putString(MediaMetadata.KEY_TITLE, "第" + mPart + "集");
        movieMetadata.addImage(new WebImage(Uri.parse(mPictureUrl)));
        movieMetadata.addImage(new WebImage(Uri.parse(mPictureUrl)));
        //  Log.e(TAG, "buildMediaInfo" + mSelectedMedia.getUrl() + "         " + mSelectedMedia.getDuration() * 1000);
        //第一条为谷歌，第二条为我加密，三 不加密   4 网路不加密
        //   return new MediaInfo.Builder("https://commondatastorage.googleapis.com/gtv-videos-bucket/CastVideos/hls/WhatCarCanYouGetForAGrand.m3u8")
        //  return new MediaInfo.Builder("https://cdn61.ytbbs.tv/cn/tv/55550/55550-1/play.m3u8?md5=QXaEWg8r8NkZJo9ln0cZwg&expires=1521010007&token=55550")
        //  return new MediaInfo.Builder("https://cdn61.ytbbs.tv/cn/tv/54063/54063-1/play.m3u8?md5=DRhHA6ToB4z43_32sex0kw&expires=1521012980&token=54063")
        // return new MediaInfo.Builder("http://cdn14.ytbbs.tv/666/ds3/play.m3u8")
        //  return new MediaInfo.Builder(mSelectedMedia.getUrl())
        // url = "https://commondatastorage.googleapis.com/gtv-videos-bucket/CastVideos/mp4/DesigningForGoogleCast.mp4";
        //url = "https://cdn61.ytbbs.tv/666/ds3/ds3s.mp4";
        LogUtils2.logi(TAG, "buildMediaInfo" + url);
        return new MediaInfo.Builder(url)
                .setStreamType(MediaInfo.STREAM_TYPE_BUFFERED)
                .setContentType("application/vnd.apple.mpegurl")
                // .setContentType("application/x-mpegurl")
                // .setContentType("videos/m3u8")
                // .setContentType("videos/mp4")
                .setMetadata(movieMetadata)
                .setStreamDuration(0)
                .build();
    }
```
**setContentType**里面[*MIME*](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)值类型非常重要。很多小伙伴投屏失败就是因为没有使用正确的__MIME__类型

###DLNA和Miracast区别
DLNA是基于文件的，媒体文件可能有各种各样的编码格式，播放器这端必须能够处理这么多种编码格式。通常为了比较好的播放体验，DLNA会先缓存一小段时间。

Miracast是实时的，它可以实时传输源端(Source)的输出。源端任何屏幕的操作都会被传输到接收(Sink)端。如果源端是播放媒体文件，源端负责先对媒体文件解码，然后再编码成H.264的格式。接收端只需要做H.264的解码就可以了。相对DLNA，Miracast对于WiFi通路的要求要更高一些。












