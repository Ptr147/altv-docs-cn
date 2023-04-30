# 字节码模块

字节码模块在将文件发送到客户端之前,将您的客户端代码转换为V8字节码。因此,  
它使客户端释放原始源文件变得**完全不可能**。

*但是*,转储生成的字节码和反转逻辑仍然是可能的,所以该模块不完全保护您的代码免受盗取。  
*但是*,反转实际工作的字节码代码又是一个非常耗时和繁重的过程,所以这将是**非常困难**的。
此外,V8字节码的结构不断变化,几乎**完全没有文档**,目前没有已知的公共"工具"  
可以将V8字节码反转为有效的JavaScript代码。

**建议仅在生产环境中使用字节码模块。这是因为源文件对客户端不可见,因此无法生成堆栈跟踪或任何其他额外的调试信息,使调试错误和其他问题变得更加困难。**  

## 设置字节码模块

设置字节码模块是一个相对简单的过程,只需要按照以下步骤操作:

1. [下载字节码模块](#下载),并将下载的`.dll`或`.so`放在`modules`目录中  
2. 在`server.toml`中将`js-bytecode-module`添加到`modules`数组中  
3. *对于您要使用字节码模块的每个资源*,将`resource.toml`中的`client-type`设置为`jsb`  
4. 启动服务器  

现在每次启动服务器时,您应该会看到类似于此的日志
```
Converted file to bytecode: path/file.js
```

仍然有一个可选步骤,但这个步骤**仅**在代码使用**动态导入**来导入任何其他文件时需要,  
这些文件未被导入。由于模块会从主文件开始递归编译其所有依赖项(使用`import`语句导入的模块),  
所以模块无法知道动态导入的文件。所以这些文件必须手动指定,以便也对其进行编译。  
要做到这一点,只需:

5. 将所有应该额外编译的文件添加到`resource.toml`中的`extra-compile-files`数组中  

该数组接受与`client-files`选项相同的路径,因此例如可以包含整个文件夹。  

## 下载  

该模块可以从以下源下载:
1. [官方alt:V下载页面](https://altv.mp/#/downloads)  
2. CDN直接 - [Windows](http://cdn.alt-mp.com/js-bytecode-module/release/x64_win32/js-bytecode-module.dll) / [Linux](http://cdn.alt-mp.com/js-bytecode-module/release/x64_linux/libjs-bytecode-module.so)
3. [字节码模块版本发布页面](https://github.com/altmp/altv-js-bytecode/releases)

## 故障排除   

### 帮助!我有一个错误提示说...  

首先, **确保您的代码没有任何错误运行**。如果代码在顶层代码(如语法错误)中有任何错误,  
该模块将无法编译。  

如果在不使用字节码模块的情况下代码正常运行,但模块仍报告错误,请联系我们的[Discord](https://discord.altv.mp)。