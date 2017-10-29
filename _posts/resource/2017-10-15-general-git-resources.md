---
layout: post
title: Git 常用资源
category: 资源
tags: Git
keywords: Git
---

![git logo](http://oxpypycim.bkt.clouddn.com/git logo.png)

如果你是一名程序员，那么一定用过或听说过Git。作为一款免费、开源的分布式版本控制系统，Git可以敏捷高效地处理任何或小或大的项目。下面整理了我在工作中常用到的一些git语法。

## 库管理

### 克隆库

```bash
git clone https://github.com/php/php-src.git
git clone --depth=1 https://github.com/php/php-src.git                                  # 只抓取最近的一次 commit
```

## 历史管理

### 查看历史

```bash
git log --pretty=oneline filename       # 一行显示
git show xxxx                           # 查看某次修改
```

### 标签功能
    
```bash    
git tag                                 # 显示所有标签
git tag -l 'v1.4.2.*'                   # 显示 1.4.2 开头标签
git tag v1.3                            # 简单打标签   
git tag -a v1.2 9fceb02                 # 后期加注标签
git tag -a v1.4 -m 'my version 1.4'     # 增加标签并注释， -a 为 annotated 缩写
git show v1.4                           # 看某一标签详情
git push origin v1.5                    # 分享某个标签
git push origin --tags                  # 分享所有标签
git push origin master                  # 上传本地当前分支到master分支
```

### 回滚操作

```bash
git reset 9fceb02                       # 保留修改
git reset 9fceb02 --hard                # 删除之后的修改
```

### 取消文件的修改

```bash
git checkout -- a.php                   # 取消单个文件
git checkout --                         # 取消所有文件的修改
```

### 添加文件

```bash
git add a.php                           # 添加文件到缓存区
git add .                               # 递归地添加当前工作目录中的所有文件
```

### 删除文件

```bash
git rm a.php                            # 直接删除文件
git rm --cached a.php                   # 删除文件暂存状态
```

### 移动文件

```bash
git mv a.php ./test/a.php               # 移动／重命名文件
```

### 查看文件修改

```bash
git diff                                # 查看未暂存的文件更新 
git diff --cached                       # 查看已暂存文件的更新 
```
    
### 暂存和恢复当前staging

```bash
git stash                               # 暂存当前分支的修改
git stash apply                         # 恢复最近一次暂存
git stash list                          # 查看暂存内容
git stash apply stash@{2}               # 指定恢复某次暂存内容
git stash drop stash@{0}                # 删除某次暂存内容
```

### 提交变动的文件

```bash
git commit -m "some message"            # 提交文件更改
git commit -a                           # 把所有已经track的文件add进来，并提交
```

### 修改 commit 历史纪录

```bash
git rebase -i 0580eab8
```

## 分支管理

### 创建分支
    
```bash
git branch develop                      # 只创建分支
git checkout -b master develop          # 创建并切换到 develop 分支
```

### 合并分支

```bash
git checkout master                     # 切换到 master 分支
git merge --no-ff develop               # 把 develop 合并到 master 分支，no-ff 选项的作用是保留原分支记录
git rebase develop                      # rebase 当前分支到 develop
git branch -d develop                   # 删除 develop 分支
```

### 克隆远程分支

```bash
git branch -r                           # 显示所有分支，包含远程分支
git checkout origin/android
```

### 修复develop上的合并错误

```bash
1. 将merge前的commit创建一个分之，保留merge后代码
2. 将develop `reset --force`到merge前，然后`push --force`
3. 在分支中rebase develop
4. 将分支push到服务器上重新merge
```

### 强制更新到远程分支最新版本

```bash
git reset --hard origin/master
git submodule update --remote -f
```

## Submodule使用

### 克隆带submodule的库

```bash
git clone --recursive https://github.com/chaconinc/MainProject
```

### clone主库后再去clone submodule

```bash
git clone https://github.com/chaconinc/MainProject
git submodule init
git submodule update
```
    
## Git设置

Git的全局设置在`~/.gitconfig`中，单独设置在`project/.git/config`下。

忽略设置全局在`~/.gitignore_global`中，单独设置在`project/.gitignore`下。

### 设置 commit 的用户和邮箱

```bash
git config user.name "xx"
git config user.email "xx@xx.com"
```

或者直接修改`config`文件

```bash
[user]
    name = xxx
    email = xxx@xxx.com
```

### 查看设置项

```bash
git config --list
```

### 设置git终端颜色

```bash
git config --global color.diff auto
git config --global color.status auto
git config --global color.branch auto
```

## 远程分支

远程分支（**remote branch**）是对远程仓库中的分支的索引。它们是一些无法移动的本地分支；只有在 Git 进行网络交互时才会更新。远程分支就像是书签，提醒着你上次连接远程仓库时上面各分支的位置。

我们用 `(远程仓库名)/(分支名)` 这样的形式表示远程分支。比如我们想看看上次同 `origin` 仓库通讯时 `master` 分支的样子，就应该查看 `origin/master` 分支。如果你和同伴一起修复某个问题，但他们先推送了一个 `iss53` 分支到远程仓库，虽然你可能也有一个本地的 `iss53` 分支，但指向服务器上最新更新的却应该是 `origin/iss53` 分支。

可能有点乱，我们不妨举例说明。假设你们团队有个地址为 `git.ourcompany.com` 的 Git 服务器。如果你从这里克隆，Git 会自动为你将此远程仓库命名为 `origin`，并下载其中所有的数据，建立一个指向它的 `master` 分支的指针，在本地命名为 `origin/master`，但你无法在本地更改其数据。接着，Git 建立一个属于你自己的本地 `master` 分支，始于 `origin` 上 `master` 分支相同的位置，你可以就此开始工作。

![git_1](http://oxpypycim.bkt.clouddn.com/git_1.png)

**图 3-22.** 一次 Git 克隆会建立你自己的本地分支 `master` 和远程分支 `origin/master`，并且将它们都指向 `origin` 上的 `master` 分支。

如果你在本地 `master` 分支做了些改动，与此同时，其他人向 `git.ourcompany.com` 推送了他们的更新，那么服务器上的 `master` 分支就会向前推进，而与此同时，你在本地的提交历史正朝向不同方向发展。不过只要你不和服务器通讯，你的 `origin/master` 指针仍然保持原位不会移动。

![git_2](http://oxpypycim.bkt.clouddn.com/git_2.png)

**图 3-23.** 在本地工作的同时有人向远程仓库推送内容会让提交历史开始分流。

可以运行 `git fetch origin` 来同步远程服务器上的数据到本地。该命令首先找到 `origin` 是哪个服务器（本例为 `git.ourcompany.com`），从上面获取你尚未拥有的数据，更新你本地的数据库，然后把 `origin/master` 的指针移到它最新的位置上.

![git_3](http://oxpypycim.bkt.clouddn.com/git_3.png)

**图 3-24.** git fetch 命令会更新 remote 索引

为了演示拥有多个远程分支（在不同的远程服务器上）的项目是如何工作的，我们假设你还有另一个仅供你的敏捷开发小组使用的内部服务器 **git.team1.ourcompany.com**。可以用 `git remote add` 命令把它加为当前项目的远程分支之一。我们把它命名为 `teamone`，以便代替完整的 Git URL 以方便使用。

![git_4](http://oxpypycim.bkt.clouddn.com/git_4.png)

**图 3-25.** 把另一个服务器加为远程仓库

现在你可以用 `git fetch teamone` 来获取小组服务器上你还没有的数据了。由于当前该服务器上的内容是你 `origin` 服务器上的子集，Git 不会下载任何数据，而只是简单地创建一个名为 `teamone/master` 的远程分支，指向 `teamone` 服务器上 `master` 分支所在的提交对象 `31b8e`。

![git_5](http://oxpypycim.bkt.clouddn.com/git_4.png)

**图 3-26.** 你在本地有了一个指向 teamone 服务器上 master 分支的索引

