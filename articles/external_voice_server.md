# 外部语音服务器  

根据玩家数量,可能需要在外部服务器上运行语音服务器,例如,将网络流量路由到另一个网络卡或为语音服务器使用专用资源。  

本文介绍外部语音服务器的设置和与alt:V服务器的连接。

## 设置外部语音服务器  

首先，必须下载语音服务器，这可以通过 [altv-pkg](https://github.com/altmp/altv-pkg) 自动完成，也可以手动完成，相关链接可在 Windows 服务器和 Linux 服务器主题下的 [CDN](https://docs.altv.mp/articles/cdn_links.html) 文章中找到

下载后,语音服务器可以放置在任何服务器上(它也可以是alt:V服务器的同一设备)。  

要配置它,创建一个名为voice.toml的文件,添加以下内容并相应地调整它:  

```toml
# 用于接收alt:V服务器语音连接的外部语音服务器的IP地址  
# 可以是专用IP或0.0.0.0以接受所有连接  
host = '0.0.0.0'  
# 与上述IP一起使用的端口  
port = 7798  
# 用于接收客户端连接的IP地址  
# 应为公共IP或0.0.0.0以接受所有连接  
playerHost = '0.0.0.0'  
# 客户端连接使用的端口  
playerPort = 7799  
# alt:V服务器与外部语音服务器共享的密钥  
secret = 1234567890
```

现在您只需启动语音服务器并继续集成 alt:V 服务器。

## 集成外部语音服务器

要集成 alt:V 服务器,只需对 server.toml 进行少量调整:

```toml
# Define the map bound size
mapBoundsMinX = -10000
mapBoundsMinY = -10000
mapBoundsMaxX = 65536
mapBoundsMaxY = 65536
mapCellAreaSize = 100 #smaller areas uses more ram and less cpu

[voice]
streamingTickRate = 10
# 语音服务器的比特率  
bitrate = 64000  
# 外部服务器的密钥(仅在使用externalHost时需要)  
# 密钥必须仅由数字组成  
externalSecret = 1234567890  
# 外部主机地址(如果语音服务器在同一台机器上,请留空127.0.0.1)  
externalHost = "127.0.0.1"  
# 外部主机端口  
externalPort = 7798  
# 外部主机公共地址(应为您的服务器IP地址,而不是localhost!)  
externalPublicHost = "xx.xx.xx.xx"  
# 外部主机公共端口  
externalPublicPort = 7799
```
