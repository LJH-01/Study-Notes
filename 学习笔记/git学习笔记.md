# git 学习笔记
> git的用处是什么?   
> 1. 自动记录文件的改动
> 2. 协同编辑   

## 1.开始学习
### 1.1历史   
2005年Linus使用两周时间用c语言创建了git原型，在2008年github上线后，git成为最流行的版本控制系统。  
### 1.2优点
> git是一个分布式版本控制系统，先说cvs和svn都是集中式版本控制系统，版本库存放在中央服务器上
>，每个人工作的时候从服务器上下载最新的版本，干完活再将版本推送回去

> git 是分布式的，每个人的电脑上都是一个完整的版本库，当协同工作时只需要
> 把改动的地方推送给别人就好了，安全性高，坏一个电脑木问题，集中式的中央服务器
> 坏了就完了
>
> ### 1.3安装  
1. 输入"git"可以查看git的安装情况
2. 安装完成之后两个命令进行初始化设置
   ``` shell
   git config --global user.name "Your Name"
   git config --global user.email "email@example.com"
   ```
3. 在合适的目录下（即你的git仓库）使用`git init` 命令来初始化一个git仓库。   
4. 使用---在git仓库的目录下或者其子目录下，创建文件，使用`git add filename.txt`命令将文件
提交到了暂存区，使用`git commit -m "commit message" `来将文件提交到版本库。
工作区---add--->暂存区---commit--->版本库

>* 因为git是分布式版本控制系统，因此每一个机器都必须自报家门，
>报上名字和Email地址，   
>* --global 参数表示对于这台机器上的每一个仓库都使用这样的配置，
>也可以对不同的仓库使用不同的设置。   
>* git 只能管理文本文件，其他版本控制系统也一样，但是windows下许多文件是二进制文件，如.docx，所以不能用来管理。
>
## 2.使用git   
### 2.1 掌握版本详情
1. 使用`git status` 查看当前仓库的状态，包括有无改动，有无提交等
2. 使用`git diff filename.txt` 可以看到文件被怎样的修改了。  
### 2.2 版本回退
> 原理：   
>* 每一个commit都可以看作是一个保存的快照，回退的点只能是某个commit。   
>* 每次commit会使用SHA1 计算出来一个很长的commit id，因为他是分布式管理系统，还要考率和其他人协作所以这里的commit id设计它不能是1，2，3…… 这样简单的数字，没有区分了   
>* 在git中“当前版本”也就是最近一次commit的版本用HEAD指示，再往前一个就是HEAD^,再上一个版本是HEAD^^ 上一百个版本是HEAD~100
>* git每次回退版本仅仅是修改了HEAD指针，因此git的版本回退非常快。    

1. `git log` 当我们需要版本回退时，使用该命令查看提交的历史。
2. `git reset --hard commit ID ` 或者`git reset --hard Head^` 使版本回退到某个特定版本或者上一个版本
3. `git reflog` 用来记录每一次命令,并显示commit id，这样可以方便找到需要的版本号

### 2.3工作区、暂存区、分支、版本库原理
1. 工作区：能看得到的目录；
2. 版本库：隐藏目录.git 是版本库
3. 暂存区、版本库：git版本库(.git文件夹)中包含了很多东西，最主要三个概念是 暂存区（stage 或 index）、分支（branch）HEAD指针。
>git自动帮我们创建的第一个分支是 master，两步提交的过程实际是:     
>`git add`之后将工作区的文件提交到了暂存区，未add的文件显示的是untracked   
>`git commit`之后，将暂存区的内容提交到了 master branch，然后将暂存区 clean，   
>最终我们的文件是位于一个branch,在这里是master。     
### 2.4管理的是修改
>git比其他版本控制系统的优秀之处就在于git跟踪的是文件的修改而不是文见本身，修改就是值文件的变动，如：第几行删除了某个字符等    
>每次 `git add`相当于是记录了一次修改，这样带来一个问题，就是每次我们修改完如果不add git就不会记录，某些场景下，例如：    
> 第一次修改 -> git add -> 第二次修改 -> git commit 则第二次修改的内容不会被提交    
>第一次修改 -> git add -> 第二次修改 -> git add -> git commit  这样相当于在提交前将两次add 合并然后提交了。会被记录。   
### 2.5撤销修改   
1. `git checkout --filename.txt` 丢弃**工作区**的修改。这里又有两个种情况：   
    1. 在工作区做了修改但是还没有add到暂存区，这时 checkout一下就没事了
    2. 在工作区修改了，然后add了一次，又在工作区修改了，这次checkout 只是将在工作区最近的修改丢弃，回到刚才add的状态。
