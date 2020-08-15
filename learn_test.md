# Git和Girhub的使用

## 设置本地Git用户名和邮箱

```git
git config --global user.name "wbf99"
git config --global user.email "2439947074@qq.com"
```

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 创建版本库repository

第一步，创建目录（windows下尽量英文），进入目录。
第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```git
$ git init
Initialized empty Git repository in F:/Projects/BLOG_Hexo/git_and _github/.git/
```

第三步，添加文件到版本库：

```git
$ git add learn_test.md #文件添加到仓库
$ git commit -m "wrote a summary file" #文件提交到仓库
[master (root-commit) ec748fd] wrote a summary file #反馈提交说明
 1 file changed, 8 insertions(+) #1个文件被改动，插入了8行内容
 create mode 100644 learn_test.md
```

注意可以先`add`多个文件之后再一次性`commit`：

```git
git add file1.txt
git add file2.txt file3.txt
git commit -m "add 3 files."
```

## 版本回退

用`git log`查看提交记录，查看由最近一次提交到最早一次提交：

```git
$ git log
commit 4c9f19aee9413a7d1589425a58a0cb8ed54810f2 (HEAD -> master)
Author: wbf99 <2439947074@qq.com>
Date:   Fri Aug 14 20:34:22 2020 +0800

    add and commit

commit 40a9cd0085d90b3d6ccca7e010edc6fd02dca9a1
Author: wbf99 <2439947074@qq.com>
Date:   Fri Aug 14 20:33:48 2020 +0800

    add and commit

commit ec748fdefc29dee171fcc8696e582e0ad5d59f9b
Author: wbf99 <2439947074@qq.com>
Date:   Fri Aug 14 19:46:11 2020 +0800

    wrote a summary file

```

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```git
$ git log --pretty=oneline
4c9f19aee9413a7d1589425a58a0cb8ed54810f2 (HEAD -> master) add and commit
40a9cd0085d90b3d6ccca7e010edc6fd02dca9a1 add and commit
ec748fdefc29dee171fcc8696e582e0ad5d59f9b wrote a summary file
```

回退到上一个版本：
首先，Git中`HEAD`表示当前版本，`HEAD^`表示上个版本，上上一个版本就是`HEAD^^`，往上100个版本是`HEAD~100`。
回退到上个版本使用命令`reset`:

```git
$ git log --pretty=oneline
c1c886e9e934a35bef8dac090b293f76102c4895 (HEAD -> master) how to reset
4c9f19aee9413a7d1589425a58a0cb8ed54810f2 add and commit
40a9cd0085d90b3d6ccca7e010edc6fd02dca9a1 add and commit
ec748fdefc29dee171fcc8696e582e0ad5d59f9b wrote a summary file

$ git reset --hard HEAD^
HEAD is now at 4c9f19a add and commit

$ git log --pretty=oneline
4c9f19aee9413a7d1589425a58a0cb8ed54810f2 (HEAD -> master) add and commit
40a9cd0085d90b3d6ccca7e010edc6fd02dca9a1 add and commit
ec748fdefc29dee171fcc8696e582e0ad5d59f9b wrote a summary file
```

如何撤销还原？只要知道`commit id`就可撤销：

```git
$ git reset --hard c1c886
HEAD is now at c1c886e how to reset
```

如果关闭了当前窗口，看不到之前的记录，可用`git reflog`查看所有操作的历史记录，从而找到需要的那个版本的`commit id`：

```git
$ git reset --hard HEAD^
HEAD is now at 4c9f19a add and commit

$ git reflog
4c9f19a (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
c1c886e HEAD@{1}: reset: moving to c1c886
4c9f19a (HEAD -> master) HEAD@{2}: reset: moving to HEAD^
c1c886e HEAD@{3}: commit: how to reset
4c9f19a (HEAD -> master) HEAD@{4}: commit: add and commit
40a9cd0 HEAD@{5}: commit: add and commit
ec748fd HEAD@{6}: commit (initial): wrote a summary file

$ git reset --hard c1c886e
HEAD is now at c1c886e how to reset
```

## 工作区和暂存区

工作区（Working Directory）：`git_and_github`文件夹。
版本库（Repository）：`git_and_github/.git`文件夹：stage（或者叫index）暂存区、默认分支`master`、指向`master`的指针`HEAD`。

需要提交的文件修改通过`git add`全部先放到暂存区，然后，一次性通过`git commit`提交暂存区的所有修改。

在工作区修改文件、增加删除文件后可用`git status`查看状态：

```git
$ git status
On branch master
Changes not staged for commit:  #工作区中修改后还未提交的
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   learn_test.md

Untracked files:  #工作区中还未被添加过的文件
  (use "git add <file>..." to include in what will be committed)
        test_LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

`git add`将两个新文件加入暂存区：

```git
$ git add learn_test.md
$ git add test_LICENSE
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   learn_test.md
        new file:   test_LICENSE
```

执行`git commit`就可以一次性把暂存区的所有修改提交到分支，此时若无新的修改`git status`查看工作区就是干净的，暂存区没有内容：

```git
$ git commit -m "understand how stage works"
[master 3362403] understand how stage works
 2 files changed, 53 insertions(+), 1 deletion(-)
 create mode 100644 test_LICENSE
$ git status
On branch master
nothing to commit, working tree clean
```

## 管理修改

第一次修改 -> `git add` -> 第二次修改 -> `git commit`
此时查看`git status`可知第二次修改没有被放入暂存区，从而也没有被提交；使用`git diff HEAD -- 文件名`可查看工作区和版本库最新版本的差别：

