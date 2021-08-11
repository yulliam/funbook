# Git撤销&回滚操作(git reset 和 get revert)

https://blog.csdn.net/asoar/article/details/84111841

‌

# **git的工作流**

‌

工作区：即自己当前分支所修改的代码，git add xx 之前的！不包括 git add xx 和 git commit xxx 之后的。

‌

暂存区：已经 git add xxx 进去，且未 git commit xxx 的。

‌

本地分支：已经git commit -m xxx 提交到本地分支的。 这里写图片描述

![img](https://gblobscdn.gitbook.com/assets%2F-MVxhDL96fVN_Pxj0l04%2F-MWY_8iqBP7UHN0mvj8A%2F-MWY__wEPEr635bwAtSm%2Fimage.png?alt=media&token=db717835-4d43-41d2-b746-8b4d1b41f2e4)

‌

# **![img](https://img-blog.csdn.net/20170517142150678?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGVvcngwMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 代码回滚**

‌

在上传代码到远程仓库的时候，不免会出现问题，任何过程都有可能要回滚代码：

‌

1、在工作区的代码



```
git checkout -- a.txt   # 丢弃某个文件，或者
git checkout -- .       # 丢弃全部
```



```
// 删除当前目录下所有没有 track 过的文件
// 不会删除 .gitignore 文件里面指定的文件夹和文件, 不管这些文件有没有被 track 过
git clean -f
git clean -df #返回到某个节点，（未跟踪文件的删除）
git clean 参数
    -n 不实际删除，只是进行演练，展示将要进行的操作，有哪些文件将要被删除。（可先使用该命令参数，然后再决定是否执行）
    -f 删除文件
    -i 显示将要删除的文件
    -d 递归删除目录及文件（未跟踪的）
    -q 仅显示错误，成功删除的文件不显示
```

‌

注意：git checkout – . 丢弃全部，也包括：新增的文件会被删除、删除的文件会恢复回来、修改的文件会回去。这几个前提都说的是，回到暂存区之前的样子。对之前保存在暂存区里的代码不会有任何影响。对commit提交到本地分支的代码就更没影响了。当然，如果你之前压根都没有暂存或commit，那就是回到你上次pull下来的样子了。

‌

2、代码git add到缓存区，并未commit提交



```
git reset HEAD .  或者
git reset HEAD a.txt
```

‌

这个命令仅改变暂存区，并不改变工作区，这意味着在无任何其他操作的情况下，工作区中的实际文件同该命令运行之前无任何变化

‌

3、git commit到本地分支、但没有git push到远程

‌

git log # 得到你需要回退一次提交的commit id git reset --hard  # 回到其中你想要的某个版 或者 git reset --hard HEAD^  # 回到最新的一次提交 或者 git reset HEAD^  # 此时代码保留，回到 git add 之前

‌

4、git push把修改提交到远程仓库 1）通过git reset是直接删除指定的commit

‌

git log # 得到你需要回退一次提交的commit id git reset --hard git push origin HEAD --force # 强制提交一次，之前错误的提交就从远程仓库删除

‌

2）通过git revert是用一次新的commit来回滚之前的commit

‌

git log # 得到你需要回退一次提交的commit id git revert  # 撤销指定的版本，撤销也会作为一次提交进行保存

‌

3） git revert 和 git reset的区别 - git revert是用一次新的commit来回滚之前的commit，此次提交之前的commit都会被保留； - git reset是回到某次提交，提交及之前的commit都会被保留，但是此commit id之后的修改都会被删除

‌

**开发过程中，你肯定会遇到这样的场景：**

‌

**场景一：**

> 糟了，我刚把不想要的代码，commit到本地仓库中了，但是还没有做push操作！

‌

**场景二：**

> 彻底完了，刚线上更新的代码出现问题了，需要还原这次提交的代码！

‌

**场景三：**

> 刚才我发现之前的某次提交太愚蠢了，现在想要干掉它！

‌

# **撤销**

‌

上述**场景一**，在未进行`git push`前的所有操作，都是在“本地仓库”中执行的。我们暂且将“本地仓库”的代码还原操作叫做“撤销”！

‌

**情况一：文件被修改了，但未执行**`**git add**`**操作(working tree内撤销)**



```
git checkout fileName
git checkout .
```

‌

**情况二：同时对多个文件执行了**`**git add**`**操作，但本次只想提交其中一部分文件**



```
$ git add *
$ git status
# 取消暂存
$ git reset HEAD 
```

‌

**情况三：文件执行了**`**git add**`**操作，但想撤销对其的修改（index内回滚）**



```
# 取消暂存
git reset HEAD fileName
# 撤销修改
git checkout fileName
```

‌

**情况四：修改的文件已被**`**git commit**`**，但想再次修改不再产生新的Commit**



```
# 修改最后一次提交 
$ git add sample.txt
$ git commit --amend -m"说明"
```

‌

**情况五：已在本地进行了多次**`**git commit**`**操作，现在想撤销到其中某次Commit**



```
git reset [--hard|soft|mixed|merge|keep] [commit|HEAD]
```

‌

具体参数和使用说明，请查看：[Git Pro深入浅出（二）中的重置揭秘部分](http://blog.csdn.net/ligang2585116/article/details/51816372#t7)

‌

# **回滚**

‌

上述**场景二**，已进行`git push`，即已推送到“远程仓库”中。我们将已被提交到“远程仓库”的代码还原操作叫做“回滚”！**注意：对远程仓库做回滚操作是有风险的，需提前做好备份和通知其他团队成员！**

‌

*如果你每次更新线上，都会打*[*tag*](http://blog.csdn.net/ligang2585116/article/details/46468709)*，那恭喜你，你可以很快的处理上述****场景二\****的情况*



```
git checkout 
```

‌

*如果你回到当前HEAD指向*



```
git checkout 
```

‌

**情况一：撤销指定文件到指定版本**



```
# 查看指定文件的历史版本
git log 
# 回滚到指定commitID
git checkout  
```

‌

**情况二：删除最后一次远程提交**

‌

*方式一：使用revert*



```
git revert HEAD
git push origin master
```

‌

*方式二：使用reset*



```
git reset --hard HEAD^
git push origin master -f
```

‌

*二者区别：*

‌

- **revert**是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在；
- **reset**是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。

‌

**情况三：回滚某次提交**



```
# 找到要回滚的commitID
git log
git revert commitID
```

‌

# **删除某次提交**



```
git log --oneline -n5
```

![img](https://gblobscdn.gitbook.com/assets%2F-MVxhDL96fVN_Pxj0l04%2F-MWY_8iqBP7UHN0mvj8A%2F-MWb9_GLybtf2XOeGs9n%2Fimage.png?alt=media&token=8c8ac787-f01e-498a-9c2e-b0b4639737a3)



```
git rebase -i "commit id"^
```

‌

**注意：**需要注意最后的*^*号，意思是commit id的前一次提交



```
git rebase -i "5b3ba7a"^
```

![img](https://gblobscdn.gitbook.com/assets%2F-MVxhDL96fVN_Pxj0l04%2F-MWY_8iqBP7UHN0mvj8A%2F-MWbASTiy3Ien3HtZ2C7%2Fimage.png?alt=media&token=06568cb0-6809-4054-a696-f7d0326f806c)

‌

在编辑框中删除相关commit，如`pick 5b3ba7a test2`，然后保存退出（如果遇到冲突需要先解决冲突）！



```
git push origin master -f
```

‌

**通过上述操作，如果你想对历史多个commit进行处理或者，可以选择**`**git rebase -i**`**，只需删除对应的记录就好。rebase还可对 commit 消息进行编辑，以及合并多个commit。**