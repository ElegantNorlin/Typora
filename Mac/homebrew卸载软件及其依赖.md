---

---









我们使用`brew uninstall 软件包`卸载homebrew安装的软件时，软件只会卸载你要卸载的软件，而不会去卸载安装此软件同时安装的依赖软件。按照一下方法即可卸载其依赖软件，并不会影响其他软件的影响。

如果没有安装`rmtree`的话需要先执行一下命令安装`rmtree`。

```shell
brew tap beeftornado/rmtree
```

我们以后在卸载软件时至需要执行以下命令：

```shell
brew rmtree 软件名
//清理brew安装旧版本的软件包
brew cleanup
```

