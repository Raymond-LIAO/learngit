
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

% 1/Jan/2016更新
阮一峰的网络日志 » 首页 » 档案

搜索 
上一篇：测试框架 Mocha 
下一篇：有没有安全的工作？  
分类： 开发者手册
常用 Git 命令清单
作者： 阮一峰
日期： 2015年12月 9日
我每天使用 Git ，但是很多命令记不住。
一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
一、新建代码库

# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
二、配置
Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
三、增加/删除文件

# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
四、代码提交

# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
五、分支

# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
六、标签

# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
七、查看信息

# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
八、远程同步

# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
九、撤销

# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到工作区
$ git checkout [commit] [file]

# 恢复上一个commit的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
十、其他

# 生成一个可供发布的压缩包
$ git archive
（完）
文档信息
版权声明：自由转载-非商用-非衍生-保持署名（创意共享3.0许可证）
发表日期： 2015年12月 9日
更多内容： 档案 » 开发者手册
购买文集： 《如何变得有思想》
社交媒体： twitter， weibo
Feed订阅： 
珠峰培训
简寻
相关文章
2015.12.24: Git 协作流程
Git 作为一个源码管理系统，不可避免涉及到多人协作。
2015.09.30: Github 的清点对象算法
使用 Github 的时候，你有没有见过下面的提示？
2015.09.23: 持续集成是什么？
互联网软件的开发和发布，已经形成了一套标准流程，最重要的组成部分就是持续集成（Continuous integration，简称CI）。
2015.09.17: 网页性能管理详解
你遇到过性能很差的网页吗？
广告（购买广告位）

