# 语音 API

alt:V内置了语音聊天功能。要启用它，您需要在`server.toml`中创建`voice`类别。它可以为空以使用默认语音设置。

例如:
> [!div class="nohljsln"]
> ```toml
> name = "My awesome server"
> 
> [voice]
> ```

## 语音频道

[VoiceChannel class in JS API reference](https://docs.altv.mp/js/api/alt-server.VoiceChannel.html)<br>
[VoiceChannel class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.VoiceChannel.html)<br>

要允许玩家彼此听到声音，您需要创建一个语音频道并将应该彼此听到的玩家添加到该频道中。

如果两个玩家同时处于两个不同的语音频道中，则类别将由`channel.priority`决定。如果两个频道具有相同的优先级，则选择非空间（2D）语音。

### VoiceChannel 构造函数

VoiceChannel类的构造函数中有两个参数:
- `isSpatial` - 语音是否空间感知（基于3D位置的声音），将其设置为false对于例如电话通话等不需要精确位置的情况很有用。
- `maxDistance` - 玩家能被听到的最大距离，设为0以使用非空间声音。


### 音频过滤器

您可以将音频过滤器应用于语音通道对象。

查看 [音频过滤器](audio_filters.md) 获得更多信息。

```js
const voice = new alt.VoiceChannel(true, 15);
voice.filter = alt.hash('walkietalkie');
```

### 音频分类

您可以将自定义音频类别设置为语音通道。为此，您需要创建一个音频过滤器并在其上设置类别。

查看 [音频过滤器](audio_filters.md), 音频类别部分更多信息。

### Example

```js
const globalVoice = new alt.VoiceChannel(true, 15);

alt.on('playerConnect', (player) => {
  globalVoice.addPlayer(player);
});
```
