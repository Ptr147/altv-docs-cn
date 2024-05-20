# Ped API

自alt:V更新15以来，您可以创建服务器端流式传输并同步的行人，通常也称为NPC。这些在服务器端创建的行人可以通过在客户端分配任务来控制，然后会自动与附近的所有玩家同步。

> [!WARNING]
> 这些行人不会自行移动和驾驶，它们需要在客户端分配任务才能实际执行某些操作。
> 对于类似GTA Online的行为，您将需要自行实现逻辑。

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

### 案例

```js
//working example

////////////////////
//SERVERSIDE:
////////////////////
alt.on("playerConnect", (player) => {

    //spawn your own player
    player.spawn(7.904539108276367, 28.82634925842285, 70.866943359375, 0);

    //create serverside NPC (posX, posY, posZ, rotX, rotY, rotZ, steamingDistance)
    let ped = new alt.Ped("u_m_m_streetart_01", new alt.Vector3(9.717802047729492, 26.340801239013672, 70.81243896484375), new alt.Vector3(0, 0, 0), 200);

    //its important to wait until Ped is ready
    alt.setTimeout(() => {

        //then u set the netOwner
        ped.setNetOwner(alt.Player.all[0]);

        //then u emit the netOwner the walkaround task (server -> client)
        ped.netOwner.emit("ped_task", ped);

    }, 2500);

});

////////////////////
//CLIENTSIDE:
////////////////////
alt.onServer("ped_task", (ped) => {

    //just a debug message for the info if event is called
    alt.log(`ped_task taskWanderInArea was called for pedID: ${ped.scriptID}`);

    // Make the specified ped roam within a 10-meter radius of the given coordinates. It will always move to a random location inside the radius, while waiting a minimum of 2 and maximum of 10 seconds before moving.
    //taskWanderInArea: ped: Ped | x: float | y: float | z: float | radius: float | minimalLength: float | timeBetweenWalks: float
    native.taskWanderInArea(ped, 9.717802047729492, 26.340801239013672, 70.81243896484375, 10, 2, 10);

});
```

## Local Ped API

Its also possible to create local (clientside only) peds that other players won't see.

### Usage

[LocalPed class in JS API reference](https://docs.altv.mp/js/api/alt-client.LocalPed.html)<br>
[LocalPed class in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Client.Elements.Entities.LocalPed.html)<br>

#### Example
Clothes shop preview.

```js
// We don't need streaming enabled
// because we need to use ped immediately
const useStreaming = false;

const player = alt.Player.local;

const ped = new alt.LocalPed(player.model, player.dimension, player.pos, player.rot, useStreaming);

// More on that below
alt.Utils.assert(ped.scriptID !== 0);

// Copying appearance, clothes etc. of player to the ped
native.clonePedToTarget(player, ped);

// Now we can change clothes of ped
alt.setPedDlcClothes(ped.scriptID, 0, 11, 1, 0);
```

> [!WARNING]
> It is important to note that the ped may not be spawned immediately due to model loading or streaming, in the above example the ped is spawned immediately (supposedly, that's why we have an `alt.Utils.assert`) because the player model is already loaded and streaming is disabled.

Waiting for ped to spawn due to model loading and streaming.
```js
const model = "player_zero";
const dimension = alt.defaultDimension; // 0 dimension
const pos = new alt.Vector3(0, 0, 71);
const rot = alt.Vector3.zero;
const useStreaming = true;
const streamingDistance = 100; // customize for your needs

const ped = new alt.LocalPed(model, dimension, pos, rot, useStreaming, streamingDistance);

// Waiting until ped spawns for 5 seconds or rejecting current promise
// (timeout value is not required, 2 seconds by default in v15)
await ped.waitForSpawn(5000);

// ped.scripID is now valid and we can use it
alt.setPedDlcClothes(ped.scriptID, 0, 11, 1, 0);
```
