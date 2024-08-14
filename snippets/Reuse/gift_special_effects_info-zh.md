在语聊房和直播等场景中，礼物是主播与观众互动的重要环节。精致的礼物效果，可以增强互动感，提升观看体验。随着行业的蓬勃发展，礼物效果也从静态到动态、从不透明到透明逐步进化。

通过播放 MP4 的礼物视频来实现动态礼物效果，是比较常见的方式。由于 MP4 不支持 Alpha 通道，在全屏播放礼物特效时，会挡住直播间的内容，从而影响用户体验。因此 ZEGO Express SDK 媒体播放器提供透明渲染能力，可以实现播放透明动态礼物的功能。

ZEGO Express SDK 媒体播放器提供 RGB 通道与 Alpha 通道分离播放 MP4 素材（RGB 与 Alpha 拼接后的 MP4 素材）的功能，实现播放透明礼物的动态效果，即播放礼物特效时，不会挡住直播间内容，大大提升用户体验。

<video poster="https://storage.zego.im/sdk-doc/Pics/Express/gift_special_effects.png" src="https://storage.zego.im/sdk-doc/doc/video/Express_Video_SDK/gift_special_effects.mp4" width="70%" muted="true" loop="true" autoplay="autoplay" preload="auto" nocontrols></video>

### 功能优势

- **开发成本低**

    开发者不需要学习复杂的动画实现方案，只需要使用设计师处理好的素材，即可实现播放透明礼物特效，提升开发效率和效果。

- **动画还原度高**

    帮助设计师实现输出素材所见即所得，即设计师在设计动效时，无需担心开发人员使用的技术栈能否还原动效复杂的 3D 效果、描边、粒子效果等。

- **兼容性好**

    规避 Lottie、Cocos2d-X 等半透明动效实现方式复杂、性能不高，以及部分机型的兼容问题较大、后期维护难度较高的问题。

### 实现原理

如下图所示，图中上半部分为一段进行了 Alpha 通道分离的素材的部分像素点，左侧使用 RGB 通道存储了礼物素材的 RGB 通道的信息，右侧使用 RGB 通道存储了礼物素材的 Alpha 通道的信息。

客户端播放时，会将右侧的视频的 R 通道提取出来，作为左侧对应像素点的 Alpha 通道的值。

渲染时，将 Alpha 通道的值除以 255，进行归一化处理，得到 0 ~ 1 之间的 Alpha 取值，最后根据 RGBA 值将每个像素点渲染到屏幕上，实现半透明效果。

![/Pics/Express/RGBtoRGBA.jpeg](/Pics/Express/RGBtoRGBA.jpeg)





























