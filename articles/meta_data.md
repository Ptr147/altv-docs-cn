# 元数据  

元数据可用于在服务器和客户端之间传递特定的值(也称为同步元数据或本地元数据),或在资源之间(全局元数据)绑定到一个实体或相应的一方(服务器或客户端)。  
例如,玩家的用户名可以绑定到它,或者警报器的状态绑定到一辆车(同步元数据)。  
同样,像当前天气这样的数据也可以绑定到资源作为元数据,以便其他资源可以查询这个值(全局元数据)。  
这些主题将在各自的子部分中更详细地讨论。

<iframe width="1280" height="720" src="https://www.youtube-nocookie.com/embed/4oQTVkx5GA0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# 类型

## 概览  

| 类型                 |  使用对象      | 可用性(设置) | 可用性(获取) | 描述                                                                                                                                   |
|----------------------|---------------|--------------------|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| 元数据            | 实体,Alt | 客户端和服务器    | 客户端和服务器    | 数据只在设置它的客户端或服务器端可用。             |  
| 本地元数据           | 玩家        | 服务器             | 客户端和服务器    | 数据是设置在玩家身上的。只有设置它的客户端可以获取数据。    |  
| (全局)同步元数据 | 实体,Alt | 服务器             | 客户端和服务器    | 数据可以在每个实体(玩家和车辆)上设置。每个客户端都可以获取数据。    |
| 流同步元数据   | 实体      | 服务器             | 客户端和服务器    | 这与同步元数据类似,但数据仅在实体处于客户端获取数据的流范围内时传输。 |

## 用法  

元数据是使用 alt 类或实体(玩家或车辆)上的 setMeta 方法设置的。  
元数据只在设置它的同一侧可用,并且可以由所有资源获取。

# [JS](#tab/tab1-0)

应用全局元数据

```js
// 使用给定的键和值设置全局元数据(单侧,跨资源)  
alt.setMeta("metaKey", "metaValue");
// 检查元数据是否设置并记录结果
if (alt.hasMeta("metaKey")) {
    alt.log("元数据被设置"); 
} else {
    alt.log("元数据未设置");
}
// 获取并保存元数据的值
const metaValue = alt.getMeta("metaKey");  
// 删除元数据
alt.deleteMeta("metaKey");
```

将 全局元(global meta) 绑定应用于实体

```js
alt.on("playerConnect", (player) => {
    // Set a player meta (single-sided, cross-resource) with a given key and value
    player.setMeta("username", player.name);
    // 检查元是否已设置并记录结果
    if (player.hasMeta("username")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
    }
    // 获取并保存元的值
    const metaValue = player.getMeta("username");
    // 删除元
    player.deleteMeta("metaKey");
});
```

监听 全局元(global meta) 修改

