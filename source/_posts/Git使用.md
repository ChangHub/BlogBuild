---
title: Git使用
author: HongChangCui
date: 2017-05-01 20:30:00
categories: 版本控制
tags: 
  - Git
cover_picture: git.jpg
---
Git是一个开源的分布式版本控制系统。
为了帮组管理Linux内核开发而开发的一个开放源码的版本控制软件。
<!--more-->
## Git与SVN区别
 - Git是分布式的版本控制系统，没有中央服务器。每个人的电脑就是一个版本库，工作不用联网，多人修改时可以互相推送。
 - SVN是集中式版本控制系统，版本库放在中央服务器，因此每次都要从中央服务器中获取最新的版本，修改后再推送到服务器上，整个过程都要联网，受网络限制。
## Git使用 
#### Git 安装（windows）

官网下载对应系统版本的安装文件点击安装即可。如图：
![Git Windwos 版安装图示][1]
安装之后，会有两个工具Git GUI（基于图形界面）和Git Bash（基于命令行）。
对于熟悉SVN操作的，可以下载类型的工具TortoiseGit工具，类似的方便工具还有SourceTree等。
#### Git配置
* 配置用户信息：Git使用用户名和邮箱作为一个标识。
```
$git config --global user.name 'xxxxx'
$git config --global user.email 'xxxx@xxx.com'
```
使用--global选项，你所有的项目都会使用这个配置。如果要在某个特定的项目中使用其他的用户，只要去掉--global选项重新配置即可，新的配置保存在当前项目.git/config文件里。
* 查看配置信息：
```
$git config --list
```
显示如图所示：
![Git 查看配置信息][2]
#### Git工作流程
![Git 工作流程][3]
#### Git基本概念
* 工作区
  电脑本地目录
* 暂存区
  英文叫做Stage或者index，存在".git"目录下index文件中，有时把暂存区叫做索引
* 版本库
  工作区隐藏的目录".git"
  ![Git 基本概念理][4]
  说明：
* "HEAD"指向master分支一个游标，命令中出现HEAD的地方可以用master代替。
* "Objects"标识区域为Git的对象库，位于".git/objects"目录下，包含了创建的各种对象及其内容。
* 当执行"git add ."命令时，同时该工作区的文件被写入对象库中的一个新对象中，该对象的ID被记录在暂存区文件索引中，暂存区的目录树被更新。【工作区=》暂存区】
* 当执行"git commmit"命令时，暂存区目录树写到版本库（对象库）中，master分支做出相应的更新，即master的目录树，就是提交时暂存区的目录树。【暂存区=》master】
* 当执行"git reset HEAD"命令时，暂存区的目录树被重写，被master分支目录树所替换，工作区不受影响。【master=》暂存区】
* 当执行"git rm --cache <file>"命令时，会从暂存区删除文件，工作区不受影响。【暂存区】
* 当执行"git checkout ."命令时，会用暂存区的文件替换工作区的文件，该操作很危险，会清除工作区未添加到暂存区的改动。
* 当执行"git checkout HEAD ."命令时，会用master分支的文件，替换暂存区和工作区的文件，该操作更危险。
* Git的其他命令：
  ![Git 命令][5]
  - Git status:查看版本库的状态。可以知道哪些文件发生变化，哪些文件没有添加到版本库中，建议每次commit命令之前，通过该命令确认库状态。
  - Git log:查看历史记录。包含每次的版本变化，每次变化对应一个ID。
  - Git merge:把服务器上下载的代码与本机代码合并，或者进行分支合并。
  - Git diff:把本地的代码与index中的代码进行比较。
#### 创建仓库
* Git initGit:使用"Git init"命令，初始化一个Git仓库，Git仓库会生成一个隐藏目录".git",该目录包含了资源的所有元数据。
* Git clone:从现有仓库中拷贝项目。
```
$ git clone <repo> <dirctory>
```
repo:Git 仓库；directory 本地目录；
#### Git分支管理
使用分支意味着可以从开发主线上分离出来，在不影响主线的同时继续工作。
* 创建分支：
```
$git branch (branchname)
```
* 切换分支：
```
$git checkout (branchname)
```
切换分支，用该分支最后提交的快照，替换你工作区的内容。
* 合并分支：
```
$git merge
```
多次合并到统一分支，也可以选择合并之后删除被并入的分支。
* 列出分支
```
$git branch
```
当执行"git init"命令时，缺省情况下，Git会创建一个"master"分支。
* 删除分支
```
$git branch -d (branchname)
```
#### Git GitHub
Git 远程仓库（GitHub）
* 添加远程仓库
```
$git remote add [shortname] [url]
```
由于本地Git库与远程仓库GitHub之间的传输是通过SSH加密的，所以需要验证配置信息：
使用一下命令生成SSH Key:
```
$git-keygen -t rsa -C "youmail@example.com"
```
成功的话，会在本地用户目录下（如：C:\Users\Administrator\）生成".ssh"文件夹，打开.id_rsa.pub,复制里面的key。
回到github，进入Account=>setting。如图：
![GitHub 用户配置][6]
左边选择"SSH and GPG Keys",点击"New SSH Key",粘贴刚刚生成的Key。
验证是否成功：
```
$ssh -T git@github.com
```
然后可以创建仓库repository。
* 本地仓库推送到远程仓库
  - git init（新建一个本地仓库）
  - git add .(添加所有文件到本地仓库)
  - git commit -m 'first commit'(提交本地仓库)
  - git remote add origin http://xxxxx/xxx.git(添加远程仓库，origin是别名）
  - git push -u origin master(将本地仓库推送到远程仓库，并将origin设为默认的远程仓库）
* 远程仓库克隆到本地仓库
  - git remote -v(查看远程仓库)
  - git fetch origin master(获取远程仓库最新版本到本地)
  - git merge origin master(远程代码合并到本地仓库)
  - git pull origin master(代替前面两步：fetch+merge)


[1]: https://www.github.com/ChangHub/BlogImages/raw/master/Git%E5%AE%89%E8%A3%85%E5%9B%BE%E7%A4%BA.jpg "Git安装图示"
[2]: https://www.github.com/ChangHub/BlogImages/raw/master/Git%E6%9F%A5%E7%9C%8B%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.jpg "Git查看配置信息"
[3]: https://www.github.com/ChangHub/BlogImages/raw/master/Git%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B.jpg "Git工作流程"
[4]: https://www.github.com/ChangHub/BlogImages/raw/master/Git%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5.jpg "Git基本概念"
[5]: https://www.github.com/ChangHub/BlogImages/raw/master/Git%E5%91%BD%E4%BB%A4.jpg "Git命令"
[6]: https://www.github.com/ChangHub/BlogImages/raw/master/GitHub%E7%94%A8%E6%88%B7%E9%85%8D%E7%BD%AE.jpg "GitHub用户配置"
