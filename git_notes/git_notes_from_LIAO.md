#git 学习笔记

##开始：创建库和文件，提交文件
首先需要创建一个Git的管理库：使用 `git init`, 之后你需要git的文件就需要放在这个目录下面（当然子目录也是可以的）。

创建文件，添加文件修改到暂存区： 使用 `git add filename`

然后就需要提交： 使用 `git commit -m "comments"`提交更改，实际上就是把**暂存区的所有内容提交到当前分支**，另外**评论和记录**是非常重要的！

需要注意的事，文件必须一个一个add，但是可以一次commit多个文件。

##查看工作区、版本库和版本控制（退回和再找回）

使用命令 `git status` 查看工作区的状态， 还有 `git diff` 查看具体区别。

commit 的本质是一个快照， 可以通过 `git log` 来查看commit了多少次，分别是什么。另外后面加上 `--pretty=oneline` 可以查看版本号。

其中HEAD表示了当前的版本。使用 `git reset --hard commit-id(ex: HEAD^)` 进行版本的退回。

版本退回之后，后悔了怎么办？ 使用 `git reflog` 还是可以找回来之前的版本id的！

##撤销工作区
需要修改怎么办？ 首先`git reset file`可以把暂存区的修改撤销掉（unstage），重新放回工作区

之后命令`git checkout -- file` 意思就是，把文件在工作区的修改全部撤销，这里有两种情况：

1. 一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

2. 一种是文件˙已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。


##删除文件
如何从版本库中删除该文件：使用命令`git rm`删掉，并且记得提交：`git commit`。

其实`git checkout`是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

##远程仓库
也就是正确地使用Github：
- 和远程仓库先关联：`git remote add origin git@github.com:....git`
- 把本地库的内容推送到远程：第一次：`git push -u origin master`
- 之后只需要： `git push origin master`
- 还有其他的选项，比如：git push -f origin master` force强制推送

##分支管理
###创建与合并分支
最开始的时候，只有master分支一条线，Git用master指向最新的提交，再用HEAD指向master
伴随着我们不断的提交，master这个分支变得越来越长，试试手创建一个新的分支啦！

