---
layout: post
title: "HTML5 Audio/Video 标签属性与事件"
keywords: ["HTML5 Audio/Video 标签属性与事件"]
description: "HTML5 Audio/Video 标签属性与事件"
category: "Audio"
tags: ["Audio"]
---
{% include JB/setup %}


标签属性：src：音乐的URLpreload：预加载autoplay：自动播放loop：循环播放controls：浏览器自带的控制条 

1  

标签属性：src：视频的URLposter：视频封面，没有播放时显示的图片preload：预加载autoplay：自动播放loop：循环播放controls：浏览器自带的控制条width：视频宽度height：视频高度 

1  

获取HTMLVideoElement和HTMLAudioElement对象 

1 //audio可以直接通过new创建对象 

2 Media = newAudio("http://www.abc.com/test.mp3");  

3 //audio和video都可以通过标签获取对象 

4 Media = document.getElementByIdx_x("media"); 

Media方法和属性： 

HTMLVideoElement 和 HTMLAudioElement 均继承自 HTMLMediaElement

01 //错误状态 

02 Media.error; //null:正常 

03 Media.error.code; //1.用户终止 2.网络错误 3.解码错误 4.URL无效 

04 //网络状态 

05 Media.currentSrc; //返回当前资源的URL 

06 Media.src = value; //返回或设置当前资源的URL 

07 Media.canPlayType(type); //是否能播放某种格式的资源 

08 Media.networkState; //0.此元素未初始化  1.正常但没有使用网络  2.正在下载数据  3.没有找到资源 

09 Media.load(); //重新加载src指定的资源 

10 Media.buffered; //返回已缓冲区域，TimeRanges 

11 Media.preload; //none:不预载 metadata:预载资源信息 auto: 

12 //准备状态 

13 Media.readyState;   //1:HAVE_NOTHING 2:HAVE_METADATA 3.HAVE_CURRENT_DATA 4.HAVE_FUTURE_DATA 5.HAVE_ENOUGH_DATA 

14 Media.seeking; //是否正在seeking 

15 //回放状态 

16 Media.currentTime = value; //当前播放的位置，赋值可改变位置 

17 Media.startTime; //一般为0，如果为流媒体或者不从0开始的资源，则不为0 

18 Media.duration; //当前资源长度 流返回无限 

19 Media.paused; //是否暂停 

20 Media.defaultPlaybackRate = value;//默认的回放速度，可以设置 

21 Media.playbackRate = value;//当前播放速度，设置后马上改变 

22 Media.played; //返回已经播放的区域，TimeRanges，关于此对象见下文 

23 Media.seekable; //返回可以seek的区域 TimeRanges 

24 Media.ended;    //是否结束 

25 Media.autoPlay; //是否自动播放 

26 Media.loop; //是否循环播放 

27 Media.play();   //播放 

28 Media.pause();  //暂停 

29 //控制 

30 Media.controls;//是否有默认控制条 

31 Media.volume = value; //音量 

32 Media.muted = value; //静音 

33 //TimeRanges(区域)对象 

34 TimeRanges.length; //区域段数 

35 TimeRanges.start(index) //第index段区域的开始位置 

36 TimeRanges.end(index) //第index段区域的结束位置 

事件： 

01 eventTester = function(e){  

02         Media.addEventListener(e,function(){  

03             console.log((newDate()).getTime(),e);  

04         });  

05     }  

06    

07     eventTester("loadstart");   //客户端开始请求数据 

08     eventTester("progress");    //客户端正在请求数据 

09     eventTester("suspend");     //延迟下载 

10     eventTester("abort");       //客户端主动终止下载（不是因为错误引起）， 

11     eventTester("error");       //请求数据时遇到错误 

12     eventTester("stalled");     //网速失速 

13     eventTester("play");        //play()和autoplay开始播放时触发 

14     eventTester("pause");       //pause()触发 

15     eventTester("loadedmetadata");  //成功获取资源长度 

16     eventTester("loadeddata");  // 

17     eventTester("waiting");     //等待数据，并非错误 

18     eventTester("playing");     //开始回放 

19     eventTester("canplay");     //可以播放，但中途可能因为加载而暂停 

20     eventTester("canplaythrough"); //可以播放，歌曲全部加载完毕 

21     eventTester("seeking");     //寻找中 

22     eventTester("seeked");      //寻找完毕 

23     eventTester("timeupdate");  //播放时间改变 

24     eventTester("ended");       //播放结束 

25     eventTester("ratechange");  //播放速率改变 

26     eventTester("durationchange");  //资源长度改变 

27     eventTester("volumechange");    //音量改变 