## Git应用总结
1.git init
	使用Git进行版本控制的第一步就是通过在你的项目根目录执行Git init命令，
	该命令会在你的根目录下创建了一个隐藏的.Git目录，该目录中包含了该项目在Git中的相关配置和信息，
	Git就是根据里面的数据对你的项目进行版本控制。

2.git status </br>
	它用于检查本地项目的状态</br>
	2.1 git有三种状态：Change, Staged, Committed。</br>
		2.1.1 Change(Unstaged)：你改动了一个，没有调用任何git命令前，就是这种状态。</br>
		2.1.2 Staged：调用git add或者git commit -a之后，进入Staged状态，表示申明要变动了。</br>
		2.1.3 Committed：Commit，生成新的版本commit号，进入此状态</br>

3.git checkout -- git_one.txt :意思就是把readme.txt文件在 工作区 做的修改全部撤销，这里有2种情况，如下：</br>
	3.1 git_one.txt还没有放到暂存区，使用 撤销修改就回到和版本库一模一样的状态</br>
	3.2 另外一种是git_one.txt 已经放入暂存区了 ，接着又作了修改，撤销修改就回到添加暂存区后的状态(*)</br>

4.git diff git_one.txt : 查看文件修改的东西(git add 命令之后,git commit命令之前)</br>

5.git log:查看提交(commit日志)

6.git reset --hard 版本号: 回复指定版本号

7.git reflog: 查看版本号

8.git remote add origin https://github.com/cwlDream/git-stu.git: Github上的项目和本地的项目关联(Github,本地有相同的项目)

9.git push -u origin master:本地项目提交到Github上第一次 以后再提交(git push origin master)就可以

10.git clone https://github.com/cwlDream/demo2.git: 克隆项目到本地

11.git checkout -b dev:创建并且切换分支

12.git branch: 查看当前的分支

13.git merge: 命令用于合并指定 分支 到 当前 分支上

分支管理策略:这个没有理解明白

14.git stash
bug分支:

15.git branch –d name: 删除分支

16.git stash list:
	工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，可以使用如下2个方法：</br>
	16.1 git stash apply恢复，恢复后，stash内容并不删除，你需要使用命令git stash drop来删除。</br>
	16.2 使用git stash pop,恢复的同时把stash内容也删除了。</br>

17.git remote –v:查看远程分支

18. git branch --set-upstream dev origin/dev:指定本地dev分支与远程origin/dev分支的链接

19.git branch -vv:查看远程分支和本地分支的关系(两个v)

20.git push -f:强制push

打标签:
21. git tag name:打标签

22.git tag -b name:删除标签

23.git push origin <tagname>: 推送标签(因为创建的标签都只存储在本地，不会自动推送到远程如果要推送某个标签到远程)

24.删除远程tag:</br>
	24.1 删除本地标签 git tag -b tagname</br>
	24.2 从远程删除,删除命令也是push:git push origin :refs/tags/<tagname> </br>



