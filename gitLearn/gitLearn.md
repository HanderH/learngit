

#### Git是什么

Git是一中分布式版本控制工具，可以让多人协同对一些文件进行修改。

#### 为什么使用Git

Git是一中分布式版本控制工具,相比其他的版本控制工具,比如svn（集中式的版本控制工具，需要一个中央服务器，每次获取都需要从中央服务器中去获取，干完后再提交给中央服务器），每个人的电脑都是一个完整的版本库，修改之后会把各自的修改推送给对方。比集中式的版本控制工具更加安全，因为每个人的电脑都有完整的版本库，某一个人的电脑坏了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

#### Git的安装使用

windows

```shell
git config --global user.name "handerh"
git config --global user.email "husj0423@163.com"
```

#### Git版本库（repository)

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

创建一个repository,选择一个目录

```shell
cd /g/yaxin_env/Git
mkdir myrepository
git init
```

会发现多了一个.git目录，这个目录Git用来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

#### Git添加文件

```shell
git add <file>
git commit -m <message>
```

```shell
 git add file1.txt
 git add file2.txt file3.txt
 git commit -m "add 3 files.
```

#### Git查看仓库的修改

##### 查看仓库的状态

查看是否有文件被修改过

```shell
git status
```

##### 查看修改内容

````
git diff readme.txt
````

#### 版本回退

在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

显示从最近到最远的提交日志

````shell
$ git log
commit f747155088017f3d14b7ee5b9b4a1511c551d181 (HEAD -> master)
Author: handerh <husj0423@163.com>
Date:   Wed Oct 9 16:24:12 2019 +0800

    append GpL

commit baf58f16aa501d63b5ff14ffdc0fda8a98975c69
Author: handerh <husj0423@163.com>
Date:   Wed Oct 9 16:15:32 2019 +0800

    add distributed

commit 0024e212cb50c68f5a9fa1afce7c03b45fcae023
Author: handerh <husj0423@163.com>
Date:   Wed Oct 9 15:45:53 2019 +0800

    a readme.txt

````

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数

commit后面的是commit id版本号

##### 回退到上一个版本

```
$ git reset --hard HEAD^
HEAD is now at baf58f1 add distributed
```

这样最新的版本append GpL已经看不到了，想再回去已经回不去了，肿么办？

==只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本==：

```shell
$ git reset --hard f747155088017f3d14b7ee5b9b4a1511c551d181
HEAD is now at f747155 append GpL
```

版本号可以不写全

==在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？==

Git提供了一个命令`git reflog`用来记录你的每一次命令：

```shell
$ git reflog
f747155 (HEAD -> master) HEAD@{0}: reset: moving to f747155088017f3d14b7ee5b9b4a1511c551d181
baf58f1 HEAD@{1}: reset: moving to HEAD^
f747155 (HEAD -> master) HEAD@{2}: commit: append GpL
baf58f1 HEAD@{3}: commit: add distributed
0024e21 HEAD@{4}: commit (initial): a readme.txt
```

可以看到append GpL的id是f747155

#### 工作区和暂存区

##### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如myrepository

##### 版本库

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![](G:\image\git\工作区和版本库.PNG)

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支

#### 撤销修改

当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`

当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按上述操作

已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库

#### 删除文件

```shell
rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

这时候你有两个选择

1. 确定删除

```shell
git rm test.txt
git commit -m "remove test.txt"
```

2. 删错了,因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```shell
git checkout -- test.txt
```

#### 远程仓库

##### 添加远程仓库

首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：learngit

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```shell
$ git remote add origin git@github.com:handerh/learngit.git
```

上面的命令是与远程仓库进行关联，handerh为github的账户名

origin是Git默认的远程仓库的叫法，也可以改成别的

下一步就是将本地仓库的所有内容推送到远程仓库

```shell
$ git push -u origin master
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 4 threads
Compressing objects: 100% (17/17), done.
Writing objects: 100% (22/22), 1.78 KiB | 260.00 KiB/s, done.
Total 22 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), done.
remote: This repository moved. Please use the new location:
remote:   git@github.com:HanderH/learngit.git
To github.com:handerh/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用git push命令，实际是把当前分支推送到远程

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

从现在起，只要本地作了提交，就可以通过命令：

```shell
git push origin master
```

把本地`master`分支的最新修改推送至GitHub

==再这里遇到一个错误==

```java
git@github.com: Permission denied (publickey). fatal: Could not read from re
```

[解决办法](https://blog.csdn.net/huang3513/article/details/52837075)

##### git从远程库克隆

```shell
git clone git@github.com:handerh/GitTest.git
git clone https://github.com/HanderH/GitTest.git
```

GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

#### git分支管理

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

##### 创建与合并分支

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支，截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支

`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

创建分支并切换分支

```shell
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令

```shell
git branch dev
git checkout dev
```

查看当前分支

```shell
$ git branch
* dev
  master
```

修改readme.txt文件，并提交

切换到master分支，查看readme.txt文件内容

```shell
git checkout master
或者
git switch master
## git switch -c dev 创建并切换到分支dev
```

发现readme.txt的文件内容并没有发生改变，是由于我们是在dev分支进行修改的，切换到master分支的内容并没有修改

合并分支

```shell
$ git merge dev
Updating 9260d48..ef4a678
Fast-forward
readme.txt | 2 +-
1 file changed, 1 insertion(+), 1 deletion(-)
```

合并后可以删除dev分支

```
git branch -d dev
```

小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

#### 解决冲突

