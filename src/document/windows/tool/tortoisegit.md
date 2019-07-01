# TortoiseGit #

## 下载地址 ##

官网下载地址：https://tortoisegit.org/download/

## 安装 ##

双击可执行程序，一路 next 即可。

## 配置免密操作 ##
+ 从开始菜单 TortoiseGit 文件夹下打开 puttyGen 

   在 puttyGen 中点击 Generate 生成公钥私钥

+ 将公钥填入 github 配置中
  
  github 账号上面：setting-->SSH and GPG keys-->New SSH key
  把id_rsa.pub里面的内容复制到 key 中，title 随便起

+ 将密钥保存成文件(后缀.ppk)

+ 从开始菜单 TortoiseGit 文件夹下打开 Pageant 

  右键通知区域中的 Pageant 图标 Add Key. 导入密钥文件

+ 验证是否配置成功

  提交一次 github 修改做为验证步骤即可。具体操作参见[基本操作](../basicop.md)