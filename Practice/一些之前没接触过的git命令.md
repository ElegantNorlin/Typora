

⚠️⚠️⚠️：团队协作中，要提交的时候最好先git pull一下项目，节约自己和他人的时间，提交记录也容易阅读。

⚠️⚠️⚠️：团队协作中，要提交的时候最好先git pull一下项目，节约自己和他人的时间，提交记录也容易阅读。

⚠️⚠️⚠️：团队协作中，要提交的时候最好先git pull一下项目，节约自己和他人的时间，提交记录也容易阅读。



在实习之前往GitHub提交代码都是很简单的那几个命令：

`git add .`将所有修改的文件提交到暂存区

git add `modify filename`将单个或者多个修改的文件提交到暂存区

`git commit -m "annotation"`将暂存区的内容提交到本地仓库

`git push`将本地仓库的内容推送到自己的远程仓库



最近实习中接触到一些新的git命令记录一下：

```shell
# 查看所有分支，包括本地分支（白色字体），也包括远程仓库分支（红色字体）
git branch -a

# 仅查看本地分支
git branch

# 删除本地分支
git branch -D 本地分支名称

# 创建新分支
git branch `new_branch_name`

# 创建新分支并切换到该分支
git branch -b `new_branch_name`

# 从远程仓库拉取并创建为新的本地分支
git branch -b `local_branch` `remote_branch`

# 切换分支
git checkout `branch_name`
```