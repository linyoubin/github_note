# 基本操作 #

## 官方仓库和私人仓库 ##
在 github.com 上有官方仓库，也有私人仓库。

官方仓库一般为该项目的第一个仓库，是由官方人员负责维护更新的。
私人仓库是以个人帐户登陆 github.com 后，从官方仓库 fork 出来的项目。

例如有一个官方仓库为： https://github.com/996icu/996.ICU （其仓库地址为：git@github.com:996icu/996.ICU.git）
![offical](../image/basicop_offical.jpg)

在该页面上点击 fork，将该项目 fork 到自己的私人仓库中。这样该项目就会保存一个私人仓库在自己的个人 github 上。

## git bash ##
[git 安装](windows/tool/git.md)之后自带了 git bash。 git bash 是一个类似于 Linux shell 的命令行窗口。可以方便地使用 git 命令进行项目的 clone, commit 等操作。

在文件浏览窗口鼠标右键-->Git Bash Here， 打开 git bash。以下命令行命令没有特殊说明的，均是在 git bash 中的操作。

### git clone ###
拷贝一个 Git 远程私人仓库到本地，让自己能够查看、修改、提交该项目。

``` shell
$ git clone git@github.com:yourname/yourrepository.git
```

>**Note:**
>
> git@github.com:yourname/yourrepository.git 为你在 github 上拥有的私人仓库路径。其中 yourname 为用户名，yourrepository 为仓库名
>
> 该命令会在当前目录下创建 git 本地仓库，目录名称为仓库名
>

### git remote ###
用于查看、操作远程仓库的命令

查看远程仓库
```
git remote -vv 
```

添加官方仓库，并命名为 offical。方便随时更新官方仓库的修改到私人仓库中。

```
git remote add official git@github.com:official/officalrepository.git
```

>**Note:**
>
> git@github.com:official/officalrepository.git 为官方仓库的路径。

### git status ###
用于查看当前本地仓库的提交状态。 可以显示未提交和没有版本监控的文件

``` shell
$ touch aa.txt
$ git status
On branch master
Your branch and 'origin/master' have diverged,
and have 3 and 3 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
        modified:   src/document/basicop.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        aa.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

>**Notes:**
>
>在本例中，有两个版本中的文件内容被修改了：README.md 和 src/document/basicop.md
>
>还有一是没有在版本中的新文件：aa.txt

### git add ###

将修改保存到暂存区，等待 commit。
   ``` shell
   git add README.md src/document/basicop.md aa.txt
   ```

也可以执行以下命令将所有修改和新加的文件全部加入暂存区
   ``` shell
   git add .
   ```

> **Note:** 如果不执行 git add, 直接执行 git commit 修改文件是不会提交到本地仓库的。
>

### git commit ###

将暂存区的文件提交到本地仓库
``` shell
git commit
```

### git pull ###

将远程仓库中的修改同步并 merge 到本地仓库中
```
git pull offical master
```

git 会自动合并代码。如果有冲突时，需要自己解决冲突并提交。

### git push ###

将本地仓库中的修改推送到远程仓库中
```
git push origin master
```

>**Note:** 一般使用 git push 将修改推送到自己的远程私人仓库，再在页面上使用 pull request 将私人仓库的修改推送到官方仓库
>

### 基本指定版本开发新功能 ###
指定基于某个版本进行功能开发。

+ 以当前 master 主分支的代码创建本地分支 test，并同时切换到 test 分支
   ``` shell
   $ git checkout -b test
   ```

+ 将当前 test 分支回退到某个版本

   获取更新 log 
   ``` shell
   $ git log
   commit 1a2db539e6e4966caa8badb044db9d5297f8a800
   ...
   commit 828c977682ecdf45eb7eec0721183811752eecfe
   ...
   ```

   选择需要回退的版本，并将当前 test 分支回退到指定版本
   ``` shell
   $ git reset --hard 828c977682ecdf45eb7eec0721183811752eecfe
   ```

+ 后续则可以在 test 分支上进行代码编写
   
   切换到 test 分支
   ```
   $ git checkout test
   ```

+ test 分支代码编写完毕后，执行合并提交

   切换回 master 分支
   ```
   $ git checkout master
   ```

   在仓库目录内，鼠标右键 TortoiseGit-->Merge，选择从需要合并的源 Branch 

   ![Merge](../image/basicop_merge.bmp)

   存在冲突时，鼠标右键冲突文件 TortoiseGit-->edit conflicts

+ 合并完代码之后，提交到远程仓库

  提交到远程仓库的操作建议在 TortoiseGit 下进行。使用该工具可以在 Windows 下以视图形式复查修改的代码，还有一些没必要提交的文件也可以识别出来。

  同时也可以先将本地多次 commit 合并成一个 commit ，再做为一次提交 push 到远程仓库，避免远程仓库有过多的无效 commit 日志。

  >**Notes:**
  >
  >只能合并自己本地分支的 commit，否则会造成无法 push 远程仓库的问题
  >