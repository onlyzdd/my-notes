## Git 的奇技淫巧

Git 是一个分布式版本管理工具，本文介绍了常用的 Git 命令和一些小技巧。

### 开卷必读

如果之前未使用过 Git，可以学习 [Git 小白教程](http://rogerdudler.github.io/git-guide/index.zh.html)入门。

1. 一定要先测试命令的效果后，再用于工作环境，以防造成不能弥补的后果。
1. 所有的命令在 Git version 2.7.4（Apple Git-66）下测试通过。
1. 统一概念：工作区、暂存区、本地仓库、远程仓库、提交

### 学习 20 个左右的 Git 命令

```bash
$ git help everyday
```

### 展示帮助信息

```bash
$ git help # 所有信息及常用命令
$ git help -a # 可用的命令
$ git help -g # 查看相关指南
$ git help <command> # 查看特定命令的用法
```

### 根据内容查找修改

```bash
$ git log -S '<a term in the source>'
```

### 回到远程仓库的状态

抛弃本地所有的修改，回到远程仓库的状态。

```bash
$ git fetch --all && git reset --hard origin/master && git clean -f -d
```

### 查看某次提交之前存在的所有文件

```bash
$ git ls-tree --name-only -r <commit-id>
```

### 重设第一个提交

**清空**所有提交，将所有提交的内容放回**暂存区**，这样就可以重新进行第一次提交了。

```bash
$ git update-ref -d HEAD
```

### 列出所有冲突的文件

```bash
$ git diff --name-only --diff-filter=U
```

### 列出某次提交中改变的所有文件

```bash
$ git diff-tree --no-commit-id --name-only -r <commit-id>
```

### 展示工作区和暂存区的不同

```bash
$ git diff
```

### 展示本地仓库任意两个提交之间的文件变动

```bash
$ git diff <commit-id> <commit-id>
```

### 展示暂存区和最近版本的不同

```bash
$ git diff --cached # or git diff --staged
```

### 展示暂存区、工作区和最近版本的不同

```bash
$ git diff HEAD
```

### 快速切换到之前的分支

```bash
$ git checkout - # or git checkout @{-1}
```

### 列出已经合并到 master 的分支

```bash
$ git branch --merged master
```

### 删除已经合并到 master 的分支

```bash
$ git branch --merged master | grep -v '^\*\| master' xargs -n 1 git branch -d
```

### 展示本地分支和远程分支的关联情况

包括所有分支及其对应的远程分支，以及各个分支的最后一次提交。

```bash
$ git branch -vv
```

### 关联远程分支

```bash
$ git branch -u origin/mybranch
```

### 删除本地分支

```bash
$ git branch -d <local_branchname>
```

### 删除远程分支

```bash
$ git push origin --delete <remote_branchname> # or git push origin :<remote_branchname>
```

### 删除本地标签

```bash
$ git tag -d <tag-name>
```

### 删除远程标签

```bash
$ git push origin :refs/tags/<tag-name>
```

### 使用版本中的最后内容撤销本地改动

```bash
$ git checkout -- <filename>
```

### 恢复：创建一个新的提交以回到指定的提交状态

```bash
$ git revert <commit-id>
```

### 重置：丢弃提交，建议在个人分支上使用

```bash
$ git reset <commit-id> # 默认参数为 mixed
$ git reset --mixed HEAD^ # 回退至上个提交，将 HEAD 指向上一个提交，重置暂存区和 HEAD 匹配，工作区不会被修改
$ git reset --soft HEAD～3 # 回退至三个版本之前，只回退提交，暂存区和工作区与回退之前保持一致
$ git reset --hard <commit-id> # 彻底回退到指定的提交，暂存区和工作区也会变为指定提交的内容
```

### 修改上一次的提交信息

```bash
$ git commit -v --amend
```

### 查看还未被合并到 master 分支的所有提交

```bash
$ git cherry -v master
# or 
$ git cherry -v master <branchname>
```

### 修改作者名

```bash
$ git commit --amend --author="Author Name <address@email.com>"
```

### 在全局配置中作者改变之后重置作者名

```bash
$ git commit --amend --reset-author --no-edit
```

### 修改远程仓库的 URL

```bash
$ git remote set-url origin <url>
```

### 增加远程仓库

```bash
$ git remote add <ref> <url>
```

### 获取所有远程仓库的列表

```bash
$ git remote # or git remote show
```

### 获取本地和远程的所有分支

```bash
$ git branch -a
```

### 只获取远程分支

```bash
$ git branch -r
```

### 暂存已修改文件的部分，而不是整个文件（此文件之前必须已进入版本库）

```bash
$ git add -p
```

### 查看两星期内的改动

```bash
$ git log --no-merges --raw --since="2 weeks ago"
# or
$ git whatchanged --since="2 weeks ago"
```

### 自 Fork 项目以来的所有提交

```bash
$ git log --no-merges --stat --reverse master..
```

### 将其他分支的某个提交放到当前分支上

```bash
$ git cherry-pick <commit-id>
```

### 查找指定提交所在的分支

```bash
$ git branch -a --contains <commit-id>
# or
$ git branch --contains <commit-id>
```

### 给 Git 命令起别名

```bash
$ git config --global alias.<handle> <command>
```

### 保存已跟踪文件的当前状态而不提交

```bash
$ git stash
# or 
$ git stash save
```

### 保存已跟踪文件的未暂存更改

```bash
$ git stash -k
# or 
$ git stash --keep-index
# or 
$ git stash save --keep-index
```

### 保存当前状态包括未跟踪的文件

```bash
$ git stash -u
# or
$ git stash save -u
# or
$ git stash save --include-untracked
```

### 保存当前状态并设置消息

```bash
$ git stash save <message>
```

### 保存所有文件（包括忽略的、已跟踪和未跟踪）的状态

```bash
$ git stash -a
# or 
$ git stash --all
# or 
$ git stash save --all
```

### 展示所有的贮藏

```bash
$ git stash list
```

### 应用贮藏而不从贮藏列表中删除贮藏

```bash
$ git stash apply <stash@{n}>
```

### 应用最近一次贮藏并从贮藏列表中删除

```bash
$ git stash pop
# or
$ git stash apply stash{0} && git stash drop stash{0}
```

### 清空所有贮藏

```bash
$ git stash clear
```

### 删除某次贮藏

```bash
$ git stash drop <stash@{n}>
```

### 从某次贮藏中拿出文件的修改

```bash
$ git checkout <stash@{n}> -- <filepath>
```

### 列出所有已跟踪的文件

```bash
$ git ls-files -t
```

### 列出所有未跟踪的文件

```bash
$ git ls-files --others
```

### 列出所有被忽略的文件

```bash
$ git ls-files --others -i --exclude-standard
```

### 在不删除文件的情况下，不跟踪文件

```bash
$ git rm --cached <filepath>
# or
$ git rm --cached -r <directorypath>
```

### 获取未被跟踪的文件/文件夹列表，以移除这些文件

```bash
$ git clean -n
```

### 移除所有未跟踪的文件

```bash
$ git clean -f
```

### 移除所有未跟踪的文件夹

```bash
$ git clean -f -d
# or
$ git clean -df
```

### 更新所有子模块

```bash
$ git submodule foreach git pull
# or
$ git submodule update --init --recursive
$ git submodule update --remote
```

### 重命名分支

```bash
$ git branch -m <new-branch-name>
# or
$ git branch -m <old-branch-name> <new-branch-name>
```

### 变基并合并到 master

```bash
$ git rebase master feature && git checkout master && git merge -
```


### 改变上次提交但不修改提交消息

```bash
$ git commit --amend --no-edit
```

### 取消关联

```bash
$ git fetch -p
# or
$ git remote prune origin
```

### 可视化展示版本树

```bash
$ git log --pretty=oneline --graph --decorate --all
# or
$ gitk --all
```

### 部署跟踪的子文件夹到远程分支

```bash
$ git subtree push --prefix subfolder_name origin gh-pages
```

### 将分支及其历史导出为文件

```bash
$ git bundle create <file> <branch-name>
```

### 从文件中导入

```bash
$ git clone repo.bundle <repo-dir> -b <branch-name>
```

### 获取当前分支的名称

```bash
$ git rev-parse --abbrev-ref HEAD
```

### 显示当前分支上最近的标签

```bash
$ git describe --tags --abbrev=0
```

### 详细显示行更改

```bash
$ git diff --word-diff
```

### 显示所有的别名和配置

```bash
$ git config --local --list
$ git config --global --list
```

### 使 Git 对大小写敏感

```bash
$ git config --global core.ignorecase false
```

### 删除全局设置

```bash
$ git config --global --unset <entry-name>
```

### 显示 branch1 有，而 branch2 没有的提交

```bash
$ git log branch1 ^branch2
```

### 新建一个没有任何提交的分支

```bash
$ git checkout --orphan <branch-name>
```

### 展示任意分支某一文件的内容

```bash
$ git show <branch-name>:<file-name>
```

### 克隆指定的单一分支

```bash
$ git clone -b <branch-name> --single-branch https://github.com/user/repo.git
```

### 在提交中查找内容

```bash
$ git log --all --grep='<given text>'
```

### 把暂存区的文件放到工作区

```bash
$ git reset <filename>
```

### 添加远程

```bash
$ git remote add <remote-nickname> <remote-url>
```

### 显示某个文件的修改历史

```bash
$ git blame <filename>
```

### 分组显示历史

```bash
$ git shortlog
```

### 回退某次合并

```bash
$ git revert -m 1 <commit-id>
```

### 分支上的提交数目

```bash
$ git rev-list --count <branch-name>
```

### 获取两个分支的公共祖先分支

```bash
$ diff -u <(git rev-list --first-parent BranchA) <(git rev-list --first-parent BranchB) | sed -ne 's/^ //p' | head -1
```

### 获取所有未推送的提交

```bash
$ git log --branches --not --remotes
# or 
$ git log @{u}..
$ git cherry -v
```

### 获取仓库名称

```bash
$ git rev-parse --show-toplevel
```

### 某段时间内的提交

```bash
$ git log --since='FEB 1 2017' --until='FEB 14 2017'
```

### 备份未跟踪的文件

```bash
$ git ls-files --others -i --exclude-standard | xargs zip untracked.zip
```

### 分支上的当前状态

```bash
$ git status --short --branch
```

### 强制推送

```bash
$ git push -f <remote-name> <branch-name>
```




