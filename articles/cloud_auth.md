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

### 云身份认证系统是如何工作的?
当客户端连接到服务器时，可以向alt:V后端发出请求。\
此请求检索客户端Rockstar帐户的相关唯一标识符，然后可以使用该标识符验证和验证客户端。

### 客户端可以伪造或篡改唯一标识符吗?
不，由于我们的后端参与了这个过程，客户不能欺骗或操纵他们的唯一标识符。\
每个操作都通过我们的后端进行验证，确保每个玩家的唯一ID保持正确和真实。

### 什么可以改变唯一的云身份验证标识符?
唯一的云身份验证标识符严格链接到您的Rockstar帐户，因此更改Rockstar帐户将更改ID。这个ID不依赖于你的硬件或任何软件。\
如果需要更严格的验证方法，您可以考虑加入额外的检查，比如硬件ID和其他选项。

### 切换Rockstar账户会改变id吗?
是的，切换Rockstar账户会改变ID，因为唯一的ID直接绑定到你的Rockstar账户。\
如果需要更严格的验证，则可以实施其他措施，如合并硬件ID和其他选项。

### 我可以创建自己的云认证实现或在alt:V之外使用它吗？
不，创建自己的云身份验证实现是不可能的。\
我们的后端在这个过程中起着关键作用，出于安全原因，我们不会透露有关其工作的详细信息。\
这使得无法重现一个独立的云认证系统。

## Usage

[Player method in JS API reference](https://docs.altv.mp/js/api/alt-server.Player.html#_altmp_altv_types_alt_server_Player_requestCloudID) <br>
[Player method in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.ConnectionInfo.html#AltV_Net_Elements_Entities_ConnectionInfo_RequestCloudId) <br>
[IConnectionInfo in JS API reference](https://docs.altv.mp/js/api/alt-server.IConnectionInfo.html#_altmp_altv_types_alt_server_IConnectionInfo_requestCloudID)
[IConnectionInfo in C# API reference](https://docs.altv.mp/cs/api/AltV.Net.Elements.Entities.IConnectionInfo.html#AltV_Net_Elements_Entities_IConnectionInfo_RequestCloudId)

记住，这个方法是异步的。

> [!WARNING]
> 此方法将在身份验证失败时拒绝 (with await = throw exception)!

如果身份验证成功，则使用标识符字符串解析方法

在发生失败的情况下，该方法将以一个字符串形式拒绝，用以识别出问题所在。

### 可能出现的错误：

- `NO_LICENSE` - 说明玩家没有有效的GTA V许可证(玩家很快会被服务器自动踢掉)
- `SERVICE_UNAVAILABLE` - 表示云认证服务关闭。
- `INTERNAL_ERROR`, `WRONG_REQUEST` 或任何其他字符串 - 用于识别内部服务问题，不应该发生的问题。如果收到此类信息，请在我们的[Issue tracker](https://github.com/altmp/altv-issues/issues)上提交一个问题。

## 示例

```js
let id;

try {
    id = await player.requestCloudID();
} catch(e) {
    if (e === 'NO_LICENSE') {
        player.kick('Unauthorized');
        return;
    } else {
        if (e !== 'SERVICE_UNAVAILABLE') reportProblem('Invalid Cloud Auth response: ' + e);
        // 回退到另一个登录方法
    }
}
```