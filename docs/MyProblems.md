####  1、从未提交过的文件可以用.gitignore
也就是添加之后从来没有提交（commit）过的文件，可以使用.gitignore忽略该文件

该文件只能作用于未跟踪的文件（Untracked Files），也就是那些从来没有被 git 记录过的文件

比如，忽略log/下的日志文件，可以在.gitignore中写


```
log/*
```
#### 2、已经推送（push）过的文件，想从git远程库中删除，并在以后的提交中忽略，但是却还想在本地保留这个文件
执行命令

```
git rm --cached Xml/config.xml
```

后面的 Xml/config.xml 是要从远程库中删除的文件的路径，支持通配符*

比如，不小心提交到git上的一些log日志文件，想从远程库删除，可以用这个命令

#### 3、已经推送（push）过的文件，想在以后的提交时忽略此文件，即使本地已经修改过，而且不删除git远程库中相应文件

执行命令
```
git update-index --assume-unchanged Xml/config.xml  
```
后面的 Xml/config.xml 是要忽略的文件的路径。如果要忽略一个目录，打开 git bash，cd到 目标目录下，执行：
```
git update-index --assume-unchanged $(git ls-files | tr '\n' ' ')
```

比如有一个配置文件 jdbc.properties 记录数据库的链接信息，每个人的链接信息肯定不一样，但是又要提供一个标准的模板，用来告知如何填写链接信息，那么就需要在git远程库上有一个标准配置文件，然后每个人根据自己的具体情况，修改一份链接信息自用，而且不会将该配置文件提交到库！



---

#### 本地修改不提交到远程仓库

```
git update-index --assume-unchanged 文件名
```


#### 取消本地忽略

```
git update-index --no-assume-unchanged 文件名
```


#### 查看分支关联信息

```
git branch -vv

git remote show origin

cat .git/config

```

#### 本地分支关联远程分支
```
git branch --set-upstream-to=origin/dev dev
```


###  查看变更比较

```
EXAMPLES
       Various ways to check your working tree

               $ git diff            (1)
               $ git diff --cached   (2)
               $ git diff HEAD       (3)

           1. Changes in the working tree not yet staged for the next commit.
           2. Changes between the index and your last commit; what you would be committing if you run "git commit" without "-a" option.
           3. Changes in the working tree since your last commit; what you would be committing if you run "git commit -a"

       Comparing with arbitrary commits

               $ git diff test            (1)
               $ git diff HEAD -- ./test  (2)
               $ git diff HEAD^ HEAD      (3)

           1. Instead of using the tip of the current branch, compare with the tip of "test" branch.
           2. Instead of comparing with the tip of "test" branch, compare with the tip of the current branch, but limit the comparison to the file "test".
           3. Compare the version before the last commit and the last commit.

       Comparing branches

               $ git diff topic master    (1)
               $ git diff topic..master   (2)
               $ git diff topic...master  (3)

           1. Changes between the tips of the topic and the master branches.
           2. Same as above.
           3. Changes that occurred on the master branch since when the topic branch was started off it.

       Limiting the diff output

               $ git diff --diff-filter=MRC            (1)
               $ git diff --name-status                (2)
               $ git diff arch/i386 include/asm-i386   (3)

           1. Show only modification, rename, and copy, but not addition or deletion.
           2. Show only names and the nature of change, but not actual diff output.
           3. Limit diff output to named subtrees.

       Munging the diff output

               $ git diff --find-copies-harder -B -C  (1)
               $ git diff -R                          (2)

           1. Spend extra cycles to find renames, copies and complete rewrites (very expensive).
           2. Output diff in reverse.

```


git diff    # 查看尚未暂存的文件更新了哪些部分

git diff --staged   # 比对已暂存文件与最后一次提交的文件差异：

git diff --cached   # 查看已经暂存起来的变化（ --staged 和 --cached 是同义词）



### 生成补丁，应用

```
git status
git diff > patch.diff

git apply patch.diff
```


### git diff  出现old mode   new mode   并且 git restore 不起作用

git diff 则显示

```
git diff xxx
old mode 100644
new mode 100755
```
原因是文件的权限改变引起的，可以设置git忽略本地的权限

* 方法1，当前项目设置
```
git config core.filemode false
```

* 方法2，git 全局设置 
```
git config --global core.filemode false
```

### 怎么嵌套git库

有种情况我们经常会遇到：某个工作中的项目需要包含并使用另一个项目。 也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。

Git 通过子模块来解决这个问题。 子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

