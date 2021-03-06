# 开发流程 #

以 github.com 上的 gitignore 项目为例，讲解开发流程。

- 首先需要在 https://github.com/github/gitignore 上 fork 该项目到自己私人仓库下。
- 在本地仓库开发完成，最终提交到官方仓库为一次完整的流程
- 本地仓库无法直接推送到官方仓库，需要先推送到私人仓库，然后在远程私人仓库中向官方仓库发起 pull request 才能合入官方仓库
- 私人仓库的 master 分支和本地仓库的 master 分支仅作为更新官方最新代码使用，不在上述分支做其他操作。需要在本地开发或修改时需要新建分支进行操作。

## 从远程私人仓库克隆到本地仓库 ##

``` shell
git clone git@github.com:linyoubin/gitignore.git
```

仓库地址在项目首页上获取
![get_repo_address](../image/get_repo_address.jpg)

## 添加远程官方仓库 ##

添加官方仓库

``` shell
git remote add offical git@github.com:github/gitignore.git
```

查看所有远程仓库地址

``` shell
git remote -vv
offical git@github.com:github/gitignore.git (fetch)
offical git@github.com:github/gitignore.git (push)
origin  git@github.com:linyoubin/gitignore.git (fetch)
origin  git@github.com:linyoubin/gitignore.git (push)
```

## 新建本地开发分支develop ##

``` shell
git checkout -b develop origin/master
```

## 修改代码 ##

修改代码，并且有多次 commit 记录。 期间不能 git pull 官方或个人仓库。误操作 git pull 解决方案请参考[rollback_git_pull](./trouble_shooting.md#回退gitpull操作)

## 合并开发分支多个commit ##

可以任意选择以下两种方式中的一种进行操作

### TortoiseGit操作 ###

show log


选择需要合并的多个 commit

右键选择combine to one commit

![combine](../image/combine_to_one_commit.jpg)

### Git操作 ###

查看本分支提交记录

``` shell
git log 
commit bc335b3a500d0ad35a6278e8d763096175b324a6 (HEAD -> develop)
Author: linyoub <linyoub@163.com>
Date:   Tue Jul 2 20:05:02 2019 +0800

    test2

commit c4be3a2e737ce171639b420446d2e237bd98fdd3
Author: linyoub <linyoub@163.com>
Date:   Tue Jul 2 20:04:41 2019 +0800

    test1

commit 50e42aa1064d004a5c99eaa72a2d8054a0d8de55 (origin/master, origin/HEAD, master)
Author: Alexey <alexey.khachatryan@gmail.com>
Date:   Thu Jun 27 19:49:08 2019 +0400

    Update Laravel.gitignore (#3098)

```

指定将前两次 commit 合并成一次提交 (50e42aa1064d004a5c99eaa72a2d8054a0d8de55 为第三个 commit 的 hash )

``` shell
git rebase -i 50e42aa1064d004a5c99eaa72a2d8054a0d8de55
```

编辑选择第一个 pick，第二个 s 进行合并

合并编辑新的提交日志 log

``` shell
git log
commit c7a7a93252013545839ca12235b7fb0094958767 (HEAD -> develop)
Author: linyoub <linyoub@163.com>
Date:   Tue Jul 2 20:04:41 2019 +0800

    test

commit 50e42aa1064d004a5c99eaa72a2d8054a0d8de55 (origin/master, origin/HEAD, master)
Author: Alexey <alexey.khachatryan@gmail.com>
Date:   Thu Jun 27 19:49:08 2019 +0400

    Update Laravel.gitignore (#3098)

```

可以看到之前的两次提交被合并成一次提交

## 更新官方最新代码到本地master ##

``` shell
git checkout master
git pull offical master
```

## 合并本地master分支到本地develop分支 ##

可以任意选择以下两种方式中的一种进行操作

### 使用TortoiseGit ###

checkout develop

rebase

右边选择 master 本地分支

start rebase

![rebase](../image/git_rebase.jpg)

期间可能需要解决冲突，冲突解决完之后，继续 rebase

>**Note:** 如果 master 分支没有修改时，会提示不需要 rebase。此时可以直接结束
>

### 使用Git ###

``` shell
git checkout develop
git rebase master
```

期间可能需要解决冲突，冲突解决完之后，继续 rebase

``` shell
git rebase --continue
```

## 推送本地develop分支到远程develop分支 ##

``` shell
git push origin develop
```

## 从远程develop分支请求合并到远程官方仓库 ##

在[官方项目](https://github.com/github/gitignore/pulls)中发起 pull request。源选择私人仓库的 develop 分支，目的选择官方仓库的 master 分支

![pull_request](../image/pull_request.jpg)

## 官方库同意并提交之后删除远程分支 ##

删除本地分支
``` shell
git checkout master
git branch -D develop
git branch -D origin/develop -r
```

删除远程分支
```
git push origin --delete develop
```

## 其他说明 ##

### 自动同步远程仓库 ###

脚本 sync_from_offical.sh 可以从官方仓库中拉取最新代码到本地仓库，再推送到远程私人仓库。

- GIT_HOME

``` shell
$cat sync_from_offical.sh
GIT_HOME=/d/personal/github/gitignore
LOCAL_BRANCH=master
OFFICAL_BRANCH="offical master"
ORIGIN_BRANCH="origin master"

DRY_RUN=0

function display_help()
{
    echo "$0 --help | -h"
    echo "$0 [--dry_run]"
    echo ""
    echo " --dry_run       : just print command, do not run it"
    echo ""

    exit $1
}

function execCMD()
{
    if [ $# -ne 1 ]; then
        echo "param is not 1: $*"
        exit -1
    fi

    echo "$ $1"

    if [ $DRY_RUN -eq 1 ]; then
        return
    fi

    $1

    ret=$?
    if [ 0 -ne $ret ]; then
        echo "Failed to exe cmd[$1], rc=$ret"
        exit -1
    fi

    echo ""
}

#read param
while [ "$1" != "" ]; do
    case $1 in
        --dry_run )         DRY_RUN=1
                    ;;
    --help | -h )       display_help 0
                    ;;
    * )                 display_help 1

    esac
    shift
done

execCMD "cd $GIT_HOME"
execCMD "git checkout $LOCAL_BRANCH"
execCMD "git pull $OFFICAL_BRANCH"
execCMD "git push $ORIGIN_BRANCH"
```
