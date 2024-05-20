# 设置CDN 

CDN(内容分发网络)是一个加快连接用户资源下载和减轻服务器负载的功能。CDN可以通过任何支持HTTP/HTTPS的文件托管服务器提供。

它的工作流程如下:客户端连接到CDN服务器,CDN服务器为客户端提供资源文件。下载完成后,客户端连接到alt:V服务器。

# 步骤教程

## 示例

在本教程中使用以下示例:

| 键   |       值        |         描述          | 
| :--: | :-----------: | :-----------------: |  
| cdnUrl  | http://connect.example.com/ | CDN服务器的URL。 |
| 服务器IP   |   127.0.0.1         | alt:V服务器的IP地址。|
| 服务器端口 |   7788                | alt:V服务器的端口。   |

## 步骤示例

1. 在`server.toml`中添加`useCdn = true`。
2. 在`server.toml`中添加`cdnUrl = "http://connect.example.com"`。
3. 使用参数`--host 127.0.0.1 --port 7788 --justpack`启动您的服务器。
4. 将`cdn_upload`文件夹中的所有生成文件添加到您的CDN服务器,文件应放在域的根目录下。(您应该能够在`http://connect.example.com/connect.json`下访问`connect.json`)。 
5. 用于直接连接,使用您的cdnUrl `connect.example.com`。

## CDN meta file

> [!CAUTION]
> alt:V 16.0 的进程中新增了一个名为 `cdn-meta.bin`的文件。\
> 确保将此文件保存在服务器的 bin 目录中。

cdn-meta.bin "文件是服务器 bin 文件夹中的一个重要文件，其中包含有关自定义车辆、踏板车和武器的信息。

该文件有两个主要用途：
- 阻止服务器在每次启动时解析这些数据
- 您无需将这些资源直接放在服务器上，而只需放在 CDN 上
