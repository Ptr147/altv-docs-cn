# The `server.toml` configuration file

The `server.toml` file is the most important configuration file, it is the main configuration file for your whole server, and will define
important configurations like server name, amount of slots, loaded resources and more.

## Basic configuration for development of JS resources
```toml
# An array of all modules (specific language support on server-side) that should be loaded
modules = ["js-module"]
# An array of all resources that should be loaded
resources = ["example"]

# Enables reconnect command, CEF debug and other developer features
debug = true
```

Here is a list of all configuration options for the `server.toml` and what they are used for:

> [!WARNING]
> DO NOT copy all options into your server config, only use the options you **really** need.

```toml
# 您的服务器显示名称 
name = "我的服务器名称"
# 您的服务器绑定地址(通常为 0.0.0.0)
host = "0.0.0.0" 
# 您的服务器端口(默认为 7788)
port = 7788 
# 可以同时在您的服务器上游戏的玩家数量 
players = 256 
# 连接您的服务器所需的密码 
password = "我的机密密码"
# 服务器是否在alt:V客户端的主列表中可见 
announce = true 
# 为主列表宣布服务器的令牌
token = "令牌" 
# 您的服务器正在运行的游戏模式 
gamemode = "自由"
# 您的服务器网站 
website = "example.com"
# 您的服务器语言 
language = "英语" 
# 您的服务器描述 
description = "我有趣的服务器"
# 是否应允许调试模式(调试模式允许调试功能,如重新连接或CEF调试器)
debug = false 
# 实体的流媒体距离 
streamingDistance = 400 
# 实体的迁移距离 
migrationDistance = 150 
# 超时倍数(必须≥1) 
timeout = 1 
# 到达 announceRetryErrorAttempts 时使用的延迟(以毫秒为单位) 
announceRetryErrorDelay = 10000 
# announceRetryErrorDelay 将使用之前的最大重试次数 
announceRetryErrorAttempts = 50
# 可以使用相同 IP 地址连接的最大玩家数 
duplicatePlayers = 4096# 定义地图边界大小 

# Enable or disable syncedMetadata
enableSyncedMetaData = true

# Key for shared resources. Can be used to share resources between multiple servers, so users don't have to download them separatedly
sharedProjectKey = "altv-shared"
# Display name for shared resources bundle (visible in alt:V client settings)
sharedProjectName = "alt:V shared"

# Max size of client to server script events in bytes
maxClientScriptEventSize = 8192
# Max size of server to client script events in bytes
maxServerScriptEventSize = 524288

#When false unknown rpc events will result in a kick
allowUnknownRPCEvents = true

# Define the map bound size
mapBoundsMinX = -10000
mapBoundsMinY = -10000
mapBoundsMaxX = 65536
mapBoundsMaxY = 65536 
mapCellAreaSize = 100 # 较小的区域使用更多的 RAM 和更少的 CPU

# 定义 colshape 计算的速率(毫秒)
colShapeTickRate = 200  

# 定义服务器使用的日志流 (console, file, stdconsole)
logStreams = [ "console", "file" ]
entityWorkerCount = 8 

# 您的服务器的标签(最多 4 个)
tags = [
    "自由",
    "有趣"
]  # 您的服务器是否应启用连接队列。 
# 需要接受或拒绝连接的 ConnectionQueueAdd 和 ConnectionQueueRemove服务器端事件 
connectionQueue = false
# 您的服务器是否应使用早期身份验证
useEarlyAuth = false
# 早期身份验证登录页面的 URL(仅在 useEarlyAuth 为 true 时使用)
earlyAuthUrl = "https://example.com/login"
# 您的服务器是否应使用 CDN
useCdn = false
# The url for the CDN page
# Outdated Notation: "https://cdn.example.com"
cdnUrl = "cdn.example.com:443"

# alt:V 服务器是否应在连接时发送所有客户端的玩家名称
sendPlayerNames = true

# 在玩家连接后 自动将玩家生成至 0,0,71
spawnAfterConnect = true

# 应加载的所有资源的数组
resources = [
    "myResource",
    # 从 alt:V 10.0 开始,您还可以使用子目录中的资源 
    "vehicles/firstVehicle",
    "vehicles/secondVehicle",
    # Since alt:V 15.0 you can even use * wildcard character to use all folders as resources
    "vehicles/*",
]
# 应加载的所有模块的数组
modules = [
    "js-module",
    "csharp-module"
]

# If dlcWhitelist contains any dlcs, it will only enable these specific game DLCs. Useful when you want to maintain a list of 'supported' DLCs on your server because of game limits, ID shifts, etc. Below defined setting would make sure to only load the mpBeach, mpBusiness and patchday27ng R* dlc contents
# See complete DLC list at go.altv.mp/dlc-list 
dlcWhitelist = [
    "mpBeach",
    "mpBusiness",
    "patchday27ng"
]

# Obfuscate resource names
hashClientResourceName = true

# Disables creation of props marked as "optional"
disableOptionalProps = false

# The amount of server side managed entities per type that can be streamed at the same time per player. If more than the set amount of entities are in streaming range, the closest n entities (as defined below) of the specific type will be streamed. Changing these values can cause performance and stability issues.
[maxStreaming]
peds = 128 # Max 220, shared type for server side created NPC peds + player peds
objects = 120 # Max 120, server side created objects
vehicles = 128  # Max 220, server side created vehicles
entities = 128 # Defined the max limit of entities, indepent of type, that can be streamed at the same time

# Configure GTA game pool sizes to extend them. These game pools define the limits of certain aspects in the game, extending them can and will cause stability and performance issues. Please test changes carefully.
# See this article for a complete list of game pools: https://docs.altv.mp/gta/articles/tutorials/overwrite_gameconfig.html
[pools]
"DrawableStore" = 240420 # Example of raised DrawableStore pool

# 实体创建/流媒体系统等的分析
[worldProfiler]
port = 7797
host = "0.0.0.0"

# Configure the amount of threads used for different kind of processing in the alt:V server. You should in total never configure more threads than your server hardware has threads. 
# For example, when your hardware has 12 threads, use 8 for sync send, 2 for receive and all other at 1.
[threads]
streamer = 1 # Processing of streamed entities
migration = 1 # Processing of netowner calculations
syncSend = 8 # Processing of sending sync data, should be always the highest amount
syncReceive = 2 # Processing of receiving sync data, should be around 1/4 of syncSend

[antiCheat]
# Enables server-side weapon checks
# For example, if a weapon is given via giveWeaponToPed native it won't be synced
weaponSwitch = true

# Enables collision checks so natives like setEntityNoCollisionEntity will not work
collision = true

# Settings related to js-module
[js-module]
# "https://nodejs.org/api/cli.html#--enable-source-maps"
source-maps = true  
# "https://nodejs.org/api/cli.html#--heap-prof"
heap-profiler = true
# 启用分析器
profiler = true
# "https://nodejs.org/api/cli.html#--experimental-global-webcrypto"
global-webcrypto = true
# "https://nodejs.org/api/cli.html#--experimental-network-imports"
network-imports = true
# Add extra cli arguments to the node environment "https://nodejs.org/api/cli.html"
extra-cli-args = ["--max-old-space-size=8192"]

# Settings related to c#-module
[csharp-module]
# Disable dependency (NuGet) check and download at server startup, this is recommended if you have a bad connection to the NuGet server (e.g china)
disableDependencyDownload = true

# Voice configuration (category needs atleast to exist in configuration, to enable voice chat)
# See this article for more info: https://docs.altv.mp/articles/voice.html
[voice]
# 语音服务器的比特率  
bitrate = 64000
# 外部服务器的密码(仅在使用 externalHost 时需要)
# 密码必须仅由数字组成  
externalSecret = "1234567890"
# 外部主机地址(如果语音服务器在同一台机器上,请留空 127.0.0.1)
externalHost = "127.0.0.1"  
# 外部主机端口
externalPort = 7798
# 外部公共主机地址(应为您的服务器的 IP 地址,而不是 localhost!)
externalPublicHost = "xx.xx.xx.xx"
# 外部公共主机端口  
externalPublicPort = 7799
