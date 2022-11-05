---
title: git总结
date: 2022-10-26 14:10:24
tags:
- git
-  尚硅谷
categories: 
- 视频学习笔记  
---

[TOC]



# 版本控制工具应该具备的功能

1、协同修改

​	 n 多人并行不悖的修改服务器端的同一个文件【你改你的，我改我的】

2、数据备份

​	不仅保存**目录和文件**的**当前状态**，还能够保存每一个提交过的**历史状态**。

3、版本管理

​	n 在保存每一个版本的文件信息的时候要做到不保存重复数据，以节约存储空间，提高运行效率。

​	这方面 SVN 采用的是**增量式管理**的方式，而 Git 采取了文件**系统快照**的方式。

4、权限控制

​	n 对团队中参与开发的人员进行权限控制。

​	n 对**团队外开发者**贡献的代码进行审核——Git 独有。

5、历史记录

​	n 查看修改人、修改时间、修改内容、日志信息。

​	n 将本地文件恢复到某一个历史状态。

6、分支管理

​	n 允许开发团队在工作过程中多条生产线同时推进任务，进一步提高效率。



# 版本控制简介



## 版本控制

工程设计领域中使用版本控制管理工程蓝图的设计过程。在 IT 开发过程中也可以使用版本控制思想管理代码的版本迭代





## 版本控制工具

思想：版本控制

实现：版本控制工具

 

集中式版本控制工具：

CVS、SVN、VSS……



---