TextArea 
千里码
留言（31条）
strider 说：
常用的也是你提到的这几个，其他的每次都要现查。
2015年12月 9日 10:08 | ∞ | 引用
JustYY.com 小赖子的英国生活和资讯。 说：
多谢 很实用
2015年12月 9日 10:25 | ∞ | 引用
yanhaijing 说：
http://yanhaijing.com/git/2014/11/01/my-git-note 类似文章，但没有阮老师写的清晰
2015年12月 9日 10:28 | ∞ | 引用
Tony 说：
想不通那些说 git 超级简单的人, 是一种什么心理!
2015年12月 9日 10:32 | ∞ | 引用
hmy 说：
git 的实现原理很简单，相对其他版本管理系统
2015年12月 9日 10:33 | ∞ | 引用
笔墨 说：
git --orphan gh-pages 也很重要，用于创建一个全新的分支，不包含原分支的提交历史，Gihthub项目主页分支用这个。
2015年12月 9日 11:20 | ∞ | 引用
SpicyCat 说：
没有git bisec就算了，连git rebase也没有？git rebase算是比较常用的了。
看log建议用gitk，不用记那么多参数。
git alias 也比较有用。
但是常用的就那么几个，不常用的还是要现查手册。
感觉阮老师这一篇略水。
2015年12月 9日 12:09 | ∞ | 引用
笔墨 说：
引用笔墨的发言：
git --orphan gh-pages 也很重要，用于创建一个全新的分支，不包含原分支的提交历史，Gihthub项目主页分支用这个。
错了，是 git checkout --orphan gh-pages
2015年12月 9日 12:24 | ∞ | 引用
Eich 说：
显示两次差异的中间不是两个点的吗？
2015年12月 9日 12:51 | ∞ | 引用
阮一峰 说：
@Eich
三个点比两个点包括更多的commit。
http://stackoverflow.com/questions/462974/what-are-the-differences-between-double-dot-and-triple-dot-in-git-com#answer-24186641
2015年12月 9日 13:37 | ∞ | 引用
Emily 说：
噗，不太会用命令行，一直用 sourceTree
2015年12月 9日 14:10 | ∞ | 引用
石樱灯笼 说：
求问开头那个图是用什么软件画的？
2015年12月 9日 17:02 | ∞ | 引用
az 说：
git pull
git add .
git commit -m 'well'
git push origin 
2015年12月 9日 21:08 | ∞ | 引用
Mark 说：
阮老師
我可以轉譯成繁中轉貼到我的blog嗎？
2015年12月10日 13:43 | ∞ | 引用
Ivan 说：
最近阮兄一直在钻研技术，什么时候再写写人文社科类的文章
2015年12月10日 13:50 | ∞ | 引用
阮一峰 说：
@Mark：非常欢迎啊。
@Ivan：只要能抽出时间，就会写。
2015年12月10日 15:39 | ∞ | 引用
zhangbo9716 说：
阮老师最近写的都是技术的文章啊。很少思想类的文章了。
2015年12月10日 19:04 | ∞ | 引用
ky 说：
不知道阮老师可否出个typescript相关的教程，网上找不到好的啊。。
2015年12月11日 03:23 | ∞ | 引用
phping 说：
引用Emily的发言：
噗，不太会用命令行，一直用 sourceTree
我也是，先前是使用的命令行操作，现在在用sourcetree.
2015年12月11日 14:29 | ∞ | 引用
doingnothing 说：
引用Tony的发言：
想不通那些说 git 超级简单的人, 是一种什么心理!
引用Tony的发言：
想不通那些说 git 超级简单的人, 是一种什么心理!
感觉就是一种推销的说辞罢了。这种说辞，让一些人觉得随便学下就好了，导致实际上经常踩到坑。
2015年12月13日 00:11 | ∞ | 引用
coder 说：
关注很长时间了，留个名
2015年12月14日 15:47 | ∞ | 引用
gostar 说：
基本都用到过。 讲的蛮好的`
2015年12月14日 18:04 | ∞ | 引用
x 说：
我们学校的老师呀，虽然没教过我。（本人计算机专业，当时 闻其名未见其人），话说现在还在计算机行业，也在写着 前端JavaScript 。搜索 tail call optimization 的时搜索到的。呵呵。自己水平还是比较菜
2015年12月15日 09:47 | ∞ | 引用
MMY 说：
阮老师，怎么在github里面加入代码演示呢？
2015年12月16日 15:14 | ∞ | 引用
ruanjf 说：
引用SpicyCat的发言：
没有git bisec就算了，连git rebase也没有？git rebase算是比较常用的了。
看log建议用gitk，不用记那么多参数。
git alias 也比较有用。
但是常用的就那么几个，不常用的还是要现查手册。
感觉阮老师这一篇略水。
fetch + rebase用得最多了
2015年12月17日 10:15 | ∞ | 引用
yager 说：
阮老师，刚才开发中用到tag的两个命令：
删除本地tag:
git tag -d tagName
删除远程tag:
git push origin :refs/tags/tagName
希望能做个补充。
2015年12月22日 19:50 | ∞ | 引用
阮一峰 说：
@yager：
谢谢补充，已经添加。
欢迎大家继续补充。
2015年12月23日 09:01 | ∞ | 引用
AnnabellChan 说：
其实只要记住对应的英文就会方便很多
比如 git checkout -b，b其实是browse的abbr.
git branch -r, r -> remote
git branch -a, a -> all
git config -l, l -> list
git commit -m, m -> message
2015年12月23日 17:05 | ∞ | 引用
Yan Xiaoxue 说：
您好，您博客的这个评论是怎么做的？
我刚开始用jekyll。
2015年12月24日 13:28 | ∞ | 引用
汪卫卫 说：
阮老师，我可以转载在我的博客里吗？方便查用。
2015年12月24日 19:47 | ∞ | 引用
大大大，大志 说：
引用Emily的发言：
噗，不太会用命令行，一直用 sourceTree
还是IDE集成的比较好，比如用webstrom中的git~ 然后结合前者中的terminal[终端面板]一起使用。
2015年12月30日 14:57 | ∞ | 引用
我要发表看法
您的留言 （HTML标签部分可用）


