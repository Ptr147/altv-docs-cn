# 排除服务器故障   

有时候在运行服务器时,会出现一些问题。大多数这些问题都是重复的并且易于解决。因此,本文收集了最常见的错误及其解决方案。

## 已知问题和解决方案  

#### 警告:要加载ES模块,请在package.json中设置 `"type":"module"` 或使用.mjs扩展名。

此问题是由package.json中缺少 `type` 配置引起的。要解决此问题,您需要在package.json中添加 `"type":"module"` 。  

#### 无法加载"js-module.dll" Win32 错误:127 (7f)

如果您进入alt:V服务器目录,您很可能会找到一个 `libnode.dll` 文件。为了解决此问题,您需要删除此文件。

#### 模块过时,模块使用SDK vXX,服务器需要SDK vXX

有时候您会遇到此错误,特别是如果有新版本发布。通常,更新您的服务器以下载最新的alt:V服务器文件(包括最新的 `csharp-module.dll` 或 `js-module.dll`)就足够了。

#### 读取server.toml第xx行,第xx列时出错。原因:无效标记  

此问题通常发生在字符串未放在双引号内时。例如,如果您的网站URL未在 `server.toml` 内的双引号内,将导致此问题。要在您的 `server.toml` 内找到问题,您应该注意错误消息,它会给您确切的行号。  

#### 主服务器连接失败。无效令牌  

Inside your `server.toml` you can find a configuration variable called `announce`. If this is set to `true`, you will need a **valid** token. Therefore, you need to copy a master list token from [Server Manager](https://my.alt-mp.com).

请记住,在开发阶段,您通常不需要主机列表公告。您可以在 `server.toml` 内将 `announce`标 志设置为 `false` 。  

#### 未处理异常:收到来自...的未处理异常  

When you use C# on your server-side, you will encounter this problem sometimes. You can easily solve this problem by updating the alt:V NuGet packages and rebuilding your resource.

#### Error Invalid get_hostfxr_path

When you use C# on your server-side and doesn't have installed .NET runtime on your computer, you will encounter this problem. You need to download .NET runtime from official website and restart alt:V server.
