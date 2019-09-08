## git详解
### 1，git结构
- 1.1 工作区
工作区就是写代码的地方，如果有新增修改删除文件都是在工作区发生变化。
工作区的文件可以通过git add 命令将文件提交到暂存区。

- 1.2 暂存区
暂存区就是用来临时存储代码的一片区域，是已经修改了但是还没有提交，将来可以提交到本地库也可以撤回来。
通过git commit 命令可以将暂存区的文件提交到本地仓库。

- 1.3 本地库
本地库就是存储的实实在在的历史版本
可以通过git push命令来将本地库的文件推送到服务器。

### 2，git命令
- 2.1 本地仓库初始化
命令：git init
作用：可以进行本地仓库的初始化，初始化过后会在本地生成一个.git的隐藏文件夹里面的内容如下。
![](http://uninote.com.cn/docs/1079089832/__pic/tQD1ci9Z.png)

- 2.2 设置签名
形式：
		用户名：lc
		email：test@163.com
作用：
		区分不同开发人员的身份
命令：
		项目级别/仓库级别，仅在当前本地仓库范围内有效。
		git config user.name lc
		git config user.email test@163.com
		
		系统级别：登陆当前操作系统的范围有效。
		git config --global user.name lc
		git config --global user.email test@qq.com

- 2.3 状态查看操作
命令：
git status
作用：
查看工作区和暂存区的内容

- 2.4 添加到暂存区操作
命令：
git add [file name]
作用：
将工作区的“新建/修改”提交到暂存区

- 2.5 提交操作
命令：
 git commit -m "msg" 
作用：
将暂存取的文件提交到本地仓库
加上额外参数的作用
-a
git commit -a -m "msg" ：加上-a参数可以直接将工作区的内容提交到本地仓库，不用走暂存区，减少添加操作。
- 2.5 查看本地仓库版本记录
基本参数
命令：git log
作用：显示最完整的日志信息
基本参数
命令：git reflog
作用：显示比较简洁的git记录，并且显示HEAD次数
加参数 --oneline
命令: git log --oneline
作用：将所有的日志信息显示在一行
加参数 --pretty=oneline
命令: git log --oneline
作用：将所有的日志信息显示在一行

- 2.6 版本前进后退
基于索引值操作：能前进能后退
git reset --hrad [索引值]
git reset --hard 63b6a76
使用^符号操作：只能后退
git reset --hard HEAD^
这里有几个^号就是后退几步
使用～符号操作：只能后退
git rest --hard HEAD~3
这里波浪线后面的数值就是后退的步数

- 2.7 reset的三个参数对比
	一，--soft参数
	仅在本地仓库移动HEAD指针

	二，--mixed参数
	在本地仓库移动HEAD指针
	重置缓存区

	三，--hard参数
	在本地仓库移动HEAD指针
	重置缓存区
	重置工作区

- 2.8 删除文件并找回
git reset --hard [指针位置]
指针位置：历史版本记录或者当前

- 2.9 比较文件差异
git diff [文件名]
	将工作区的文件和暂存区的文件进行比较
git diff [版本记录] [文件名]
	将指定文件和版本记录里面的文件进行比较
- 3.0 创建远程库
在github上面可以创建一个远程库

- 2.3.1 创建本地远程库别名
	git remote add [远程库别名] [远程库地址]
	可以给远程库在本地取一个别名，但是推送代码到远程库的时候远程库的别名会采用origin

- 2.3.2 查看远程库
	使用
	git remote -v
	可以查看远程库

- 2.3.3 推送文件到远程库
	git push [远程库别名] [远程库分支]
	可以将文件提交到远程库

- 2.3.4 git 克隆远程库
	git clone [远程库地址]
	可以将文件从远程库拉取下来，
	拉下来有3个效果：
	1，自动初始化本地库
	2，获取完整代码
	3，获取远程库别名信息

- 2.3.5 git 远程仓库拉取文件
	git pull = fetch + merge
	拉取：git fetch [远程库] [远程分支名] 
	合并：git merge [远程库/远程分支名]

- 2.3.6 git 远程分支关联
	使用git在本地新建一个分支后，需要做远程分支关联。如果没有关联，git会在下面的操作中提示你显示的添加关联。

	关联目的是在执行git pull, git push操作时就不需要指定对应的远程分支，你只要没有显示指定，git pull的时候，就会提示你。

	解决方法就是按照提示添加：

	git branch --set-upstream-to=origin/remote_branch  your_branch

	其中，origin/remote_branch是你本地分支对应的远程分支；your_branch是你当前的本地分支。

- 2.4.0 创建分支
git branch [分支名]

- 2.4.1 查看分支
git barnch -v  
git branch -a 查看所有分支

- 2.4.2 切换分支
git checkout [分支名]

- 2.4.3 合并分支
	第一步：需要切换到想要合并代码的分支上面
	第二步：使用git merge [分支名]来合并


	例如：当前所在分支是dev，现在dev上面做了修改，master分支想要合并dev上面的代码，就要切换到master分支上面进行merge操作 git merge dev
### 3，git工作流
- git 集中式开发
集中式开发就是所有人都在同一个分支上面开发，然后将代码提交到同一个远程分支上面

- git flow 工作模式
开发中使用的最多的也是这种
有一个主干分支，有一个线上分支，还有一些不同功能的分支，比如专门改bug的分支
主干分支主要用来合并代码。
线上分支合并从主干分支的代码。

- git Forking 工作模式
在git flow的基础上，可以接受不受信任和保护的代码。相当于就是跨团队开发了。