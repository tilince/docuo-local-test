<div class="mk-warning">

需要在推流（[startPublishingStream\|_blank](@startPublishingStream)）前或预览（[startPreview\|_blank](@startPreview)）前设置好相关视频配置，在推流后仅支持编码分辨率和码率的修改。
</div>

调用 [setVideoConfig\|_blank](@setVideoConfig) 接口修改推流视频配置，可通过该接口设置视频帧率、码率、视频采集分辨率和视频编码输出分辨率。若不进行特殊设置，则 SDK 会根据选定的场景，自动设置贴合该场景的分辨率、码率、帧率，以获取最佳体验效果，详情请参考 [场景音视频配置](!video_proflie)。

<div class="mk-hint">

如果拉流端需要拉取 60 帧的流，需要联系技术支持，详情可参考 FAQ [ZEGO Express SDK 是否支持拉 60 帧的流\|_blank](https://doc-zh.zego.im/faq/Pull_the_stream_of_60_frames)。
</div>

以设置视频采集分辨率为 360p ，编码分辨率为 360p ，码率为 600 kbps，帧率为 15 fps 为例：





