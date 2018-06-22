#GIT
##GTI 简介
GIT是一款开源的，快速的，可扩展的分布式版本控制系统。
###分布式与集中式的区别

集中式版本控制系统，版本库集中存放在中央服务器，干活时用的是自己的电脑，需要先从中央服务器获取最新的版本，然后开始干活，干完活将版本推送到中央服务器。
中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。

分布式没有中央服务器，每个人的电脑都是一个完整的版本库，它是如何实现多人协作的呢？将各自的修改部分推送给对方，就可以看到对方的修改了。
##安装GIT
GIT适合在linux、Unix、Mac和windows上运行。

Debian操作系统：

	sudo apt-get install git

CentOS操作系统：

	yum -y install git

windows操作系统：

	官网下载安装程序

[下载官网](https://git-scm.com/downloads)

每个机器都要自报家门,因为Git是分布式的
<pre>
git config --global user.name "zyzfzb"
git config --global user.email 17681899286@163.com
</pre>
##创建版本库

什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

*  创建一个空目录
<pre>
[root@localhost ~]# mkdir learngit
[root@localhost ~]# cd learngit
[root@localhost learngit]# pwd
/root/learngit
</pre>
- 通过git init命令将目录变为GIT可管理的仓库
<pre>
[root@localhost learngit]# git init
Initialized empty Git repository in /root/learngit/.git/
</pre>
- 通过ls -ah查看到如下目录表示仓库创建成功
<pre>
[root@localhost learngit]# ls -ah
.  ..  .git
</pre>
**注意：**
windows不要使用记事本编辑任何文本，用notepad++替代，记事本在文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题。

##将文件添加到版本库
-  编写一个readme.txt,一定要放到你刚刚创建的learngit目录下或者其子目录下

<pre>
[root@localhost learngit]# cat readme.txt 
Git is a version control system.
Git is free softeware .
</pre>
- 将文件添加到从仓库，并提交到仓库
<pre>
[root@localhost learngit]# git add readme.txt
[root@localhost learngit]# git commit
Aborting commit due to empty commit message.
[root@localhost learngit]# git commit -m "wrote a readme file"
[master (root-commit) c94ab43] wrote a readme file
 Committer: root <root@localhost.localdomain>
</pre>


>-m 参数后面的是对本次提交的说明，最好是有意义的内容这样你就能方便的找到改动的内容

- 一次提交多个文件

<pre>
git add file1 file2 fil3
git commit -m "add 3 files"
</pre>
## GIT 的回退功能
- git reset 版本回退

如上面的readme.txt有3个版本，我们要回退到版本2实现如下
<pre>
[root@localhost git]# git reset --hard 7b782
HEAD is now at 7b78263 readme version2
[root@localhost git]# cat readme.txt 
Version 2
Git is a fast scalable and distributed revision control system .
</pre>
只要你知道版本id就能够回退到任意版本，所以记录版本id也是一个重要的是工作。
当你回退到版本2后，通过查看提交日志是无法查看到版本3的.为什么呢？因为，你都回到版本2了，查看到的日志自然是版本2时间段的日志，而版本3是版本2未来的日志，你自然无法看到关于版本3的日志了。所以说提前记录好版本ID是一个很重要的工作
<pre>
[root@localhost git]# git log
commit 7b782637e9e3fae72afbfc513fc41e0bc6e58be0
Author: zyzfzb <17681899286@163.com>
Date:   Fri Jun 22 09:13:03 2018 +0800

    readme version2

commit 3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7
Author: zyzfzb <17681899286@163.com>
Date:   Fri Jun 22 09:11:12 2018 +0800

    readme version1
</pre>

- git reflog 查看提交版本历史记录

如果很不幸你没有记录版本ID，但你想返回未来的某个版本该如何操作呢？
git reflog可以查看提交版本的历史记录帮助你找到你需要的版本ID

<pre>
[root@localhost git]# git reflog
b94463c HEAD@{0}: reset: moving to b94463ce412850da3a5e69735b5d73b3e894b681
3543a2a HEAD@{1}: reset: moving to 3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7
7b78263 HEAD@{2}: reset: moving to 7b782
b94463c HEAD@{3}: reset: moving to b9446
7b78263 HEAD@{4}: reset: moving to 7b782
b94463c HEAD@{5}: commit: readme version3
7b78263 HEAD@{6}: commit: readme version2
3543a2a HEAD@{7}: commit (initial): readme version1
</pre>
HEAD 的值由小到大，时间越来越远

其实它就是查看日志文件，在linux下一切皆文件
<pre>
[root@localhost .git]# pwd
/root/git/.git
[root@localhost .git]# tree logs/
logs/
├── HEAD
└── refs
    └── heads
        └── master
</pre>
下面我们看一下HEAD文件可以清楚的看到版本ID以及版本的变化过程
<pre>
[root@localhost .git]# cat logs/HEAD 
0000000000000000000000000000000000000000 3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7 zyzfzb <17681899286@163.com> 1529629872 +0800	commit (initial): readme version1
3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7 7b782637e9e3fae72afbfc513fc41e0bc6e58be0 zyzfzb <17681899286@163.com> 1529629983 +0800	commit: readme version2
7b782637e9e3fae72afbfc513fc41e0bc6e58be0 b94463ce412850da3a5e69735b5d73b3e894b681 zyzfzb <17681899286@163.com> 1529630621 +0800	commit: readme version3
b94463ce412850da3a5e69735b5d73b3e894b681 7b782637e9e3fae72afbfc513fc41e0bc6e58be0 zyzfzb <17681899286@163.com> 1529632980 +0800	reset: moving to 7b782
7b782637e9e3fae72afbfc513fc41e0bc6e58be0 b94463ce412850da3a5e69735b5d73b3e894b681 zyzfzb <17681899286@163.com> 1529633212 +0800	reset: moving to b9446
b94463ce412850da3a5e69735b5d73b3e894b681 7b782637e9e3fae72afbfc513fc41e0bc6e58be0 zyzfzb <17681899286@163.com> 1529633859 +0800	reset: moving to 7b782
7b782637e9e3fae72afbfc513fc41e0bc6e58be0 3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7 zyzfzb <17681899286@163.com> 1529633877 +0800	reset: moving to 3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7
3543a2a9f4ddbfd7a66e85d5c433764cc0f078b7 b94463ce412850da3a5e69735b5d73b3e894b681 zyzfzb <17681899286@163.com> 1529633888 +0800	reset: moving to b94463ce412850da3a5e69735b5d73b3e894b681

</pre>

## 工作区和暂存区
### 工作区 （working directory）
我们创建的Git learngit 这些目录就是一个工作区。
### 版本库 .git
在工作区下有一个隐藏的目录 .git，它不是工作区，它是版本库，里面有版本、日志等文件
<pre>
[root@localhost git]# ls -ah
.  ..  .git  readme.txt
[root@localhost git]# tree -L 1 .git/
.git/
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
├── index
├── info
├── logs
├── objects
├── ORIG_HEAD
└── refs

</pre>
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

### 暂存区
我们在添加文件到索引就是添加到了暂存区，在linux里就是.git目录里的index文件。


当我们添加修改的内容到（索引）暂存区后确认无问题就会提交修改，这个提交就是将文件提交到当前分支如：master 

一旦文件提交，而你又没有再对文件进行修改，此时工作区就是"干净"的。
<pre>
[root@localhost git]# git status
# On branch master
nothing to commit, working directory clean
</pre>
## Git跟踪管理修改而不是文件
在Git中，添加一行、删除一行、创建一个新文件都是一个修改。

为什么手Git管理的是修改而非文件？

<pre>
[root@localhost git]# cat LICENSE.txt 
copyright......
[root@localhost git]# echo "copyright2......" >>LICENSE.txt 
[root@localhost git]# cat LICENSE.txt 
copyright......
copyright2......
[root@localhost git]# git commit -m "LICENSE version2"
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   LICENSE.txt
#
(use "git add" and/or "git commit -a")

</pre>
如上面，没有将修改添加到暂存区（索引），直接提交，修改就不成功，提示no changes added to commit 

然后我们将修改先加到暂存区，然后再提交,此时就会一个文件修改了。
<pre>
[root@localhost git]# git add LICENSE.txt
[root@localhost git]# git commit -m "LICENSE version2"
[master 04fa287] LICENSE version2
 1 file changed, 1 insertion(+)

</pre>
由此可见Git跟踪的是修改内容而非文件。只有将修改加到暂存区（index），然后提交，文件修改才能生效，而直接提交文件是无法成功提交修改的。

## 撤销修改
如果你修改之后，猛然发现有个错误。该如何撤销呢？

	git checkout -- readme.txt
> 说明：这里我是将 readme.txt文件的 version 3 修改为 version4

情景1：修改好文件，还没有添加到暂存区
这种情况使用 git checkout之后，撤销修改就回到版本库一模一样的状态。
<pre>
[root@localhost git]# vim readme.txt 
[root@localhost git]# cat readme.txt
Version 4
       Git is a fast, scalable, distributed revision control system with an unusually rich command
       set that provides both high-level operations and full access to internals.


[root@localhost git]# git checkout -- readme.txt
[root@localhost git]# cat readme.txt
Version 3
       Git is a fast, scalable, distributed revision control system with an unusually rich command
       set that provides both high-level operations and full access to internals.



</pre>

情景2：修改好文件，已经添加到暂存区，然后又修改了
<pre>
[root@localhost git]# cat readme.txt 
Version 3
Git is a fast scalable and distributed revision control system  with an unusually rich command.

[root@localhost git]# vim readme.txt 
[root@localhost git]# cat readme.txt
Version 4
Git is a fast scalable and distributed revision control system  with an unusually rich command.

[root@localhost git]# git add readme.txt
[root@localhost git]# echo "verion 5" >>readme.txt 
[root@localhost git]# cat readme.txt
Version 4
Git is a fast scalable and distributed revision control system  with an unusually rich command.

verion 5
[root@localhost git]# git checkout readme.txt
[root@localhost git]# cat readme.txt
Version 4
Git is a fast scalable and distributed revision control system  with an unusually rich command.

</pre>
还有一种方法也可以
<pre>
2、使用git reset HEAD filename 也可以把暂存区的修改撤销
</pre>

情景3：修改好文件，已经git add,然后又修改了，并且再次git add,最后git commit 了

	这种情景几种处理方法
	1、使用git checkout，撤销到git commit之前，来到git add之后
	2、使用版本回退 git reset --hard commit_ID，回退到上一版本

### 小结
Git撤销
git checkout -- filename
没有commit使用git checkout命令撤销，git checkout撤销总是将文件回到最近一次git commit或者是git add时的状态

## 删除文件
Git中删除文件有两种选择

1. 直接使用rm删除
2. 使用git rm删除并且 git commit


**注意：误删可以用 git checkout -- filename 恢复文件**
<pre>
[root@localhost test]# git status
# On branch master
# Changes not staged for commit:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	deleted:    readme.txt
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	help
#	readme
no changes added to commit (use "git add" and/or "git commit -a")

</pre>
如上面使用rm 删除，再使用git status查看就能看到 deleted： readme的提示

## 远程仓库
当你在本地端创建一个Git仓库后，我们再在Git Hub创建一个Git仓库，这样让两个仓库保持远程同步就能够实现让GitHub上作为备份而且又可以让其他人通过仓库协作。
### 添加远程仓库
1. 创建一个Git仓库，首先登入GitHub，然后，在右上角找到“New repository” -->输入仓库名-->Create repository
2. 在本地主机上生成ssh key密钥对，将公钥复制到GitHub里
3. 我们可以将这个Git仓库克隆也可以将本地的仓库与之关联，下面我们将本地仓库与之关联，然后将本地仓库内容推送到GitHub仓库
<pre>
[root@localhost git]# git remote add origin git@github.com:zyzfzb/git.git
</pre>
说明：origin 是远程库的名字，是Git默认的叫法，也可以修改。
后面的就是你的Git远程库的地址了，这个是SSH推送用的地址，还有一个是HTTPS的访问地址，了解下。

<pre>
[root@localhost ~]# cd .ssh/
[root@localhost .ssh]# ls -ah
.  ..  known_hosts

[root@localhost .ssh]# ssh-keygen -t rsa  #连续三下回车
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:xqy5qVoPYljN4+wHLr40nje289LrevieJrudlC9F5WU root@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|          . E    |
|         o o     |
|   o   o. .      |
|  . +  .S        |
| o o.. =.        |
|. =.=+=.         |
| +.*X=**         |
| .**B^&o.        |
+----[SHA256]-----+
[root@localhost .ssh]# ls -a
.  ..  id_rsa  id_rsa.pub  known_hosts

[root@localhost .ssh]# cat id_rsa.pub  

将下面的公钥复制到GitHub(右上角-->settings-->SSH and GPG keys-->New SSH Keys-->然后复制到Key框里即可)
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNnhj6eNZA97+qtQ3NJc29WiTusbGKth+6rH+Dae/xuhv//hEr+ZM1ZU5/1cPDnRvTMXfTCNyyi21eEQNKjCryyEoDzPrx4JNrjaKBLH3OjJDd80s+cvWrcphw5qCSHKymAwatWdVvbTzXJHfIN2+pXszQqh1bFTmlHmO9tDBByR8RRuu2A2VUGjzMQLKh9X97fDPP8P2HyoGaJE6gajnQz3EqUbV+NefEqnPp/ki+VImt1erdjCtpEEIpHGUPHsUornBR3PHRY+v5nArwSlQHxHi4b/vB81QFqlr5oLTmML5O4lGJy7v0FOUDJ9bec8EvIKC9dK6DaHZMnNzMTqjB root@l


</pre>

<pre>
[root@localhost git]# git push -u origin master
Counting objects: 9, done.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (9/9), 834 bytes | 0 bytes/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To git@github.com:zyzfzb/git.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
[root@localhost git]# 

</pre>
