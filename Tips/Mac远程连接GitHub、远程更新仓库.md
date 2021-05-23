---

---







### 获取SSH远程连接密钥

mac自带SSH，所以不需要安装SSH

下列命令在`~/.ssh`目录下会生成SSH密钥`id_rsa`(私钥)、`id_rsa.pub`(公钥)和`known_hosts`(不要删除，留着不用动就好了)

```shell
$ ssh-keygen -t rsa -C "youremail"
```

上述代码执行过程需要输入直接回车！

上述命令中的`youremail`是GitHub的账号，也就是注册用的邮箱📮。

### 复制公钥

使用下列命令查看SSH公钥：

```shell
cat ~/.ssh/id_rsa.pub
```

复制公钥

### GitHub添加公钥

打开GitHub，按照`settings/SSH and GPG keys`点击`New SSH key`按钮新建一个公钥，密钥标题写什么都可以，内容直接粘贴上刚才复制的密钥。

### 查看公钥是否添加成功

执行下列代码

```shell
ssh -T git@github.com
```

如果出现下列输出则说明连接成功

```shell
Hi **********! You've successfully authenticated, but GitHub does not provide shell access.
```

### 克隆GitHub仓库到本地

先进入要存放仓库的目录

```shell
cd yourdir
```

克隆仓库

```shell
git clone 仓库https链接
```

clone仓库可能过程不太顺利，最好开启终端代理，这样应该就畅通无阻了

### 将仓库改动从工作区添加到缓存区

```shell
git add .
```

### 将缓存区内容推送到本地仓库

```shell
git commit -m "注释内容"
```

### 将本地仓库推送到GitHub

```shell
git push
```

如果本地只有一个分支，直接`git push`即可，否则`git push 分支名称`
