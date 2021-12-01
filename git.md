# GIT管理

# 1. Git是什么

## 1.1. 概述

Git是分布式版本控制系统，与SVN类似的集中化版本控制系统相比，集中化版本控制系统如果中央服务器宕机则会影响数据和协同开发。

Git是分布式的版本控制系统，客户端不只是提取最新版本的快照，而且将整个代码仓库镜像复制下来。如果任何协同工作用的服务器发生故障了，也可以用任何一个代码仓库来恢复。而且在协作服务器宕机期间，你也可以提交代码到本地仓库，当协作服务器正常工作后，你再将本地仓库同步到远程仓库。

## 1.2. 特性

- 能够对文件版本控制和多人协作开发
- 拥有强大的分支特性，所以能够灵活地以不同的工作流协同开发
- 分布式版本控制系统，即使协作服务器宕机，也能继续提交代码或文件到本地仓库，当协作服务器恢复正常工作时，再将本地仓库同步到远程仓库。
- 当团队中某个成员完成某个功能时，通过pull request操作来通知其他团队成员，其他团队成员能够review code后再合并代码。

# 2. 为什么要用Git

- 能够对文件版本控制和多人协作开发
- 拥有强大的分支特性，所以能够灵活地以不同的工作流协同开发
- 分布式版本控制系统，即使协作服务器宕机，也能继续提交代码或文件到本地仓库，当协作服务器恢复正常工作时，再将本地仓库同步到远程仓库。
- 当团队中某个成员完成某个功能时，通过pull request操作来通知其他团队成员，其他团队成员能够review code后再合并代码。

# 3. Git 命令思维导图

![](D:\Files\笔记\Git\image\git-map.png)



## Git 状态

​	版本控制就是对文件的版本控制，要对文件进行修改、提交等操作，首先要知道文件当前在什么状态，不然可能会提交了现在还不想提交的文件，或者要提交的文件没提交上。 
  **· Untracked** ：未跟踪，此文件在文件夹中，但并没有加入到  **git** 库，不参与版本控制．通过 `git add` 状态变为 *staged*．
  **· Unmodify** ：文件已经入库，未修改，即版本库中的文件快照内容与文件夹中完全一致．这种类型的文件有两种去处，如果它被修改，而变为 *Modified*. 如果使用 `git rm` 移出版本库，则成为 *Untrachekd*文件
  **· MOdified** ：文件已修改，仅仅是修改，并没有进行其他的操作．这个文件也有两个去处，通过 `git add`  可进入暂存 *staged* 状态，使用 `git checkout` 则丢弃修改过，返回到 *unmodify* 状态，这个 `git checkout` 即从库中取出文件，覆盖当前修改！
  **· staged** ：暂存状态，执行 `git commit` 则将修改同步到库中，这时库中的文件和本地文件又变为一致，文件为 *Unmodify* 状态. 执行 `git reset HEAD file name` 取消暂存，文件状态为 *Modified*

```bash
[root@CentOS7 ~]#mkdir git_data
[root@CentOS7 ~]#cd git_data/
[root@CentOS7 ~/git_data]#git init
Initialized empty Git repository in /root/git_data/.git/
[root@CentOS7 ~/git_data]#ls -la
total 4
drwxr-xr-x  3 root root   18 Nov 30 19:15 .
dr-xr-x---. 8 root root 4096 Nov 30 19:15 ..
drwxr-xr-x  7 root root  119 Nov 30 19:15 .git
[root@CentOS7 ~/git_data]#echo 'Hello Git' > ./hello.txt
[root@CentOS7 ~/git_data]#cat hello.txt
Hello Git
[root@CentOS7 ~/git_data]#git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   hello.txt
nothing added to commit but untracked files present (use "git add" to track)
[root@CentOS7 ~/git_data]#git add hello.txt
[root@CentOS7 ~/git_data]#git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#   new file:   hello.txt
#
[root@CentOS7 ~/git_data]#git commit -m 'new file Hello.txt'
[master (root-commit) 5e471dd] new file Hello.txt
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
[root@CentOS7 ~/git_data]#git status
# On branch master
nothing to commit, working directory clean
```

# Git 忽略文件