2. `git reset HEAD filename.txt` 如果把胡话已经add到暂存区了，使用这个命令将 (unstage) 将暂存区的内容失效。注意！这时候只是将暂存区撤销了，工作取得内容还要手动去改，或者用上一步的撤销工作区修改的命令。   
3. 如果已经commit了，在本地的话还是可以回退版本解决，但是如果推送到了远程仓库那就真的惨了。   
### 2.6 删除文件  
1. 删除工作区的文件---这是后版本库里还有，而且git知道了这个变化，版本库和工作区不一致   。
2. 若真的删除 两个命令`git rm filename.txt`  `git commit` 删除版本库。   
3. 误删了，`git checkout --filename.txt` 撤销工作区修改，从版本库在复制到工作区一份。
## 3.远程仓库--- nb1.0   
>理论准备：本地git仓库和github仓库之间是通过SSH加密传输的所以需要在本地生成自己的SSH 密钥，包含一个公钥个一个私钥。   
>公钥给github， 私钥自己保存。生成方法和添加步骤如下：    
### 3.1生成SSH密钥和设置github仓库。  
1. `ssh-keygen -t rsa -C "myemail@example.com"` 一路回车使用默认值即可。公钥和私钥会保存在用户主目录下 .ssh/  名字是 id_rsa 和 id_rsa.pub。   
2. “Account settings”-->“SSH Keys”--->“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容,
GitHub允许你添加多个Key。假定你有若干电脑，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。   
### 3.2添加远程仓库   
1. 在github上创建一个新的仓库，这时远程有一个空的仓库，本地有一个仓库。   
2. `git remote add origin git@github.com:TieQinRui/Study-Notes.git` 将本地仓库和远程仓库关联起来，以这种方式，远程仓库叫orign 本地叫master，也可以改名，但是这是git默认的，一看orign就知道是远程的。`git remote rm origin` 将本地仓库和远程仓库解关联。    
3. 第一次push 的时候要加上-u 参数` git push -u origin master` 以后就可以使用`git push remote origin` 了  
4. 某台设备第一次和github地址连接的时候会有SSH警告，yes就行了，商用要谨慎。（这一步的意思是git第一次链接github需要你自己确认SSH密码是不是有安全问题）  
> 其他实际问题：在创建了本地仓库，执行了本地仓库和远程关联后，**本地仓库和远程仓库不一致**   
>1. 远程仓库和本地仓库都已经有文件了，那么关联不会有问题，直接提交不能成行，解决办法：   
>   * 第一步，`git pull --allow-unrelated-histories origin  master` 忽略不相干历史将远程pull到本地。这时候的结果是远程的并不会覆盖本地的，只是本地的多出来了远程的。   
>   * 第二步，`git push origin master` 可以提交了，然后远程多出来本地的。   
>2. 本地仓库干净，远程仓库有内容。  
>   * 这是后push 会报错，直接 git pull origin master先把远程库拿回来再说。   
>3. 本地库有内容，远程库空白（ignore和readme也没有）。   
>   * 这里可以直接 `git push origin master` 即可成功。   
>4. 本地仓库和远程仓库都无内容。先关联了，这时情况和3是一样的，直接push即可。  
####测试这些最后的结论就是，首次提交的时候如果本地库和远程库不同时： 
1. 如果远程库不空，或者是以前的库清空的，则应该先将远程库pull到本地，这是会报历史不相干，选择 允许不相关的历史，的选项，会将远程库pull到本地，不覆盖本地内容。  
2. 如果远程库为空，且是新建的（建的时候连ignore，readme也没有）。可以直接 `git push origin master`   
3. 具体还是应该根据本地和远程的是否冲突，根据报错信息来应对。（等于没说……）   
### 3.3 从远程clone  
1. `git clone git@github.com:TieQinRui/learngit.git` 当先有了远程库，我们可以使用克隆的方法克隆到本地就很省事了，在适当的目录下执行此命令。  
2. `git clone https://github.com/TieQinRui/gitlearn.git`使用https协议也可以，一样的，网上讲这种方法会慢一些。

