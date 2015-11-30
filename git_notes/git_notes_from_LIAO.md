
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

`git checkout -b dev` 我们创建*dev*分支，然后切换到*dev*分支: -b表示创建并切换，相当于：
- `git branch dev`
- `git checkout dev`

然后，使用`git branch`命令查看当前分支,**当前分支**前面会标一个*号

当dev分支的工作完成，我们就可以切换回master分支，发现刚刚完成的工作不见了！因为刚刚是对dev进行的操作，master并没有变化。

`git merge dev`：合并指定分支到当前分支，具体的说，是将dev这个分支，合并到了我们的当前分支master。

`git branch -d dev`：删除dev分支。

最后加一句，**Git鼓励大量使用分支**。

###解决冲突
当出现冲突时，具体的说：

	Auto-merging readme.txt
	CONFLICT (content): Merge conflict in readme.txt
	Automatic merge failed; fix conflicts and then commit the result.

也就是readme.txt文件存在冲突，必须手动解决冲突后再提交。
`git status`也可以告诉我们冲突的文件: `git log --graph --pretty=oneline --abbrev-commit`

用`git log --graph`命令可以看到分支合并图。

###分支管理策略
通常，合并分支时，如果可能，Git会用**Fast forward**模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用**Fast forward**模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。具体地说：

	git merge --no-ff -m "merge with no-ff" dev
	Merge made by the 'recursive' strategy.

###Bug分支
处理bug时，正确的使用分支结构，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
Git提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：`git stash`,完成之后，就可以使用`git status`看到工作区是干净的，可以专心修复bug。

正确的bug修复流程：

1. （假设要从master这个分支修复），首先创建临时分支`issue-101`

		git checkout master
		git checkout -b issue-101

2. 修改文件`readme.txt`

		git add readme.txt 
		git commit -m "fix bug 101"

3. 修复完成后，切换到master分支，并完成合并，最后删除`issue-101`分支

		git checkout master
		git merge --no-ff -m "merged bug fix 101" issue-101
		git branch -d issue-101

完成了bug修复之后，需要找回之前存储的工作区，使用`git stash list`查看之前存储的工作区，然后恢复，有以下两个方法：

1. 使用`git stash apply`恢复，然后使用`git stash drop`来删除
2. 使用`git stash pop`，恢复的同时把stash内容也删了(似乎这种更加方便？)


###新功能分支
每添加一个新功能，最好新建一个feature分支
当需要删除（销毁）一个分支的时候：

	$ git branch -d feature-vulcan
	error: The branch 'feature-vulcan' is not fully merged.
	If you are sure you want to delete it, run 'git branch -D feature-vulcan'.

所以需要按照提示强行删除：使用 `git branch -D feature-vulcan`

###多人协作
要查看远程库的信息，用`git remote`
或者用`git remote -v`显示更详细的信息


