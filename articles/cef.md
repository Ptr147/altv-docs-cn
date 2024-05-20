# CEF - 基于Chromium的嵌入式框架   

Chromium嵌入式框架(也称为CEF)是一个浏览器引擎,用于alt:V渲染客户端网页和资源——就像在普通浏览器中一样。CEF将使用系统语言环境。  

## 调试模式

要在客户端启用 CEF 的调试模式/服务器，必须在 [客户端配置] (~/articles/configs/client.md) 中启用 `debug = true` 设置。

然后,调试服务器在`localhost:9222`(在Chromium或较旧版本的Chrome)或`chrome://inspect`(在较新版本的Chrome)中可用。

> [!提示]
> CEF不工作?[点击此处](~/articles/troubleshooting/client.md#webview-not-rendering-on-linux)查看故障排除步骤。