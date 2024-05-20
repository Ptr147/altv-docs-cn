# 分支

在 alt:V 中有多个开发分支,代表 alt:V 客户端和服务器的不同开发状态。服务器和客户端需要在同一分支上才能连接。对普通用户来说,推荐使用发布分支。

# 分支概览

这里将概述可用分支:

## 发布

<img src="https://cdn.alt-mp.com/static/images/updates/branch_release.png" width="100px"/>

该分支用于生产，被视为稳定分支。这是主目录可用的唯一分支。如果运行的是公共服务器，则该服务器应运行此分支。图标为绿色。

## RC(发布候选)

<img src="https://cdn.alt-mp.com/static/images/updates/branch_rc.png" width="100px"/>

此分支用于下一次更新，每次更新发布前至少 1-2 周都会在此分支中发布。它可以被视为基本稳定的分支，最后的错误会在合并到发布版本之前得到修复。如果您运行的是公共服务器，则应在更新前使用此分支将服务器代码更新到 alt:V 的新版本。图标为橙色。

## Dev(开发)

<img src="https://cdn.alt-mp.com/static/images/updates/branch_dev.png" width="100px"/>

此分支包含最新功能，并非所有功能都会移至 RC。此分支可能包含错误和未完成的功能，功能也可能发生变化。如果你想要最新的功能，可以在此分支上进行测试，帮助我们查找错误。图标为蓝色。

## 内部

<img src="https://cdn.alt-mp.com/static/images/updates/branch_internal.png" width="100px"/>

该分支不公开，只有 alt:V 的开发者可以使用。该图标为紫色。

# 如何更改分支?

更改分支很简单，对于客户端，你需要打开 altv.toml，然后将分支更改为你想使用的值（release、rc、dev）。对于服务器，你需要从 alt:V 网站或使用 [altv-pkg](https://github.com/altmp/altv-pkg)，为你想要的分支重新下载新文件。s