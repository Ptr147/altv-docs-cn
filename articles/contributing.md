# 如何贡献

本文档包含为alt:V文档做出贡献的一组指南。  

## 文档结构

结构由一个或多个链接到核心仓库的仓库组成(您当前所在的仓库)。

核心仓库负责在文档生成过程中包括链接的存储库。我们使用DocFx生成器完成此任务。

每个链接的仓库都有类似于此的目录结构:
```
└───docs
    │   build.cmd
    │   build.ps1
    │   docfx.json
    │   index.md
    │   toc.yml
    │
    ├───api
    └───articles
```

`docfx.json` 是主要的配置文件,用于本地测试目的。如果您想了解更多关于其格式的信息,请参阅[此处](https://dotnet.github.io/docfx/tutorial/docfx.exe_user_manual.html#3-docfxjson-format)。  

`build.cmd`和`build.ps1`是用PowerShell编写的脚本,用于在发布更改到核心存储库之前本地测试文档。稍后我们将解释它们的工作原理以及如何使用它们。

`index.md`和`toc.yml`是所需的主要概念文档(有些例外情况)。  

`toc.yml`文件(目录或目录)是YAML文档,它指定当前目录中的文档结构,并负责链接其他子目录以方便导航。如果您想了解更多关于其格式的信息,请参阅此[文章](https://dotnet.github.io/docfx/tutorial/docfx.exe_user_manual.html#23-generate-documentation-command-docfx-build)。

`index.md`文件是Markdown文档,出现在每个目录中,充当起始页面。  

`api` 包含API文档,这些文档大部分是从项目的源代码自动生成的,然后由文档生成器合并。当自动生成时,我们不直接编辑这些文件,而是使用称为覆盖文档的文件,这允许我们覆盖文件而无需编辑源。  

`articles` 包含概念文档,这将是您花费大部分时间的地方。  

与`api`文件夹相反,该文件夹中的文件是由贡献者手动创建的。

## 链接的存储库

我们在文档中包括社区创建的模块,如果您想为特定语言的文档做出贡献,请切换到下面发布的存储库:

| 语言 | 存储库  
| ---      | ---
| JS       | [](https://github.com/altmp/altv-types)
| C#       | [](https://github.com/FabianTerhorst/coreclr-module)

## 构建工具  

如前所述,每个链接的存储库都应包含构建工具来轻松测试和验证您的工作。  

目前,我们只提供在安装了PowerShell的Windows上做这件事的方法。  

> [!WARNING]
> 我们和当前的文档生成器(尽管在DocFx v3中可能会改变)不支持其他操作系统。

实际代码在`build.ps1`中定义,其中`build.cmd`用作包装程序(默认情况下,在Windows资源管理器中不允许通过简单地打开它们来执行PowerShell脚本)。  

命令行参数:

| 参数          | 描述                                           | 用法  
| ---             | ---                                         | ---
| `port `         | 更改DocFx用于托管网站的端口。                 | `./build.ps1 -port 80`
| `cleanMetadata` | 每次运行都会删除所有生成的输出。               | `./build.ps1 -cleanMetadata`
| `cleanOnly`     | 只删除依赖项。                                | `./build.ps1 -cleanOnly`

## 初始设置

要开始贡献的冒险,您需要安装程序,例如Git、带有npm包管理器的Node,如果您想使用C#文档,请使用具有".NET桌面开发"工作负载的Visual Studio或带有".NET桌面生成工具"工作负载的Visual Studio生成工具。  

1. 复刻存储库 - https://github.com/altmp/altv-docs/fork
1. 在Windows上打开终端。
1. 转到您要工作的目录。
1. 克隆您的存储库,用您的GitHub用户名替换USER:
    ```bash
    git clone https://github.com/USER/altv-docs
    ```
1. 将 altmp/altv-docs 存储库添加为远程存储库 
    ```bash
    git remote add upstream https://github.com/altmp/altv-docs
    ```
1. 如果您的复刻仓库中有 `docs` 目录,请切换到该目录。其中应包含文档结构中描述的文件。  

## 生成和测试

执行`build.ps1`脚本时,首先我们下载依赖项,例如DocFx工具、插件、模板或链接的存储库等。

> [!WARNING]
> 请记住,目前我们不会自动更新依赖项,但手动删除它们应该会触发重新下载。

下一步是生成项目元数据,这对API文档很有必要。  

最后,执行DocFx生成,生成的站点默认情况下在 `localhost:8080` 可用。  

要停止运行脚本或使用最新更改更新网站,只需通过按`Ctrl+C`组合键终止运行脚本。之后,如果需要,可以再次执行脚本。  

如果到目前为止一切正常工作,您现在可以开始贡献。

> [!TIP]
> 强烈建议您应该在单独的分支上工作，而不是在 `master`上工作，这样您就可以在存储库更新时轻松地合并更改。

## 创建文档

在开始 创建/编辑 文件之前,您应该首先考虑要添加什么。  

> [!WARNING]
> 您不能编写与GTA:V相关的文档,如 modding、数据等。这些内容属于我们的wiki网站。  

标题应尽可能简短、易于理解且最好有吸引力。  

要做的另一件事是为文档选择适当的类别。您应该能在 ``articles/`` 目录中找到可用类别,它们看起来像文件夹。

现在,如果您计划只制作一篇文档,您可以转到步骤[单篇文档](#单篇文档)。  
如果不是,您必须为它们创建类别(如果您没有找到一个类别)。遵循步骤[系列文档](#系列文档)中定义的说明。  

### 系列文档

和之前一样,您必须为类别想出一个名称,它也应尽可能简短、易于理解且最好有吸引力(它也不能与文档名称相同)。  

当您有一个名称时,在 ``articles/`` 目录中创建一个带有该名称的新文件夹。  

> [!TIP]
> 请记住,名称应该是小写的,只包含字母数字,空格应替换为连字符。  

您可以进入该文件夹并在其中创建两个名为 ``index.md`` 和 ``toc.yml`` 的文件。  
这些文件是必需的,尽管 ``index.md`` 现在可以为空。  

在我们专注于实际制作文档之前,我们必须使您的类别在目录中可见。 

让我们打开 ``articles/toc.yml`` 文件,它现在类似于这样:
```yaml
- name: Getting Started
  href: getting-started/
  topicHref: getting-started/index.md
- name: How to Contribute
  href: contributing.md

```

我们将复制 ``Getting Started`` 并在下面附加修改后的详细信息。  

> [!CAUTION]
> 确保 ``How to Contribute``项始终位于底部。

> Before:
> ```yaml
> - name: Getting Started
>   href: getting-started/
>   topicHref: getting-started/index.md
> - name: How to Contribute
>   href: contributing.md
> 
> ```
> After:
> ```yaml
> - name: Getting Started
>   href: getting-started/
>   topicHref: getting-started/index.md
> - name: Fancy Category
>   href: fancy-category/
>   topicHref: fancy-category/index.md
> - name: How to Contribute
>   href: contributing.md
> 
> ```

如果到目前为止您做的一切都正确无误,您的类别现在应该可见。  

让我们回到之前创建的 ``index.md`` 文件。

我们类别中的索引文件应包含简短的摘要和  
何种文章被展示。

现在我们可以转到下面的[下一步](#单篇文档)。  

### 单篇文档

现在您可以在 ``articles/`` 目录(或您的类别目录)中创建以标题为文件名和 `.md` 扩展名的文件。    

> [!TIP]
> 请记住,文件名应该是小写的,只包含字母数字,空格应替换为连字符。

现在,该文件将包含在生成过程中,但它在目录(也称为ToC)中尚不可见。  

要实现这一点,我们需要在同一目录中的 `toc.yml` 文件中添加新条目,以使您的文件完全可见。

> 之前:
> ```yaml
> - name: Getting Started
>   href: getting-started/
>   topicHref: getting-started/index.md
> - name: How to Contribute
>   href: contributing.md
> 
> ```
> 之后:
> ```yaml
> - name: Getting Started
>   href: getting-started/
>   topicHref: getting-started/index.md
> - name: Fancy Article
>   href: fancy-article.md
> - name: How to Contribute
>   href: contributing.md
> 
> ```

如果您正在创建 __系列文档__ 您的``toc.yml``文件目前很可能是空的。  
您不需要复制上面的代码,您只需要添加新项目。

现在我们可以切换到文件内容和第一个标题格式为 ``Heading 1`` 的文本。

> 案例:
> ```md
> # Contribution
> 
> ... content here ...
> 
> ```

如果您遇到Markdown的问题,您可以在[此处](https://www.markdownguide.org/basic-syntax)刷新记忆。  

## 发布您的工作  

完成对更改的工作后,您可以提交拉取请求,将您的更改合并到存储库中并包含在文档中。  

提交拉取请求之前,请确保您已验证工作。  

如果您有疑问或需要帮助解决某些问题,请随时在alt:V Discord服务器[#scripting-offtopic](https://discord.com/channels/371265202378899476/576771706119520287)询问。