![image-20220617231352365](http://img.fgcy.xyz/image-20220617231352365.png)

---



集中式版本控制工具有单点故障的风险：即服务器一旦宕机机会丢失全部版本信息【只有服务器上有完整的版本信息，当前机器上最多有当前版本】





分布式版本控制工具：

Git、Mercurial、Bazaar、Darcs

---

**![image-20220617231717843](http://img.fgcy.xyz/image-20220617231717843.png)**

---

避免单点故障风险：每一台主机上都有完整的版本信息





# Git简介

## Git 简史

---

![image-20220617232412626](http://img.fgcy.xyz/image-20220617232412626.png)

---



## Git 官网和 Logo

> 官网地址：https://git-scm.com/



---

![image-20220617232535553](http://img.fgcy.xyz/image-20220617232535553.png)

---



## Git 的优势

1、大部分操作在本地完成，不需要联网
2、完整性保证【通过对比hash值来确定文件是否更改，是否丢失】
3、**尽可能添加数据**而不是**删除或修改数据**
4、分支操作非常快捷流畅
5、与 Linux 命令全面兼容





## Git 安装



---

![image-20220617233104595](http://img.fgcy.xyz/image-20220617233104595.png)

---





---

![image-20220617233152618](http://img.fgcy.xyz/image-20220617233152618.png)

---



---

![image-20220617233219227](http://img.fgcy.xyz/image-20220617233219227.png)

---



---

![image-20220617234028587](http://img.fgcy.xyz/image-20220617234028587.png)

---





---

![image-20220617234221154](http://img.fgcy.xyz/image-20220617234221154.png)

---



---

![image-20220617234240042](http://img.fgcy.xyz/image-20220617234240042.png)

---



---

![image-20220617234253546](http://img.fgcy.xyz/image-20220617234253546.png)

---





---

![image-20220617234419961](http://img.fgcy.xyz/image-20220617234419961.png)

---



## Git 结构



---

![image-20220617234640290](http://img.fgcy.xyz/image-20220617234640290.png)

---





## Git 和代码托管中心

代码托管中心的任务：维护远程库

1、局域网环境下

​			GitLab 服务器

2、外网环境下
		GitHub
		Gitee【码云】







## 本地库和远程库



团队内部协作

---

![image-20220617235415430](http://img.fgcy.xyz/image-20220617235415430.png)

---





跨团队协作

---

![image-20220617235449698](http://img.fgcy.xyz/image-20220617235449698.png)

---



# 解决Git中文乱码问题

~~~shell
git config --global core.quotepath false
git config --global i18n.commit.encoding utf-8
git config --global i18n.logoutputencoding utf-8
export LESSCHARSET=utf-8
~~~





# Git 命令行操作



## 本地库初始化

> git init



~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料
$ ll
total 15021
-rw-r--r-- 1 fgcy 197121 3978870 Jan 17 11:25 'Git&GitHub.bmp'
-rw-r--r-- 1 fgcy 197121 3583371 Jan 17 11:25 'Git&GitHub.pdf'
-rw-r--r-- 1 fgcy 197121 7792408 Jun 17 22:56 'Git&GitHub_.docx'
drwxr-xr-x 1 fgcy 197121       0 Jun 17 23:54  git总结.assets/
-rw-r--r-- 1 fgcy 197121    8457 Jun 18 00:00  git总结.md
-rw-r--r-- 1 fgcy 197121     162 Jun 17 23:03 '~$t&GitHub_.docx'

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 #当前目录
~~~



~~~shell
$ git init
Initialized empty Git repository in D:/learn/尚硅谷/Git&github学习资料/.git/

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master) #当前目录【主分支】
~~~



> ls -la    查看所有文件包括隐藏文件【.开头的文件】

~~~shell
$ ls -la                                    #查看所有文件包括隐藏文件【.开头的文件】
total 15033
drwxr-xr-x 1 fgcy 197121       0 Jun 18 00:03  ./
drwxr-xr-x 1 fgcy 197121       0 Apr 23 16:42  ../
drwxr-xr-x 1 fgcy 197121       0 Jun 18 00:01  .git/
-rw-r--r-- 1 fgcy 197121 3978870 Jan 17 11:25 'Git&GitHub.bmp'
-rw-r--r-- 1 fgcy 197121 3583371 Jan 17 11:25 'Git&GitHub.pdf'
-rw-r--r-- 1 fgcy 197121 7792408 Jun 17 22:56 'Git&GitHub_.docx'
drwxr-xr-x 1 fgcy 197121       0 Jun 17 23:54  git总结.assets/
-rw-r--r-- 1 fgcy 197121    8757 Jun 18 00:03  git总结.md
-rw-r--r-- 1 fgcy 197121     162 Jun 17 23:03 '~$t&GitHub_.docx'

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
~~~



> ll    查看git目录下的文件

~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ ll .git                                #查看git目录下的文件
total 7
-rw-r--r-- 1 fgcy 197121  23 Jun 18 00:01 HEAD
-rw-r--r-- 1 fgcy 197121 130 Jun 18 00:01 config
-rw-r--r-- 1 fgcy 197121  73 Jun 18 00:01 description
drwxr-xr-x 1 fgcy 197121   0 Jun 18 00:01 hooks/
drwxr-xr-x 1 fgcy 197121   0 Jun 18 00:01 info/
drwxr-xr-x 1 fgcy 197121   0 Jun 18 00:01 objects/
drwxr-xr-x 1 fgcy 197121   0 Jun 18 00:01 refs/
~~~

注意：

.git 目录中存放的是  **本地库**相关的子目录和文件，不要删除，也不要胡乱修改。



## 设置签名

作用：区分不同开发人员的身份

辨析：这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系。



+ 命令
  项目级别/仓库级别：仅在**当前本地库范围**内有效

​		信息保存位置：./.git/config  文件

~~~shell
git config user.name fgcy
git config user.email fgcy@2000.com
~~~



实例：

~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git config user.name swx

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git config user.email swx@learn_git.com

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ cat .git/config
[core]
        repositoryformatversion = 0
        filemode = false
        bare = false
        logallrefupdates = true
        symlinks = false
        ignorecase = true
[user]
        name = swx
        email = swx@learn_git.com

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
~~~



+ 系统用户级别：登录当前操作系统的用户范围

~~~shell
git config --global user.name swx
git config --global user.email swx@learn_git.com
~~~

信息保存位置：~/.gitconfig  文件



~~~shell
$ git config --global user.name fgcy
fgcy@fgcy MINGW64 ~

fgcy@fgcy MINGW64 ~
$ git config --global user.email fgcy@2000.com

fgcy@fgcy MINGW64 ~
~~~



~~~shell
total 18371
drwxr-xr-x 1 fgcy 197121       0 Jun 18 00:21 ./
drwxr-xr-x 1 fgcy 197121       0 Mar 16 21:50 ../
-rw-r--r-- 1 fgcy 197121      96 Jun 18 00:21 .gitconfig
-rw-r--r-- 1 fgcy 197121       0 Jan 15 20:05 .mongorc.js
drwxr-xr-x 1 fgcy 197121       0 Apr  1 00:01 .npminstall_tarball/
drwxr-xr-x 1 fgcy 197121       0 May 27 10:43 .picgo/
drwx
(END)

~~~



查看家目录

~~~shell
$ cd ~

fgcy@fgcy MINGW64 ~
$ pwd
/c/Users/fgcy
~~~

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              

~~~shell
$ cat .gitconfig
[user]
        name = fgcy
        email = fgcy@2000.com
[credential "https://gitee.com"]
        provider = generic

~~~



+ 级别优先级
  就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名
  如果只有系统用户级别的签名，就以系统用户级别的签名为准
  二者都没有不允许







# Git 命令行操作之基本操作

状态查看

> git status   



添加

将工作区的“新建/修改”添加到暂存区

> git add [file name]   



提交

将暂存区的内容提交到本地库

> git commit -m "commit message" [file name]



查看历史记录

> git log





## 查看工作区、暂存区状态

~~~shell
$ git status
On branch master												#当前在主分支上

No commits yet													#在当前本机上的版本库中没有任何提交

Untracked files:												 #未被追踪的文件【只在工作区，未添加到缓存区】
  (use "git add <file>..." to include in what will be committed) # 将文件添加到暂存区
        Git&GitHub.bmp                                           #红色的字
        Git&GitHub.pdf											 #红色的字
        "git\346\200\273\347\273\223.assets/"					 #红色的字
        "git\346\200\273\347\273\223.md"						 #红色的字
        note_for_git.docx										 #红色的字

nothing added to commit but untracked files present (use "git add" to track)
~~~



## 将文件添加到暂存区

~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git add  note_for_git.docx             						# 将文件添加到暂存区
~~~



查看工作区、暂存区状态

~~~shell
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)					 #从暂存区移除
        new file:   note_for_git.docx							 #一个新的文件被添加到缓存区  绿色的字

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp											 #红色的字
        Git&GitHub.pdf											 #红色的字
        "git\346\200\273\347\273\223.assets/"					 #红色的字
        "git\346\200\273\347\273\223.md"						 #红色的字

~~~



## 将暂存区的文件移除

~~~shell
$ git rm --cached note_for_git.docx                    #将暂存区的文件移除【文件从此以后不被追踪】
rm 'note_for_git.docx'

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
      Git&GitHub.bmp                                             #红色的字
        Git&GitHub.pdf											 #红色的字
        "git\346\200\273\347\273\223.assets/"					 #红色的字
        "git\346\200\273\347\273\223.md"						 #红色的字
        note_for_git.docx										 #红色的字

nothing added to commit but untracked files present (use "git add" to track)
~~~



## 将暂存区的文件添加到版本库

~~~shell
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        note_for_git.docx

nothing added to commit but untracked files present (use "git add" to track)

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git add note_for_git.docx

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git commit note_for_git.docx
~~~



输入添加注释

~~~shell
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   note_for_git.docx
#
# Untracked files:
#       Git&GitHub.bmp
#       Git&GitHub.pdf
#       "git\346\200\273\347\273\223.assets/"
#       "git\34
~~~



vim编辑器下

> set nu    显示行号



修改第一行

~~~shell
this is the commit comment for commit the note_for_git.doc
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   note_for_git.docx
#
# Untracked files:
#       Git&GitHub.bmp
#       Git&GitHub.pdf
#       "git\346\200\273\347\273\223.assets/"
#       "git\34
~~~



提交到版本库

~~~shell
[master (root-commit) 959b098] this is the commit comment for commit the note_for_git.doc
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 note_for_git.docx
~~~



查看提交到版本库后状态

~~~shell
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"

nothing added to commit but untracked files present (use "git add" to track)
~~~





## 将note_for_git.doc文件修改后

~~~shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   note_for_git.docx

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

no changes added to commit (use "git add" and/or "git commit -a")
~~~



将note_for_git.doc文件修改后添加到暂存区【也可以直接添加到版本库】

~~~shell
$ git add note_for_git.docx

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)       #将文件从暂存区中撤出
        modified:   note_for_git.docx

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

~~~



提交到版本库

~~~shell
$ git commit -m "note_for_git.doc second commit" note_for_git.docx
[master 6a297c1] note_for_git.doc second commit
 1 file changed, 0 insertions(+), 0 deletions(-
~~~



## Git 添加多个文件到暂存区

方法一：

> git add file1 file2 file3 file 4



方法二：

> git add file1
>
> git add file2
>
> git add file3



方法三：

添加指定目录下的文件config目录下及子目录下所有文件,

> git add config/*



home目录下的所有.php文件

> git add home/*.php



方法四：

添加所有文件

> git add .

> git add --all



方法五：

添加文件夹

> git add 文件夹名字



## Git 提交多个文件到版本库

方法一：

> git commit file1 file2 file3 -m "comment"



方法二：

提交所有在暂存区中的文件

> git commit -m "comment"



## 查看历史记录



> git log



~~~shell
$ git log
commit 6a297c1ee3d2163740be7a58ead809ec65808a30 (HEAD -> master)   	#HEAD---》指向当前版本库中的版本
Author: swx <swx@learn_git.com>										# 项目级别配置
Date:   Sat Jun 18 10:22:07 2022 +0800

    note_for_git.doc second commit

commit 959b09820e76e212a49fce3db723e9e3dfa2c9c5
Author: swx <swx@learn_git.com>
Date:   Sat Jun 18 10:01:09 2022 +0800

    this is the commit comment for commit the note_for_git.doc

~~~





多屏显示控制方式：

空格向下翻页 

b 向上翻页

q 退出





> git log --pretty=oneline

~~~shell
$ git log --pretty=oneline
6a297c1ee3d2163740be7a58ead809ec65808a30 (HEAD -> master) note_for_git.doc second commit
959b09820e76e212a49fce3db723e9e3dfa2c9c5 this is the commit comment for commit the note_for_git.doc

~~~





> git log --oneline   只能看到当前所在版本之前的

~~~shell
$ git log --oneline
6a297c1 (HEAD -> master) note_for_git.doc second commit
959b098 this is the commit comment for commit the note_for_git.doc

~~~





> git reflog       最常用

~~~shell
$ git reflog
6a297c1 (HEAD -> master) HEAD@{0}: commit: note_for_git.doc second commit
959b098 HEAD@{1}: commit (initial): this is the commit comment for commit the note_for_git.doc

~~~

HEAD@{移动到该版本需要多少步}





## 版本穿梭【前进后退】



本质：HEAD指针移动

---

![image-20220618105844245](http://img.fgcy.xyz/image-20220618105844245.png)

---



- 基于索引值操作[推荐]

不用考虑是前进还是后退

> git reset --hard [局部索引值]

~~~shell
$ git reflog
b2c89a7 (HEAD -> master) HEAD@{0}: commit: fifth commit for note_for_git.doc
abfb1c4 HEAD@{1}: commit: fourth commit for note_for_git.doc
68c5839 HEAD@{2}: commit: third commit for note_for_git.doc
6a297c1 HEAD@{3}: commit: note_for_git.doc second commit
959b098 HEAD@{4}: commit (initial): this is the commit comment for commit the note_for_git.doc

$ git reset --hard 959b098
HEAD is now at 959b098 this is the commit comment for commit the note_for_git.doc

$ git reflog
959b098 (HEAD -> master) HEAD@{0}: reset: moving to 959b098
b2c89a7 HEAD@{1}: commit: fifth commit for note_for_git.doc
abfb1c4 HEAD@{2}: commit: fourth commit for note_for_git.doc
68c5839 HEAD@{3}: commit: third commit for note_for_git.doc
6a297c1 HEAD@{4}: commit: note_for_git.doc second commit
959b098 (HEAD -> master) HEAD@{5}: commit (initial): this is the commit comment for commit the note_for_git.doc

~~~





- 使用^符号：只能后退

> git reset --hard HEAD^ 





~~~shell
$ git log --oneline
b2c89a7 (HEAD -> master) fifth commit for note_for_git.doc
abfb1c4 fourth commit for note_for_git.doc
68c5839 third commit for note_for_git.doc
6a297c1 note_for_git.doc second commit
959b098 this is the commit comment for commit the note_for_git.doc

~~~

~~~shell
$ git reset --hard HEAD^
HEAD is now at abfb1c4 fourth commit for note_for_git.doc

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git log --oneline
abfb1c4 (HEAD -> master) fourth commit for note_for_git.doc
68c5839 third commit for note_for_git.doc
6a297c1 note_for_git.doc second commit
959b098 this is the commit comment for commit the note_for_git.doc

~~~



~~~shell
$ git reset --hard HEAD^^
HEAD is now at 6a297c1 note_for_git.doc second commit

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git log --oneline
6a297c1 (HEAD -> master) note_for_git.doc second commit
959b098 this is the commit comment for commit the note_for_git.doc

~~~

注：一个^表示后退一步，n 个表示后退 n 步



- 使用~符号：只能后退

> git reset --hard HEAD~n

注：表示后退 n 步



~~~shell
$ git reset --hard HEAD~1
HEAD is now at 959b098 this is the commit comment for commit the note_for_git.doc

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git log --oneline
959b098 (HEAD -> master) this is the commit comment for commit the note_for_git.doc

~~~



## reset 命令的三个参数对比

+  --soft 参数

​		仅仅在本地库移动 HEAD 指针

---

![image-20220618112214470](http://img.fgcy.xyz/image-20220618112214470.png)

---



+ --mixed 参数

​		 在本地库移动 HEAD 指针

​		重置暂存区

---

![image-20220618112353918](http://img.fgcy.xyz/image-20220618112353918.png)

---





+ --hard 参数

​		在本地库移动 HEAD 指针

​		重置暂存区

​		重置工作区



---

![image-20220618112430982](http://img.fgcy.xyz/image-20220618112430982.png)

---



使用了--mixed参数后

使用

> git reset --hard HEAD  移动到当前指针指向的版本







## 删除文件并找回

前提：删除前，文件存在时的状态提交到了本地库。

> 操作：git reset --hard [指针位置]



删除操作已经提交到本地库：指针位置指向历史记录
删除操作尚未提交到本地库：指针位置使用 HEAD





添加一个文件【本地库】

~~~shell
$ vim aaa.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git add aaa.txt
warning: LF will be replaced by CRLF in aaa.txt.
The file will have its original line endings in your working directory

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git commit -m "new aaa.text" aaa.txt
warning: LF will be replaced by CRLF in aaa.txt.
The file will have its original line endings in your working directory
[master 1beefc9] new aaa.text
 1 file changed, 2 insertions(+)
 create mode 100644 aaa.txt

~~~



在工作区删除文件

~~~shell
$ ll
total 13466
-rw-r--r-- 1 fgcy 197121 3978870 Jan 17 11:25 'Git&GitHub.bmp'
-rw-r--r-- 1 fgcy 197121 3583371 Jan 17 11:25 'Git&GitHub.pdf'
-rw-r--r-- 1 fgcy 197121      49 Jun 18 11:30  aaa.txt
drwxr-xr-x 1 fgcy 197121       0 Jun 18 11:24  git总结.assets/
-rw-r--r-- 1 fgcy 197121   24992 Jun 18 11:32  git总结.md
-rw-r--r-- 1 fgcy 197121 6182824 Jun 18 11:28  note_for_git.docx
-rw-r--r-- 1 fgcy 197121     162 Jun 18 11:29 '~$te_for_git.docx'

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ rm -rf aaa.txt

~~~



查看状态

~~~shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    aaa.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

no changes added to commit (use "git add" and/or "git commit -a")
~~~



将删除aaa.txt文件的记录添加到版本库中

~~~shell
$ git add aaa.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git commit -m "delete aaa.txt" aaa.txt
[master 9580539] delete aaa.txt
 1 file changed, 2 deletions(-)
 delete mode 100644 aaa.txt

~~~



查看状态

~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

nothing added to commit but untracked files present (use "git add" to track)
~~~



---

![image-20220618113855583](http://img.fgcy.xyz/image-20220618113855583.png)

---



版本回退



~~~shell
$ git reflog
9580539 (HEAD -> master) HEAD@{0}: commit: delete aaa.txt
1beefc9 HEAD@{1}: commit: new aaa.text
b2c89a7 HEAD@{2}: reset: moving to b2c89a7
959b098 HEAD@{3}: reset: moving to HEAD~1
6a297c1 HEAD@{4}: reset: moving to HEAD^^
abfb1c4 HEAD@{5}: reset: moving to HEAD^
b2c89a7 HEAD@{6}: reset: moving to b2c89a7
959b098 HEAD@{7}: reset: moving to 959b098
959b098 HEAD@{8}: reset: moving to 959b098
959b098 HEAD@{9}: reset: moving to 959b098
959b098 HEAD@{10}: reset: moving to 959b098
b2c89a7 HEAD@{11}: commit: fifth commit for note_for_git.doc
abfb1c4 HEAD@{12}: commit: fourth commit for note_for_git.doc
68c5839 HEAD@{13}: commit: third commit for note_for_git.doc
6a297c1 HEAD@{14}: commit: note_for_git.doc second commit
959b098 HEAD@{15}: commit (initial): this is the commit comment for commit the note_for_git.doc

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git reset --hard 1beefc9
HEAD is now at 1beefc9 new aaa.text

~~~



---

![image-20220618113944206](http://img.fgcy.xyz/image-20220618113944206.png)

---



~~~shell
$ cat aaa.txt
aaaaaaaaaaaaaaaaaaaaaa
bbbbbbbbbbbbbbbbbbbbbbbbb

~~~





## 比较文件差异

> git diff [文件名]

将工作区中的文件和暂存区进行比较



1、新建一个文件apple.txt

~~~shell
$ vim apple.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)

$ cat apple.txt
aaaaaaaaaaaaaaaaaaa
bbbbbbbbbbbbbbbbbbbb
cccccccccccccccccccc
dddddddddddddd
eeeeeeeeeeee

~~~

2、将apple.txt文件添加到暂存区

3、修改apple.txt文件 加一行fffffffffffffffffff

4、比较暂存区和工作区的apple.txt文件区别

~~~shell
$ git diff apple.txt
warning: LF will be replaced by CRLF in apple.txt.
The file will have its original line endings in your working directory
diff --git a/apple.txt b/apple.txt
index a5d954f..6ba728d 100644
--- a/apple.txt
+++ b/apple.txt
@@ -3,3 +3,4 @@ bbbbbbbbbbbbbbbbbbbb
 cccccccccccccccccccc
 dddddddddddddd
 eeeeeeeeeeee
+ffffffffffffffffff

~~~

---



>  git diff [本地库中历史版本] [文件名]

将工作区中的文件和本地库历史记录比较



~~~shell
$ git diff HEAD^ apple.txt
warning: LF will be replaced by CRLF in apple.txt.
The file will have its original line endings in your working directory
diff --git a/apple.txt b/apple.txt
new file mode 100644
index 0000000..6ba728d
--- /dev/null
+++ b/apple.txt
@@ -0,0 +1,6 @@
+aaaaaaaaaaaaaaaaaaa
+bbbbbbbbbbbbbbbbbbbb
+cccccccccccccccccccc
+dddddddddddddd
+eeeeeeeeeeee
+ffffffffffffffffff

~~~









不带文件名比较多个文件【比较当前工作区和指定版本库中文件】

1、当前版本中没有apple.txt文件

2、 修改aaa.txt文件



~~~shell
$ git diff HEAD
warning: LF will be replaced by CRLF in apple.txt.
The file will have its original line endings in your working directory
diff --git a/aaa.txt b/aaa.txt
index f4dd32e..8aa184e 100644
--- a/aaa.txt
+++ b/aaa.txt
@@ -1,2 +1,5 @@
 aaaaaaaaaaaaaaaaaaaaaa
-bbbbbbbbbbbbbbbbbbbbbbbbb
+bbbbbbbbbbbbbbbbbbbbbbbb
+ccccccccccccccccccccccccc
+dddddddddddddddddddd
+b
diff --git a/apple.txt b/apple.txt
new file mode 100644
index 0000000..6ba728d
--- /dev/null
+++ b/apple.txt
@@ -0,0 +1,6 @@
+aaaaaaaaaaaaaaaaaaa
+bbbbbbbbbbbbbbbbbbbb
+cccccccccccccccccccc
+dddddddddddddd
+eeeeeeeeeeee
+ffffffffffffffffff

~~~





# 分支管理

## 什么是分支？

在版本控制过程中，使用多条线同时推进多个任务



---

![image-20220618132917353](http://img.fgcy.xyz/image-20220618132917353.png)

---





## 分支的好处？

1、同时并行推进多个功能开发，提高开发效率
2、各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可







## 分支操作

查看分支

> git branch -v

~~~shell
$ git branch -v
* master 1beefc9 new aaa.text    #只有一个分支，且正处于该分支上

~~~



创建分支

> git branch [分支名]

~~~shell
$ git branch hot_fix

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git branch -v
  hot_fix 1beefc9 new aaa.text
* master  1beefc9 new aaa.text						#有两个分支，且正处于master分支上

~~~



切换分支

> git checkout [分支名]

~~~shell
$ git checkout hot_fix
Switched to branch 'hot_fix'               #切换到hot_fix分支
M       aaa.txt
A       apple.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (hot_fix)
$ git branch -v
* hot_fix 1beefc9 new aaa.text						#有两个分支，且正处于hot_fix分支上
  master  1beefc9 new aaa.text

~~~



合并分支

> git merge 分支名

这个分支名是想要合并到当前分支的分支的名字



步骤：

1、切换到hot_fix分支上

2、修改apple.txt文件

3、提交到本地库

4、查看分支

~~~shell
$ git branch -v
* hot_fix 1dd3fe0 test branch hot_fix                #hot_fix是绿色的；说明hot_fix已经向前推进了一步
  master  1beefc9 new aaa.text

~~~

5、切换到主分支上，既是接收修改的分支



6、执行合并操作

~~~shell
$ git merge hot_fix
Updating 1beefc9..1dd3fe0
Fast-forward
 apple.txt | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 apple.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)

~~~

7、查看新文件

~~~shell
$ cat apple.txt
aaaaaaaaaaaaaaaaaaa
bbbbbbbbbbbbbbbbbbbb
cccccccccccccccccccc
dddddddddddddd
eeeeeeeeeeee
ffffffffffffffffff    edit by hot_fix
~~~





## 解决冲突

在多个不同分支上修改同一个文件的同一行



1、在master分支上



冲突的表现

~~~shell
$ git merge hot_fix
Auto-merging aaa.txt
CONFLICT (content): Merge conflict in aaa.txt
Automatic merge failed; fix conflicts and then commit the result.
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master|MERGING)

~~~



~~~shell
$ cat aaa.txt
aaaaaaaaaaaaaaaaaaaaaa
bbbbbbbbbbbbbbbbbbbbbbbb
ccccccccccccccccccccccccc
dddddddddddddddddddd
<<<<<<< HEAD
b edit by master
=======
b edit by hot_fix

>>>>>>> hot_fix

~~~



---

![image-20220618141934643](http://img.fgcy.xyz/image-20220618141934643.png)

---



~~~shell
aaaaaaaaaaaaaaaaaaaaaa
bbbbbbbbbbbbbbbbbbbbbbbb
ccccccccccccccccccccccccc
dddddddddddddddddddd
# <<<<<<< HEAD                           删除特殊符号 解决冲突
b edit by master
# =======
# b edit by hot_fix

#>>>>>>> hot_fix

~~~



~~~shell
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)             #终止合并

Unmerged paths:
  (use "git add <file>..." to mark resolution)             #标记为已解决
        both modified:   aaa.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

no changes added to commit (use "git add" and/or "git commit -a")

~~~



~~~shell
$ git add aaa.txt

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master|MERGING)
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)           #此时已解决冲突，需要结束合并

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

~~~



注意：结束合并时不能带文件名；

~~~shell
$ git commit -m "xxx" aaa.txt
fatal: cannot do a partial commit during a merge.

~~~



~~~shell
$ git commit -m "resolve confilict"
[master d18042e] resolve confilict

~~~



~~~shell
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git&GitHub.bmp
        Git&GitHub.pdf
        "git\346\200\273\347\273\223.assets/"
        "git\346\200\273\347\273\223.md"
        ~$te_for_git.docx

nothing added to commit but untracked files present (use "git add" to track)

~~~



冲突的解决：
第一步：编辑文件，删除特殊符号
第二步：把文件修改到满意的程度，保存退出
第三步：git add [文件名]
第四步：git commit -m "日志信息"
注意：此时 commit 一定不能带具体文件名







# Git 基本原理



## 哈希



---

![image-20220618142954163](http://img.fgcy.xyz/image-20220618142954163.png)

---



哈希是一个系列的加密算法，各个不同的哈希算法虽然加密强度不同，但是有以下几个共同点：
①不管输入数据的数据量有多大，输入同一个哈希算法，**得到的加密结果长度固定**。
②**哈希算法确定**，**输入数据确定**，**输出数据能够保证不****变**
③哈希算法确定，**输入数据有变化**，**输出数据一定有变化**，而且通常变化很大
④哈希算法**不可逆**【不可由密文退出明文】
Git 底层采用的是 SHA-1 算法





哈希算法可以被用来验证文件。原理如下图所示：

---

![image-20220618143528516](http://img.fgcy.xyz/image-20220618143528516.png)

---



Git 就是靠这种机制来从根本上保证数据完整性的





## 保存版本的机制

- 集中式版本控制工具的文件管理机制：
  以文件变更列表的方式存储信息。这类系统将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异



---

![image-20220618143913452](http://img.fgcy.xyz/image-20220618143913452-16555350918013.png)

---





- Git 的文件管理机制
  Git 把数据看作是小型文件系统的一组快照。每次提交更新时 Git 都会对当前的全部文件制作一个快照并保存这个快照的索引

  为了高效，如果文件没有修改，  Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件

  所以 Git 的工作方式可以称之为快照流



---

![image-20220618145402107](http://img.fgcy.xyz/image-20220618145402107.png)

---







+ Git 文件管理机制细节
  Git 的“提交对象”

---

![image-20220618144434429](http://img.fgcy.xyz/image-20220618144434429.png)

---

1、每一个文件都进行hash取到hash值

2、生成一个树对象，里面包含所有文件的信息；它本身也有一个hash值

3、生成一个提交对象，提交对象中有一个树对象；它本身也有一个hash值



4、提交对象及其父对象形成的链条

---

![image-20220618145935765](http://img.fgcy.xyz/image-20220618145935765.png)

---

5、通过这个提交对象实现版本管理





## Git 分支管理机制

之前从来没有提交过；就是root commit





- 分支的创建

SVN会将所有的目录和文件都复制一份，给到这个分支





而Git会创建一个新的指针指向当前提交对象

---

![image-20220618150604655](http://img.fgcy.xyz/image-20220618150604655.png)

---







- Git分支的切换

HEAD指针指向新的分支指针；

表明现在的所有操作都是基于HEAD指针指向的分支所指向的提交对象

---

![image-20220618150802391](http://img.fgcy.xyz/image-20220618150802391.png)

---





在testing分支上提交一个版本，就会生成一个新的提交对象；

testing分支的指针指向新的提交对象；

HEAD指针【当前版本】，处于testing分支上；当前的所有操作都是在testing分支上进行的；

---

![image-20220618151044134](http://img.fgcy.xyz/image-20220618151044134.png)

---





- 将分支切换为master

仅仅需要移动HEAD指针；所以git的切换分支是非常快的

---

![image-20220618151623231](http://img.fgcy.xyz/image-20220618151623231.png)

---





master和testing分别在f30ab提交对象的基础上，提交了一次；

分别创建一个提交对象

---

![image-20220618152048936](http://img.fgcy.xyz/image-20220618152048936.png)

---







# GitHub

> GitHub 首页就是注册页面：https://github.com/



---

![image-20220618183045901](http://img.fgcy.xyz/image-20220618183045901.png)

---





---

![image-20220618183136212](http://img.fgcy.xyz/image-20220618183136212.png)

---





## 设置远程库地址别名

> git remote -v   查看当前所有远程地址别名

~~~shell
$ git remote -v

#没有的话，什么都不会显示
~~~



> git remote add [别名] [远程地址]    添加远程地址别名

~~~shell
$ git remote add origin https://github.com/fgcy-333/-_learn_git.git

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git remote -v
origin  https://github.com/fgcy-333/-_learn_git.git (fetch)
origin  https://github.com/fgcy-333/-_learn_git.git (push)

~~~



> git remote remove [别名]    删除远程地址别名

~~~shell
$ git remote remove origin

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git remote -v

~~~



## 仓库改名

---

![image-20220618185027793](http://img.fgcy.xyz/image-20220618185027793.png)

---







## 推送

> git push [别名] [分支名]



报错：

~~~shell
$ git push origin master
fatal: unable to access 'https://github.com/fgcy-333/-_learn_git.git/': SSL certificate problem: unable to get local issuer certificate

~~~



解决：

~~~shell
# 产生原因：一般是这是因为服务器的SSL证书没有经过第三方机构的签署，所以才报错

#解除ssl验证后
git config --global http.sslVerify "false"

#再次git即可
$ git push origin master
warning: ----------------- SECURITY WARNING ----------------
warning: | TLS certificate verification has been disabled! |
warning: ---------------------------------------------------
warning: HTTPS connections may not be secure. See https://aka.ms/gcm/tlsverify for more information.
warning: ----------------- SECURITY WARNING ----------------
warning: | TLS certificate verification has been disabled! |
warning: ---------------------------------------------------
warning: HTTPS connections may not be secure. See https://aka.ms/gcm/tlsverify for more information.
Enumerating objects: 31, done.
Counting objects: 100% (31/31), done.
Delta compression using up to 16 threads
Compressing objects: 100% (25/25), done.
Writing objects: 100% (31/31), 6.48 MiB | 4.23 MiB/s, done.
Total 31 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (5/5), done.
To https://github.com/fgcy-333/-_learn_git.git
 * [new branch]      master -> master

~~~



期间需要登录一次github进行授权

---

![image-20220618183957528](http://img.fgcy.xyz/image-20220618183957528.png)

---





输入一次账号密码后，就不用再输入；

因为window会记住密码；

如果要在同一台机器上切换账号需要先删除凭证；

---

![image-20220618191003778](http://img.fgcy.xyz/image-20220618191003778.png)

---



---

![image-20220618191026444](http://img.fgcy.xyz/image-20220618191026444.png)

---





## 克隆

> git origin [远程地址]



---

![image-20220618184407742](http://img.fgcy.xyz/image-20220618184407742.png)

---





~~~shell
$ git clone https://github.com/fgcy-333/-_learn_git.git
Cloning into '-_learn_git'...
remote: Enumerating objects: 31, done.
remote: Counting objects: 100% (31/31), done.
remote: Compressing objects: 100% (20/20), done.
remote: Total 31 (delta 5), reused 31 (delta 5), pack-reused 0
Receiving objects: 100% (31/31), 6.48 MiB | 2.58 MiB/s, done.
Resolving deltas: 100% (5/5), done.

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git相关资料

~~~



~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git相关资料
$ ll
total 145289
drwxr-xr-x 1 fgcy 197121         0 Jun 18 18:44 -_learn_git/
-rw-r--r-- 1 fgcy 197121 148765327 Jan 17 10:39 MarkdownPad+注册机+Awesomium_sdk.rar
-rw-r--r-- 1 fgcy 197121        86 Jan 17 10:35 打开.md文件.txt
drwxr-xr-x 1 fgcy 197121         0 Jan 17 10:36 笔记/
drwxr-xr-x 1 fgcy 197121         0 Jan 17 10:35 课件/


~~~



## 克隆远程库有三个效果

1、完整的把远程库下载到本地
2、创建 origin 远程地址别名
3、初始化本地库





## 团队成员邀请



----

![image-20220618190610280](http://img.fgcy.xyz/image-20220618190610280.png)

---



---

![image-20220618190714279](http://img.fgcy.xyz/image-20220618190714279.png)

---





## 拉取

> pull=fetch+merge



> git fetch [远程库地址别名] [远程分支名]        拉取远程分支内容



~~~shell
$ git fetch origin master
remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 7 (delta 1), reused 7 (delta 1), pack-reused 0
Unpacking objects: 100% (7/7), 10.13 KiB | 864.00 KiB/s, done.
From https://github.com/fgcy-333/note_learn_git
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master

~~~





远程仓库中的内容：

---

![image-20220618191821586](http://img.fgcy.xyz/image-20220618191821586.png)

---



拉取后，工作区中的目录结构：

~~~shell
fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ ll
total 13491
-rw-r--r-- 1 fgcy 197121 3978870 Jan 17 11:25 'Git&GitHub.bmp'
-rw-r--r-- 1 fgcy 197121 3583371 Jan 17 11:25 'Git&GitHub.pdf'
-rw-r--r-- 1 fgcy 197121     117 Jun 18 14:20  aaa.txt
-rw-r--r-- 1 fgcy 197121     134 Jun 18 13:43  apple.txt
drwxr-xr-x 1 fgcy 197121       0 Jun 18 18:50  git总结.assets/
-rw-r--r-- 1 fgcy 197121   40530 Jun 18 18:55  git总结.md
-rw-r--r-- 1 fgcy 197121 6182824 Jun 18 11:28  note_for_git.docx
-rw-r--r-- 1 fgcy 197121     162 Jun 18 11:29 '~$te_for_git.docx'
~~~



想要访问拉取下来的内容想要切换到origin/master分支

~~~shell
$ git checkout origin/master

$ ll
total 13489
-rw-r--r-- 1 fgcy 197121 3978870 Jan 17 11:25 'Git&GitHub.bmp'
-rw-r--r-- 1 fgcy 197121 3583371 Jan 17 11:25 'Git&GitHub.pdf'
drwxr-xr-x 1 fgcy 197121       0 Jun 18 18:50  git总结.assets/
-rw-r--r-- 1 fgcy 197121   40530 Jun 18 19:21  git总结.md                #这个是github上的
-rw-r--r-- 1 fgcy 197121 6182824 Jun 18 11:28  note_for_git.docx		#这个是github上的
-rw-r--r-- 1 fgcy 197121     162 Jun 18 11:29 '~$te_for_git.docx'


~~~



> **git merge [远程库地址别名/远程分支名]**     合并远程分支

切换到master分支上

~~~shell
$ git merge origin/master
Updating d18042e..55f92f6
Fast-forward
 aaa.txt                          |    5 -
 apple.txt                        |    6 -
 "git\346\200\273\347\273\223.md" | 2088 ++++++++++++++++++++++++++++++++++++++
 3 files changed, 2088 insertions(+), 11 deletions(-)
 delete mode 100644 aaa.txt
 delete mode 100644 apple.txt
 create mode 100644 "git\346\200\273\347\273\223.md"


~~~





> git pull [远程库地址别名] [远程分支名]







要点
如果不是基于 GitHub 远程库的最新版所做的修改，不能推送，必须先拉取最新版本







## 跨团队协作



fork

---

![image-20220618194217405](http://img.fgcy.xyz/image-20220618194217405.png)

---





---

![image-20220618194251531](http://img.fgcy.xyz/image-20220618194251531.png)

---





修改完成后发送推送请求

---

![image-20220618194553116](http://img.fgcy.xyz/image-20220618194553116.png)

---



---

![image-20220618194619302](http://img.fgcy.xyz/image-20220618194619302.png)

---





对话

---

![image-20220618194832140](http://img.fgcy.xyz/image-20220618194832140.png)

---



---

![image-20220618194854012](http://img.fgcy.xyz/image-20220618194854012.png)

---



---

![image-20220618194926693](http://img.fgcy.xyz/image-20220618194926693.png)

---



审核代码

---

![image-20220618194956371](http://img.fgcy.xyz/image-20220618194956371.png)

---



​                                        





代码合并

---

![image-20220618195120221](http://img.fgcy.xyz/image-20220618195120221.png)

---



最后将远程库修改拉取到本地





## SSH 登录



 进入当前用户的家目录

>  cd ~



 删除.ssh 目录

>  rm -rvf .ssh



运行命令生成.ssh 密钥目录

> ssh-keygen -t rsa -C [登录github的邮箱]          [注意：这里C这个参数是大写的C]

~~~shell
$ cd ~

fgcy@fgcy MINGW64 ~
$ rm -rvf .ssh
removed '.ssh/known_hosts'
removed directory '.ssh'

fgcy@fgcy MINGW64 ~
$ ssh-keygen -t rsa -C zwj1061499050@126.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/fgcy/.ssh/id_rsa):
Created directory '/c/Users/fgcy/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/fgcy/.ssh/id_rsa
Your public key has been saved in /c/Users/fgcy/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:C/awNz8LrJpK6Rtk25kBGOBBae09h9A0QZB/ZhHsIWg zwj1061499050@126.com
The key's randomart image is:
+---[RSA 3072]----+
|=oo.B=o..        |
|.=.E o.+         |
|o.+ + + o        |
|   o = *         |
|  o . X S        |
| o + = * .       |
|  = + . B        |
| o . . o +.      |
|  +oo..   oo     |
+----[SHA256]-----+

~~~







 进入.ssh 目录查看文件列表

> cd .ssh
>
> ls -lF



查看 id_rsa.pub 文件内容

>   cat id_rsa.pub



~~~shell
$ cd .ssh/

fgcy@fgcy MINGW64 ~/.ssh
$ ll
total 5
-rw-r--r-- 1 fgcy 197121 2610 Jun 18 19:59 id_rsa
-rw-r--r-- 1 fgcy 197121  575 Jun 18 19:59 id_rsa.pub

fgcy@fgcy MINGW64 ~/.ssh
$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADbBmKS71IyBDR14ybEFMNQfK48yr9MAFKf1dnA60TF39U2b2WDvsQkqmSQ1IWbSkMfxHSscmPxlMfDPDj5zW1jeU0nQpdsHGJ77HQgfjMOiOjJCdnsrjfN+8k= zwj1061499050@126.com


~~~



 复制 id_rsa.pub 文件内容，登录 GitHub，点击用户头像→Settings→SSH and GPG keys

----

![image-20220618200430035](http://img.fgcy.xyz/image-20220618200430035.png)

----



 New SSH Key

---

![image-20220618200519617](http://img.fgcy.xyz/image-20220618200519617.png)

---



输入复制的密钥信息



回到 Git bash 创建远程地址别名

---

![image-20220618201141298](http://img.fgcy.xyz/image-20220618201141298.png)

---



~~~shell
$ git remote add origin_ssh git@github.com:fgcy-333/note_learn_git.git

fgcy@fgcy MINGW64 /d/learn/尚硅谷/Git&github学习资料 (master)
$ git remote -v
origin  https://github.com/fgcy-333/note_learn_git.git (fetch)
origin  https://github.com/fgcy-333/note_learn_git.git (push)
origin_ssh      git@github.com:fgcy-333/note_learn_git.git (fetch)
origin_ssh      git@github.com:fgcy-333/note_learn_git.git (push)

~~~

 推送文件

> git push origin_ssh master



报错：

~~~shell
$ git push origin_ssh master
ssh: connect to host github.com port 22: Connection refused
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

~~~



解决：

1、首先到 ipaddress 输入 github.com 查找到其IP地址

---

![image-20220618202537557](http://img.fgcy.xyz/image-20220618202537557.png)

---

2、将查到的IP地址和网址映射放到你的本地 hosts 文件中即可，例子：140.82.112.3 github.com
3、 windows 下 host 默认地址： C:\Windows\System32\drivers\etc



# IDEA 集成 Git



## 配置 Git 忽略文件 



IDEA 特定文件 

1、.idea

2、xxx.iml

【这些文件与集成开发工具相关；与项目无关】

---

![image-20220619103341111](http://img.fgcy.xyz/image-20220619103341111.png)

---

3、Maven 工程的 target 目录 





问题 1:为什么要忽略他们？ 
答：与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能够屏蔽 IDE 工具之
间的差异





问题 2：怎么忽略？ 
1）创建忽略规则文件 xxxx.ignore（前缀名随便起，建议是 git.ignore） 
这个文件的存放位置原则上在哪里都可以，为了便于让~/.gitconfig 文件引用，建议也放在用
户家目录下



1、定位到git的家目录

2、创建一个名为 git.ignore的文件

---

![image-20220619104230553](http://img.fgcy.xyz/image-20220619104230553.png)

---

3、编写git.ignore文件

~~~txt
# Compiled class file 
*.class 
 
# Log file 
*.log 
 
# BlueJ files 
*.ctxt 
 
# Mobile Tools for Java (J2ME) 
.mtj.tmp/ 
 
# Package Files # 
*.jar 
*.war 
*.nar 
*.ear 
*.zip 
*.tar.gz 
*.rar 
 
# virtual machine crash logs, 
hs_err_pid* 
 
.classpath 
.project 
.settings 
target 
.idea 
*.iml 

~~~

4、在.gitconfig 文件中引用忽略配置文件（此文件在 Windows 的家目录中）



---

![image-20220619104436822](http://img.fgcy.xyz/image-20220619104436822.png)

---

5、配置[core] excludefile

~~~txt
[user]
	name = fgcy
	email = fgcy@2000.com
[credential "https://gitee.com"]
	provider = generic
[http]
	sslVerify = false
[core] 
 excludesfile = C:/Users/fgcy/git.ignore 
~~~

注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”



## 在IDEA中定位Git程序

> Ctrl+Alt+S 进入设置

---

![image-20220619105123296](http://img.fgcy.xyz/image-20220619105123296.png)

---



## 初始化本地库 

VCS【version controller setting】

---

![image-20220619105642818](http://img.fgcy.xyz/image-20220619105642818.png)

---



---

![image-20220619105821983](http://img.fgcy.xyz/image-20220619105821983.png)

---



初始化完成

---

![image-20220619105935434](http://img.fgcy.xyz/image-20220619105935434.png)

---



## 清除初始化本地库

---

![image-20220619110059831](http://img.fgcy.xyz/image-20220619110059831.png)

---







## 添加到暂存区 

右键点击项目选择 Git -> Add 将项目添加到暂存区



以单个文件为例

---

![image-20220619110340947](http://img.fgcy.xyz/image-20220619110340947.png)

---



---

![image-20220619110540783](http://img.fgcy.xyz/image-20220619110540783.png)

---





---

![image-20220619110433027](http://img.fgcy.xyz/image-20220619110433027.png)

---







## 提交到本地库



---

![image-20220619110644105](http://img.fgcy.xyz/image-20220619110644105.png)

---



---

![image-20220619110751030](http://img.fgcy.xyz/image-20220619110751030.png)

---





整个项目添加到暂存区

---

![image-20220619110913769](http://img.fgcy.xyz/image-20220619110913769.png)

---





整个项目提交到本地库

---

![image-20220619111256602](http://img.fgcy.xyz/image-20220619111256602.png)

---







## 颜色



红色 : 未被追踪 【必须先添加到暂存区，再添加到本地库】

绿色 ：已经添加到暂存区【直接添加到本地库】

黑色 ：已经提交到版本库【无需做变更】

蓝色 ：被追踪过了，但修改了【既可以先添加到暂存区，再添加到本地库；又可以直接添加到本地库】







## 切换版本 



---

![image-20220619131522652](http://img.fgcy.xyz/image-20220619131522652.png)

---



---

![image-20220619131624427](http://img.fgcy.xyz/image-20220619131624427.png)

---





## 创建分支

1、方式一：

---

![image-20220619132157613](http://img.fgcy.xyz/image-20220619132157613.png)

---





方式二：

---

![image-20220619132251885](http://img.fgcy.xyz/image-20220619132251885.png)

---





## 分支切换

---

![image-20220619132441716](http://img.fgcy.xyz/image-20220619132441716.png)

---







## 合并分支



---

![image-20220619133104743](http://img.fgcy.xyz/image-20220619133104743.png)

---





---

![image-20220619133252485](http://img.fgcy.xyz/image-20220619133252485.png)

---



## 代码冲突



---

![image-20220619133649038](http://img.fgcy.xyz/image-20220619133649038.png)

---





---

![image-20220619134617699](http://img.fgcy.xyz/image-20220619134617699.png)

---





# IDEA集成Github









---

![image-20220619135248264](http://img.fgcy.xyz/image-20220619135248264.png)

---





问题描述:

We were unable to authorize you in GitHub. Sorry for inconvenience, please try again later.
我们无法在GitHub中授权您。很抱歉给您带来不便，请稍后再试。
(We were unable to authorize you in GitHub. Sorry for inconvenience, please try again later.我们无法在GitHub中授权您。很抱歉给您带来不便，请稍后再试)



解决：

---

![image-20220721123937156](http://img.fgcy.xyz/image-20220721123937156.png)

---





---

![image-20220721124014498](http://img.fgcy.xyz/image-20220721124014498.png)

---







---

![image-20220721124043134](http://img.fgcy.xyz/image-20220721124043134.png)

---





---

![image-20220721124121791](http://img.fgcy.xyz/image-20220721124121791.png)

---





----

![image-20220721124153016](http://img.fgcy.xyz/image-20220721124153016.png)

---







---

![image-20220721124218534](http://img.fgcy.xyz/image-20220721124218534.png)

---







## 分享项目到Github

---

![image-20220619135820436](http://img.fgcy.xyz/image-20220619135820436.png)

---





---

![image-20220619140008604](http://img.fgcy.xyz/image-20220619140008604.png)

---







## 使用SSH连接push项目



- 添加 remote





---

![image-20220619141222874](http://img.fgcy.xyz/image-20220619141222874.png)

---







---

![image-20220619141152318](http://img.fgcy.xyz/image-20220619141152318.png)

---





---

![image-20220619141422693](http://img.fgcy.xyz/image-20220619141422693.png)

---



注意：push 是将本地库代码推送到远程库，如果本地库代码跟远程库代码版本不一致，
push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程
库的版本高！因此一个成熟的程序员在动手改本地代码之前，一定会先检查下远程库跟本地
代码的区别！如果本地的代码版本已经落后

切记：

**要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！** 





## 拉取代码合并本地库



1、在github上修改代码

---

![image-20220619141803723](http://img.fgcy.xyz/image-20220619141803723.png)

---











拉取

---

![image-20220619152924984](http://img.fgcy.xyz/image-20220619152924984.png)

---





---

![image-20220619153256127](http://img.fgcy.xyz/image-20220619153256127.png)

---



发生冲突后，手动解决



## 克隆代码到本地







----

![image-20220619154027885](http://img.fgcy.xyz/image-20220619154027885.png)

---





---

![image-20220619154139179](http://img.fgcy.xyz/image-20220619154139179.png)

---

注意：该目录必须没有任何文件；即空目录





三个效果：

1、将完整的项目和版本下载下来

2、给远程库地址起了别名

~~~shell
$ git remote -v
origin  https://github.com/fgcy-333/lab27.git (fetch)
origin  https://github.com/fgcy-333/lab27.git (push)

~~~

3、初始化了本地库

~~~shell
$ ls -la
total 36
drwxr-xr-x 1 fgcy 197121     0 Jun 19 15:44 ./
drwxr-xr-x 1 fgcy 197121     0 Jun 19 15:44 ../
drwxr-xr-x 1 fgcy 197121     0 Jun 19 15:44 .git/              #初始化了本地库
drwxr-xr-x 1 fgcy 197121     0 Jun 19 15:46 .idea/
-rw-r--r-- 1 fgcy 197121 14225 Jun 19 15:44 lab27.iml
-rw-r--r-- 1 fgcy 197121  2637 Jun 19 15:44 pom.xml
drwxr-xr-x 1 fgcy 197121     0 Jun 19 15:44 src/

~~~







# IDEA集成Gitee



## 步骤



1、在IDEA中下载插件

---

![image-20220619161928404](http://img.fgcy.xyz/image-20220619161928404.png)

---



2、重启IDEA

3、添加账号

---

![image-20220619162112699](http://img.fgcy.xyz/image-20220619162112699.png)

___





## 分享本地项目到gitee上



方法一：

---

![image-20220619162438238](http://img.fgcy.xyz/image-20220619162438238.png)

---





错误：

IDEA集成Gitee后push项目到Gitee卡在Loading Account Information For gitee.com 不动



---

![image-20220619162531106](http://img.fgcy.xyz/image-20220619162531106.png)

---



原因：

你当前的Gitee账户下没有一个自建的仓库，导致信息验证失败，一直卡在验证信息阶段！



解决方案：
        随便在你的Gitee账号中创建一个仓库，然后再试一下    



注意：

随意创建的仓库不要与你使用IDEA上传的仓库同名。然后再尝试push，push完成之后可以删除随意创建的仓库，此后就无需再创建仓库





方法二：



1、在码云中新建一个仓库 【获取远程库地址】





2、为gitee的远程库地址器别名

---

![image-20220619163850330](http://img.fgcy.xyz/image-20220619163850330.png)

---

注：origin 是GitHub中远程库的别名



3、将别名与远程库地址绑定

---

![image-20220619164103538](http://img.fgcy.xyz/image-20220619164103538.png)

---



4、选择别名为gitee-blog的经行推送

---

![image-20220619164214338](http://img.fgcy.xyz/image-20220619164214338.png)

---





## 拉取项目

---

![image-20220619164530673](http://img.fgcy.xyz/image-20220619164530673.png)

---



---

![image-20220619164558016](http://img.fgcy.xyz/image-20220619164558016.png)

---





# 码云复制Github



1、复制某个gihub仓库的https路径

---

![image-20220619165432198](http://img.fgcy.xyz/image-20220619165432198.png)

---



2、在码云中新建一个项目，选择导入已有项目

---

![image-20220619165555943](http://img.fgcy.xyz/image-20220619165555943.png)

---





3、复制github中的远程库地址连接，初始化信息

---

![image-20220619170041952](http://img.fgcy.xyz/image-20220619170041952.png)

---



4、完成

---

![image-20220619170203013](http://img.fgcy.xyz/image-20220619170203013.png)

---



tip：

---

![image-20220619170327444](http://img.fgcy.xyz/image-20220619170327444.png)

---







# IDEA集成GitLab

1、安装Gitlab插件

---

![image-20220619171505184](http://img.fgcy.xyz/image-20220619171505184.png)

---



2、设置 GitLab 插件 

---

![image-20220619171538115](http://img.fgcy.xyz/image-20220619171538115.png)

---



---

![image-20220619171606936](http://img.fgcy.xyz/image-20220619171606936.png)

---

此界面说明成功

---

![image-20220619171631815](http://img.fgcy.xyz/image-20220619171631815.png)

---





3、自定义远程库连接

---

![image-20220619171750591](http://img.fgcy.xyz/image-20220619171750591.png)

---



注意：gitlab 网页上复制过来的连接是

> http://gitlab.example.com/root/git-test.git，

 

需要手动修改为

>  http://gitlab-server/root/git-test.git 





4、选择 gitlab 远程连接，进行 push

---

![image-20220619172221231](http://img.fgcy.xyz/image-20220619172221231.png)

---



注意：

首次向连接 gitlab，需要登录帐号和密码，用 root 帐号和我们修改的密码登录即可

---

![image-20220619172302127](http://img.fgcy.xyz/image-20220619172302127.png)

---



只要 GitLab 的远程库连接定义好以后，对 GitLab 远程库进行 pull 和 clone 的操作和
Github 和码云一致，此处不再赘述

