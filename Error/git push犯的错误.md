---

---





### git push犯的错误

问题描述：

有一次git push的时候报错了，错误如下：

```shell
 ! [rejected]        branch -> branch (fetch first)
error: failed to push some refs to 'github.com:*****/****.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

看了一下描述的错误，应该是说我的本地仓库不包含远程仓库的内容，也就是并没有跟上远程仓库的更新，应该先git pull,然后再git push,git pull之前记得先把这次要push的改进先保存下来。