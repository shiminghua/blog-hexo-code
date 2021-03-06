---
title: git常用命令
date: 2018-03-20 22:59:35
categories:
- git
---
# git 常用命令

总结工作中常用的 git 命令

## 专用名词

- workspace：工作区
- index / stage：暂存区
- repository：仓库区（本地仓库）
- remote：远程仓库

## 强制覆盖本地文件

```code
- git fetch --all
- git reset --hard origin/master
- git pull
```

## 远程仓库地址变更

- 删除后添加：git remote rm origin; git remote add origin [url]
- 修改命令：git remote set-url origin [url]

## 一、新建代码库

- 在当前目录新建一个代码库：$ git init
- 新建一个目录，将其初始化为一个Git代码库：$ git init [project-name]
- 下载一个项目和它的整个代码史：$ git clone [url]

## 二、配置

Git 的配置文件为 .gitconfig ，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）.

- 显示当前的git配置：$ git config --list
- 编辑git配置文件：$ git config -e [--global]
- 设置提交代码时的用户信息：
  - $ git config [--global] user.name "[name]"
  - $ git config [--global] user.email "[email address]"
- 凭证存储:
  - $ git config --global credential.helper store

## 三、增加/删除文件

- 添加指定文件到暂存区：$ git add [file1] [file2] ...
- 添加指定目录到暂存区，包括子目录：$ git add [dir]
- 添加当前目录的所有文件到暂存区：$ git add .
- 添加每个变化前，都会要求确认；对于同一个文件的多处变化，可以实现粉刺提交：$ git add -p
- 删除工作区文件，并且将这次删除放入暂存区：$ git rm [file2] [file2] ...
- 停止追踪指定文件，但该文件会保留在工作区：$ git rm --cached [file]
- 改名文件，并将这个改名放入暂存区：$ git mv [file-original] [file-renamed]

## 四、代码提交

- 提交暂存区到仓库：$ git commit -m [message]
- 提交暂存区的指定文件到仓库：$ git commit -m [file1] [file2] ... -m [message]
- 提交工作区自上次commit之后的变化，直接到仓库区：$ git commit -a
- 提交时显示所有diff信息：$ git commit -v
- 使用一次新的commit，代替上一次提交；如果代码没有任何变化，则用来改写上一次commit的提交信息：$ git commit --amend -m [message]
- 重做上一次commit，并包括指定文件的新变化：$ git commit --amend [file1] [file2] ...

## 五、分支

- 列出所有本地分支：$ git branch
- 列出所有远程分支：$ git branch -r
- 列出所有本地和远程分支：$ git branch -a
- 新建一个分支，但依然停留在当前分支：$ git branch [branch-name]
- 新建一个分支，并切换到该分支：$ git checkout -b [branch]
- 新建一个分支，指向指定commit：$ git branch [branch] [commit]
- 新建一个分支，与指定的远程分支建立追踪关系：$ git branch --track [branch] [remote-branch]
- 切换到指定分支，并更新工作区：$ git checkout [branch-name]
- 切换到上一个分支：$ git checkout -
- 建立追踪关系，在现有分支与指定的远程分支之间：$ git branch --set-upstream [branch] [remote-branch]
- 合并指定分支到当前分支：$ git merge [branch]
- 选择一个commit，合并进当前分支：$ git cherry-pick [commit]
- 删除分支：$ git branch -d [branch-name]
- 删除远程分支：
  - $ git push origin --delete [branch-name]
  - $ git branch -dr [remote/branch]

## 六、标签

- 列出所有tag：$ git tag
- 新建一个 tag 在当前 commit ：$ git tag [tag]
- 新建一个 tag 在指定 commit ：$ git tag [tag] [commit]
- 删除本地 tag ：$ git tag -d [tag]
- 删除远程 tag ：$ git push origin :refs/tags/[tagName]
- 查看 tag 信息：$ git show [tag]
- 提交指定 tag ：$ git push [remote] [tag]
- 提交所有 tag ：$ git push [remote] --tags
- 新建一个分支，指向某个 tag ：$ git checkout -b [branch] [tag]

## 七、查看信息

- 显示有变更的文件：$ git status
- 显示当前分支的版本历史：$ git log
- 显示 commit 历史，以及每次commit发生变更的文件：$ git log --stat
- 搜索提交历史，根据关键词：$ git log -S [keyword]
- 未完待续

## 八、远程同步

- 下载远程仓库的所有变动：$ git fetch [remote]
- 显示所有远程仓库：$ git remote -v
- 显示某个远程仓库的信息：$ git remote show [remote]
- 增加一个新的远程仓库，并命名：$ git remote add [shortname] [url]
- 取回远程仓库的变化，并与本地分支合并：$ git pull [remote] [branch]
- 上传本地指定分支到远程仓库：$ git push [remote] [branch]
- 强行推送当前分支到远程仓库，即使有冲突：$ git push [remote] --force
- 推送所有分支到远程仓库：$ git push [remote] --all

## 九、撤销

- 恢复暂存区的指定文件到工作区：$ git checkout [file]
- 恢复某个commit的指定文件到暂存区和工作区：$ git checkout [commit] [file]
- 恢复暂存区的所有文件到工作区：$ git checkout .
- 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变：$ git reset [file]
- 重置暂存区与工作区，与上一次commit保持一直：$ git reset --hard
- 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变: $ git reset [commit]
- 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致：$ git reset --hard [commit]
- 重置当前HEAD为指定commit，但保持暂存区和工作区不变：$ git reset --keep [commit]
- 新建一个commit，用来撤销指定commit，后者的所有变化都将被前者抵消，并且应用到当前分支：$ git revert [commit]
- 暂时将未提交的变化移除，稍后再移入：
  - $ git stash
  - $ git stash pop

## 十、其他

- 生成一个可供发布的压缩包：$ git archive

## 远程仓库相关命令

- 检出仓库：$ git clone git://github.com/jquery/jquery.git
- 查看远程仓库：$ git remote -v
- 添加远程仓库：$ git remote add [name] [url]
  - $ git remote add origin https://minghua_shi@bitbucket.org/minghua_shi/test.git
  - $ git push -u origin master
- 删除远程仓库：$ git remote rm [name]
- 修改远程仓库：$ git remote set-utl --push [name] [newUrl]
- 拉取远程仓库：$ git pull [remoteName] [localBranchName]
- 推送远程仓库：$ git push [remoteName] [localBranchName]
- 如果想把本地的某个分支test提交到远程仓库，并作为远程仓库的master分支，或者作为另外一个名叫test的分支，如下：
  - $ git push origin test:master // 提交本地test分支作为远程的master分支
  - $ git push origin test:test   // 提交本地test分支作为远程的test分支