## 4.分支管理-nb2.0   
>git 分支管理又是它的优势之一,git 分支的创建、删除、切换非常快。  
### 4.1 创建和合并分支  
>原理： 在git中可以有很多分支，不同分支有不同的指针指向不同的提交点，最初只有一个master分支，和master指针，新建一个dev分支，相当于是
>新建了一个dev指针，在dev分支上开发，分支往前走，但是master指针不再往前走了，Head指针指向当前提交，合并的时候修改的也只是指针。  
1. `git branch` 查看所有分支。标 * 的是当前分支。  
2. `git branch <branchname>` 创建branch。   
![first](image/1.png)   
![2](image/2.png)    
3. `git switch <branchname>` 切换到某个branch。    
![](image/3.png)   
4. `git merge branchname` 合并某分支到当前分支。     
![](image/4.png)     
5. `git branch -d <branchname>` 删除某分支。     
![](image/5.png)        

> 不能删除当前的master分支，除非master分支不是当前默认的主分支   
### 4.2冲突管理 
1. 创建新分支并且提交。  
2. 回到master做不同的修改提交   
![](image/conflict.png)
3. 在master合并分支
4. 直接查看文件会详细列出冲突内容，方便修改。
5. 解决冲突再提交，然后合并成功。     
![](image/solveConflict.png)
* `git log --graph` 以图形方式显示分支。  
#### 往master上merge的时候总会按照dev去更细master。只要别在master上commit，就不会冲突了。
### 4.3分支管理策略  
1. master分支应该是非常稳定的，然后创建dev分支，在dev分支上干活。每个人有自己的分支，把自己的分支汇总到dev上，在发布版本的时候才将dev合并到master分支上。
2. 合并分支时，使用--no-ff 参数，禁用fast forward 合并（丢弃分支信息），可以创建一个新的commit，保留分支信息。有因为它穿件了新的commit 故要加上commit信息，命令为`git merge --no-ff -m "commit information" branchname`   
### 4.4 bug分支  
>* 场景：在dev上开发，突然，master上有bug要马上解决，但是当前dev又来不及提交，直接切换的话会有问题（不清楚），因此需要保存现场，   
>* 命令`git bash` 这样可一放心切换走，完事回来的时候，使用`git stash list` 可以显示所有的git stash。  
>* `git stash apply stash@{X}`+`git stash drop stash@{X}`  或者直接弹出 `git stash pop`   
>* 在master上修复了bug但是因为我们的dev是一个早期分支，故也有bug，可以用 `git cherry-pick fixbugcommitID`命令只提交解决bug的修改，如果恰好bug发生在dev正在改动的文件，那么这时cherry-pick 会冲突，需要解决冲突。

