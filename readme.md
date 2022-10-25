# Git教程
## Git简介
Git是目前世界上最先进的分布式版本控制系统；
### Git的诞生
Linus，2005年，两周时间用C写了Git；
2008年，GitHub上线；
### 集中式vs分布式
集中式版本控制系统最大的毛病就是必须联网才能工作，有一个中央服务器；
分布式版本控制系统没有中央服务器，每个人的电脑都是一个服务器，也会有一台充当“中央服务器”的电脑，主要是用来方便“交换”大家的修改。
Git的优势还在于强大的分支管理。
## 安装Git
### Linux
`sudo apt-get install git`
### Mac
通过homebrew安装，`brew install git`
通过安装xcode，集成了git
### Windows
下载安装程序，Git bash

安装完成后，还要设置：
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
## 创建版本库
版本库又叫仓库，英文名叫repository
`$ git init`初始化git仓库，将所在目录变成git仓库
__把文件添加到版本库__
分两步：1. git add <file>，可以一次添加多个文件；
2. git commit -m <message>，确认提交，-m后跟随提交的说明，建议必须有

## 时光穿梭机
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`
* 穿梭前，用`git log`可以查看提交历史，可以看到commit_id，以便确定回退到哪个版本；
* 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本；

### 工作区和暂存区
工作区（working directory）：文件所在的目录
版本库（repository）：工作区中的一个隐藏目录`.git`
版本库中有：暂存区（stage，或者叫index），git为我们创建的第一个分支（master），以及指向master的一个指针（HEAD）；
需要提交的文件修改统统先放到暂存区，然后一次性提交暂存区的所有修改；

### 管理修改
**为什么Git必其他版本控制系统设计的优秀，因为Git跟踪并管理的是修改，而非文件。**
每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

### 撤销修改
场景一：当你该乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`
场景二：当你不但该乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景一，第二部按场景一操作。
场景三：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考**版本回退**一节，不过前提是没有推送到远程库。

### 删除文件
`rm test.txt`，在目录中删除某文件
`git rm test.txt`，在版本库中删除某文件
`git commit -m "remove test.txt"`，提交删除文件的修改
`git checkout -- test.txt`，删错了，版本库中有test.txt，可以从版本库中恢复，如果没有添加到版本库中，那么是无法恢复的
总结：命令`git rm`用于删除一个文件，如果一个文件已经被提交到版本库，那么你永远不要担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 远程仓库
GitHub注册
由于本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：
`$ ssh-keygen -t rsa -C "youremail@example.com"`
用户主目录下`$HOME`应该有`.ssh`目录，其下应有`id_rsa`和`id_rsa.pub`文件，用以保存ssh key。
GitHub允许添加多个key。多台电脑可以都生成ssh key，然后都添加到GitHub，就可以在多台设备上推送了。
提示：GitHub上的免费托管仓库，任何人都可以看到，但只有自己才能修改。
### 添加远程库
关联一个远程库：`git remote add origin git@server-name:path/repo-name.git`
关联一个远程库时必须给远程库指定一个名字，`origin`时默认习惯命名；
关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；
伺候，每次本地提交后，只要有必要，就可以使用玲玲`git push origin master`推送最新的修改；
`git remote -v`，查看远程库信息；
`git remote rm origin`，删除远程库（逻辑上解绑，实际物理上删除远程库需要去GitHub删除）；
### 从远程库克隆
`$ git clone git@github.com:yourname/repo-name.git`
从远程克隆一个本地库；
除了git@github.com这种格式，还有`https://github.com/yourname/repo-name.git`这样的地址，https除了速度慢以外，最大的麻烦事每次推送必须输入口令；

## 分支管理
其他版本控制系统如SVN都有分支管理，但用过之后你会发现，慢如蜗牛。
但Git的分支是与众不同的，无论创建、切换和删除分支，Git在一秒钟之内就能完成，无论1个文件还是1w个文件。
### 创建与合并分支
master是一个分支，在Git里，这个分支叫做主分支，HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支；
如果有了新的分支，如dev，HEAD指向dev，就意味着当前分支在dev上；

