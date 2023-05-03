# Discord OAuth

Discord OAuth(开放授权)用于通过Discord帐户识别用户。  
使用内置的OAuth令牌请求,您将接收一个令牌,可用于根据Discord API识别用户。

> [!TIP]
> 令牌验证的返回数据可以在[Discord API文档](https://discord.com/developers/docs/resources/user#user-object)中找到。

# 一步步教程  

## 准备  

在使用OAuth令牌之前,您需要创建Discord应用程序。  
<br>
要创建新应用程序,您需要:  
- 打开[Discord 开发者门户](https://discord.com/developers/applications)并使用Discord帐户登录  
- 单击右上角的"新建应用程序"按钮  
- 填写您的应用程序名称并单击创建  
- 转到"OAuth2",然后转到"常规"  
- 单击"添加重定向",输入 `http://127.0.0.1` 并单击底部工具栏中的"保存更改"  
- 复制顶部的"客户端ID" - 下一步需要它  

## 客户端代码

```js
// 在这里输入 client id
const DISCORD_APP_ID = '1234567890';

async function getOAuthToken() {
    try {
        const token = await alt.Discord.requestOAuth2Token(DISCORD_APP_ID);
        alt.emitServer('token', token);
    } catch (e) {
        // 错误可能是由于无效的应用程序id、不协调的服务器问题或用户拒绝访问。
        alt.logError(e);
    }
}

getOAuthToken();
```

## 服务端代码

```js
alt.onClient('token', async (player, token) => {
    // 使用对Discord api的GET请求验证令牌
    const request = await axios
        .get('https://discordapp.com/api/users/@me', {
            'headers': {
                'Content-Type': 'application/x-www-form-urlencoded',
                'Authorization': `Bearer ${token}`
            }
        })
        .catch((err) => {
            return null;
        });

    // 检查请求是否成功，以及是否包括必要的属性
    if (!request || !request.data || !request.data.id || !request.data.username) {
        player.kick('Authorization failed');
        return;
    }

    // 返回属性示例
    alt.log(`Id: ${request.data.id}`);
    alt.log(`Name: ${request.data.username}#${request.data.discriminator}`);
});
```