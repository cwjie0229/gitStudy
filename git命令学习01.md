# git命令学习

跟着廖雪峰老师的网站总结的几个常用的命令，由于使用的Window操作系统所以就直说该操作系统下操作了。

## 安装
首先是安装，从网下下载一个，然后选择默认命令安装即可，这里没有什么可说的。安装完成后一般点击鼠标右键找到"Git Bash Here"弹出一个类似命令行窗口的东西说明Git安装成功。

![](http://oqsrtiv7s.bkt.clouddn.com/1-1.png)

安装完成之后在命令行中输入设置

	git config --global user.name "Your Name"
	git config --global user.email "email@example.com"

## 创建版本库

创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

	$ mkdir gitproject01
	$ cd gitproject01
	$ pwd
	/d/study/gitproject01

注释：我在D盘在有一个文件夹Study中点击鼠标右键“Git Bash Here”

![](http://oqsrtiv7s.bkt.clouddn.com/1-2.png)

## 初始化：

	git init

![](http://oqsrtiv7s.bkt.clouddn.com/1-3.png)

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository）

注意：

首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

## 添加文件
	
	//第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
	1. git add
	// 第二步，使用命令git commit，完成。
	2. git commit -m "提示消息"
	//  要随时掌握工作区的状态，使用git status命令
 	git status
	//  如果git status告诉你有文件被修改过，用git diff可以查看修改内容

![](http://oqsrtiv7s.bkt.clouddn.com/1-4.png)


	git diff

## 版本回退
查看历史记录

	git log

![](http://oqsrtiv7s.bkt.clouddn.com/1-5.png)

git log命令显示从最近到最远的提交日志，我们可以看到2次提交，最近一次提交的是" second commit
"上一次是"first commit" ,如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

![](http://oqsrtiv7s.bkt.clouddn.com/1-6.png)

版本回退

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

	git reset --hard HEAD^

已经回退成功：

![](http://oqsrtiv7s.bkt.clouddn.com/1-7.png)


最新的那个版本append GPL已经看不到了！

想再回去已经回不去了，其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是8b5ba3670f75dc20ecebf561841e576770168c1c ...，于是就可以指定回到未来的某个版本：
	

	git reset --hard 8b5ba3670

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

![](http://oqsrtiv7s.bkt.clouddn.com/1-8.png)

已经回到最新的

![](http://oqsrtiv7s.bkt.clouddn.com/1-9.png)

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？

在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：

	git reflog


![](http://oqsrtiv7s.bkt.clouddn.com/1-10.png)

## 撤销修改

	git checkout -- README.txt

命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

1. 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

2. 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

没有提交之前的，可以撤销

### 小结

1. 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

2. 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

3. 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

## 删除文件
先添加一个新文件test.txt到Git并且提交：

	 git add test.txt
	 git commit -m "add test.txt"

![](http://oqsrtiv7s.bkt.clouddn.com/1-11.png)

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

	rm test.txt

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：

![](http://oqsrtiv7s.bkt.clouddn.com/1-12.png)

一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
	
	git rm test.txt
	git commit -m "remove test.txt"

![](http://oqsrtiv7s.bkt.clouddn.com/1-13.png)

文件已经删除


![](http://oqsrtiv7s.bkt.clouddn.com/1-14.png)
