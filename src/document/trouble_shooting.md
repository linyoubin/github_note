# 常见问题 #
## 回退 git pull 操作 ##
### 问题描述 ###
在本地分支多个 commit 之后，错误执行 git pull/merge 同步了官方最新代码，会扰乱自身的 commit 时间线，与官方的提交穿插在一直，导致无法将本地的多次 commit 合并成一个 commit.

- git pull 之前本地分支的多个 commit 是连续的
   ``` shell
    $git log
    commit d2bd7aa608990e413862ff938bda9e91cce63fa0 (HEAD -> develop)
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:27:50 2019 +0800

        develop2

    commit 66469a5006219eee25d2a54e17bfbb4f600e26cf
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:26:55 2019 +0800

        develop1

    commit 7604979fb20c56477a95f41cc1697f6c462ff2e0
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:25:41 2019 +0800

        init
    ```

- git pull 之后会扰乱本地分支的 commit 日志，导致无法合并多次 commit
    ``` shell
    $git log
    commit 4af43367295a8f437a00b594327eaecc9e6e4826 (HEAD -> develop)
    Merge: d2bd7aa d7323f0
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:29:43 2019 +0800

        merge

    commit d2bd7aa608990e413862ff938bda9e91cce63fa0
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:27:50 2019 +0800

        develop2

    commit d7323f00c18e55f171aa9332dbda7cb6830e339a (origin/master, master)
    Author: YouBin Lin <linyoub@163.com>
    Date:   Wed Jul 3 11:27:14 2019 +0800

        Update README.md

    commit 66469a5006219eee25d2a54e17bfbb4f600e26cf
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:26:55 2019 +0800

        develop1

    ```
    中间穿插了另一个 master 分支的提交，还多了一个 merge commit

### 解决方法 ###
可以通过命令 git reflog 查看 git pull 操作的日志，结合 git reset 将本地仓库恢复到 git pull 之前的提交状态。

>**Note:** 只能恢复到 commit 的状态。如果存在未 commit 的文件需要自行备份再恢复过来
>

- 通过 git reflog 查看操作日志。选择 git pull 之前的 commit 操作 hash
    ```shell
    $git reflog
    4af4336 (HEAD -> develop) HEAD@{0}: commit (merge): merge
    d2bd7aa HEAD@{1}: checkout: moving from master to develop
    d7323f0 (origin/master, master) HEAD@{2}: pull origin master: Fast-forward
    7604979 HEAD@{3}: checkout: moving from develop to master
    d2bd7aa HEAD@{4}: commit: develop2
    66469a5 HEAD@{5}: commit: develop1
    7604979 HEAD@{6}: checkout: moving from master to develop
    7604979 HEAD@{7}: commit (initial): init
    ```

- 通过 git reset 命令将当前分支恢复到 d2bd7aa 的提交状态
    ``` shell
    $git reset --hard d2bd7aa
    HEAD is now at d2bd7aa develop2
    ```

    查看 git log 确认结果
    ``` shell
    $git log
    commit d2bd7aa608990e413862ff938bda9e91cce63fa0 (HEAD -> develop)
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:27:50 2019 +0800

        develop2

    commit 66469a5006219eee25d2a54e17bfbb4f600e26cf
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:26:55 2019 +0800

        develop1

    commit 7604979fb20c56477a95f41cc1697f6c462ff2e0
    Author: linyoub <linyoub@163.com>
    Date:   Wed Jul 3 11:25:41 2019 +0800

        init
    ```

