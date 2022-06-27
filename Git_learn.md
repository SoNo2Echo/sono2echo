[toc]

## 什么是Git 
最伟大的分布式版本控制系统（哈哈哈）

## 为什么要用Git？
还有其他如CVS SVN这样的集中式的版本控制系统但是都不怎么好用，因为比较慢。然后Linus就花两周时间开源了一个分布式系统Git。用的C语言写的。

***
**配置Git**
`git config --global user.name "Your Name"`

`git config --global user.email "email@example.com"`

## 创建一个版本库
版本库又叫仓库，repository，目录中的文件都可以被Git管理，删除修改都被跟踪。

当然了创建一个版本库非常easy，如下

```
mkdir 仓库名字
cd 仓库目录
pwd用于显示当前目录

最最最重要的命令
git init
也不是很重要就是要把你创建的目录变成仓库哈哈哈
```
不要用Windows的记事本来改文件

## 把大象放进冰箱要三步而代码放进Git只要两步
```
1. git add filename
2. git commit -m "wrote a new file"
```

`git status` 用来随时查看仓库的当前状态


## OK Git的时光穿梭机
首先你可以通过`git diff filename.txt`查看你修改了什么内容，假设这个文件是你上周改的这周绝壁忘记了啊。

### 重生之版本退回
`git log` 可以查看历史记录，打印输出的时间线是从近到远
`git log --pretty=oneline` 打印信息输出更简洁

==*commit 3ef3eb5275b4bf9fdae5aca741868efe19c0eb9a*== 这个一大串的字符是commit id（版本号）

在Git中==HEAD==表示当前版本，==HEAD^== ，最大到==HEAD^^== ，再大就是==HEAD^100==这种写法。

退回到上个版本的命令 `git reset --hard HEAD^`

举个栗子：
1.例如假设你写了三个版本1，2，3。当前你处于3状态你通过`git reset`回到了2版本，但是你后悔了，你想从现在2穿越到3去，这时候只要找到3版本的commit id就可以使用`git reset --hard commit id`回去了。

2.你还可以使用`git reflog`查看你每一次命令就可以看到之前的版本号。

***
~~亚雷ma，又到理论部分了~~
## 玩归玩，闹归闹，【工作区、暂存区】你还得知道

git和其他版本控制器不同在于又暂存区这个概念。

**工作区**即为电脑里看得到的目录，如我们创建的一个文件夹就是工作区。

我们往git版本库里添加东西的时候是分两步执行的，add和commit，那么add操作就是把==文件修改添加到暂存区==。commit实际上==把暂存区的所有内容提交到当前分支==，（目前我理解分支每个人就是一个分支，多人协作每个人在自己的分支上完成修改）

==目前理解就是先add再commit==

## 撤销修改

`git checkout -- filename.txt`
把filename.txt在工作区的修改全部撤销，有两种情况：

1.filename.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2.已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
就是让这个文件回到最近一次git commit或git add时的状态。

如果`git add`到暂存区了，还没有commit可以使用`git reset HEAD filename.txt`

**~~亚雷吗~~这部分感觉很乱先码一下廖雪峰老师的小结把orz**

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。