```js
alt.on("globalMetaChange", (key, newValue, oldValue) => {
    // 当应用于alt的元发生更改时调用此事件
    // 您也可以使用 alt.hasMeta 和 alt.getMeta 方法自行检查
    if (alt.hasMeta("metaKey")) {
        const metaValue = alt.getMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

# [TS](#tab/tab1-1)

接口中的 元(meta) 声明(可以添加到.ts或.d.ts文件中)

```ts
declare module "alt-shared" {
  // 通过接口合并扩展接口
  export interface ICustomGlobalMeta {
    metaKey: string
  }
}
```

用法与JS中相同：

**Server & Client**

```ts
const metaValue /* string | undefined */  = alt.getMeta("metaKey");
```

# [C#](#tab/tab1-2)

应用全局元(global meta)

```cs
// 使用给定的键和值设置全局元(单面/跨资源)
Alt.SetMetaData("metaKey", "metaValue");
// 检查元是否已设置并记录结果
if (Alt.HasMetaData("metaKey"))
{
    Alt.Log("Meta is set");
}
else
{
    Alt.Log("Meta isn't set");
}
// 获取并保存元的值
// 使用C#时，需要传递元值的类型
var valueIsFetched = Alt.GetMetaData<string>("metaKey", out var metaData);
// 删除元
Alt.DeleteMetaData("metaKey");
```

将全局元绑定应用于实体

```cs
Alt.OnPlayerConnect += (player, reason) =>
{
    // 使用给定的键和值设置玩家元(单面/交叉资源)
    player.SetMetaData("username", player.Name);
    // 检查元是否已设置并记录结果
    if (player.HasMetaData("username"))
    {
        Alt.Log("Meta is set");
    }
    else
    {
        Alt.Log("Meta isn't set");
    }
    // 获取并保存元的值
    // 使用C#时，需要传递元值的类型
    var valueIsFetched = player.GetMetaData<string>("username", out var metaData);
    // 删除元
    player.DeleteMetaData("metaKey");
};
```

侦听实体的元更改

```cs
Alt.OnMetaDataChange += (entity, key, value) =>
{
    // 当应用于实体的元发生更改时调用此事件
    // 您也可以使用 entity.HasMetaData 或 entity.GetMetaData 方法自己检查它
    if (entity.HasMetaData("metaKey")) {
        var valueIsFetched = entity.GetMetaData<string>("metaKey", out var metaValue);
        // <使用该值做一些事情>
    }
};
```
---

## 本地元(Local Meta)

本地元总是设置为播放器实例，并且只能在服务器端进行修改。
这些数据可以从所有资源访问，但在客户端只能从设置元的相应播放器访问。

# [JS](#tab/tab2-0)

**Server**

将本地元绑定应用于实体

```js
alt.on("playerConnect", (player) => {
    // 使用给定的键和值设置玩家本地元(由服务器设置，同步到服务器和客户端，跨资源)
    player.setLocalMeta("username", player.name);
    // 检查是否设置了本地元并记录结果
    if (player.hasLocalMeta("username")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
    }
    // 获取并保存本地元的值
    const metaValue = player.getLocalMeta("username");
    // 删除元(仅在服务器上可用)
    player.deleteLocalMeta("username");
});
```

侦听本地元更改

```js
alt.on("localMetaChange", (player, key, newValue, oldValue) => {
    // 本地元更改时调用此事件
    // 您也可以使用p layer.hasLocalMeta 和 player.getLocalMeta 方法自行检查
    if (player.hasLocalMeta("metaKey")) {
        const metaValue = player.getLocalMeta("metaKey");
        // <使用该值做一些事>
    }
});
```

**Client**

检查是否设置了本地元

```js
alt.on("spawned", () => {
    // 检查是否设置了本地元
    if (alt.hasLocalMeta("username")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
        return;
    }
    // 获取并保存本地元的值
    const metaValue = alt.getLocalMeta("username");
});
```

侦听本地元更改

```js
alt.on("localMetaChange", (key, newValue, oldValue) => {
    // 本地元更改时调用此事件
    // 您也可以使用 alt.hasLocalMeta 和 alt.getLocalMeta 方法自行检查
    if (alt.hasLocalMeta("metaKey")) {
        const metaValue = alt.getLocalMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

# [TS](#tab/tab2-1)

接口中的元声明(可以添加到.ts或.d.ts文件中)

```ts
declare module "alt-shared" {
  // 通过接口合并扩展接口
  export interface ICustomPlayerLocalMeta {
    username: string
  }
}
```

用法与JS中相同:

**Server**

```ts
const metaValue /* string | undefined */ = player.getLocalMeta("username");
```

**Client**

```ts
const metaValue /* string | undefined */ = alt.getLocalMeta("username");
```

# [C#](#tab/tab2-2)

**Server**

将本地元绑定应用于实体

```cs
Alt.OnPlayerConnect += (player, reason) =>
{
    // 使用给定键的和值设置玩家本地元(由服务器设置，同步到服务器和客户端，跨资源)
    player.SetLocalMetaData("username", player.Name);
    // 检查是否设置了本地元并记录结果
    if (player.HasLocalMetaData("username"))
    {
        Alt.Log("Meta is set");
    }
    else
    {
        Alt.Log("Meta isn't set");
    }
    // 获取并保存本地元的值
    // 使用C#时，需要传递元值的类型
    var valueIsFetched = player.GetLocalMetaData<string>("username", out var metaData);
    // 删除元
    player.DeleteLocalMetaData("username");
};
```
---

## 同步元(Synced Meta)

同步元被分发到所有资源和客户端，可以通过以下方式使用:

- 全局（应用于alt）
- 绑定到实体（应用于玩家或车辆）

# [JS](#tab/tab3-0)

**Server**

应用全局同步元

```js
// 使用给定的键和值设置全局同步元(服务器和客户端，跨资源)
alt.setSyncedMeta("metaKey", "metaValue");
// 检查元是否已设置并记录结果
if (alt.hasSyncedMeta("metaKey")) {
    alt.log("Meta is set");
} else {
    alt.log("Meta isn't set");
}
// 获取并保存元的值
const metaValue = alt.getSyncedMeta("metaKey");
// 删除元
alt.deleteSyncedMeta("metaKey");
```

Applying a global synced meta bound to a entity

```js
alt.on("playerConnect", (player) => {
    // 使用给定的键和值设置同步元(服务器和客户端，跨资源)
    player.setSyncedMeta("metaKey", "metaValue");
    // 检查元是否已设置并记录结果
    if (player.hasSyncedMeta("metaKey")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
    }
    // 获取并保存元的值
    const metaValue = player.getSyncedMeta("metaKey");
});
```

侦听同步的元更改

```js
alt.on("syncedMetaChange", (entity, key, newValue, oldValue) => {
    // 当应用于实体的同步元发生更改时调用此事件
    // 您也可以使用 entity.hasSyncedMeta(key) 方法自己进行检查
    if (entity.hasSyncedMeta("metaKey")) {
        const metaValue = entity.getSyncedMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

**Client**

正在检查是否设置了全局同步元

```js
alt.on("spawned", () => {
    // 检查是否设置了全局同步元
    if (alt.hasSyncedMeta("metaKey")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
    }
    // 获取并保存元的值
    const metaValue = alt.getSyncedMeta("metaKey");
});
```

侦听全局同步的元更改

```js
alt.on("globalSyncedMetaChange", (key, newValue, oldValue) => {
    // 当应用于 alt 的同步元发生更改时调用此事件
    // 您也可以使用 alt.hasSyncedMeta(key) 方法自行检查
    if (alt.hasSyncedMeta("metaKey")) {
        const metaValue = alt.getSyncedMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

侦听同步的元更改

```js
alt.on("syncedMetaChange", (entity, key, newValue, oldValue) => {
    // 当应用于实体的同步元发生更改时调用此事件
    // 您也可以使用 entity.hasSyncedMeta(key) 方法自己进行检查
    if (entity.hasSyncedMeta("metaKey")) {
        const metaValue = entity.getSyncedMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

# [TS](#tab/tab3-1)

接口中的元声明(可以添加到.ts或.d.ts文件中)

```ts
declare module "alt-shared" {
  // 通过接口合并扩展接口

  export interface ICustomGlobalSyncedMeta {
    globalMetaKey: string
  }

  export interface ICustomPlayerSyncedMeta {
    playerMetaKey: string
  }
}
```

用法与JS中相同:

**Server & Client**

```ts
const globalMetaValue /* string | undefined */ = alt.getSyncedMeta("globalMetaKey");
const playerMetaValue /* string | undefined */ = player.getSyncedMeta("playerMetaKey");
```

# [C#](#tab/tab3-2)

**Server**

应用全局元

```cs
Alt.OnPlayerConnect += (player, reason) =>
{
    // 使用给定键的和值设置玩家本地元(由服务器设置，同步到服务器和客户端，跨资源)
    player.SetLocalMetaData("username", player.Name);
    // 检查是否设置了本地元并记录结果
    if (player.HasLocalMetaData("username"))
    {
        Alt.Log("Meta is set");
    }
    else
    {
        Alt.Log("Meta isn't set");
    }
    // 获取并保存本地元的值
    // 使用C#时，需要传递元值的类型
    var valueIsFetched = player.GetLocalMetaData<string>("username", out var metaData);
    // 删除元
    player.DeleteMetaData("username");
};
```
---

## 流同步元数据(Stream Synced Meta)

同步元被分发到所有资源和客户端，并且可以绑定到实体使用。
它基本上与同步元相同，但只发送到实体流范围内的客户端。

# [JS](#tab/tab4-0)

**Server**

将流同步元绑定到实体

```js
alt.on("playerConnect", (player) => {
    // 使用给定的密钥和值设置流同步元(服务器和客户端，跨资源)
    player.setStreamSyncedMeta("metaKey", "metaValue");
    // 检查元是否已设置并记录结果
    if (player.hasStreamSyncedMeta("metaKey")) {
        alt.log("Meta is set");
    } else {
        alt.log("Meta isn't set");
    }
    // 获取并保存元的值
    const metaValue = player.getStreamSyncedMeta("metaKey");
    // 删除元
    player.deleteStreamSyncedMeta("metaKey");
});
```

侦听流同步的元更改

```js
alt.on("streamSyncedMetaChange", (entity, key, newValue, oldValue) => {
    // 当应用于实体的流同步元发生更改时调用此事件
    // 您也可以使用 entity.hasStreamSyncedMeta(key) 方法自行检查
    if (entity.hasStreamSyncedMeta("metaKey")) {
        const metaValue = entity.getStreamSyncedMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

**Client**

侦听流同步的元更改 (basically the same as on the server side)

```js
alt.on("streamSyncedMetaChange", (entity, key, newValue, oldValue) => {
    // 当应用于实体的流同步元发生更改时调用此事件
    // 您也可以使用 entity.hasStreamSyncedMeta(key) 方法自行检查
    if (entity.hasStreamSyncedMeta("metaKey")) {
        const metaValue = entity.getStreamSyncedMeta("metaKey");
        // <使用该值做一些事情>
    }
});
```

# [TS](#tab/tab4-1)

接口中的元声明(可以添加到.ts或.d.ts文件中)

```ts
declare module "alt-shared" {
  // 通过接口合并扩展接口

  // alt.Player and alt.Vehicle
  export interface ICustomEntityStreamSyncedMeta {
    entityMetaKey: string
  }

  // only alt.Player
  export interface ICustomPlayerStreamSyncedMeta {
    playerMetaKey: number
  }

  // only alt.Vehicle
  export interface ICustomVehicleStreamSyncedMeta {
    vehicleMetaKey: boolean
  }
}
```

用法与JS中相同:

**Server & Client**

```ts
const entityMetaValue /* string | undefined */ = entity.getSyncedMeta("entityMetaKey");
const playerMetaValue /* number | undefined */ = player.getSyncedMeta("playerMetaKey");
const vehicleMetaValue /* boolean | undefined */ = vehicle.getSyncedMeta("vehicleMetaKey");
```

# [C#](#tab/tab4-2)

**Server**

将流同步元绑定到实体

```cs
Alt.OnPlayerConnect += (player, reason) =>
{
    // 使用给定的键和值设置流同步元(由服务器设置，同步到服务器和客户端，跨资源)
    player.SetStreamSyncedMetaData("username", player.Name);
    // Check if the stream synced meta is set and log the result
    if (player.HasStreamSyncedMetaData("username"))
    {
        Alt.Log("Meta is set");
    }
    else
    {
        Alt.Log("Meta isn't set");
    }
    // 获取并保存流同步元数据的值
    // 使用C#时，需要传递元值的类型
    var valueIsFetched = player.GetStreamSyncedMetaData<string>("username", out var metaData);
    // 删除元
    player.DeleteStreamSyncedMetaData("username");
};
```
---

## 谈论(Remarks)

> [!WARNING] 
> 只有当客户端还不知道（新的）数据时，才会调用元更改事件。
> 如果此数据已知，则必须独立查询和处理，例如通过 gameEntityCreate 事件。

让我们以 **streamSyncedMeta** 的示例案例为例,其中键为 `trackWidth` 用于设置车辆的轨道宽度。

仅当客户端还不知道此值时,才会调用事件 `streamSyncedMetaChange` \  
如果现在离开流范围,然后返回到该范围,则会创建车辆,但不会触发 'streamSyncedMetaChange' 事件。
要确保始终应用值,还必须使用 `gameEntityCreate` 事件。

这里是一个示例:

# [Server](#tab/tab5-0)

```js
const vehicle = new alt.Vehicle("t20", 0, 0, 72, 0, 0, 0);
vehicle.setStreamSyncedMeta("trackWidth", 0.85);
```

# [Client](#tab/tab5-1)

```js
// 这在元数据首次向客户端通信时调用
alt.on("streamSyncedMetaChange", (entity, key, newValue, oldValue) => {
    if (key === "trackWidth") setTrackWidth(entity, newValue);
});

// 这在客户端上首次创建车辆时立即调用
// 在这里,我们检查车辆是否具有元数据,如果是,则处理它
alt.on("gameEntityCreate", (entity) => {
    if (entity.hasStreamSyncedMeta("trackWidth")) {
        const trackWidth = entity.getStreamSyncedMeta("trackWidth");
        setTrackWidth(entity, trackWidth);
    }
});

// 如果适用,可以使用 `gameEntityDestroy` 事件来撤消只在实体存在时才存在的更改
alt.on("gameEntityDestroy", (entity) => {
   // 例如,删除元数据更改
});

// 为了避免在两个事件中重复代码,我们将其移动到自己的函数中
function setTrackWidth(entity, trackWidth) {
    for (let i = 0; i < entity.wheelsCount; i++) {
        entity.setWheelTrackWidth(i, trackWidth);
    }
}
```
