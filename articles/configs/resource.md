#  `resource.toml` 配置文件

`resource.toml` 配置文件对于每个资源都是必需的,它必须位于资源的主目录中。<br>
它包含一些配置选项,用于配置您的资源,例如资源的类型及其依赖项。

以下是 `resource.toml` 中所有配置选项及其用途的列表:
```toml
# 资源的服务器端类型(必须加载正确的模块)  
type = "js"
# 资源的客户端类型(必须加载正确的模块)
client-type = "js"  
# 服务器启动时将加载的主要服务器端文件   
main = "server.js"
# 客户端启动时将加载的主要客户端文件
client-main = "client.js"
# 客户端可以访问的文件(client-main 文件不需要包含在此处)
client-files = [
    "myFile.js",
    # 您还可以使用通配符模式提供对整个目录的访问权限
    "client/*"  
]
# 在服务器上播放所需的权限(必须接受这些权限,否则玩家无法加入)
required-permissions = [
    "Screen Capture" # 屏幕截图
]
# 在服务器上播放的可选权限(用户可以拒绝这些权限)  
optional-permissions = [
    "WebRTC"
]  
# 此资源的依赖项(在资源加载之前加载所有依赖项)
deps = [
    "myOtherResource"
]
```
