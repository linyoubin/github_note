# Git-svn #

## 下载 svn 代码 ##

+ 命令格式

   ``` shell
   git svn clone <svn仓库路径> [本地文件夹名] [其他参数]
   ```

+ 指定从 svn 上版本从 38000 开始到最新的的代码同步到到本地目录

   ``` shell
   $ git svn clone -r38000:HEAD -s http://svn/sequoiadb --prefix=svn/
   ```

   >**Note:**
   >
   >-s 告诉 Git 该 Subversion 仓库遵循了基本的分支和标签命名法则，也就是标准布局。如果你的主干( trunk，相当于非分布式版本控制里的 master 分支，代表开发的主线），分支( branches )或者标签( tags )以不同的方式命名，则应做出相应改变。
   >
   >-s 参数其实是-T trunk -b branches -t tags 的缩写，这些参数告诉 git 这些文件夹与git分支、tag、master的对应关系。
   >
   >--prefix=svn/ 给 svn 的所有 remote 名称增加了一个前缀 svn，这样比较统一，而且可以防止 warning: refname 'xxx' is ambiguous.

## 工作流程 ##

+ 查看所有分支

   ``` shell
   $ git branch -a
     * master
     remotes/svn/sequoiadb_2.8
     remotes/svn/trunk
   ```

+ 创建本地分支与远程分支的对应关系
   ``` shell
   $ git checkout -b sequoiadb_2.8 remotes/svn/sequoiadb_2.8
   ```

+ 在本地工作，commit 到对应分支上

   在 sequoiadb_2.8 分支上再创建本地 story1 分支
   ```
   $ git checkout sequoiadb_2.8
   $ git checkout -b story1
   ```

   切换到本地 story1 分支，修改代码
   ```
   $ git checkout story1
   ```
   
   验证通过后将改动合并到 sequoiadb_2.8 分支
   ``` shell
   $ git checkout sequoiadb_2.8
   $ git svn rebase
   $ git merge story1      //建议在 windows 下操作，有冲突时，直接用 TortoiseGit 解决冲突
   ```

   提交代码到 svn
   ``` shell
   $ git svn dcommit -m "commit log"
   ```

   story1 分支不需要后，删除该分支
   ```
   $ git branch -d story1
   ```

## 手工添加远程 svn 分支 ##

+ 添加远程分支信息

    命令格式
    ``` shell
    git config --add svn-remote.<远程分支名称>.url <svn地址，要包含具体分支路径>
    git config --add svn-remote.<远程分支名称>.fetch :refs/remotes/<远程分支名称>
    ```

    > **Note:**
    >
    > 说明：此处的“远程分支名称”可以随意填写，只要这三个保持一致即可。

    示例
    ``` shell
    $ git config --add svn-remote.svn/sequoiadb_2.8.url http://svn/sequoiadb/branches/sequoiadb_2.8
    $ git config --add svn-remote.svn/sequoiadb_2.8.fetch :refs/remotes/svn/sequoiadb_2.8
    ```

+ 根据远程分支创建本地分支

    命令格式
    ``` shell
    git svn fetch <远程分支名称> 获取svn仓库该分支的代码
    git checkout -b <本地分支名> <远程分支名称>
    ```

    示例
    ``` shell
    $ git svn fetch svn/branch1
    $ git checkout -b branch1 svn/branch1
    ```

