<h1>git 命令</h1>



git diff filename:比较工作区和暂存区

git diff HEAD -- filename:比较工作区和版本库的最新版本

如果git diff输出空白就说明工作区是干净的（干净应该就是指与比较的区相同）

 

 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

 

 查看分支：git branch

 

创建分支：git branch <name>

 

切换分支：git checkout <name>或者git switch <name>

 

创建+切换分支：git checkout -b <name>或者git switch -c <name>

 

合并某分支到当前分支：git merge <name>

 

删除分支：git branch -d <name>

 

·    查看远程库信息，使用`git remote -v`；

·    本地新建的分支如果不推送到远程，对其他人就是不可见的；

·    从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

·    在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

·    建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

·    从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

 ·    命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

·    命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

·    命令`git tag`可以查看所有标签。

·    命令`git push origin <tagname>`可以推送一个本地标签；

·    命令`git push origin --tags`可以推送全部未推送过的本地标签；

·    命令`git tag -d <tagname>`可以删除一个本地标签；

·    命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

​              git push --set-upstream origin <branchname> 在远程建立一个新的分支并推送上去。
