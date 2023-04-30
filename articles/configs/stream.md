# `stream.toml` 配置文件

`stream.toml` 配置文件对每个DLC资源(例如新车辆)都是必需的。它包含一些配置选项,如GXT值和元文件。

以下是 `stream.toml` 中所有配置选项及其用途的列表:
```toml  
# 包含应为此DLC加载的所有文件
files = [
    # 您还可以使用通配符模式提供对整个目录的访问权限
    "stream/assets/*"  
]
# 您的DLC使用的元文件
[meta]  
"stream/carcols.meta" = "CARCOLS_FILE"  
"stream/vehicles.meta" = "VEHICLE_METADATA_FILE"
"stream/handling.meta" = "HANDLING_FILE"

# 您的DLC的GXT条目
[gxt]  
"myveh" = "我的车辆"
```
