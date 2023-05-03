# 设置EarlyAuth  

Early Auth是一个用于保护alt:V服务器免受可能的DDoS攻击的函数,此函数仅在announce设置为true时工作。  
其工作方式如下:客户端通过服务器列表连接到服务器,early auth函数打开一个外部登录页面,玩家可以在其中进行身份验证。如果身份验证成功,登录页面会向客户端发送带有令牌的POST请求。然后,网站会在服务器的防火墙中将客户端的IP白名单化。现在,客户端可以连接,游戏服务器可以通过给定的身份验证令牌识别客户端。身份验证令牌需要由登录页面生成。

> [!TIP]  
> 要在rc或dev分支中测试Early Auth,请确保您的客户端[altv.toml](~/articles/configs/client.md)文件包含以下设置:
> ```yaml
> earlyAuthTestURL = 'http://url_or_ip_to_your_early_auth:3000/earlyauth.html'
> ```

# 步骤 教程

## 示例值

在本教程中使用以下示例值:

| 键     |             值             |             描述             |
| ------ | :-------------------------------: | :-------------------------------: |
|   token           |   0123456789                                  |   用于主列表的令牌(需要通过Discord从Masterbot请求)         |
|   earlyAuthUrl    |   https://login.example.com/index.html        |   外部登录页面的URL                                      |
|   authToken       |   authToken0123456789                         |   网站生成的令牌                                         |

## 步骤示例

1. 将 `announce = true` 添加到 `server.toml` 中。
2. 将您的令牌添加到 `server.toml` 中的 `token = 0123456789` 。   
3. 将 `useEarlyAuth = true` 添加到 `server.toml` 中。  
4. 将 `earlyAuthUrl = 'https://login.example.com/index.html'` 添加到 `server.toml` 中。
5. 将 <span style="color:red">Function 1</span> 添加到您的登录页面,成功登录后触发此函数和防火墙白名单函数。
6. 将 authToken 检查添加到 playerConnect 事件,例如 <span style="color:red">Function 2</span>  
7. 现在 earlyAuth 登录已准备就绪。

<span style="color:red">Function 1</span>

```html
<script>
    function setToken(token) {
        alt.emit('pushToken', token);
    }
</script>
```

<span style="color:red">Function 2</span>

```js
alt.on("playerConnect", (player) => {
     if(player.authToken != "authToken0123456789")
     {
         player.kick();
     }
});
```

## 请求alt:V名称  
要从玩家处请求 alt:V 名称,您可以在 early auth 中使用下面的代码段。

```html
<script>
    alt.emit("requestPlayerName")

    alt.on("playerName", (name) => {
        //在 earlyauth 中随意使用同样的内容  
    })
</script>
```

## 存储和检索数据  
由于CEF缓存在重新启动之间被清除,所以有一种备选方法可以使用alt:V LocalStorage持久存储数据。  
以下代码段说明了如何使用它:
```html
<script>
    // 订阅 localStorage 事件以获取请求的数据  
    alt.on("localStorage", (key, value) => {
        if (key === "lastLogin") {
            // 对检索的数据执行某些操作,存储在变量"value"中
        }
    });
    
    // 请求 LocalStorage 的键的数据  
    // alt.emit("requestLocalStorage", key);
    alt.emit("requestLocalStorage", "lastLogin");
    
    // 在 LocalStorage 中存储数据  
    // alt.emit("setLocalStorage", key, value);
    alt.emit("setLocalStorage", "lastLogin", Date.now());
</script>
```
此数据现在将持久存储,直到删除alt:V客户端的缓存文件夹。

## 额外信息
如果要关闭 early auth 窗口,必须使用 `alt.emit('closeMe')`
