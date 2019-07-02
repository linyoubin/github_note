# Git 常用配置 #

代码中编译后，会存在大量临时生成的文件，而这些文件是我们不想提交的。为了避免每次提交时要处理不必要的修改文件，可以将此种类型的文件直接忽略掉。

## 忽略版本中不存在的文件 ##

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

## 忽略已经在版本中的文件 ##

当文件已经存在的版本中，但是想忽略这些文件的修改时，可以采用此方法。如果 version.hpp 里面填了版本的公共定义，每次编译后会根据 git 版本自动更新版本的信息。这种文件每个人编译时都会修改，但是不应该提交到版本中。

+ 忽略已经在版本中的文件
   ```
   $ git update-index --assume-unchanged version.hpp
   ```

   取消该忽略则使用命令，可以重新跟踪该文件的提交状态
   ```
   $ git update-index --no-assume-unchanged version.hpp
   ```

+ 列取已经在版本中忽略的文件
   ```
   $ git ls-files -v | grep '^[[:lower:]]'
   h version.hpp
   ```

   该命令太冗长，可以做成别名更方便使用
   ```
   $ git config --global alias.ignored '!git ls-files -v | grep "^[[:lower:]]"'
   ```

   使用别名 git ignored 查看
   ```
   $ git ignored
   h version.hpp
   ```


## 恢复不想提交的版本文件 ##
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