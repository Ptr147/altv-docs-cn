  
#玩家连接队列  

本文将解释9.0更新中添加的玩家连接队列。  

## 什么是连接队列

由于当前底层网络连接,队列中的每个玩家实际上将占用一个插槽并增加玩家数。<br>当玩家队列激活时,一旦连接被接受,就会触发playerConnect事件。<br> 
连接队列将试图连接到服务器的玩家排队。然后您可以在任何时候接受或拒绝即将到来的连接。  

## 用法

### 配置

您必须在server.toml中设置/添加以下字段
```toml
connectionQueue = true  
```

### 玩家队列事件当您接受/拒绝连接或玩家通过取消连接断开连接时,会触发connectionQueueRemove事件。  

| 事件名称                    | 描述                                                 | 
| --------------------- | ------------------------------------------------------------------ | 
| connectionQueueAdd    | 一旦玩家被添加到连接队列,就会触发事件                     | 
| connectionQueueRemove | 一旦玩家从连接队列中移除,就会触发事件                 |   

### 连接信息您可以在``connectionQueueAdd``和``connectionQueueRemove``事件的第一个参数中访问连接信息类实例。

函数

| 函数名称                 | 描述                                               | 
| ------------------------------ | -------------------------------------------------------------------- |
| connectionInfo.accept()        | 接受玩家连接到服务器。                         |
| connectionInfo.decline(reason) | 拒绝玩家连接到服务器,并可选提供原因。    |

属性

| 属性名称               | 描述                                            |
| --------------------------- | ----------------------------------------------------- | 
| connectionInfo.name         | 玩家名称。                                          |
| connectionInfo.socialID     | 玩家社交俱乐部id。                                | 
| connectionInfo.discordID    | 玩家Discord id。                                    |
| connectionInfo.ip           | 玩家IP地址。                                     | 
| connectionInfo.hwidHash     | 玩家hwidHash。                                      | 
| connectionInfo.hwidExHash   | 玩家hwidExHash。                                    |
| connectionInfo.authToken    | 如果使用了提前认证,则为玩家认证令牌。             | 
| connectionInfo.isDebug      | 玩家客户端是否处于调试模式。    | 
| connectionInfo.branch       | 玩家客户端分支。                                 | 
| connectionInfo.build        | 玩家客户端生成。                                  |
| connectionInfo.cdnUrl       | 如果使用,则为CDN URL。                                      | 
| connectionInfo.passwordHash | 在服务器连接模态框中输入的密码哈希。 |

### 脚本用法

# [Javascript](#tab/tabid-1)

```js
async function getDataFromDatabase() {
    return new Promise(resolve => setTimeout(resolve(true), 5000));
}

async function processPlayerInQueue(connectionInfo) {
    if (await getDataFromDatabase());
        connectionInfo.accept();
}

alt.on('connectionQueueAdd', processPlayerInQueue);
alt.on('connectionQueueRemove', (connectionInfo) => {
    console.log("玩家已从队列中移除,即使现在我接受连接,它也已处理!");
});
```

# [Typescript](#tab/tabid-2)

```ts
async function getDataFromDatabase(): Promise<true> {
    return new Promise(resolve => setTimeout(resolve(true), 5000));
}

async function processPlayerInQueue(connectionInfo: alt.ConnectionInfo): Promise<void> {
    if (await getDataFromDatabase());
        connectionInfo.accept();
}

alt.on('connectionQueueAdd', processPlayerInQueue);
alt.on('connectionQueueRemove', (connectionInfo: alt.ConnectionInfo) => {
    console.log("玩家已从队列中移除,即使现在我接受连接,它也已处理!");
});
```

# [C#](#tab/tabid-3)

```csharp
using AltV.Net.Async;
using AltV.Net.Types;

namespace Example
{
    class ExampleResource : AsyncResource
    {
        private async Task<bool> GetDataFromDatabase()
        {
            await Task.Delay(5000);
            return true;
        }

        private async Task OnConnectionQueueAdd(IConnectionInfo connectionInfo)
        {
            if (await this.GetDataFromDatabase())
                connectionInfo.Accept();
        }

        private Task OnConnectionQueueRemove(IConnectionInfo _)
        {
            Console.WriteLine("玩家已从队列中移除,即使现在我接受连接,它也已处理!");
            return Task.CompletedTask;
        }

        public override void OnStart()
        {
            AltAsync.OnConnectionQueueAdd += this.OnConnectionQueueAdd;
            AltAsync.OnConnectionQueueRemove += this.OnConnectionQueueRemove;
        }

        public override void OnStop()
        {
        }
    }
}
```