>####总的来说，就是，在分支下进行的工作，如果不commit的话，回到master，就会显示出你在分支下你添加的工作。这个时候，你在master下修改完bug提交后，正在分支进行的工作也会提交了。为了避免这个情况，你就在分支下，git stash将工作隐藏，这个时候，切换到master时候，修改了bug，提交。分支的内容不会被提交上去。   
### 4.5 future分支
1. 开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。   
### 4.6 多人协作
1. 远程仓库就是一个远程备份而已，一定要摒弃服务器的思维，远程仓库和本地仓库是等价的，只是因为要多人协作，才有具有了一些像服务器的特征罢了，因为git是分布式的，所以，想要在远程建立分支的话，先在本地建立分支，然后将分支推送到远程即可。`git branc dev` + `git push origin dev` 而push和pull操作相当于一个是本地同步到远程，一个远程同步到本地。   
2. 所以开发的时候，应该是在dev分支上开发，然后 在本地dev分支上推送到远程dev，当dev开发到了一定程度，在本地将dev merge到master上，然后在同步回远程。  
3. 多人协作的过程，在本地建立新的分支如dev，然后将本地的dev分支推送到远程，这样远程就有了一个dev分支。其他人可以拉取远程的dev分支。开发完成，也可以删除远程分支。
4. > 命令：
   >1. `git branch -a` 产看所有分支，本地和远程   
   >2. `git push <远程主机名 origin> <本地分支名>:<远程分支名>` 是push的完整命令。可以有多种省略的写法。   
   >`git push origin master` 相当于省略了远程分支名，将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。其他集中省略写法还是忘了好了。   
   >3. `git pull origin <remote_branch>：<local_branch>` 是pull的完整命令。有简写方案。   
   >`git pull origin <remote_branch>` 将远程分支同步到当前所在分支。   
   >4. `git branch --set-upstream branch-name origin/branch-name` 建立本地分支和远程分支的关联关系，这样使用pull 和push 命令时就不用再打上一大串了，可以方便使用简写。
   >5. ` git push origin --delete dev ` 删除远程分支dev。  
   >6. `git checkout -b 本地分支名 origin/远程分支名` 从远程拉取指定分支。   
### 4.7 git rebase   
参看：[git rebase 详解](http://jartto.wang/2018/12/11/git-rebase/)   
总的来说这个命令可以解决一些提交污染，使版本变更更清晰，但是同时也经常被认为是一个危险的操作，因为这个操作改变了提交历史。  
#### 就使用过程来说：   
===只对尚未推送或分享给别人的本地修改执行变基操作清理历史；  
===从不对已推送至别处的提交执行变基操作  
## 5.标签管理
> 标签tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。   
>
> ### 5.1 创建标签  
1. `git tag <name>` 在当前分支上将《那么》标签打在最近的一次commit（即head）上。
2. `git tag <name> commitID` 将标签打在特定的提交上。
3. `git tag` 以名称顺序列出所有的标签。  
4. ` git tag -a v0.1 -m "version 0.1 released" 1094adb` -a 指定标签名字， -m指定标签信息，使用`git show <tagname>可以查看标签信息。
5. >标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。   

### 5.2操作标签   
1. 命令`git push origin <tagname>`可以推送一个本地标签；  
2. 命令`git push origin --tags`可以推送全部未推送过的本地标签；    
3. 命令`git tag -d <tagname>`可以删除一个本地标签；   
4. 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。实际是推送空的标签覆盖远程标签。  
## 6.git其他使用   
###6.1 github 参与开源项目
可以fork一个开源仓库，然后从自己的GitHub仓库clone到本地，修改后推送到自己的github仓库，再然后发起 pull-request   

### 6.2 gitee 和github差不多  
可以使用一个本地仓库同时关联这两个仓库，甚至更多。只是不能叫origin了
`git remote add gitee git@gitee.com:liaoxuefeng/learngit.git` 推送命令也相应的改变。  
### 6.3 自定义git  
1. .ignore文件，不用自己手动去写了，可以在github上选用组合。
2. 配置别名可以省去手敲复杂命令的过程<https://www.liaoxuefeng.com/wiki/896043488029600/898732837407424>   
3. 图形化用户界面 sourceTree, git GUI 等。  
### 6.4 自己搭建git服务器 
> 原理：因为git是分布式版本控制系统，github的远程仓库和本地的仓库相比除了可以24小时开机并没有什么不同。因此自己搭建git服务器实际上是自己买台机器，让他24小时运行git，并作一些配置而已。[详细参考](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)   