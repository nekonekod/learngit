# git学习笔记

---

## 1. 创建版本库

1. 创建工作区目录
2. 然后使用**git init**命令初始化一个仓库
3. 目录下会多一个.git目录，可用ls -ah查看


## 2. 提交

1. 使用**git add file**命令添加文件到版本库

2. 使用**git commit**命令提交到本地仓库

`git commit -m 'comment'`
其中 -m是本地提交的说明，可以多次add后一并commit

## 3. 版本回退

1. 修改文件
   使用**git status**命令可以查看仓库当前的状态
   使用**git diff**命令能具体查看修改的内容

2. 提交修改的文件

`git add readme.txt`
`git commit -m 'mofity readme'`

3. 使用**git log**命令查看提交记录

使用**git log --pretty=oneline**优化输出
使用**git log --graph --pretty=oneline**显示图形
在git中 **HEAD**表示当前版本，上一个版本是HEAD^，上上版本是HEAD^^，往上100个版本是HEAD~100 

![](https://www.liaoxuefeng.com/files/attachments/001384907584977fc9d4b96c99f4b5f8e448fbd8589d0b2000/0)

4. 回退版本

使用**git reset**命令可以回退版本
回退到上一个版本
`git reset --hard HEAD^`
回到某个id的版本
`git reset --hard commit_id`

![](https://www.liaoxuefeng.com/files/attachments/001384907594057a873c79f14184b45a1a66b1509f90b7a000/0)

使用**git reflog**查看命令历史，可以回到未来的版本

## 4. 工作区和版本库

> **工作区（Working Directory）**

比如learngit文件夹

> **版本库（Respository）**

.git目录

> **暂存区**（Stage）

git add后修改所存放的位置

git自动创建了master分支和指向master的指针HEAD

![](https://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

git add命令把工作区要提交的所有修改放到暂存区（Stage），执行commit后一次性把暂存区的所有修改提交到分支

**如果不add到暂存区，是不会commit到版本库的**

## 5. 撤销修改

1. 使用**git checkout -- file**命令可以放弃工作区的修改，回到最近一次commit或者add的状态，注意必须要有`--`符号

2. 使用**git reset HEAD file**可以把已经add到暂存区的修改撤销掉（unstage），重新回到工作区，然后再使用git checkout -- file命令,HEAD表示会到最新的版本

3. 如果错误的修改已经提交到了版本库，可以使用git reset --hard commit_id回退版本，但前提是还没有push到远程仓库

## 6. 删除文件

   工作区文件删除后，在还没有将修改提交到暂存区前：

1. 恢复文件：**git checkout -- file**

2. 删除文件：**git rm file**

   或者直接git rm file也会将工作区的文件删除

## 7. 远程仓库

1. 使用**ssh-keygen**命令创建sshkey

2. 在用户主目录下会有.ssh目录，会有id_rsa和id_rsa.pub两个文件，前者是私钥，后者是公钥

3. 登陆github，在账号设置里添加ssh key

## 8. 添加远程库

1. 在github上Create repository后在本地仓库下运行**git remote add origin xxx**命令就可以关联本地仓库和远程仓库：

   `git remote add origin xxx`

2. 把本地库内容推送到远程库

   `git push -u origin master`

   第一次提交-u参数会把本地分支推送给远程仓库并关联起来，以后就可以使用**git push origin master**推送到远程仓库了

3. 使用**git push origin dev:remote_dev**把本地分支推送到远程分支（没有则自动创建remote_dev，":remote_dev"可以省略）

4. 使用**git remote**命令查看远程仓库，使用**git remote -v**查看抓取和推送的地址

## 9. 从远程仓库抓取

使用**git clone xxx**命令可以从远程仓库克隆

## 10. 分支管理

1. 使用**git checkout -b dev**命令创建dev分支并切换到dev分支

   使用**git checkout -b dev origin/dev**从远程仓库创建分支

2. 使用**git checkout master**命令切换到master分支

3. 使用**git branch**命令查看分支信息

4. 使用**git merge xxx** 命令将xxx分支合并到当前分支

   将dev分支合并到master分支上：

   `git checkout master`
   `git merge dev`

5. 使用**git branch -d dev**删除dev分支，使用**git branch -D dev**强行删除分支

6. 使用**git pull**命令从远程抓取，使用**git branch --set-upstream dev origin/dev**设置本地分支和远程分支的链接

7. 分支策略：master应该时稳定的，干活都在dev分支上

![](https://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

## 11. 处理冲突

合并时发生冲突时，可以使用git status查看冲突文件

git会使用`<<<<`,`=====`,`>>>>`标记不同分支的内容

修改完文件后git add，git commit

## 12. STASH

1. 使用**git stash**命令可以储藏工作现场
2. 使用**git stash list**命令可以查看储存的工作现场
3. 使用**git stash apply**可以恢复工作现场，但是stash的内容不消失
4. 使用**git stash pop**可以恢复现场，并且删除stash内容

## 13. 标签管理

1. 使用**git tag <name>** 命令创建标签，使用**git tag**命令查看标签

   使用**git tag <name> <commit_id>**给指定某次的提交创建标签

   使用**git tag -a <name> -m <message>**创建标签描述

2. shying**git tag -d <name>**删除标签

## 14. 忽略文件
1. 在工作区创建.gitignore文件
   检查.gitignore文件是否标准时git status是否显示working directory clean，如果确实想添加被忽略的文件，可以使用git add -f xxx强制添加

2. e.g.

 \# 注释

 \# 忽略foo.txt文件

 foo.txt

 \# 忽略所有的html文件

 *.html

 \# foo.html例外

 !foo.html

 \# 忽略所有的.o和.a文件

 *.[oa]

 \# 忽略dbg文件和dbg目录

 dbg

 \# 忽略dbg目录，不忽略dbg文件

 dbg/

 \# 忽略dbg文件，不忽略dbg目录

 dbg

 !dbg/

 \# 只忽略当前目录下的dbg目录和文件，子目录的dbg不忽略

 /dbg


## 14. 设置别名

使用`git config --global alias.st status`设置别名（git status —> git st） 

```g
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```