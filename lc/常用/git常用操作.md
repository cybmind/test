- git基础
	- `git init` 将当前目录初始化为git本地仓库，会生成一个*** .git*** 的文件夹。
	- `git clone [url]` 从现有仓库克隆代码,会将服务器上面的代码拉到本地。
	- `git status` 会列出已经做出了变化的所有文件。
	- `git add 文件名` 会暂存已经被做出变化的文件。
	- `git reset HEAD 文件名` 会撤销你不想暂存的文件。
	- `git commit -m '备注'` 提交一次更新
	- `git commit -a -m '备注'` 也是提交一次更新，但是不用先跟踪文件变化。
	- `git commit --amend` 修改git commit的提交信息。
	- `git diff` 查看尚未使用***git add***暂存的文件修改了那些信息。
	- `git diff --cached` 差可能已经使用***git add*** 暂存的文件和上次提交之间的差异。
	- `git rm 文件名` 移除文件。
	- `git rm -f 文件名` 强制移除文件。
	- `git mv file_from file_to` 移动文件。
	- `git mv file_name1 file_name2` 修改文件名。
	- `git log` 查看提交历史。
	- `git log -p -1` 查看最近一次的修改。
	- `git log --pretty=oneline` 将每次提交放在一行显示，非常有利于查看日志。
	- `git checkout 文件名` 取消对该文件的修改，这个操作会将该文件恢复到你没有修改的时候。
	- 远程仓库的使用
	- `git remote` 列出已有的远程仓库。
		`ssh-add   ~/.ssh/id_rsa` 在更新了私钥克隆代码不生效的时候可以使用该命令重新指定私钥文件
	- `git remote set-url origin 地址` 切换git远程源
	- `git checkout -b dev origin/dev`，作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
## git忽略已经修改过的文件
```
$ git update-index --assume-unchanged /path/to/file       #忽略跟踪
$ git update-index --no-assume-unchanged /path/to/file  #恢复跟踪
```
		但是忽略的文件多了，想找出所有被忽略的文件，可以使用下面的办法，
		`git ls-files -v | grep '^h\ '`
		提取文件路径，方法如下
		`git ls-files -v | grep '^h\ ' | awk '{print $2}'`
		所有被忽略的文件，取消忽略的方法，如下
		`git ls-files -v | grep '^h' | awk '{print $2}' |xargs git update-index --no-assume-unchanged `
### 未完待续