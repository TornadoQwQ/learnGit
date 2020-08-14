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
```
$ git add learn_test.md
$ git commit -m "wrote a summary file"
[master (root-commit) ec748fd] wrote a summary file
 1 file changed, 8 insertions(+)
 create mode 100644 learn_test.md
```