克隆主仓库
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace
$ git clone https://github.com/hexojs/hexo-starter.git
Cloning into 'hexo-starter'...
remote: Enumerating objects: 32, done.
remote: Counting objects: 100% (32/32), done.
remote: Compressing objects: 100% (27/27), done.
remote: Total 199 (delta 16), reused 9 (delta 5), pack-reused 167
Receiving objects: 100% (199/199), 40.45 KiB | 9.00 KiB/s, done.
Resolving deltas: 100% (95/95), done.


```

进入主仓库目录

```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace
$ cd hexo-starter/

```

查看主仓库目录
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace/hexo-starter (master)
$ ls -al
total 17
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 ./
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 ../
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 .git/
-rw-r--r-- 1 qiaoyafeng 197121   71 十二月 24 10:34 .gitignore
-rw-r--r-- 1 qiaoyafeng 197121    0 十二月 24 10:34 _config.landscape.yml
-rw-r--r-- 1 qiaoyafeng 197121 2545 十二月 24 10:34 _config.yml
-rw-r--r-- 1 qiaoyafeng 197121  641 十二月 24 10:34 package.json
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 scaffolds/
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 source/
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 themes/



```

使用子模块
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace/hexo-starter (master)
$ git submodule add https://github.com/hexojs/hexo-theme-phase.git themes/phase
Cloning into 'H:/mygitspace/hexo-starter/themes/phase'...
remote: Enumerating objects: 141, done.
remote: Counting objects: 100% (141/141), done.
remote: Compressing objects: 100% (107/107), done.
remote: Total 586 (delta 85), reused 60 (delta 33), pack-reused 445
Receiving objects: 100% (586/586), 326.63 KiB | 67.00 KiB/s, done.
Resolving deltas: 100% (254/254), done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory


```

查看主仓库目录：
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace/hexo-starter (master)
$ ls -al
total 18
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:37 ./
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 ../
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:37 .git/
-rw-r--r-- 1 qiaoyafeng 197121   71 十二月 24 10:34 .gitignore
-rw-r--r-- 1 qiaoyafeng 197121  102 十二月 24 10:37 .gitmodules
-rw-r--r-- 1 qiaoyafeng 197121    0 十二月 24 10:34 _config.landscape.yml
-rw-r--r-- 1 qiaoyafeng 197121 2545 十二月 24 10:34 _config.yml
-rw-r--r-- 1 qiaoyafeng 197121  641 十二月 24 10:34 package.json
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 scaffolds/
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:34 source/
drwxr-xr-x 1 qiaoyafeng 197121    0 十二月 24 10:36 themes/

```

运行查看状态`git status` 多了文件.gitmodules和文件夹themes/phase
```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitmodules
        new file:   themes/phase

```

查看子模块配置文件.gitmodules内容：

```
[submodule "themes/phase"]
        path = themes/phase
        url = https://github.com/hexojs/hexo-theme-phase.git

```


查看主仓库配置：
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace/hexo-starter (master)
$ git config -l
credential.helper=manager
http.sslcainfo=D:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
http.sslbackend=openssl
diff.astextplain.textconv=astextplain
core.autocrlf=true
core.fscache=true
core.symlinks=false
user.name=qiao_yafeng
user.email=qiao_yafeng@outlook.com
core.filemode=false
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/hexojs/hexo-starter.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
submodule.themes/phase.url=https://github.com/hexojs/hexo-theme-phase.git
submodule.themes/phase.active=true

```
仓库配置中共有两个仓库的路径分别是：
```
remote.origin.url=https://github.com/hexojs/hexo-starter.git
submodule.themes/phase.url=https://github.com/hexojs/hexo-theme-phase.git
```
进入子模块仓库：

```
$ cd themes/phase/
```
查看子模块仓库配置：
```
qiaoyafeng@qiaoyafeng-PC MINGW64 /h/mygitspace/hexo-starter/themes/phase (master)
$ git config -l
credential.helper=manager
http.sslcainfo=D:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
http.sslbackend=openssl
diff.astextplain.textconv=astextplain
core.autocrlf=true
core.fscache=true
core.symlinks=false
user.name=qiao_yafeng
user.email=qiao_yafeng@outlook.com
core.filemode=false
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
core.worktree=../../../../themes/phase
remote.origin.url=https://github.com/hexojs/hexo-theme-phase.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master

```
子模块仓库配置仅有子模块仓库的路径：
```
remote.origin.url=https://github.com/hexojs/hexo-theme-phase.git
```
