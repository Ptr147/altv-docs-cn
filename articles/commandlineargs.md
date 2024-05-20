# 命令行参数

命令行参数意味着您可能需要将它们传递给单独的'.exe'或编译的服务器或客户端版本。

`<exe 或文件> --option1 --option2 --option3`

## 服务器启动选项

| 键   |             描述            |             注释                 |  
| ------ | :-------------------------------: | :-------------------------------: |
|   --config [路径]             |   服务器配置文件路径([路径]需要替换为配置路径)   |   |
|   --logfile [路径]            |   日志文件路径,将只输出一个日志文件到指定路径,而不是将它们放入日志文件夹([路径]需要替换为日志路径)         |   优先级高于--logfolder   |
|   --logfolder [路径]          |   日志文件夹路径([路径]需要替换为日志文件夹路径)   |       优先级低于--logfile   |
|   --no-logfile                |   禁用日志记录到日志文件   |   |
|   --extra-res-folder [路径]   |   额外资源文件夹路径([路径]需要替换为额外资源文件夹路径)   |   |
|   --justpack                  |   在根文件夹中创建包和resources.json供cdn下载   |   | 
|   --host [主机]               |   指定服务器应绑定的主机([主机]需要替换为主机)   |   |
|   --port [端口]               |   指定服务器应绑定的端口([端口]需要替换为端口)   |   |
|   --convert-config-format     |   将所有".cfg"配置文件转换为".toml"格式   |   |
|   --no-regenerate-rpf-cache   |   不为 rpf 文件生成缓存   |   |
|   --help                      |   显示所有cli参数的帮助文本    |   |

## 客户端启动选项

| 键       |             描述           | 
| ------    | :-------------------------------: |
|   -noupdate                 |   跳过alt:V更新(仅在dev [https://docs.altv.mp/articles/branches.html#dev-development)分支上有效) |
|   -connecturl [url]         |   直接连接到指定的url([url]需要替换为连接url)   |
|   -debug                    |   启用调试模式   |
|   -branch [分支]          |   设置要使用的分支([分支]需要替换为release、rc或dev)   |
|   -skipprocessconfirmation  |   跳过关闭已运行的GTA V/alt:V实例的确认消息   |  
|   -linux                    |   启用 linux 模式   |

## 服务端命令

| 键       |             描述           | 
| ------    | :-------------------------------: |
|   start [resourcename]    |   按名称启动服务器资源     |
|   stop [resourcename]     |   按名称停止服务器资源     |
|   restart [resourcename]  |   按名称重启服务器资源 (如果[`客户端文件'](https://docs.altv.mp/articles/configs/resource.html) 被更改，客户端也会重新下载这些文件)  |
