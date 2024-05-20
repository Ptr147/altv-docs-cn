# 虚拟实体 API

在alt:V更新15中，我们添加了虚拟实体API。它为您提供了一种简单的方式来同步您自己的实体，例如查看[此演示](https://discord.com/channels/371265202378899476/384874419446743041/1106288579598110741)，其中的火粒子是虚拟实体。

## 用法

服务端

[VirtualEntity class in JS API reference](https://docs.altv.mp/js/api/alt-server.VirtualEntity.html)<br>
[VirtualEntityGroup class in JS API reference](https://docs.altv.mp/js/api/alt-server.VirtualEntityGroup.html)<br>
[VirtualEntity class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.VirtualEntity.html)<br>
[VirtualEntityGroup class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.VirtualEntityGroup.html)<br>

客户端

[VirtualEntity class in JS API reference](https://docs.altv.mp/js/api/alt-client.VirtualEntity.html)<br>
[VirtualEntityGroup class in JS API reference](https://docs.altv.mp/js/api/alt-client.VirtualEntityGroup.html)<br>
[VirtualEntity class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.VirtualEntity.html)<br>
[VirtualEntityGroup class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.VirtualEntityGroup.html)<br>

该API由两个类组成：group和entity。<br>

- Group用于设置每个玩家流的实体的最大数量。<br>
- Entity被分配到一个组中。<br>

### 什么是“每个玩家流的最大实体数”?<br>

举个例子：我们有球作为虚拟实体，在游戏中玩家周围有100个虚拟实体。如果我们将最大实体数量设置为3，玩家将只看到最接近的3个。

<img src="https://i.imgur.com/yUZKwQQ.png" width="400px"/>

黄色球是对玩家进行流式传输的虚拟实体。<br>
绿色圆圈表示球的流式传输范围（在这个例子中，所有球的流式传输距离相同）。<br>
蓝色箭头表示流式传输距离。

### 示例

在这个例子中，我们将使用JavaScript API来同步赌场幸运轮。

Server

```js
import alt from 'alt-server';

// 服务器上只能有一个幸运转盘
const maxEntitiesInStream = 1;

const luckyWheelGroup = new alt.VirtualEntityGroup(maxEntitiesInStream);

const pos = new alt.Vector3(0, 5, 72);
const streamingDistance = 100;
// 初始流同步元数据
const initialData = {
    // 在您的游戏模式中，很可能会创建不同类型的虚拟实体。
    type: 'luckyWheel',
};
const entity = new alt.VirtualEntity(luckyWheelGroup, pos, streamingDistance, initialData);

// 每5秒旋转一次
alt.setInterval(async () => {
    entity.setStreamSyncedMeta('spinStartTime', alt.getNetTime());
    await alt.Utils.wait(2000);
    entity.deleteStreamSyncedMeta('spinStartTime');
}, 5_000);

// 如果需要，我们还可以设置维度
// entity.dimension = 123；

// 或者对所有玩家隐藏实体
// entity.visible = false；
```

Client

```js
import alt from 'alt-client';

// alt.LocalObject or null
let luckyWheelObject = null;
// alt.Utils.EveryTick or null
let currentSpinTick = null;

const isItLuckyWheel = (object) => {
    // 如果它不是虚拟实体，则忽略它
    if (!(object instanceof alt.VirtualEntity)) return false;

    // 我们在服务器上设置的初始数据流同步元
    if (object.getStreamSyncedMeta('type') !== 'luckyWheel') return false;

    return true;
};

const startSpin = (startSpinTime) => {
    alt.log('Lucky wheel start spin time:', startSpinTime);

    currentSpinTick = new alt.Utils.EveryTick(() => {
        const currentSpinTime = (alt.getNetTime() - startSpinTime);

        // 旋转已经结束了
        if (currentSpinTime > 2000) {
            currentSpinTick.destroy();
            currentSpinTick = null;

            luckyWheelObject.rot = alt.Vector3.zero;
            return;
        }

        let currentNormalized = currentSpinTime / 2000;
        let degrees = currentNormalized * 360;

        luckyWheelObject.rot = new alt.Vector3(0, degrees, 0).toRadians();
    })
}

alt.on('worldObjectStreamIn', (object) => {
    if (!isItLuckyWheel(object)) return;

    alt.log('Lucky wheel stream in');

    // 确保没有已创建的对象
    alt.Utils.assert(luckyWheelObject == null);

    const rot = alt.Vector3.zero;
    luckyWheelObject = new alt.LocalObject('vw_prop_vw_luckywheel_02a', object.pos, rot);

    const spinStartTime = object.getStreamSyncedMeta('spinStartTime');
    if (spinStartTime == null) return;
    startSpin(spinStartTime);
})

alt.on('worldObjectStreamOut', (object) => {
    if (!isItLuckyWheel(object)) return;

    alt.log('Lucky wheel stream out');

    luckyWheelObject?.destroy();
    luckyWheelObject = null;
    currentSpinTick?.destroy();
    currentSpinTick = null;
})

alt.on('streamSyncedMetaChange', (object, key, value) => {
    if (!isItLuckyWheel(object)) return;

    // 确保我们已经创建了对象
    alt.Utils.assert(luckyWheelObject != null);

    alt.log('Lucky wheel change', { key: value });

    switch (key) {
        case 'spinStartTime':
            // 值被删除
            if (value == null) return

            startSpin(value);
            break;
        case 'type':
            // 忽略
            break
        default:
            alt.logError('Lucky wheel unknown stream synced meta change key:', key);
    }
});
```
