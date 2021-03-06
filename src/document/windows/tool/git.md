# Git #

## 下载地址 ##

官网下载地址：https://git-scm.com

腾讯下载地址：https://pc.qq.com/detail/13/detail_22693.html

腾讯的链接比较快，官网的速度太慢。

## 安装 ##

windows 下双击可执行程序安装即可。注意安装时选择换行符的规则：
![Line](../../../image/windows/tool/git_install_line.png)

## 配置免密操作 ##

+ 在文件浏览窗口中，右键鼠标打开 Git Bash Here （不用进行你的资源库目录，在外面就行）
+ 使用 ssh-keygen 生成密钥

    ``` shell
    ssh-keygen -t rsa -C "your_email@example.com" 
    ```

    >**Note:**
    >
    >其中 your_email@example.com 为你在 github 的帐户邮箱名

    后面会提示你输入密码

    ``` shell
    Enter passphrase (empty for no passphrase): [Type a passphrase]  
    # Enter same passphrase again: [Type passphrase again] 
    ```

    生成成功之后会给出类似如下提示

    ``` shell
    # Your identification has been saved in /Users/you/.ssh/id_rsa.  
    # Your public key has been saved in /Users/you/.ssh/id_rsa.pub.  
    # The key fingerprint is:  
    # 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
    ```

+ 将公钥填入 github 配置中
  
  github 账号上面：setting-->SSH and GPG keys-->New SSH key
  把id_rsa.pub里面的内容复制到 key 中，title 随便起

+ 验证是否配置成功

   ``` shell
   ssh -T git@github.com
   ```

## Git 常用配置 ##

1. 配置换行符，防止 windows 和 linux 换行符不同导致文件内容不一致

``` shell
git config --global core.autocrlf false
```

2. 配置用户信息

``` shell
git config --global user.name "linyoubin"
git config --global user.email "linyoubin@sequoiadb.com"
```

3. 当 git log 无法分页显示时，设置 pager(需要重启客户端才能生效)

``` shell
git config --global core.pager 'C:/cygwin64/bin/less.exe -R'
```