有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等
在主目录下建立“.gitignore”文件,此文件有如下规则：
 1 .忽略文件中的空行或以井号（#）开始的行将会被忽略
 2 .可以使用Linux通配符。例如：星号（* ）代表任意多个字符，问号（？）代表一个字符，方括号（［abc］）代表可选字符范围， 大括号（｛string1,string2,…｝）代表可选的字符串等
 3 .如果名称的最前面有一个感叹号（！），表示例外规则，将不被忽略
 4 .如果名称的最前面是一个路径分隔符（/ ）,表示要忽略的文件在此目录下，而子目录中的文件不忽略
 5 .如果名称的最后面是一个路径分隔符（/ ）,表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）

```
#为注释 txt
*.txt                # 忽略所有.txt结尾的文件，这样的话上传就不会被选中！
!lib.txt			 # 但:13 b.txt除夕卜
/temp				 # 仅忽略项目根目录下的TODO文件，不包括其它目录tep
build/ 				 # 忽略build/目录下的所有文件
doc/*.txt		     # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```





# 1. Git常用命令

| 分类 | 子类                        | git command                                                  | zsh alias |
| ---- | --------------------------- | ------------------------------------------------------------ | --------- |
| 分支 | 查看当前分支                | `git branch`                                                 | gb        |
|      | 创建新分支,仍停留在当前分支 | git branch                                                   |           |
|      | 创建并切换到新分支          | git checkout -b                                              | gcb       |
|      | 切换分支                    | git checkout                                                 |           |
|      | 合并分支                    | git checkout #切换到要合并的分支git merge –no-ff #合并指定分支到当前分支 |           |
| 提交 | 查看状态                    | git status                                                   | gst       |
|      | 查看修改部分                | git diff --color                                             | gd        |
|      | 添加文件到暂存区            | git add --all                                                |           |
|      | 提交本地仓库                | git commit -m ""                                             |           |
|      | 推送到指定分支              | git push -u origin                                           |           |
|      | 查看提交日志                | git log                                                      | -         |

# 2. git rebase

如果信息修改无法生效，设置永久环境变量：export EDITOR=vim

帮助信息：

```bash
# Rebase 67da308..6ef692b onto 67da308 (1 command)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

## 2.1. 合并多余提交记录

```bash
#以交互的方式进行rebase
git rebase -i master

#合并多余提交记录：s, squash = use commit, but meld into previous commit
pick 6ef692b FIX: Fix parsing docker image version error
s 3df667y FIX: the second push
s 3fds95t FIX: the third push
保存退出

# 进入修改交互界面
删除需要删除的提交记录，保存退出

#查看提交记录是否已被修改
git log


#最后强制提交到分支
git commit --force -u origin fix/add-unit-test-for-global-role-revoking
```

## 2.2. 修改提交记录

```bash
#以交互的方式进行rebase
git rebase -i master

#修改提交记录：e, edit = use commit, but stop for amending
e 6ef692b FIX: Fix parsing docker image version error
e 5ty697u FIX: Fix parsing docker image version error
#保存退出

git commit --amend
#修改提交记录内容，保存退出

git rebase --continue
git commit --amend
#修改下一条提交记录，保存退出

git rebase --continue
git status # 查看状态提示

#最后强制提交到分支
git commit --force -u origin fix/add-unit-test-for-global-role-revoking

#查看提交记录是否已被修改
git log
```

# 3. git设置忽略特殊文件

## 3.1. 忽略文件的原则

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

## 3.2. 设置的方法

在项目的workdir 下编辑 .gitignore 文件，文件的路径填写为workdir的相对路径。

```bash
.idea/         #IDE的配置文件
_build/
server/server  #二进制文件
```

## 3.3. gitignore 不生效解决方法

原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

# 4. Git分支重命名

假设分支名称为oldName 想要修改为 newName

**1. 本地分支重命名(还没有推送到远程)**

```bash
git branch -m oldName newName
```

**2. 远程分支重命名 (已经推送远程-假设本地分支和远程对应分支名称相同)** a. 重命名远程分支对应的本地分支

```bash
git branch -m oldName newName
```

b. 删除远程分支

```bash
git push --delete origin oldName
```

c. 上传新命名的本地分支

```bash
git push origin newName
```

d.把修改后的本地分支与远程分支关联

```bash
git branch --set-upstream-to origin/newName
```

# 5. 代码冲突

```bash
git checkout master
git pull
git checkout <branch>
git rebase -i master
fix conflict
git rebase --continue
git push --force -u origin <branch>
```

# 6. 修改历史提交的用户信息

1、克隆并进入你的仓库

```bash
git clone --bare https://github.com/user/repo.git
cd repo.git
```

2、创建以下脚本，例如命名为rename.sh

```bash
#!/bin/sh

