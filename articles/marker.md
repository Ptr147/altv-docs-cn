# 标记 API

自alt:V更新15以来，新增了用于创建客户端标记的类。这些客户端标记更容易创建和使用，因为它们会自动在客户端进行流式传输。

标记是在3D空间中用于可视化标记特定位置的符号。

> [!WARNING]
> 最近创建的标记会在玩家靠近定义的流式传输距离时自动传输给玩家。
> 
> 出于性能原因，目前标记的流式传输数量限制为最多2000个同时传输。

## Usage

[Marker class in JS API reference](https://docs.altv.mp/js/api/alt-client.Marker.html)<br>
[Marker class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.Marker.html)<br>

标记的外观由其类型定义，可查看[MarkerTypes](https://docs.altv.mp/js/api/alt-client.MarkerType.html)以获取所有可用类型。<br>

### 示例

<img src="https://i.imgur.com/UwdrKCf.gif">

```js
// Create green rotating dollar sign marker
const marker = new alt.Marker(29, new alt.Vector3(0, 0, 71), new alt.RGBA(0, 128, 0, 255));
marker.rotate = true;
```

以下是一个示例，展示了一个只有在坐标范围内小于100.0时才可见的流式传输标记。

```js
const marker = new alt.Marker(29, new alt.Vector3(0, 0, 71), new alt.RGBA(0, 128, 0, 255), true, 100.0);
```