# GIT TIPS
### 创建版本库

> 一个文件夹可以通过``git init``使其变为一个git仓库，即repository。
>
> git提交文件需要两步，一、使用``git add <file>``。二、使用``git commit -m " "``。这样的特性允许git一个提交很多文件。

> 要查看当前仓库的状态使用``git status``命令，查看文件修改的细节使用``git diff <file>``查看
>
> 

> git所能管理的版本是每次的commit，所以commit时的说明就至关重要了。
### 版本回退
> git可以回退到任意版本，head表示的是当前版本head^表示上一版本，head^^表示上上一版本，head~100表示上100版本。
>
> 回退版本使用的是``git reset --hard commit_id``,commit_id可以使用``git log``查看各个版本信息，好看点的使用``git log --pretty=oneline``。
>
> commit_id可以通过``git reflog``来查看，就是查看以前都执行了什么命令
### 管理修改
> git管理的是修改而不是文件，这里可以从一个例子看出：对一个文件第一次修改，然后add，再第二次修改，不add，然后commit，结果是第一次的修改被保存了，然而第二次的修改没被保存。
>
> 关于管理修改有命令``git diff head -- filenane``这个命令可以把工作区的文件与当前版本比较差异。
### 撤销修改
> 对于修改有3中情况：
>
> 1.在工作区修改之后为add到缓存区。
>
> A：可以使用``git checkout -- file``，这个命令仅对工作区有作用，也就是说当使用这个命令之后文件会回退到add或者commit之后，仅仅是把在工作区里面的修改撤销掉。
>
> 2.修改add到缓存区了。
>
> A：已经知道``git checkout -- file``对已经add的文件修改无法撤销了，所以要使用``git reset head file``,回到第一种情况。
>
> 3.修改已经commit。
>
> A：对于这种情况，和版本回退处理方式一样。

### 删除文件

>当在工作区的文件删除``rm file``之后有两种情况：
>
>1.想在版本库里也删除这个文件。
>
>A：使用``git rm file``命令和``git commit -m ""``，在版本库里也删除文件。
>
>2.想撤销删除。
>
>A：使用``git checkout -- filename``将文件file恢复到最新的版本，就是最近的一次commit。

---

## 远程仓库github

### 添加仓库

> github和git之间的通信通过ssh加密，所以使用``ssh-keygen -t rsa -C "youremail@example.com"``来在本地生成sshkey，然走添加到github信任的sshkey里面，可以添加很多sshkey，这样就可以很多机器都可以关联github仓库了。
>
> 完成之后可以使用``git remote add origin git@github.com:githubname/git-repos.git``来关联本地仓库和github上的仓库。
>
> 以后就可以使用``git push -u origin master``来推送本地仓库到github上的仓库。其中``-u``第一次写，作用是：不但把本地上的master分支上的内容推送到远程的master分支上，而且会把本地的master分支和远程的master分支关联起来，以后push和clone就可以简化命令了。

### clone仓库

> ``git clone git@github.com:githubname/git-repos.git``clone仓库。

## 分支管理

### 创建和切换分支

>git鼓励使用分支完成功能，然后在合并到master分支上。
>
>**head**指向的是当前提交的，实际上head指向的是master，master指向的才是当前提交。
>
>- 创建和切换的命令是``git checkout -b name``表示的是创建一个分支，并切换到它。
>- 其是由两个命令合成的，即``git branch name``和``git checkout name``，**注意：``git checkout -- filename``是撤销修改**
>- 查看分支的命令是``git branch``其中使用``*``标识出来的是当前分支。
>- 合并分支命令``git merge br_name``其中命令的意思是合并br_name到当前分支。
>- 删除分支是``git branch -d br_name``
>

### 解决冲突

> 当两个分支修改同一个文件的同一个地方时，在合并的时候会出现冲突，需要手动解决冲突在合并。
>
> **大概就是将冲突选择统一的结果**
>
> ``git log --graph --pretty==oneline --abbrev-commit``查看分支情况。

### 分支管理策略

> git合并分支的时候默认的是fast forward模式，这种模式下，删除分支之后就会丢掉分支信息。
>
> ``git merge --no-ff -m "  " br_name``这种是没有fast forward模式，原理是在当前分支新commit一次然后和合并的分支merge，所以需要有``-m "  "``，这样可以很好的管理分支。

> 丢弃未合并的分支使用``git branch -D br_name``强制删除。

### bug分支

> 有时在一个分支上干活，还没到commit的地步，但是突然有些东西需要去其他分支处理，比如修复bug，这样就需要使用``git stash``将这个分支挂起，其工作空间也是干净的。
>
> - 当回到这个分支工作时，可以使用``git stash list``查看现在存在的stash。
> - ``git stash apply``恢复stash。
> - ``git stash drop``删除stash。
> - ``git stash pop``恢复stash并删除stash。

[360](http://hao.360.cn)

### 多人协作

>使用``git remote ``和``git remote -v``可以查看远程仓库的信息。使用``git push origin master``推送主分支到远程仓库，同时也可以推送其他分支。需要推送的分支大概有：
>
>- master分支，时刻要保持其与远程仓库同步。
>- 工作分支，一般都不会在主分支上工作，成员工作一般都是在新建的工作分支上工作。需要推送到远程仓库
>
>

> **多人协作流程：**
>
> - 在从远程仓库clone时，只会clone master分支，所以要早工作分支上工作就需要将origin的工作分支创建到本地，使用``git checkout -b br_name origin/br_name``,最好远程的工作分支和本地的工作分支一样的名字。
>
> - 当在本地分支上修改完成后需要推送到远程的分支，但团队中的其他人也已经推送了这个分支，有冲突，所以需要将远程分支的最新更新拉到本地，
>
>   ``git pull``,但是会出现失败，**本地的工作分支没有和远程的分支建立连接**，使用``git branch --set-upstream br_name origin/br_name``建立连接。然后在pull。
>
> - 出现冲突，和处理冲突的方法一样，解决冲突后就可以push了。



## 标签管理

### 创建标签

> 有时commit_id太过繁杂，不宜清晰的标明版本，所以就可以使用标签。
>
> ``git tag tag_name``创建一个标签，这个标签是在head上的，也可以用``git tag tag_name commit_id ``在其他commit上创建tag。
>
> 查看tag使用``git tag``查看。
>
> 也可以使用``git tag -a <tag_name> -m "  " <commit_id>``给tag加上说明。
>
> 查看一个tag的详细信息可以使用``git show <tag_name>``。

### 操作标签

> - 删除tag使用``git tag -d <tag_name>``。
> - 将tag推送到远程仓库使用的是``git push origin <tag_name>``
> - 将所有未推送的tag都推送到远程仓库，使用``git push origin --tags``
> - 删除远程仓库的tag，``git push origin :refs/tags/<tag_name>``​















