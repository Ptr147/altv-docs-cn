# 对象 API

自alt:V更新V15以来，您可以创建服务器端流式传输并同步的物体。这些服务器端创建的物体是动态的，并将通过完整的物理同步给附近的所有玩家

> [!WARNING]
> 只有在需要物理效果时才使用它们，因为它们可能会对服务器性能和整体流量产生显著影响。
> 
> 对于不需要在附近玩家之间同步任何物理效果的静态物体，请使用客户端创建的LocalObject。

[LocalObject class in JS API reference](https://docs.altv.mp/js/api/alt-client.LocalObject.html)<br>
[LocalObject class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.LocalObject.html)<br>

在默认服务器配置下，最多同时传输最接近的120个物体。最大传输数值可以在服务器配置中进行编辑。

参见 [服务器配置](configs/server.md) [maxStreaming] 部分。

<br>

> [!TIP]
> 查看 <a href='https://forge.plebmasters.de/objects'>Pleb Masters: Forge</a> 所有对象的完整列表。

## Usage

[Object class in JS API reference](https://docs.altv.mp/js/api/alt-server.Object.html)<br>
[Object class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.Object.html)<br>

### Example

```js
// 创造物理同步足球
let object = new alt.Object("p_ld_soc_ball_01",  new alt.Vector3(0,0,70), new.alt.Vector3(0,0,0));
```