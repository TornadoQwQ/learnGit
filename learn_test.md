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