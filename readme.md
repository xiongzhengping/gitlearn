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
为什么Git必其他版本控制系统设计的优秀，因为Git跟踪并管理的是修改，而非文件。
每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

### 撤销修改

