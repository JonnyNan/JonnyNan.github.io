---
title: git系列之时光穿梭
date: 2019-08-13 16:25:47
tags: [git,版本管理]
---

# 时光穿梭

上次内容我们已经生成了一个git仓库，并且创建了一个readme.txt文件。
通过 下面命令添加到git仓库
```
$ git add readme.txt 
$ git commit -m "add file"
```

下面我们来编辑下readme.txt文件,比如改成：
```
Git is a distributed version control system.
Git is free software.
```

通过 git status 命令来查询当前仓库状态
```
git status
```

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

上面输出内容告诉我们，readme.txt 文件被修改过了，但是没有准备提交的修改。


通过  git diff 命令 查看修改内容

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个distributed单词。

知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是git add：
```
$ git add readme.txt

```
再第二步 git commit -m 之前 我们先 看看当前状态

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
	modified:   readme.txt
```
git status告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了：
```
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，我们再用git status命令看看仓库的当前状态：

```
$ git status
On branch master
nothing to commit, working tree clean
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

# 小结
要随时掌握工作区的状态，使用git status命令。

如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

