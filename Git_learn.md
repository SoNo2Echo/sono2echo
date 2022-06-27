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

**~~亚雷吗~~这部分感觉很乱先码一下廖雪峰老师的小结吧orz**

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## 删除文件

`git rm filename.txt`

1.从版本库中删除文件就使用 `git rm`命令来删除文件，然后要`git commit`
2.错删了，直接`git checkout -- filename.txt`,此命令就是用版本替换工作区的版本。

***
## 添加一个远程GitHub仓库
1.新建一个GitHub仓库
2.将GitHub仓库与本地仓库关联，在本地git使用以下命令：

`git remote add origin git@github.com:self_username/hubname.git`

远程库名字就是origin，这是git的默认叫法。
3.将本地库内容推送到远程GitHub使用以下命令：

`git push -u origin master`

>把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

4.从现在开始本地提交后只需要用`git push origin master`

### ~~删库 ---> 跑路~~
地址写错了或者想要删除远程库可以用`git remote rm hub_name`
删之前查看以下库信息`git remote -v`

>此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

## 克隆一个库? ~~CV工程~~
如果是从0开发，那么最好首先创建一个远程库。
使用`git clone git@github.com:Eechookevin/gitskill.git`

## 创建与合并分支

1.创建分支并且切换到分支 `git checkout -b dev` == `git branch dev + git checkout dev`
2.查看分支 `git branch` 分支前面会有一个*
3.在分支上做操作然后add commit后切回master，会发现master上的文件没有改变（因为还没有合并）
4.合并分支和master `git merge dev`
5.合并完成后删除dev分支 `git branch -d dev`
***

**切换分支和撤销很像`git checkout <branch>`，撤销多个-- `git checkout -- <file>`,所以引入switch更科学。**
创建分支 `git switch -c dev`
切换到已有分支 `git switch master`

## 分支冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

==合并操作（ merge ）只对对当前所在分支产生影响；无论是否存在冲突，合并之后，feature分支都不会发生变化。==

## 分支管理策略
合并分支的时候git会使用**fast forward**模式，合并完成之后会删除分支信息。
使用`--no-ff`模式 `git merge --no-ff -m "merge with no-ff" dev` 合并并创建一个新的commit。
>合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。