---
title: Homebrew
date: 2022-03-30 17:15:17
tags:
- Homebrew 
- mac
---



### 一、什么是 Homebrew ？

[Homebrew](https://link.zhihu.com/?target=https%3A//brew.sh/index_zh-cn.html) 官网有一句话：Homebrew complements macOS. （ Homebrew 使 macOS 更完整。）Homebrew 是 macOS 的套件管理工具，是高效下载软件的一种方法，相当于 Linux 下的 `yum`、`apt-get` 神器，用于下载存在依赖关系的软件包。通俗地说，Homebrew 是类似于 Mac App Store 的一个软件商店。

### 二、Homebrew 的好处

通过 Homebrew 下载的软件都来自于官网，绝对放心软件的安全性。而且它尽可能地利用系统自带的各种库，使得软件包的编译时间大大缩短，基本上不会造成冗余。

### 三、Homebrew 的安装

1.  安装方法极其简单，使用系统**终端**（**Terminal**）应用，输入以下命令：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

若以上安装失败，并提醒：

Failed to connect to [http://raw.githubusercontent.com](https://link.zhihu.com/?target=http%3A//raw.githubusercontent.com) port 443: Connection refused.

则可以尝试使用国内源进行安装，详情请看 [Gitee / CunKai / HomebrewCN](https://link.zhihu.com/?target=https%3A//gitee.com/cunkai/HomebrewCN)。（或者科学上网）

2\. 测试 Homebrew 是否正确安装。

```bash
$ brew -h
```

3\. 若上一步输入命令，回车后提示：`brew：command not found`。则需要进行环境配置，若成功则跳过该步骤：

a. 终端输入：`sudo vim ~/.zshrc`。

b. 在 `.zshrc` 文件的末尾添加配置：`export PATH=/usr/local/bin:$PATH`。

c. 在 `vim` 模式下，按下 `i` 键进入编辑模式；编辑完成后，按 `Esc` 键退出编辑模式；输入 `:wq` 保存退出（ `w` 为 write 写入，`q` 为 quit 退出）。

d. 刷新环境变量：`source ~/.zshrc`。

e. 再次输入 `brew -h` 测试。

从 macOS Catalina 开始，Mac 将使用`zsh`作为默认登录 Shell 和交互式 Shell（[详看](https://link.zhihu.com/?target=https%3A//support.apple.com/zh-cn/HT208050)）。若仍使用`bash`的话，将配置写入`~/.bash_profile`即可。

### 四、切换 Homebrew 源

可能有朋友会遇到过 `brew update` 会卡住的情况，在国内的话可以切换为清华或中科院的镜像。

个人比较推荐切换为国内源，安装包明显速度快很多。

以 USTC（中科院）镜像为例：

1.  替换 Homebrew 源

```text
$ cd "$(brew --repo)"
$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

2\. 切换 Homebrew Core 源

```text
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

3\. 切换 Homebrew Cask 源

```text
cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
```

> 关于更多 USTC Homebrew 镜像说明，请看 [Homebrew 源使用帮助](https://link.zhihu.com/?target=http%3A//mirrors.ustc.edu.cn/help/brew.git.html)。

### 五、Homebrew 安装/卸载命令

其实细心的朋友可能会发现，Homebrew 的安装命令好像有两个：

```text
$ brew install <package>
$ brew install --cask <package>
```

这两者有什么区别呢？

官方描述：[Homebrew Cask](https://link.zhihu.com/?target=https%3A//github.com/Homebrew/homebrew-cask) 扩展了 Homebrew，并为 Atom 和 Google Chrome 等 GUI macOS 应用程序的安装和管理带来了优雅、简单和快速。 为此，我们提供了友好的 CLI 工作流来管理作为二进制文件分发的 macOS 应用程序。

我们执行如下搜索命令，会发现：

```text
frankie@iMac ~ %   brew search google

==> Formulae
aws-google-auth                          google-sparsehash
google-authenticator-libpam              google-sql-tool
google-benchmark                         googler
google-go                                googletest
google-java-format

==> Casks
google-ads-editor
google-analytics-opt-out
google-backup-and-sync
...
```

以上搜索命令，可以看到搜索关键词 `google`，结果会出现 `Formulae` 和 `Casks` 两种分类，有何区别？

*   **「Formulae」**一般是那些命令行工具、开发库、字体、插件等不含 GUI 界面的软件。
*   **「Cask」**是指那些含有 GUI 图形化界面的软件，如 Google Chrome、FireFox 、Atom 等。

其实所有的 Homebrew Cask 命令都以 `brew` 开头，这对 Casks 和 Formulae 均适用。

所以，使用 Homebrew 安装软件，只要使用如下命令即可：

```text
$ brew install <package>
```

其他一些命令：

```text
$ brew uninstall <package>  # 卸载
$ brew reinstall <package>  # 重装
```

### 六、Homebrew 其他命令

1.  软件搜索命令

支持关键字、模糊搜索。假设我们想安装一个叫 Alfred 的软件，但不知道 Homebrew 是否支持安装该应用，我们可通过该方法查询。如输入 `brew search alf` 会列出所有符合条件的结果。

```text
$ brew search <key words>
```

2\. 更新软件

获取最新的包，但该命令会先检查 Homebrew 本身是否有更新。

```text
$ brew update

```

很多朋友在这个操作，会卡在 Updating Homebrew… 按照以上方法切换至国内源几乎都能解决。除此之外，你可以科学上网。

3\. 更新软件

如 `brew upgrade highlight`

```text
$ brew upgrade              # 更新所有
$ brew upgrade <package>    # 更新指定软件
```

4\. 查看 Homebrew 下载的软件存放路径

```bash
$ brew --cache
```

5\. 列出已安装的软件

```text
$ brew list             # 所有的软件，包括 Formulae  和 Cask
$ brew list --formulae  # 所有已安装的 Formulae
$ brew list --cask      # 所有已安装的 Casks
```

6\. 列出可更新的软件

```text
$ brew outdated
```

7\. 清理旧版本软件

如 `brew cleanup wget`

```text
$ brew cleanup            # 清理所有旧版本的包
$ brew cleanup <package>  # 清理指定的旧版本包
$ brew cleanup -n         # 查看可清理的旧版本包
```

8\. 强制卸载某个软件

如 `brew uninstall --force wget`

```text
$ brew uninstall --force <package>

```

9\. 锁定某个不想更新的软件

如 `brew pin wget`

```text
$ brew pin <package>       # 锁定指定包
$ brew unpin <package>     # 取消锁定指定包
```

10\. 查看已安装软件的依赖

```text
$ brew deps --installed --tree

```

11\. 查看软件的信息

如 `brew info wget`

```text
$ brew info <package>     # 显示某个包信息
$ brew info               # 显示安装的软件数量、文件数量以及占用空间
```