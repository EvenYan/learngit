# Git基本操作
### 创建git仓库：
>     初始化一个Git仓库，使用git init命令。
>     添加文件到Git仓库，分两步：
>     第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
>     第二步，使用命令git commit，完成。

### 查看工作状态：
>     要随时掌握工作区的状态，使用git status命令。
>     如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

###版本回退：	     
	HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命git reset --hard commit_id。返回上一个版本用： git reset 		 --hard HEAD^
	穿梭前，用gitlog可以查看提交历史，以便确定要回退到哪个版本。如果先输出信息太多，可以用： git log --pretty=oneline
	
	要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

查看git工作区：
	 git status


撤销修改：
>     场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

>     场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

>     场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

	
	
#####**删除文件：**
>     确定删除：先git rm test.txt，再git commit -m "remove test.txt"
>     误删：git checkout -- test.txt

>     注：命令gitrm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。


远程仓库：
	git clone git@github.com:EvenYan/testgit.git
	git@github.com:EvenYan/testgit.git
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin git@github.com:EvenYan/testgit.git
	git push -u origin master


创建与合并分支：
	查看分支：git branch
	创建分支：git branch <name>
	切换分支：git checkout <name>
	创建+切换分支：git checkout -b <name>
	合并某分支到当前分支：git merge <name>
	删除分支：git branch -d <name>

	
合并冲突：
	当master和dev分支同时同时提交了修改后，git无法快速合并，在master分支上运行git merge dev快速合并时会提示冲突。修复冲突后再在master分支提交就可以修复冲突，然后删除dev分支：git checkout -d dev即可。再运行git log --graph --pretty=oneline --abbrev-commit就可以看到分支合并图。
	

分支管理策略：

	通常，合并分支时，如果可能，Git会用Fastforward模式，但这种模式下，删除分支后，会丢掉分支信息。
	如果要强制禁用Fastforward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。	
	
	命令为：git merge --no-ff -m "merge with no-ff" dev（-m后加合并信息）


	
BUG分支：
>     当在dev分支的工作没有完成时，先把工作现场gitstash一下，然后在有bug的分支，假设是master上创建一个临时分
>     支issue-001去修复bug。修复后，切换到有bug的分支，再用命令：git merge --no-ff -m "merged bug fix 101"
>     issue-001合并bug分支。最后回到dev分支，用git status命令发现工作区是干净的，用git stash list查看工作现
>     场，用git stash apply恢复工作现场，恢复后，可以用git stash drop来删除stash内容，也可以用git stash pop
>     命令，在回复的同时把stash的内容也删了。再用git stash list查看，就看不到任何stash内容了。你可以多次stash，
>     恢复的时候，先用gitstash list查看，然后恢复指定的stash，用命令：git stash apply stash@{0}

#####**小结**
>     修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
>     当手头工作没有完成时，先把工作现场gitstash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。


Feature分支
	
	