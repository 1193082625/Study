# 2017-6-9学习git教程总结

## 1、分布式VS集中式

**集中式版本控制系统**：版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本。中央服务器就好比是一个图书馆，要改一本书，必须先从图书馆借走，自己改完了，再放回图书馆。集中式版本控制系统最大的毛病是必须联网才能工作。遇到网速慢的话，可能提交一个10M的文件就需要5分钟。SVN是目前用的最多的集中式版本库控制系统

**分布式版本控制系统** ：首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。这样工作的时候，就不需要联网了。项目人员之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多。分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样工作，只是交换修改不方便而已。分布式版本控制系统除了Git以及促使Git诞生的BitKeeper外，还有类似Git的Mercurial和Bazaar等，这些分布式版本控制系统各有特点，但最快，最简单也最流行的依然是Git。

**为什么Git比其他版本控制系统设计的优秀？**

因为Git跟踪并管理的是修改，而非文件。

## 2、安装

__在Linux上安装Git__ ：如果碰巧用Debin或Ubuntu Linux，通过一条 sudo apt-get install git就可以直接完成Git的安装。

__在windows上安装__ ：msysgit是Windows版的Git，从[https://git-for-windows.github.io](https://git-for-windows.github.io)下载（网速慢的同学请移步[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)），然后按默认选项安装即可。安装完成后，在开始菜单里找到“Git”-> 'Git Bash'，蹦出一个类似命令行窗口的东西，就说明Git安装成功。

安装完成后，还需要最后一步设置，在命令行输入：

git config --global user.name "Your Name"

git config --global user.email "email@example.com"

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门（名字和email地址）。注意`git config`命令的`--global`参数，用了这个参数，表示这台机器上所有的Git仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和email地址。

## 3、版本库 

版本库又名仓库（repository），可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

**创建版本库** ：

1、**创建空目录** ：选一个合适的地方，创建一个空目录：mkdir directoryName(目录名称) ;  cd directoryName 

2、**将目录初始化成一个Git仓库** ：通过git init命令把这个目录变成Git可以管理的仓库：git init 。命令执行成功后当前目录下会多出一个.git文件夹，这个文件夹是Git来跟踪管理版本库的。可用ls -ah 命令查看当前目录下包含的文件

3、**添加文件到Git仓库** ：

​         git add  <file> 添加文件（实际上是把文件修改添加到暂存区）

​         git commit -m '注释' 添加文件到Git仓库   （实际上是将暂存区的所有内容提交到当前分支，当创建Git版本库时，Git自动为我们创建了唯一一个master分支。Git commit 就是往master分支上提交更改）

​	每当文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦把文件改乱了或者误删了文件，可以从最近的一个commit恢复。每次修改，如果不add到暂存区，那就不会加入到commit中

## 4、远程仓库 

​	实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

### 先有本地库，后有远程库


​	**GitHub** 网站就是提供Git仓库托管服务的，只要注册一个GitHub账号，就可以免费获得**Git远程仓库** 

​	**1、创建SSH Key** ：ssh-keygen -t rsa -C "youremail@example.com"。一路回车，使用默认值即可。最终在用户主目录（C:\Users\Administrator）下生成.ssh目录。这个目录下包含**id_rsa** 和**id_rsa.pub** 这两个文件。这两个就是SSH Key的密钥对。id_rsa.pub是公钥

​	**2、登录GitHub，打开setting —>SSH keys页面** ，点击“Add SSH Key”，填上任意title，在Key文本框里粘贴id_rsa.pub文件的内容

​	**3、创建远程新仓库** ：在GitHub上"new repository"

​	**4、关联一个远程库，把本地仓库的内容推送到GitHub仓库** ：git remote add origin <远程仓库的SSH地址> 

​		_添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的。如果有报错：remote origin already exists . 解决办法：$ git remote rm origin ，然后再重新执行命令就可以了。_

​	**5、第一次推送本地master分支的所有内容到远程仓库上** ：git push -u origin master

​		_用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，第一次推送master分支时，加了-u参数，Git会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一样了_  

​	**6、后续本地改动按需推送到远程仓库** ：git push origin master（也可以直接 git push）

### 假设从零开发，最好的方式是先创建远程库，然后，从远程库克隆

​	**1、创建远程新仓库** ：参见上文

​	**2、克隆一个本地库** ：git clone <远程仓库的SSH地址>

## 5、分支

在实际开发中，应该按照几个基本原则进行分支管理：

​	首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不在上面开发。

​	开发都在dev分支上，比如1.0版本发布时，把dev分支合并到master上，在master分支发布1.0版本

​	合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

## 6、忽略特殊文件

在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件

## 7、命令 

**git status** ：仓库当前的状态（可以告诉我们冲突的文件，用于解决冲突）

 **git diff** ：查看文件具体的修改内容

**git log** ：显示从最近到最远的提交日志

**git log --pretty=oneline** ：简化显示的日志信息

_在Git中，用**HEAD** 表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上个版本就是HEAD^^_ 

**git reset --hard HEAD^** ：把当前版本回退到上一个版本

**git reset --hard commit_id** ：把当前版本指定到某个版本（版本号没必要写全，前几位就可以了）

**git reflog** ：查看命令历史，以便确定要回到那个版本

**git diff HEAD -- <file>** ：查看工作区和版本库里最新版本的区别

**git checkout -- <file>** ：丢弃工作区的修改（命令中的**--** 很重要，如果没有**--** ，就变成了切换到另一个分支的命令），把误删的文件恢复到最新版本（误删恢复）

**git reset HEAD <file>** ：把暂存区的修改撤销掉，重新放回工作区（git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当用HEAD时，表示最新的版本）

**rm <file>** ：删除文件

**git rm <file>** ：从版本库中删除文件（命令完成后，需要commit提交一下）

**git checkout -b <dev>** ：创建新分支，命名为dev，然后切换到dev分支

**git branch** ：列出所有分支，当前分支前面会标一个*号

**git checkout <master>** ：切换回master分支

**git merge <dev>** ：把指定分支（dev）的工作成果合并到当前分支上

**git branch -d <dev>** ：删除指定分支（dev）

**git log --graph --pretty=oneline --abbrev-commit** ：查看分支的合并情况

**git merge --no-ff -m '注释' <dev>** ：**--no-ff** 参数表示禁用Fast forward（快速合并）模式，因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去

**git stash** ：把当前工作现场“储藏”起来，等以后恢复现场后继续工作

**git stash list** ：查看保存起来的工作现场

**git stash apply** ：恢复工作现场，但是恢复后，stash内容并不删除，需要用**git stash drop** 删除

**git stash pop** ：恢复工作现场的同时把stash内容也删了

**git branch -D <name>** ：强行删除没有被合并过的分支

**git remote** ：查看远程库的信息

**git remote -v** ： 查看远程库的详细信息

**git tag** ：查看所有标签

**git tag <版本号>** ：在最新提交的commit上打一个新标签

**git tag <版本号>** <commit_id> ：给对应的commit打标签

**git show <tagname>** ：查看tagname标签信息

**git tag -a <tagname> -m '注释'** ：指定标签信息

**git tag -d <标签>** ：删除标签



