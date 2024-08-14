## 前提条件

在实现同步播放视频之前，请确保：

- 已在 [ZEGO 控制台\|_blank](https://console.zego.im) 创建项目，并申请有效的 AppID 和 AppSign，详情请参考 [控制台 - 项目管理 - 项目信息\|_blank](#12107)。
- 已在项目中集成 ZegoAccurateSyncMediaPlayer SDK，详情请参考 [精准同步播放器 - 快速开始 - 集成 SDK\|_blank](!ZegoAccurateSyncMediaPlayerSDK-Integrated_CopyrightedVideo_SDK)。
- 已在项目中集成 ZEGO Express SDK，实现基本的实时音视频功能，详情请参考 [实时音视频 - 快速开始 - 集成 SDK|_blank](!ExpressVideoSDK-Integration/SDK_Integration)。

## 示例 Demo

ZEGO 提供了 [示例 Demo\|_blank](!DownloadDemo/DownloadDemo)，以供开发者进一步了解。


## 实现流程

![/Pics/iOS/ZegoCopyrightedVideo/Third_Party_Video_Resource_Implementation_Host_Android.png](http://doc.oa.zego.im/Pics/iOS/ZegoCopyrightedVideo/Third_Party_Video_Resource_Implementation_Host_Android.png)

### 1 开通服务

在实现影视资源同步播放之前，请联系 ZEGO 商务人员开通服务。

### 2 初始化 Express SDK

<div class="mk-warning">

实现影视资源同步播放时，ZegoAccurateSyncMediaPlayer SDK 的信令传输，是基于 ZEGO Express SDK 的房间信令通道实现的，所以首先，开发者需要初始化 ZEGO Express SDK。
</div>

1. 创建界面。根据场景需要，为你的项目播放视频的用户界面。我们推荐你在项目中添加如下元素：

    - 播放视频窗口
    - 操作按钮（包含播放，暂停，进度条等功能）

2. 创建引擎。   
调用 ZEGO Express SDK 的 [createEngine\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~ZegoExpressEngine#create-engine) 接口，将申请到的 AppID 和 AppSign 分别传入参数“appID”和“appSign”，创建引擎单例对象。   
注册回调，可将实现了 [IZegoEventHandler\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~IZegoEventHandler) 的对象传入参数 “eventHandler”。   

    ```java
    ZegoEngineProfile zegoEngineProfile = new ZegoEngineProfile();
    // 请通过 ZEGO 控制台获取，格式为：1234567890
    zegoEngineProfile.appID = Config.appID;
    // 请通过 ZEGO 控制台获取，格式为："0123456789012345678901234567890123456789012345678901234567890123"（共64个字符）
    zegoEngineProfile.appSign = Config.appSign;    
    zegoEngineProfile.application = application;
    // 实时通讯场景接入
    zegoEngineProfile.scenario = ZegoScenario.COMMUNICATION;
    // 创建引擎
    ZegoExpressEngine.createEngine(zegoEngineProfile, new IZegoEventHandler() {
        @Override
        public void onRoomExtraInfoUpdate(String roomID, ArrayList<ZegoRoomExtraInfo> roomExtraInfoList) {
            // 透传房间事件信息到 ZegoAccurateSync
            ZegoAccurateSync.roomExtraInfoUpdated(roomID, roomExtraInfoList)
        }
    });
    ```

### 3 初始化 ZegoAccurateSyncMediaPlayer SDK

1. 在 application 的 onCreate 时调用 `initReportOnApplicationCreate` 来启动上报引擎，这一步只能在 application 的 `onCreate` 这里执行。

    ``` java
    class YourApplication extends Application {
        ...
        @Override
        public void onCreate() {
            super.onCreate();
            ...
            ZegoAccurateSync.initReportOnApplicationCreate(this);
            ...
        }
        ...
    }
    ```

2. 调用 ZegoAccurateSyncMediaPlayer SDK 的 [init\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSync#init) 接口，传入 userID，设置用户角色为 “ ZegoAccurateSyncUserRoleHost”，初始化 ZegoAccurateSyncMediaPlayer SDK。

    <div class="mk-hint">

    同一个 AppID 内，需保证 “userID” 全局唯一，建议开发者将 “userID” 与自己业务的账号系统进行关联。

    </div>

    ```java
    ZegoAccurateSyncConfig zegoAccurateSyncConfig = new ZegoAccurateSyncConfig(
        // appID 请通过 ZEGO 控制台获取，格式为：1234567890    
        your_appID, "your_user_id",
        ZegoAccurateSyncUserRole.ZegoAccurateSyncUserRoleHost);
    ZegoAccurateSync.init(application, zegoAccurateSyncConfig) { errorCode, jsonObject ->
    };
    // 设置事件回调
    ZegoAccurateSync.setEventHandler { videoID, roomID ->
        ...
    };
    ```

### 4 登录房间

调用 ZEGO Express SDK 的 [loginRoom\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~ZegoExpressEngine#login-room) 接口登录房间。roomID 和 user 的参数由开发者的本地业务生成，但是需要满足以下条件：
- 同一个 AppID 内，需保证 “roomID” 全局唯一。  
- 需要和 <a href="#4_3">3 初始化 ZegoAccurateSyncMediaPlayer SDK</a> 中的 userID 一致。

```java
// 创建用户对象，ZegoUser 的构造方法 userWithUserID 会将 “userName” 设为与传的参数 “userID” 一样。“userID” 与 “userName” 不能为 “null”，否则会导致登录房间失败。
String roomID = "your_room_id";
ZegoUser user = new ZegoUser("your_user_id");

// 登录房间
ZegoExpressEngine.getEngine().loginRoom(roomID, user);
```

<div class="mk-warning">

必须收到 [onRoomStateChanged\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~IZegoEventHandler#on-room-state-changed) 指示登录房间成功，后续才能成功 <a href="#4_6">加载视频</a>。

</div>

### 5 透传房间附加消息

在 ZEGO Express SDK 的 [IZegoEventHandler\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~IZegoEventHandler) 回调中，注册 [onRoomExtraInfoUpdate\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~IZegoEventHandler#on-room-extra-info-update) 监听，将房间内的附加消息，透传给 ZegoAccurateSyncMediaPlayer SDK。再调用 ZegoAccurateSyncMediaPlayer SDK 的 [roomExtraInfoUpdated\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSync#room-extra-info-updated) 接口，同步“主持人端”和“观众端”的视频播放进度。

``` Java
ZegoAccurateSync.roomExtraInfoUpdated(roomID, roomExtraInfoList);
```

### 6 创建播放器，加载视频资源。

调用 ZegoAccurateSyncMediaPlayer SDK 的 [createMediaPlayer\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSync#create-media-player) 创建播放器，设置播放界面视图。调用 ZegoAccurateSyncMediaPlayer SDK 的 [loadVideo\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#load-video) 接口加载视频资源。

<div class="mk-warning">

- 加载视频前，请确认是否收到 [onRoomStateChanged\|_blank](/article/api?doc=Express_Video_SDK_API~java_android~class~IZegoEventHandler#on-room-state-changed) 指示 <a href="#4_4">登录房间</a> 成功，否则可能会导致加载视频失败。
- 播放器支持的格式有：MP3、MP4、FLV、WAV、AAC、M3U8 和 MKV，如需支持其它格式，请联系 ZEGO 技术支持。
</div>

```java
ZegoAccurateSyncMediaPlayer mediaPlayer = ZegoAccurateSync.createMediaPlayer();
    
mediaPlayer.setPlayerCanvas(ZegoAccurateSyncMediaPlayerCanvas(your_textureView));
mediaPlayer.loadVideo(
            "your_video_url",    // URL 为需要播放的视频资源地址。
            start_position    // 一般填 0，即从头开始播，但实际情况会根据该影片在房间里的进度来跳转，如果房间里没有播放该视频，则从头开始播放。
        ) { errorCode ->
        };
```

### 7 开始播放

主持人登录房间，并且播放器设置完成后，可以调用 ZegoAccurateSyncMediaPlayer SDK 的 [start\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#start) 接口，开始播放选择的视频资源了。

<div class=mk-warning>

- 只有 [loadVideo\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#load-video) 成功后，才能调用 [start\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#start) 开始播放视频。
- 如果在播放视频资源过程中需要切换视频，不能直接调用 [loadVideo\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#load-video)，需要先调用 [stop\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#stop) 接口停止播放当前视频，具体做法请参看 <a href="#4_10">停止播放</a>。

</div>

``` Java
mediaPlayer.start(); //开始播放
```

### 8 暂停/恢复播放

播放过程中，主持人可以调用 ZegoAccurateSyncMediaPlayer SDK 的 [pause\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#pause) 接口暂停播放，然后调用 ZegoAccurateSyncMediaPlayer SDK 的 [resume\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#resume) 接口恢复播放。


``` Java
mediaPlayer.pause();//暂停播放

mediaPlayer.resume();//恢复播放
```

<div class="mk-hint">

- 主持人端调用 [pause\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#pause) 接口暂停播放时，房间内的所有观众端会同步暂停；调用 [resume\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#resume) 接口恢复播放时，房间内的所有观众端会同步恢复。（如果观众主动点击了暂停按钮，则观众端不会恢复。）
- 观众端也可以调用 [pause\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#pause) 接口暂停播放，但只暂停自己的播放进度，并且不受主持人的影响；观众端调用 [resume\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#resume) 接口恢复播放时，会继续播放视频（如果此时主持人已暂停播放，则观众端的影片仍为暂停状态），但不会同步主持人的视频进度，而是从原来暂停的地方继续播放。

</div>

### 9 跳转到指定时间播放

播放过程中，主持人如果想要改变播放进度，跳转到指定时间的位置播放，可以调用 ZegoAccurateSyncMediaPlayer SDK 的 [seekTo\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#seek-to) 接口，传入时间（单位为 ms），进行跳转。

```Java
mediaPlayer.seekTo(your_start_position);
```

### 10 停止播放

播放过程中，主持人可以调用 ZegoAccurateSyncMediaPlayer SDK 的 [stop\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#stop) 接口，直接停止播放。

``` Java
mediaPlayer.stop(); //停止播放
```

## 常用功能

### 播放器播放状态变化回调通知

播放过程中，播放器的状态发生变化时，开发者可以通过 ZegoAccurateSyncMediaPlayer SDK 的 [ZegoAccurateSyncMediaPlayerEventHandler\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayerEventHandler) 回调接口，获取相关的状态通知，并在回调中根据不同状态处理业务逻辑。


```java
ZegoAccurateSyncMediaPlayerEventHandler playerEventHandler = new ZegoAccurateSyncMediaPlayerEventHandler() {
    @Override
    public void onStateUpdate(@NonNull ZegoAccurateSyncMediaPlayer mediaPlayer, @NonNull ZegoAccurateSyncMediaPlayerState state, int errorCode) {
        //用户播放器状态变化，可以在这里更改播放按钮状态
    }

    @Override
    public void onNetworkEvent(@NonNull ZegoAccurateSyncMediaPlayer mediaPlayer, @NonNull ZegoAccurateSyncMediaPlayerNetworkEvent networkEvent) {
        //用户播放器网络状况变化
    }

    @Override
    public void onPlayingProgress(@NonNull ZegoAccurateSyncMediaPlayer mediaPlayer, long millisecond) {
        //播放器进度变化，可以用来刷新改变当前播放进度
    }

    @Override
    public void onVideoProgressGapWithServer(@NonNull ZegoAccurateSyncMediaPlayer mediaPlayer, long progressGap) {
        //与服务器的延迟，可以用于提示或者调试用
    }
};
```

### 进度同步

主持人注册了 ZegoAccurateSyncMediaPlayer SDK 的 [onVideoProgressGapWithServer\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayerEventHandler#on-video-progress-gap-with-server) 后，如果在播放过程中，主持人的进度与服务器的进度不同步，主持人会收到相关通知。此时如果主持人调用 [syncServerProgress\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSync#sync-server-progress) 接口，房间内所有人会向服务器同步进度。

``` Java
// videoID 为房间内正在播放的视频的 ID
String videoID = "your_video_id";
ZegoAccurateSync.syncServerProgress(videoID);
```

<div class="mk-hint">

自定义影视资源的 videoID，需要调用 ZegoAccurateSyncMediaPlayer SDK 的 [loadVideo\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#load-video) 接口加载成功后，才能从 ZegoAccurateSyncMediaPlayer 的 属性 [videoID\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSyncMediaPlayer#video-id) 获得。
</div>

### 资源回收

1. 退出房间    
主持人调用 ZEGO Express SDK 的 [logoutRoom\|_blank](/article/api?doc=express-video-sdk_API~objectivec_ios~class~ZegoExpressEngine#logout-room) 接口，退出房间。

    ``` Java
    // 退出房间
    ZegoExpressEngine.getEngine().logoutRoom();
    ```

    <div class="mk-hint">

    主持人退出房间后，如果没有暂停或停止视频播放，房间内的观众可以继续观看当前视频。
    </div>

2. 销毁播放的视频资源     
播放视频结束后，主持人如需销毁本地的视频播放资源，可以调用 ZegoAccurateSyncMediaPlayer SDK 的 [unInit\|_blank](/article/api?doc=ZegoAccurateSyncMediaPlayerSDK_API~java_android~class~ZegoAccurateSync#un-init) 接口，销毁本地资源，并反初始化 SDK。

    ``` Java
    // 销毁播放器
    mediaPlayer.destroyMediaPlayer();

    // 释放 ZegoAccurateSyncMediaPlayer 资源
    ZegoAccurateSync.unInit();
    ```

3. 销毁引擎    
如果退出房间，不需要使用到引擎资源，可以调用 ZEGO Express SDK 的 [destroyEngine\|_blank](/article/api?doc=express-video-sdk_API~objectivec_ios~class~ZegoExpressEngine#destroy-engine) 接口，销毁引擎。

    ``` Java
    ZegoExpressEngine.destroyEngine{ };
    ```



























