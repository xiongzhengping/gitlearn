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
其他版本控制系统如SVN豆油分支管理，但用过之后你会发现，慢如蜗牛。
但Git的分支是与众不同的，无论创建、切换和删除分支，Git在一秒钟之内就能完成，无论1个文件还是1w个文件。
### 创建与合并分支
master是一个分支，在Git里，这个分支叫做主分支，HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支；
如果有了新的分支，如dev，HEAD指向dev，就意味着当前分支在dev上；
![master分支](https://www.liaoxuefeng.com/files/attachments/919022325462368/0 "master分支")

