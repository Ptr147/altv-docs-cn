# Ped API

自alt:V更新15以来，您可以创建服务器端流式传输并同步的行人，通常也称为NPC。这些在服务器端创建的行人可以通过在客户端分配任务来控制，然后会自动与附近的所有玩家同步。

> [!WARNING]
> 这些行人不会自行移动和驾驶，它们需要在客户端分配任务才能实际执行某些操作。
> 对于类似GTA Online的行为，您将需要自行实现逻辑。

使用便捷的LocalPed类来生成仅在客户端流式传输的行人，其他玩家不会看到。

[LocalPed class in JS API reference](https://docs.altv.mp/js/api/alt-client.LocalPed.html)<br>
[LocalPed class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.LocalPed.html)<br>

在默认服务器配置下，最多同时传输最接近的128个行人。最大传输数值可以在服务器配置中进行编辑。

> [!WARNING]
> 行人的流式实体池与用于流式传输玩家的相同。无论是NPC还是玩家，最接近的行人将首先进行流式传输。

参见 [服务器配置](configs/server.md) [maxStreaming] 部分。

<br>

> [!TIP]
> 参见 <a href='https://forge.plebmasters.de/peds'>Pleb Masters: Forge</a> 所有行人列表。

## Usage

[Ped class in JS API reference](https://docs.altv.mp/js/api/alt-server.Ped.html)<br>
[Ped class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.Ped.html)<br>

### Example

```js
// 创建服务器端同步，看起来像一头牛
let ped = new alt.Ped("A_C_Cow", new alt.Vector3(0, 0, 70), new alt.Vector3(0, 0, 0));
```

要分配任务，您需要在NetOwner的客户端进行应用。

```js
// server side
ped.netOwner.emit("ped_task", ped);

// client side
alt.onServer("ped_task", (ped) => {
   // 让指定的ped在给定坐标的10米半径内漫游。它将始终移动到半径内的随机位置，同时在移动前至少等待2秒，最多等待10秒。
   natives.taskWanderInArea(ped, 0, 0, 70, 10, 2, 10);
});
```