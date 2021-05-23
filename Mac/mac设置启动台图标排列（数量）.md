---

---











通过 命令行
 设置 Lauchpad 图标的列数

```shell
defaults write com.apple.dock springboard-columns -int 7
```

设置 Lauchpad 图标的行数

```shell
defaults write com.apple.dock springboard-rows -int 7
```

重置 Lauchpad

```shell
defaults write com.apple.dock ResetLaunchPad -bool TRUE;
```

重启 Dock

```shell
killall Dock
```