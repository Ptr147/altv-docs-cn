# 检查点 API

自alt:V更新15以来，新增了用于创建客户端检查点的类。这些客户端检查点更容易创建和使用，因为它们会自动在客户端进行流式传输。

## 用法

[Checkpoint class in JS API reference](https://docs.altv.mp/js/api/alt-client.Checkpoint.html)<br>
[Checkpoint class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.Checkpoint.html)<br>

> [!TIP]
> 检查点继承了[ColShape 类](https://docs.altv.mp/js/api/alt-client.Colshape.html)，因此colShape事件也会在其上起作用。
### 示例

该示例在本地玩家进入检查点时将检查点颜色更改为绿色。当本地玩家不在其中时，检查点颜色将为红色。

<img src="https://i.imgur.com/Fb8c07U.png">
<img src="https://i.imgur.com/QQlM132.png">

```js
alt.on('consoleCommand', commandName => {
    if (commandName !== 'addcp') return;

    const iconColor = new alt.RGBA(255, 255, 255, 100);
    const pos = alt.Player.local.pos.sub(0, 0, 1);
    const nextPos = pos.add(2, 1, 0);

    new alt.Checkpoint(2, pos, nextPos, 1, 2, new alt.RGBA(255, 0, 0, 100), iconColor, 100);
});

function handleCheckpointEnterLeave(checkpoint, entity, state) {
    if (entity !== alt.Player.local) return;

    checkpoint.color = state ? new alt.RGBA(0, 255, 0, 100) : new alt.RGBA(255, 0, 0, 100);
}

alt.on('entityEnterColshape', (colShape, entity) => {
    if (!(colShape instanceof alt.Checkpoint)) return;

    handleCheckpointEnterLeave(colShape, entity, true);
});

alt.on('entityLeaveColshape', (colShape, entity) => {
    if (!(colShape instanceof alt.Checkpoint)) return;

    handleCheckpointEnterLeave(colShape, entity, false);
});
```