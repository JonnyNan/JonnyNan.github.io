---
title: git系列之安装并创建版本库
date: 2019-08-12 15:22:43
tags:
---

## git的安装

- windows安装无脑下一步即可


- linux：

    ubuntu :
    ```
    $ sudo apt install git 
    ```

    arch: 
    ```
    $ sudo pacman -S git
    ```
    
    centos:
    ```
    $ sudo yum install git
    ```
    
- macos: 
    ```
    brew install git
    ```
    
- linux和macos 也可以源码编译安装。

## git安装后的配置

- Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

    ```
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"
    ```
注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

## git创建版本库

什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

找一个空目录执行下面命令：
```
$ git init
```

Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，修改或删除此文件夹内容会造成git仓库被破坏。

如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。不过，不建议你使用自己正在开发的公司项目来学习Git，否则造成的一切后果概不负责。

### 把文件添加到版本库
首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

比如，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的.


现在新建一个文件：readme.txt,内容如下：

```
Git is a version control system.
Git is free software.
```

此文件一定要放在git仓库的目录下，子目录下也可。否则git再厉害也找不到此文件。

将一个文件放到git仓库只需2步：

- 第一步，用命令git add告诉Git，把文件添加到仓库：
```
$ git add readme.txt
```

- 第二步，用命令git commit告诉Git，把文件提交到仓库：
```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入-m "xxx"行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。

git commit命令执行成功后会告诉你，1 file changed：1个文件被改动（我们新添加的readme.txt文件）；2 insertions：插入了两行内容（readme.txt有两行内容）。

### 为什么git添加文件要两步走？
因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：
```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

## 小结

初始化一个Git仓库，使用git init命令。

添加文件到Git仓库，分两步：

使用命令git add \<file>，注意，可反复多次使用，添加多个文件；
使用命令git commit -m \<message>，完成。



