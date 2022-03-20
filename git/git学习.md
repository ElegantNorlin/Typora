

* 设置用户信息

  ```shell
  git config --global user.name "your name"
  git config --global user.email "your email address"
  ```

* 查看用户配置信息

  ```shell
  git config --global user.name
  git config --global user.email
  ```

* 将修改从工作区提交到暂存区

  ```shell l
  //把所有修改都提交到暂存区
  git add .
  ```

* 将暂存区内容提交到本地仓库

  ```
  //-m参数懈怠了本次提交的内容描述
  git commit -m "description"
  ```

* 查看当前分支和修改内容

  ```shell
  git status
  ```

* 查看提交日志

  ```shell
  git log
  ```

* 版本回退

  ```shell
  git reset --hard commitID
  ```

* 查看本地分支

  ```shell
  git branch
  ```

* 查看本地分支和远程分支（本地分支为白色和绿色字体，远程分支为红色字体）

  ```shell
  git branch -a
  ```

* 创建本地分支

  ```shell
  git branch new_branch_name
  ```

* 切换本地分支

  ```shell
  git checkout branch_name
  ```

* 合并分支

  ```shell
  git merge branch_name
  ```

* 删除分支

  ```shell
  //强制删除分支
  git branch -D branch_name 
  
  //删除分支时，需要做各种检查
  git branch -d branch_name
  ```

* 拉取远程分支并在本地创建（在本地创建dev分支，并与远程的dev分支关联）

  ```shell
  git checkout -b env origin/dev
  ```

  