# 资源

Resources are one of the main parts of the alt:V server. They handle the data and gamescripts for server- and clientside used by the alt:V server.<br>
Resources are represented as a subfolder of the `resources/` folder in the alt:V server root.

# resource.toml

A resource (folder) is required to contain at minimum a `resource.toml` configuration file (except rpf resources, see rpf section below for more info). Depending on which resource type you are using, the properties you specify in the config file may differ between each other.<br>
A resource has a folder structure like this:

> [!div class="nohljsln"]
>```
> resources/
>   [RESOURCE_NAME]/
>     resource.toml
>```

请记住,这仅仅是资源正常工作的最低要求。 每种类型都有自定义规范。

# 类型  

目前可用的类型有:

## js  

此资源类型提供了在服务器端和客户端以JavaScript编写游戏脚本的功能。
更多信息请参见:[入门](~/altv-types/docs/articles/index.md)  

## csharp  

此资源类型提供了在服务器端以C#编写游戏脚本,在客户端以JavaScript编写游戏脚本的功能。  
更多信息请参见:[入门](~/coreclr-module/docs/articles/index.md)  

## dlc

此资源类型用于向客户端提供修改后的内容(如车辆、MLO、服装)。您需要一个`stream.toml`来使此类型工作。

Since v15 this resource type also allows to replace GTA base files. See `overrides` below.

stream.toml

           
|                   键                      |                                        描述                                       |  
| :----------------------------------------: | :-----------------------------------------------------------------------------------------------------: |
| files     | 要发送到客户端的文件的路径。                             |
| meta     | 元文件和相应的数据文件的路径(格式:[PATH = DATA_FILE_TAG])。      |
| gxt       | gxt文件的路径。|
| overrides | Game file mount path and path to the replacement (Format: [MOUNT_PATH = REPLACEMENT_PATH]) |

### 替换(Replacements)

To override a file you need to first it's full mount path. Mount path is specified in format `group:/path/to/file`. See [list of mounts](https://gist.githubusercontent.com/martonp96/59f731446c7f17db3f400c2be458c4a4/raw/38b73edc0a61cbd24383d75a41c515718aafded6/gistfile1.txt). 

Example replacement:

File structure:
> [!div class="nohljsln"]
>```
> resources/
>   [RESOURCE_NAME]/
>     replaces/
>       water_empty.xml
>     resource.toml
>```

stream.toml 
> [!div class="nohljsln"]
>```
> [overrides]
> 'commoncrc:/data/levels/gta5/water.xml' = 'replaces/water_empty.xml'
> ```

## asset-pack

此资源类型是为其他资源提供内容(如图片、视频、网页、JSON文件)。 此内容可以通过多种方式提供:

1. Webview:`http://assets/[packName]/filePath`  
2. Javascript: `import '@[packName]/filePath';`  
3. FileAPI:使用`@[packName]`之前的路径。

> [!div class="nohljsln"]
>```
> type = asset-pack
> client-files = [ ... ]
> ```

## rpf

This resource type allows you to load .rpf DLC directly. All .rpf archives are loaded before game start, and therefore can load anything what normal singleplayer DLC can.

resource.toml

| Key      | Description           |
| :------: | :-------------------: |
| type     | `rpf`                 |
| main     | Path to the .rpf file |

To simplify loading process, you can load rpf DLCs without creating `resource.toml`. Just create a resource folder and put a `dlc.rpf` file in it.

Example:
- Download https://www.gta5-mods.com/maps/community-mission-row-pd/download/75282
- Create `server/resources/nice_police_mlo` folder
- Copy `/RageMP and SP/mrpd/dlc.rpf` to `nice_police_mlo` folder
- Add `nice_police_mlo` to resources list in server.toml