# learn-git

learn-git

##Git is a distributed version control system.

##Git is free software distributed under the GPL.

##Git has a mutable index called stage.

##Git tracks changes of files.

## Creating a new branch is quick.


查看分支： git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name>

创建+切换分支：git checkout -b <name>或者git switch -c <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


#### Creating a new branch is quick AND simple.


### 分支管理策略

* 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

* 如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

* 准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：

```text
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)


```


> * 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
> * 当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；
> * 在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
