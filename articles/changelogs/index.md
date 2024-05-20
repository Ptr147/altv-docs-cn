# 16.0.0-Release

## 16.0.153
> [!WARNING]
> 此更新是最新的。

> [!TIP]
> 此更新于 2024 年 4 月 16 日发布

### 客户端
- GTA 的兼容性 - 更新 3179

### 启动器
- 已添加缓存路径配置
- 如果设置了游戏路径，已添加平台自动检测功能
- 更新了中国的镜像
- 移除了 skipLauncherPatcher 配置

## 16.0.147
> [!TIP]
> 此更新于 2024 年 3 月 30 日发布

### 服务端
- 为流式比特集添加了缓存功能
- 改进了播放器连接/断开事件的性能
- 提高了尺寸检查的性能
- 改进了实体流处理器的性能和稳定性
- 改进了实体容器
- 改进了迁移管理器的性能
- 改进了单元管理器的实体获取
- 改进了属性同步错误日志
- 改进了同步限制错误日志
- 改进了 alt.getEntitiesInDimension 和 alt.GetEntitiesInDimension()。
- 修正了单元格形状管理器
- 修正了玩家到玩家附件偏移
- 修正了轮子获取器的默认值
- 修正了 wheelsCount 获取器 [#2062](https://github.com/altmp/altv-issues/issues/2062)
- 修复了 onStreamSyncedMetaChange 事件 [#2231](https://github.com/altmp/altv-issues/issues/2231)
- 修正了玩家骨骼的顺序
- 修复了玩家与踏板的连接
- 修复了踏板骨骼的 ID
- 修复了组件启动顺序

### 客户端
- 已将高跟鞋和道具的可绘制项从 255 增加到 4294967296
- 改进了内部调试程序
- 改进了远程音频加载超时
- 将备份功能迁移至启动（启动器），以减少权限相关问题
- 添加了新的 medal.tv api
- 添加了运行时大小验证
- 添加了编译时模式初始化
- 添加了按索引获取流文件名的函数
- 修复了绕行函数中缺少 NULL 检查的问题
- 修复了改装车辆导致的崩溃
- 修复了替换数据文件失效的问题
- 修正了调用堆栈日志记录
- 修复了 isTalking 超时
- 修复了 rml 元素移除调用
- 修复了 rml 元素创建调用
- 修正了 rml 元素事件 [#2209](https://github.com/altmp/altv-issues/issues/2209)
- 修正了 gameEntityDestroy 事件 [#2236](https://github.com/altmp/altv-issues/issues/2236)
- 修正了静态模式初始化
- 修复了带衣服、道具和帽子的车辆退出动画重置问题
- 修正了音频输出中的内存泄漏
- 修复了武器损坏事件
- 修复了本地行人、本地物体和本地车辆的游戏实体创建和销毁事件
- 改进了 rmlui 的互斥机制
- 改进了音频输出的移除
- 改进了微冻结

## 16.0.0
> [!TIP]
> 此更新于 2024 年 1 月 12 日发布

### 服务端
- 在 server.toml 中添加了配置 `masterlistRelay`, 其默认值为 `“”`
- 为server.toml添加了配置`mtu`，其默认值为`1400
- 为server.toml添加了配置`enableSyncedMetaData`，它的默认值是`true
- 为 JS 模块的 `ConnectionInfo` 添加了 `versionMajor` 属性，为 C# 模块的 `IConnectionInfo` 添加了 `VersionMajor` 属性
- 为 JS-Module 的 `ConnectionInfo` 添加了 `versionMinor` 属性，为 C#-Module 的 `IConnectionInfo` 添加了 `VersionMinor` 属性
- 为 JS-Module 的 `IVehicleModel` 添加了 `modelHash` 属性，为 C#-Module 的 `VehicleModelInfo` 添加了 `ModelHash` 属性
- 为 JS-Module 的 `WeaponModelInfo` 添加了属性 `modelName
- 为 JS-Module 的 `alt.Vehicle` 添加了 `setBadge` 方法，为 C#-Module 的 `Alt.Vehicle` 添加了 `setBadge` 方法
- 为 JS-Module 和 C#-Module 添加了类 `VehicleBadgePosition` 。
- 为 JS 模块添加了`playerConnectDenied`事件，为 C# 模块添加了`OnPlayerConnectDeniedEvent`事件
- 为 JS-Module 中的 `player.addDecoration` 添加了参数 `count` 和为 C#-Module 中的 `Player.AddDecoration` 添加了参数 `count 
- 废弃了 JS 模块中 `ConnectionInfo` 的 `build` 属性
- 从 C# 模块中的 `IConnectionInfo` 中移除属性 `MajorVersion


#### 客户端
- 为 JS 模块添加了 `alt.getServerTime` 方法，为 C# 模块添加了 `alt.ServerTime` 方法
- 已添加方法 `alt.getPoolSize` 到 JS-Module 和 `alt.GetPoolSize` 到 C#-Module
- 为 JS-Module 和 C#-Module 添加了方法 `alt.getPoolCount
- 为 JS 模块添加了 `alt.getPoolEntities` 方法，为 C# 模块添加了 `alt.GetPoolEntities` 方法
- 为 JS-Module 和 C#-Module 添加了 `alt.getVoicePlayers` 方法
- 为 JS 模块添加了 `alt.removeVoicePlayer` 方法，为 C# 模块添加了 `alt.RemoveVoicePlayer` 方法
- 为 JS-Module 添加了 `alt.getVoiceSpatialVolume` 方法，为 C#-Module 添加了 `alt.GetVoiceSpatialVolume` 方法
- 已添加方法 `alt.setVoiceSpatialVolume` 到 JS-Module 和 `alt.SetVoiceSpatialVolume` 到 C#-Module
- 为 JS-Module 和 C#-Module 添加了 `alt.getVoiceNonSpatialVolume` 方法
- 已添加方法 `alt.setVoiceNonSpatialVolume` 到 JS-Module 和 `alt.SetVoiceNonSpatialVolume` 到 C#-Module
- 为 JS-Module 和 C#-Module 添加了方法 `alt.addVoiceFilter`.
- 为 JS 模块添加了 `alt.removeVoiceFilter` 方法，为 C# 模块添加了 `alt.RemoveVoiceFilter` 方法
- 为 JS 模块添加了 `alt.getVoiceFilter` 方法，为 C# 模块添加了 `alt.GetVoiceFilter` 方法
- 为 JS-Module 和 C#-Module 添加了方法 `alt.updateClipContext
- 为 JS-Module 的 `Entity` 添加了 `getSyncInfo` 方法，为 C#-Module 的 `IEntity` 添加了 `SyncInfo` 方法
- 为 JS-Module 的 `AudioFilter` 添加了 `addVolumeEffect` 方法 和 为 C#-Module 的 `AudioFilter` 添加了 `AddVolumeEffect` 方法
- 为 JS 模块添加了`ISyncInfo`类，为 C# 模块添加了`SyncInfo`类
- 为 TextLabel 添加了 xml 样式支持 ([more details](https://docs.altv.mp/articles/textlabel.html))