![master分支](https://www.liaoxuefeng.com/files/attachments/919022325462368/0 "master分支")

查看分支：`git branch`
创建分支：`git branch <name>` ？我没看到这个用法，创建分支用的是`git checkout -b dev`，`git switch -c dev`；
切换分支：`git checkout <name>`或者`git switch <name>`
创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
合并某分支到当前分支：`git merge <name>`
删除分支：`git branch -d <name>`
### 解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑成我们希望的内容，再提交。

当merge出现冲突的时候，冲突的文件内会提示冲突的内容。可以直接修改。

`$ git log --graph --pretty=oneline --abbrev-commit`可以查看树状分支合并图。

### 分支管理策略
通常，合并分支时，Git会用`fast-forword`模式，在这种模式下，删除分支后，会丢掉分支信息。
如果强制禁用`fast-forword`模式，merge时就会生成一个新的commit，这样，就可以从分支历史上看出分支信息。

`git merge --no-ff -m "merge with no-ff" dev`

在`git log --graph --pretty=oneline --abbrev-commit`中，可以显示dev分支的提交信息。（即使dev分支被删除后）

**分支策略**

首先，master分支应该非常稳定，仅用来发布新版本，平时不在上面干活。

干活都在dev分支上，dev是不稳定的，到某个时刻，比如1.0版本发布时，再把dev分支合并到master分支上。

每个人都有自己的分支，时不时的往dev上合并即可。

![分支管理策略](https://www.liaoxuefeng.com/files/attachments/919023260793600/0 "分支管理策略")

### bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`以下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

### feature分支

开发一个新的功能，最好新建一个分支，在上面开发，完成后，合并，最后，删除该分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除；

### 多人协作

从远程克隆仓库时，实际上自动把本地的master分支和远程的master分支对应起来，远程仓库的master分支默认名称是origin；

#### 推送分支

推送分支时，要指定本地分支；

* master分支是主分支，需要时刻与远程同步；
* dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
* bug分支只用于在本地修复bug，没必要推送到远程，除非有明确的要求；
* feature分支是否推送到远端，取决于是否和他人合作开发；

#### 抓取分支

多人协作时，大家都会向master和dev上推送各自的修改；

从远程库clone时，默认情况只能看到本地的master分支；

如果需要在dev分支上开发，那就必须创建远程库origin的dev分支到本地；

`$ git checkout -b dev origin/dev`

是不是把dev分支推送到远程：

`$ git push origin dev`

如果大家都对origin/dev分支推送了提交，那么后提交的人会提示推送失败，有冲突，先用`git pull`把最新的提交从`origin/dev`抓下来，在本地合并，解决冲突后，再推送；

git pull失败，是因为没有指定本地dev分支与远程origin/dev分支的链接

`git branch --set-upstream-to=origin/dev dev`

因此，多人协作的工作模式通常是这样：
1. 首先，可以试图用`git push origin <branchname>`推送自己的修改；
2. 如果推送失败，则因为远程文件比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，在用`git push origin <branch_name>`推送就能成功。

如果`git pull`提示`no tracking information`，则说明本地分支与远程分支的链接关系没有创建，用命令`git branch --set-upstream-to=origin/dev dev`

### Rebase

rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比；

## 标签管理

发布一个版本时，我们通常现在版本库中打一个标签（tag），这样就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来，所以，标签也是版本库的一个快照。

git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像，但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

tag是一个让人容易记住的有意义的名字，跟某个commit绑定在一起。

### 创建标签

* 命令`git tag <tagname>`用于新建一个标签，默认为HEAD，也可以指定一个commit_id`git tag <tagname> commit_id`；
* 命令`git tag -a <tagname> -m "blablabla...."`可以指定标签对应的信息
* 命令`git tag`可以查看所有标签

### 操作标签

* `git push origin <tagname>`，推送一个本地标签；
* `git push origin --tags`，推送全部未推送过的本地标签；
* `git tag -d <tagname>`，可以删除一个本地标签；
* `git push origin :refs/tags/<tagname>`，可以删除一个远程标签；