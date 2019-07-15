{{indexmenu_n>12}}

# Windows SDK指南

## 1\. 下载资源

  - 可以下载Demo、SDK、API文档  
    [现在下载](https://github.com/ucloud/urtc-win-demo.git)  
    \===== 2. 开发语言以及系统要求 =====
  - 开发语言：C++  
    \* 系统要求：Windows 7 及以上版本的 Windows 系统  
    \===== 3. 开发环境 =====
  - Visual Studio 2015 及其它c++ 开发环境  
    \* Win32 Platform  
    \===== 4. 搭建开发环境 =====



    *导入 SDK\\

4.1. 将 sdk/include 目录添加到项目的 INCLUDE 目录下。  
4.2. 将 sdk/lib 目录放入项目的 LIB 目录下。  
4.3. 将 sdk/dll 下的 dll 文件复制到你的可执行文件所在的目录下。  
\===== 5. 初始化 =====

    *继承实现UCloudRtcEventListener，用作事件处理

``` cpp

Class UcloudRtcEventListenerImpl ： public UcloudRtcEventListener {
……
};
UcloudRtcEventListener* eventhandler = new UcloudRtcEventListenerImpl

```

    *初始化引擎

``` cpp

m_rtcengine = UCloudRtcEngine::sharedInstance(eventhandler);
m_rtcengine = UCloudRtcEngine::sharedInstance(UCloudRtcEventListener实现类);
m_rtcengine->setSdkMode (1); // 1 testmode 0 normal
m_rtcengine->setTokenSecKey(TEST_SECKEY);//测试模式下设置自己的秘钥
m_rtcengine->setStreamRole(STREAM_BOTH);
m_rtcengine->setAudioOnlyMode(false);
m_rtcengine->setAutoPublishSubscribe(false, true);
m_rtcengine->configLocalAudioPublish(false);
m_rtcengine->configLocalCameraPublish(true);
m_rtcengine->configLocalScreenPublish(false);
m_rtcengine->setVideoProfile(UCLOUD_RTC_VIDEO_PROFILE_640_360);

```

## 6.建立通话

    *加入房间

``` cpp
tUCloudRtcAuth auth;
auth.mAppId = appid;
auth.mRoomId = roomid;
auth.mUserId = userid;
auth.mUserToken = "1223222";
m_rtcengine->joinChannel(auth);
```

    *发布本地流

``` cpp
tUCloudRtcMediaConfig config;
config.mAudioEnable = true;
config.mVideoEnable = true;
m_rtcengine->publish(UCLOUD_RTC_MEDIATYPE_VIDEO, config.mVideoEnable,
            config.mAudioEnable)
```

    *取消发布本地流

``` cpp
tUCloudRtcVideoCanvas view;
view.mVideoView = (int)m_localWnd->GetVideoHwnd();
view.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;
m_rtcengine->stopPreview(view);
m_rtcengine->unPublish(UCLOUD_RTC_MEDIATYPE_VIDEO);
```

    *订阅流

``` cpp
m_rtcengine->subscribe(tUCloudRtcStreamInfo & info)
```

    *取消订阅流

``` cpp
m_rtcengine->unSubscribe(tUCloudRtcStreamInfo& info)
```

    *离开房间

``` cpp
m_rtcengine->leaveChannel ()
```

    *编译、运行，开始体验吧！