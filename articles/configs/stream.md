# `stream.toml` 配置文件

每个 dlc 资源(例如新车辆)都需要 `stream.toml` 配置文件。它具有一些配置选项,如 GXT 值和元文件。

这里是 `stream.toml` 的所有配置选项及其用途的列表:
```toml
# 包含要为此 dlc 加载的所有文件的数组
files = [
    # 您还可以使用 glob 模式提供对整个目录的访问权限
    "stream/assets/*" 
]
# 您的 dlc 使用的元文件
[meta]
"stream/carcols.meta" = "CARCOLS_FILE"
"stream/vehicles.meta" = "VEHICLE_METADATA_FILE"
"stream/handling.meta" = "HANDLING_FILE"

# 您的 dlc 的 GXT 条目 
[gxt]
"myveh" = "我的车辆"

```
