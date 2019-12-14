# git

git status 查看工作区的状态，可以告诉工作区有没有被改过，

git diff 可以查看被修改的内容

使用git log命令查看 git 版本变更的信息，即每次commit后的信息

使用命令git reset --hard commit_id 命令实现版本回退。

git reflog  当版本回退了，又后悔了，想回去，在回退得有版本号，可以用git reflog命令查看每次的命令

工作区----add---->暂存区-------commit---->版本库

（能看到的是工作区，有个看不见的暂存区，和看不见的版本库）

git checkout -- file可以丢弃工作区的修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

git reset HEAD <file> 可以把暂存区的修改撤销掉

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name>

创建+切换分支：git checkout -b <name>或者git switch -c <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用git log --graph命令可以看到分支合并图。
