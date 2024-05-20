# 权限系统  

权限按资源逐一设置（在`resource.toml`中），必须由加入服务器的用户接受

## 权限类型  

| 权限       |                                     描述                                      |
| ---------------- | :----------------------------------------------------------------------------------: |  
| Screen Capture(屏幕截图)   | 这允许对alt:V 进程(仅限GTA)和任何Web视图进行屏幕截图 |
| WebRTC           |                        这允许JS内部进行点对点RTC                        |
| Clipboard Access(剪贴板访问) |                    这允许复制内容到用户的剪贴板                    |
| Extended Voice API(扩展语音API) | 这允许手动控制语音，如通过服务器激活麦克风、静音等 |

## 必需与可选权限  

权限可以是可选的或必需的。当它们是可选的时,即使用户不同意它们也仍允许玩家加入服务器,但是您将无法访问权限所涵盖的功能。必需的权限的行为不同,如果用户不接受它们,则无法访问服务器。


`resource.toml`
```toml
required-permissions = [
    "Screen Capture",
    "WebRTC",
    "Clipboard Access"
]

optional-permissions = [
    "Screen Capture",
    "WebRTC",
    "Clipboard Access"
]
```

> [!提示]
> 要检查用户是否批准了相应的权限,可以使用方法 [alt.getPermissionState](https://docs.altv.mp/js/api/alt-client.html#_altmp_altv_types_alt_client_getPermissionState)。