git filter-branch --env-filter '
OLD_EMAIL="your-old-email@example.com"          #修改参数为你的旧提交邮箱
CORRECT_NAME="Your Correct Name"                #修改参数为你新的用户名
CORRECT_EMAIL="your-correct-email@example.com"  #修改参数为你新的邮箱名
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

3、执行脚本

```bash
chmod +x rename.sh

sh rename.sh
```

4、查看新 Git 历史有没有错误。

```bash
#可以看到提交记录的用户信息已经修改为新的用户信息
git log
```

5、确认提交内容，重新提交（可以先把rename.sh移除掉）

```bash
git push --force --tags origin 'refs/heads/*'
```

# 7. 撤销已经push的提交

```bash
# 本地仓库回退到某一版本
git reset -hard <commit-id>
# 强制 PUSH，此时远程分支已经恢复成指定的 commit 了
git push origin master --force
```





# Git 命令详解

# 1. 示意图

![这里写图片描述](https://res.cloudinary.com/dqxtn0ick/image/upload/v1510578830/article/git/git-arch.png)图片 - 这里写图片描述

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

# 2. Git 命令分类

## 2.1. 新建代码库

```bash
# 在当前目录新建一个Git代码库
$ git init
# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]
# 下载一个项目和它的整个代码历史
$ git clone [url]
```

## 2.2. 配置

Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```bash
# 显示当前的Git配置
$ git config --list
# 编辑Git配置文件
$ git config -e [--global]
# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

## 2.3. 增加/删除文件

```bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...
# 添加指定目录到暂存区，包括子目录
$ git add [dir]
# 添加当前目录的所有文件到暂存区
$ git add .
# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p
# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...
# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

## 2.4. 代码提交

```bash
# 提交暂存区到仓库区
$ git commit -m [message]
# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]
# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a
# 提交时显示所有diff信息
$ git commit -v
# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]
# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

## 2.5. 分支

```bash
# 列出所有本地分支
$ git branch
# 列出所有远程分支
$ git branch -r
# 列出所有本地分支和远程分支
$ git branch -a
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 新建一个分支，并切换到该分支
$ git checkout -b [branch]
# 新建一个分支，指向指定commit
$ git branch [branch] [commit]
# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]
# 切换到指定分支，并更新工作区
$ git checkout [branch-name]
# 切换到上一个分支
$ git checkout -
# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]
# 合并指定分支到当前分支
$ git merge [branch]
# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]
# 删除分支
$ git branch -d [branch-name]
# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

## 2.6. 标签

```bash
# 列出所有tag
$ git tag
# 新建一个tag在当前commit
$ git tag [tag]
# 新建一个tag在指定commit
$ git tag [tag] [commit]
# 删除本地tag
$ git tag -d [tag]
# 删除远程tag
$ git push origin :refs/tags/[tagName]
# 查看tag信息
$ git show [tag]
# 提交指定tag
$ git push [remote] [tag]
# 提交所有tag
$ git push [remote] --tags
# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

## 2.7. 查看信息

```bash
# 显示有变更的文件
$ git status
# 显示当前分支的版本历史
$ git log
# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat
# 搜索提交历史，根据关键词
$ git log -S [keyword]
# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s
# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature
# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]
# 显示指定文件相关的每一次diff
$ git log -p [file]
# 显示过去5次提交
$ git log -5 --pretty --oneline
# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
# 显示暂存区和工作区的差异
$ git diff
# 显示暂存区和上一个commit的差异
$ git diff --cached [file]
# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD
# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]
# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"
# 显示某次提交的元数据和内容变化
$ git show [commit]
# 显示某次提交发生变化的文件
$ git show --name-only [commit]
# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]
# 显示当前分支的最近几次提交
$ git reflog
```

## 2.8. 远程同步

```bash
# 下载远程仓库的所有变动
$ git fetch [remote]
# 显示所有远程仓库
$ git remote -v
# 显示某个远程仓库的信息
$ git remote show [remote]
# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]
# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]
# 上传本地指定分支到远程仓库
$ git push [remote] [branch]
# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
# 推送所有分支到远程仓库
$ git push [remote] --all
```

## 2.9. 撤销

```bash
# 恢复暂存区的指定文件到工作区
$ git checkout [file]
# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]
# 恢复暂存区的所有文件到工作区
$ git checkout .
# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]
# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard
# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]
# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]
# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]
# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

## 2.10. 其他

```bash
# 生成一个可供发布的压缩包
$ git archive
# 设置换行符为LF
git config --global core.autocrlf false
#拒绝提交包含混合换行符的文件
git config --global core.safecrlf true
```

# 1. Git commit规范

## 1.1. 格式

```bash
<type>(<scope>): <subject>
```

示例：

```bash
fix(ngRepeat): fix trackBy function being invoked with incorrect scope
```

## 1.2. type

主要的提交类型如下：

| Type       | 说明                                             | 备注         |
| ---------- | ------------------------------------------------ | ------------ |
| `feat`     | 提交新功能                                       | 常用         |
| `fix`      | 修复bug                                          | 常用         |
| `docs`     | 修改文档                                         |              |
| `style`    | 修改格式，例如格式化代码，空格，拼写错误等       |              |
| `refactor` | 重构代码，没有添加新功能也没有修复bug            |              |
| `test`     | 添加或修改测试用例                               |              |
| `perf`     | 代码性能调优                                     |              |
| `chore`    | 修改构建工具、构建流程、更新依赖库、文档生成逻辑 | 例如vendor包 |

## 1.3. scope

表示此次commit涉及的文件范围，可以使用`*`来表示涉及多个范围。

## 1.4. subject

描述此次commit涉及的修改内容。

- 使用祈使句（动词开头）、动宾短语。
- 第一个字母不要大写。
- 不要以`.`句号结尾。

# 2. Git commit工具

安装`commitizen`和`cz-conventional-changelog`。

```bash
npm install -g commitizen cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

使用cz-cli

```bash
$ git cz
cz-cli@4.0.3, cz-conventional-changelog@3.0.1

? Select the type of change that you're committing: (Use arrow keys)
❯ feat:     A new feature
  fix:      A bug fix
  docs:     Documentation only changes
  style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
  refactor: A code change that neither fixes a bug nor adds a feature
  perf:     A code change that improves performance
  test:     Adding missing tests or correcting existing tests
(Move up and down to reveal more choices)
```



# git plugin

The git plugin provides many [aliases](https://www.huweihuang.com/linux-notes/git/git-alias-zsh.html#aliases) and a few useful [functions](https://www.huweihuang.com/linux-notes/git/git-alias-zsh.html#functions).

To use it, add `git` to the plugins array in your zshrc file:

```bash
plugins=(... git)
```

## Aliases

### 常用

| Alias                | Command                                                      |
| :------------------- | :----------------------------------------------------------- |
| g                    | git                                                          |
| **ga**               | git add                                                      |
| **gaa**              | git add --all                                                |
| **gcmsg**            | git commit -m                                                |
| -------------------- | ------------------------------------------------------------ |
| ggp                  | git push origin $(current_branch)                            |
| ggf                  | git push --force origin $(current_branch)                    |
| gp                   | git push                                                     |
| gl                   | git pull                                                     |
| ggl                  | git pull origin $(current_branch)                            |
| -------------------- | ------------------------------------------------------------ |
| **gco**              | git checkout                                                 |
| gcb                  | git checkout -b                                              |
| gcm                  | git checkout master                                          |
| gb                   | git branch                                                   |
| gba                  | git branch -a                                                |
| gcf                  | git config --list                                            |
| gd                   | git diff                                                     |

### 完整列表

| Alias                | Command                                                      |
| :------------------- | :----------------------------------------------------------- |
| g                    | git                                                          |
| **ga**               | git add                                                      |
| **gaa**              | git add --all                                                |
| gapa                 | git add --patch                                              |
| gau                  | git add --update                                             |
| gav                  | git add --verbose                                            |
| gap                  | git apply                                                    |
| gb                   | git branch                                                   |
| gba                  | git branch -a                                                |
| gbd                  | git branch -d                                                |
| gbda                 | -                                                            |
| gbD                  | git branch -D                                                |
| gbl                  | git blame -b -w                                              |
| gbnm                 | git branch --no-merged                                       |
| gbr                  | git branch --remote                                          |
| gbs                  | git bisect                                                   |
| gbsb                 | git bisect bad                                               |
| gbsg                 | git bisect good                                              |
| gbsr                 | git bisect reset                                             |
| gbss                 | git bisect start                                             |
| gc                   | git commit -v                                                |
| gc!                  | git commit -v --amend                                        |
| gcn!                 | git commit -v --no-edit --amend                              |
| gca                  | git commit -v -a                                             |
| gca!                 | git commit -v -a --amend                                     |
| gcan!                | git commit -v -a --no-edit --amend                           |
| gcans!               | git commit -v -a -s --no-edit --amend                        |
| gcam                 | git commit -a -m                                             |
| gcsm                 | git commit -s -m                                             |
| gcb                  | git checkout -b                                              |
| gcf                  | git config --list                                            |
| gcl                  | git clone --recurse-submodules                               |
| gclean               | git clean -id                                                |
| gpristine            | git reset --hard && git clean -dfx                           |
| gcm                  | git checkout master                                          |
| gcd                  | git checkout develop                                         |
| **gcmsg**            | git commit -m                                                |
| **gco**              | git checkout                                                 |
| gcount               | git shortlog -sn                                             |
| gcp                  | git cherry-pick                                              |
| gcpa                 | git cherry-pick --abort                                      |
| gcpc                 | git cherry-pick --continue                                   |
| gcs                  | git commit -S                                                |
| gd                   | git diff                                                     |
| gdca                 | git diff --cached                                            |
| gdcw                 | git diff --cached --word-diff                                |
| gdct                 | git describe --tags $(git rev-list --tags --max-count=1)     |
| gds                  | git diff --staged                                            |
| gdt                  | git diff-tree --no-commit-id --name-only -r                  |
| gdv                  | -                                                            |
| gdw                  | git diff --word-diff                                         |
| gf                   | git fetch                                                    |
| gfa                  | git fetch --all --prune                                      |
| gfg                  | -                                                            |
| gfo                  | git fetch origin                                             |
| gg                   | git gui citool                                               |
| gga                  | git gui citool --amend                                       |
| ggf                  | git push --force origin $(current_branch)                    |
| ggfl                 | git push --force-with-lease origin $(current_branch)         |
| ggl                  | git pull origin $(current_branch)                            |
| ggp                  | git push origin $(current_branch)                            |
| ggpnp                | ggl && ggp                                                   |
| ggpull               | git pull origin "$(git_current_branch)"                      |
| ggpur                | ggu                                                          |
| ggpush               | git push origin "$(git_current_branch)"                      |
| ggsup                | git branch --set-upstream-to=origin/$(git_current_branch)    |
| ggu                  | git pull --rebase origin $(current_branch)                   |
| gpsup                | git push --set-upstream origin $(git_current_branch)         |
| ghh                  | git help                                                     |
| gignore              | git update-index --assume-unchanged                          |
| gignored             | -                                                            |
| git-svn-dcommit-push | git svn dcommit && git push github master:svntrunk           |
| gk                   | gitk --all --branches                                        |
| gke                  | gitk --all $(git log -g --pretty=%h)                         |
| gl                   | git pull                                                     |
| glg                  | git log --stat                                               |
| glgp                 | git log --stat -p                                            |
| glgg                 | git log --graph                                              |
| glgga                | git log --graph --decorate --all                             |
| glgm                 | git log --graph --max-count=10                               |
| glo                  | git log --oneline --decorate                                 |
| glol                 | git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' |
| glols                | git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --stat |
| glod                 | git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset' |
| glods                | git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset' --date=short |
| glola                | git log --graph --pretty='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --all |
| glog                 | git log --oneline --decorate --graph                         |
| gloga                | git log --oneline --decorate --graph --all                   |
| glp                  | `_git_log_prettily`                                          |
| gm                   | git merge                                                    |
| gmom                 | git merge origin/master                                      |
| gmt                  | git mergetool --no-prompt                                    |
| gmtvim               | git mergetool --no-prompt --tool=vimdiff                     |
| gmum                 | git merge upstream/master                                    |
| gma                  | git merge --abort                                            |
| gp                   | git push                                                     |
| gpd                  | git push --dry-run                                           |
| gpf                  | git push --force-with-lease                                  |
| gpf!                 | git push --force                                             |
| gpoat                | git push origin --all && git push origin --tags              |
| gpu                  | git push upstream                                            |
| gpv                  | git push -v                                                  |
| gr                   | git remote                                                   |
| gra                  | git remote add                                               |
| grb                  | git rebase                                                   |
| grba                 | git rebase --abort                                           |
| grbc                 | git rebase --continue                                        |
| grbd                 | git rebase develop                                           |
| grbi                 | git rebase -i                                                |
| grbm                 | git rebase master                                            |
| grbs                 | git rebase --skip                                            |
| grh                  | git reset                                                    |
| grhh                 | git reset --hard                                             |
| groh                 | git reset origin/$(git_current_branch) --hard                |
| grm                  | git rm                                                       |
| grmc                 | git rm --cached                                              |
| grmv                 | git remote rename                                            |
| grrm                 | git remote remove                                            |
| grset                | git remote set-url                                           |
| grt                  | -                                                            |
| gru                  | git reset --                                                 |
| grup                 | git remote update                                            |
| grv                  | git remote -v                                                |
| gsb                  | git status -sb                                               |
| gsd                  | git svn dcommit                                              |
| gsh                  | git show                                                     |
| gsi                  | git submodule init                                           |
| gsps                 | git show --pretty=short --show-signature                     |
| gsr                  | git svn rebase                                               |
| gss                  | git status -s                                                |
| gst                  | git status                                                   |
| gsta                 | git stash push                                               |
| gsta                 | git stash save                                               |
| gstaa                | git stash apply                                              |
| gstc                 | git stash clear                                              |
| gstd                 | git stash drop                                               |
| gstl                 | git stash list                                               |
| gstp                 | git stash pop                                                |
| gsts                 | git stash show --text                                        |
| gstall               | git stash --all                                              |
| gsu                  | git submodule update                                         |
| gts                  | git tag -s                                                   |
| gtv                  | -                                                            |
| gtl                  | gtl(){ git tag --sort=-v:refname -n -l ${1}* }; noglob gtl   |
| gunignore            | git update-index --no-assume-unchanged                       |
| gunwip               | -                                                            |
| gup                  | git pull --rebase                                            |
| gupv                 | git pull --rebase -v                                         |
| gupa                 | git pull --rebase --autostash                                |
| gupav                | git pull --rebase --autostash -v                             |
| glum                 | git pull upstream master                                     |
| gwch                 | git whatchanged -p --abbrev-commit --pretty=medium           |
| gwip                 | git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit --no-verify --no-gpg-sign -m "--wip-- [skip ci]" |

### Deprecated

These are aliases that have been removed, renamed, or otherwise modified in a way that may, or may not, receive further support.

| Alias  | Command                                              | Modification                                           |
| :----- | :--------------------------------------------------- | ------------------------------------------------------ |
| gap    | git add --patch                                      | new alias `gapa`                                       |
| gcl    | git config --list                                    | new alias `gcf`                                        |
| gdc    | git diff --cached                                    | new alias `gdca`                                       |
| gdt    | git difftool                                         | no replacement                                         |
| ggpull | git pull origin $(current_branch)                    | new alias `ggl` (`ggpull` still exists for now though) |
| ggpur  | git pull --rebase origin $(current_branch)           | new alias `ggu` (`ggpur` still exists for now though)  |
| ggpush | git push origin $(current_branch)                    | new alias `ggp` (`ggpush` still exists for now though) |
| gk     | gitk --all --branches                                | now aliased to `gitk --all --branches`                 |
| glg    | git log --stat --max-count = 10                      | now aliased to `git log --stat --color`                |
| glgg   | git log --graph --max-count = 10                     | now aliased to `git log --graph --color`               |
| gwc    | git whatchanged -p --abbrev-commit --pretty = medium | new alias `gwch`                                       |

## Functions

### Current

| Command                | Description                           |
| :--------------------- | :------------------------------------ |
| current_branch         | Return the name of the current branch |
| git_current_user_name  | Returns the `user.name` config value  |
| git_current_user_email | Returns the `user.email` config value |

### Work in Progress (WIP)

These features allow to pause a branch development and switch to another one (*"Work in Progress"*, or wip). When you want to go back to work, just unwip it.

| Command          | Description                                     |
| :--------------- | :---------------------------------------------- |
| work_in_progress | Echoes a warning if the current branch is a wip |
| gwip             | Commit wip branch                               |
| gunwip           | Uncommit wip branch                             |

### Deprecated

| Command            | Description                             | Reason                                                       |
| :----------------- | :-------------------------------------- | :----------------------------------------------------------- |
| current_repository | Return the names of the current remotes | Didn't work properly. Use `git remote -v` instead (`grv` alias) |

