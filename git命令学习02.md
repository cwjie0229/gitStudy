# 远程仓库

一般使用Github作为远程仓库来使用，这里就不再介绍GitHub的使用了

直接开始使用git命令

## 创建与合并分支

创建分支

	git checkout -b dev

![](http://oqsrtiv7s.bkt.clouddn.com/1-15.png)

git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
	//创建
	$ git branch dev
	//切换
	$ git checkout dev

查看当前分支
	
	git  branch

![](http://oqsrtiv7s.bkt.clouddn.com/1-16.png)

git branch命令会列出所有分支，当前分支前面会标一个*号。

切换分支

	git checkout master

合并分支
	
	git merge dev
	
删除分支

	git branch -d dev
	

实例：

1. 现在主分支指文件如下：


![](http://oqsrtiv7s.bkt.clouddn.com/1-17.png)

2. 创建并切换分支 dev

	git checkout -b dev

![](http://oqsrtiv7s.bkt.clouddn.com/1-18.png)

3. 查看当前分支

![](http://oqsrtiv7s.bkt.clouddn.com/1-19.png)

4. 在dev分支下添加文件dev01.test git add  git commit
 

		 git add dev01.txt 
		 git commit -m "branch test"

5. 切换到主分支

		git checkout master


![](http://oqsrtiv7s.bkt.clouddn.com/1-20.png)

当前主分支下并没有 dev01.txt 文件


6. 合并分支dev

		git merge dev


命令行

![](http://oqsrtiv7s.bkt.clouddn.com/1-21.png)

主分支下有文件 dev01.txt 说明合并成功


![](http://oqsrtiv7s.bkt.clouddn.com/1-22.png)


7. 删除分支 	

	 	git branch -d dev

![](http://oqsrtiv7s.bkt.clouddn.com/1-23.png)

7. 查看分支当前有无分支

		git branch 	

![](http://oqsrtiv7s.bkt.clouddn.com/1-24.png)

已经没有dev分支了。

## 解决冲突

如果修改的文件在主分支和分分支上都有不同的修改，那么提交时就会出现冲突，这是需要手动解决冲突，才可以继续提交。

## 分支策略
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

## Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：

而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug

幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

	git stash

首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

	 git checkout master

	 git checkout -b issue-101

修改提交

	$ git add readme.txt 
	$ git commit -m "fix bug 101"


修复完成后，切换到master分支，并完成合并，最后删除issue-101分支：
	//切换主分支
	git checkout master
	//合并
	git merge --no-ff -m "merged bug fix 101" issue-101

	//删除

	git branch -d issue-101

现在，是时候接着回到dev分支干活了！

	 git checkout dev

	 git status

但是需要恢复一下，有两个办法

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；


二是用git stash pop，恢复的同时把stash内容也删了：

再用git stash list查看，就看不到任何stash内容了：

	git stash list

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

	git stash apply stash@{0}

## feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

## 多人协作

查看远程库的信息

	git remote

或者，用git remote -v显示更详细的信息：

	git remote -v

### 推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

	git push origin master

如果要推送其他分支，比如dev，就改成：

	git push origin dev

### 抓取分支

	git pull


1. 首先，可以试图用git push origin branch-name推送自己的修改；

1. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

1. 如果合并有冲突，则解决冲突，并在本地提交；

1. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

### 多人协作小结

> 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
> 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

## 标签管理

创建标签

	git tag v1.0

查看所有标签

	git tag
默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：
比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：

	 git tag v0.9 6224937


查看标签信息

	git show <tagname>

还可以通过-s用私钥签名一个标签：

	git tag -s v0.2 -m "signed version 0.2 released" fec145a

删除标签

	git tag -d v0.1

如果要推送某个标签到远程，使用命令git push origin <tagname>：

	git push origin v1.0

或者，一次性推送全部尚未推送到远程的本地标签：

	git push origin --tags

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	git tag -d v0.9

然后，从远程删除。删除命令也是push，但是格式如下：

	git push origin :refs/tags/v0.9

## 忽略特殊文件

在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被.gitignore忽略了：

如果你确实想添加该文件，可以用-f强制添加到Git：

	
	git add -f App.class

或者你发现，可能是.gitignore写得有问题，需要找出来到底哪个规则写错了，可以用git check-ignore命令检查：

	git check-ignore -v 文件名称

## 配置别名
我们只需要敲一行命令，告诉Git，以后st就表示status：

	git config --global alias.st status

### 配置文件
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

每个仓库的Git配置文件都放在.git/config文件中：

别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。

