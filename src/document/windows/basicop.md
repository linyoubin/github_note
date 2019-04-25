# 基本操作 #

## git bash ##
[git 安装](tool/git.md)之后自带了 git bash。 git bash 是一个类似于 Linux shell 的命令行窗口。可以方便地使用 git 命令进行项目的 clone, commit 等操作。

在文件浏览窗口鼠标右键-->Git Bash Here， 打开 git bash。以下命令行命令没有特殊说明的，均是在 git bash 中的操作。

### git clone ###
拷贝一个 Git 仓库到本地，让自己能够查看、修改、提交该项目。

``` shell
$ git clone git@github.com:yourname/yourrepository.git
```

>**Note:**
>
> git@github.com:yourname/yourrepository.git 为你在 github 上拥有的仓库路径。其中 yourname 为用户名，yourrepository 为仓库名
>
> 该命令会在当前目录下创建 git 本地仓库，目录名称为仓库名
>

### 常用配置 ###

代码中编译后，会存在大量临时生成的文件，而这些文件是我们不想提交的。为了避免每次提交时要处理不必要的修改文件，可以将此种类型的文件直接忽略掉。

#### 忽略版本中不存在的文件 ####

+ 生成忽略文件（在项目根目录执行命令，如果文件已存在则跳过此步骤）
   ```
   $ touch .gitignore
   ```

+ 打开文件 .gitignore , 将确认不需要提交的文件填入该文件中。样例如下：
   ```
   $ cat .gitignore
   /bin/
   /build/
   /.gitignore
   /code/auto_gen.hpp
   ```

>**Note:**
>
> .gitignore中的文件在切换不同分支时，在其它分支也会一直存在。
>

#### 恢复不想提交的版本文件 ####
当文件已经存在的版本中，但是想忽略这些文件的修改时，可以采用此方法。如果 version.hpp 里面填了版本的公共定义，每次编译后会根据 git 版本自动更新版本的信息。这种文件每个人编译时都会修改，但是不应该提交到版本中。

生成 .dirtylist
```
$ touch .dirtylist
```

将文件列表加入 .dirtylist 中，样例如下：
```
$ cat .dirtylist
version.hpp
src/version.hpp
```

转换换行符为 unix 形式
```
$ dos2unix.exe .dirtylist
```

将 .dirtylist 中的所有文件恢复成未修改的状态
```
$ for file in `cat .dirtylist`; do git checkout -- $file ; done
```

做为别名 dirtyclean 方便使用
```
$ git config --global alias.dirtyclean '!for file in `cat .dirtylist`; do git checkout -- $file ; done'

$ git dirtyclean
```

### git add ###
建议在 TortoiseGit 中添加并提交。

+ 如果发现有需要忽略的非版本中的文件，可以加入 .gitignore 中，参照[忽略版本中不存在的文件](basicop.md#忽略版本中不存在的文件#)。

+ 如果要恢复不需要修改的文件，可以加入到 .dirtylist 中，参照[恢复不想提交的版本文件](basicop.md#恢复不想提交的版本文件#)。

### git status ###

### git commit ###

### 指定某个版本创建分支 ###

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

   ![Merge](../../image/windows/basicop_merge.bmp)

   存在冲突时，鼠标右键冲突文件 TortoiseGit-->edit conflicts

