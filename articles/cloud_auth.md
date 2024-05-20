# 云身份验证

随着alt:V更新15有一个新的唯一的玩家标识符验证我们的云认证服务。与socialID, socialClubName, hwidHash或hwidExHash不同，新的标识符不能伪造，因此可以用于身份验证和其他敏感系统。

> [!WARNING]
>要在服务器上使用云身份验证(Cloud Auth)功能，您需要在[alt:V Patreon](https://go.altv.mp/patreon)上拥有Silver+套餐。请在[Server Manager](https://my.alt-mp.com/)上链接您的Patreon账户，并确保已添加您的服务器并生成了新的服务器令牌以获取该功能的访问权限。

## FAQ

### 什么是云身份认证?
Cloud Auth是由alt:V开发的唯一身份识别解决方案。\
这个系统使用alt:V后端为每个玩家创建一个特殊的ID，它直接链接到他们的Rockstar帐户。

### 云身份认证有多安全?
云认证系统具有较高的安全性。\
该功能利用我们的后端来设置和确认客户端标识符，确保安全连接，防止伪造，并为该过程增加了额外的安全层级。

#### 云认证系统是如何工作的？
当客户端连接到服务器时，可向 alt:V 后台发出请求。\
该请求会检索客户 Rockstar 账户的相关唯一标识符，然后可用于验证客户身份。

### 客户端可以伪造或篡改唯一标识符吗?
不，由于我们的后端参与了这个过程，客户不能欺骗或操纵他们的唯一标识符。\
每个操作都通过我们的后端进行验证，确保每个玩家的唯一ID保持正确和真实。

#### 什么情况会改变唯一云认证标识符？
唯一云认证标识符与您的Rockstar账户严格关联，因此更改Rockstar账户就会更改该标识符。
该 ID 不依赖于您的硬件或任何软件。
如果需要更严格的验证方法，您可以考虑加入额外的检查，如硬件 ID 和其他选项。

#### 我能创建自己的云认证实施或在 alt:V 之外使用它吗？
不，创建自己的云认证实施是不可能的。
我们的后台在整个过程中起着关键作用，出于安全考虑，我们不会公开其工作原理的详细信息。
这使得我们无法复制一个独立的云认证系统。

## 用法

[Player cloudID in JS API reference](https://docs.altv.mp/js/api/alt-server.Player.html#_altmp_altv_types_alt_server_Player_cloudID) <br>
[Player CloudId in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.Player.html#AltV_Net_Elements_Entities_Player_CloudId) <br>
[IConnectionInfo cloudID in JS API reference](https://docs.altv.mp/js/api/alt-server.IConnectionInfo.html#_altmp_altv_types_alt_server_IConnectionInfo_cloudID) <br>
[IConnectionInfo CloudId in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.IConnectionInfo.html#AltV_Net_Elements_Entities_IConnectionInfo_CloudId)

记住，这个方法是异步的。

> [!WARNING]
> 此方法将在身份验证失败时拒绝 (with await = throw exception)!

如果身份验证成功，则使用标识符字符串解析方法

在发生失败的情况下，该方法将以一个字符串形式拒绝，用以识别出问题所在。
认证成功时，云 id 属性返回非空字符串

如果失败，云 id 属性返回空字符串，结果属性返回错误代码

[Player cloudAuthResult in JS API reference](https://docs.altv.mp/js/api/alt-server.Player.html#_altmp_altv_types_alt_server_Player_cloudAuthResult) <br>
[Player CloudAuthResult in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.Player.html#AltV_Net_Elements_Entities_Player_CloudAuthResult) <br>
[IConnectionInfo cloudAuthResult in JS API reference](https://docs.altv.mp/js/api/alt-server.IConnectionInfo.html#_altmp_altv_types_alt_server_IConnectionInfo_cloudAuthResult) <br>
[IConnectionInfo CloudAuthResult in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.IConnectionInfo.html#AltV_Net_Elements_Entities_IConnectionInfo_CloudAuthResult)

### 可能出现的错误：

- `NO_LICENSE` - 说明玩家没有有效的GTA V许可证(玩家很快会被服务器自动踢掉)
- `SERVICE_UNAVAILABLE` - 表示云认证服务关闭。
- `INTERNAL_ERROR`, `WRONG_REQUEST` 或任何其他字符串 - 用于识别内部服务问题，不应该发生的问题。如果收到此类信息，请在我们的[问题跟踪](https://github.com/altmp/altv-issues/issues)上提交一个问题。
- `NO_BENEFIT` - 服务器没有解锁所需的功能
- `VERIFY_FAILED` - 由于不明原因，您的服务器无法从 alt:V 服务中获取玩家的云 ID

[Enum in JS API reference](https://docs.altv.mp/js/api/alt-server.CloudAuthResult.html)\
[Enum in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Data.CloudAuthResult.html)

## 示例

```js
import * as alt from 'alt-server';

alt.on('playerConnect', (player) => {
    const result = player.cloudAuthResult;
    // 0 means success
    if (result !== 0) {
        alt.logError('Failed to authorize player:', player.name, 'error code:', result);
        return;
    }
    
    alt.log('Player:', player.name, 'connected with cloud id:', player.cloudID);
});
```
