##Git Note & Cheatsheet

本文是在阅读完OSChina对Pro Git的翻译版后整理的笔记和Cheatsheet。  
[OSChina Progit阅读地址](http://git.oschina.net/progit/)  
[Github Progit托管地址](https://github.com/progit/progit)

-------
###常用命令速记
`git config --global user.name <username>` ：指定提交用户名  
`git config --global user.email <e-mail>` ：指定提交用邮箱  
`git config --global core.editor <editor>` ：指定编辑器  
`git config --global merge.tool <mergetool>` ：指定合并工具  
`git config --list` ：查询所有的Git配置信息  
`git config <setting option>` ：查询某配置信息，如user.name  
`git config alias.<alias-name> '<command>'` ：设置自定义命令  
`git clone <git repository>` ：从目标克隆指定仓库  
`git init` ：Git仓库初始化  
`git add <file/folder>` ：将指定文件加入暂存区  
`git add -i` ：交互式shell模式  
`git status` ：查看暂存区状态  
`git diff` ：比较**工作目录**和**暂存区快照**的差异  
`git diff --staged` ：比较**上次提交的快照**和**暂存区快照**的差异  
`git diff --check` ：显示多余白字符  
`git diff <branch-master>...<branch-feature>` ：特性分支相对于两个分支共同祖先将要引入的新代码  
`git commit -m <commit message>` ：提交暂存区中的文件  
`git commit -a` ：git add -A + git commit  
`git commit -amend` ：提交暂存区中的文件，并与上一次提交合并  
`git rm <file/folder>` ：删除已被跟踪的文件  
`git rm --cached <file/folder>` ：将已被跟踪的文件移出跟踪列表（不删除）   
`git mv <src file> <dest file>` ：移动文件并加入暂存区  
`git log` ：显示日志  
`git log -p -2` ：展开显示每次提交的内容差异，仅显示最近的两次提交  
`git log --stat` ：仅显示媒体提交的改动行数  
`git log --pretty=oneline` ：仅显示SHA和提交信息  
`git log --pretty=format:"%h - %an, %ar : %s"` ：以定制格式显示提交历史  
`git log --graph` ：以图表形式显示提交历史  
`git reset HEAD <file>` ：取消对该文件的追踪  
`git checkout -- <file>` ： 将该文件恢复为上次提交快照版本  
`git checkout <branch-name>` ：切换到指定分支  
`git branch <branch-name>` ：默认以当前分支为原型创建新分支  
`git branch -d <branch-name>` ：删除指定分支  
`git branch --no-merged` ：查看尚未于当前分支合并的分支  
`git checkout -b <branch-name>` ：git branch + git checkout  
`git checkout -b <branch-name> <remote-name/branch-name>` ：以远程分支为原型创建新分支  
`git remote -v` ：查看所有远程仓库名称和对应的url  
`git remote add <remote-name> <git repository>` ：添加远程仓库  
`git remote show <remote-name>` ：显示远程仓库详细信息  
`git remote rename <old-remote-name> <new-remote-name>` ：变更远程仓库名称  
`git remote rm <remote-name>` ：移除指定远程仓库  
`git fetch <remote-name>` ：抓取指定远程仓库到本地  
`git merge <branch-name>` ：将指定分支合并到当前分支  
`git pull <remote-name>` ：git fetch + git merge  
`git push <remote-name> <branch-name>` ：将当前分支推送到指定仓库中的同名分支  
`git push <remote-name> <local-branch-name>:<remote-branch-name>` ：推送指定分支到指定仓库的指定分支（如本地分支留空则为删除远程分支www）  
`git tag` ：列出所有标签  
`git tag -l <exp>` ：列出符合匹配模式的标签  
`git tag <tag-name>` ：创建指定名称的标签  
`git tag -v <tag-name>` ：验证标签  
`git push <remote-name> <tag-name>` ：推送标签（git push默认不推送）  
`git push <remote-name> --tags` ：推送所有标签  
`git rebase <branch-name>` ：以指定的分支与当前分支的公共祖先A为基准，将当前分支相对于A的变动重新应用于指定分支。衍合操作使得当前分支成为指定分支的快进版本，但同时使得当前分支之前的提交对象悬空，并会导致远程分支失效，因此最好不要对公开分支做衍合操作。  
`git rebase <base-branch> <current-branch>` ：作用同上  
`git rebase -i <HEAD~<N>>` ：在衍合最近N次提交的同时对提交对象进行交互式操作  
`git request-pull <remote-name/branch-name> <branch-name>` ：请求合并本地分支  
`git format-patch <remote-name/branch-name>` ：生成.patch补丁文件  
`git am *.patch` ：应用.patch补丁文件  
`git merge-base <branch-one> <branch-two>` ：得到两个分支的合并基准  
`git cherry-pick <commit-SHA1>` ：只合并某次提交  
`git reflog` ：HEAD分支的变动记录  
`git show HEAD^<N>` ：查看HEAD指向提交的第N父提交，N留空则为第一父提交  
`git show HEAD~<N>` ：查看HEAD指向提交的前N父提交，如HEAD~3等价于HEAD^^^  
`git stash` ：将当前的变更工作进行储藏  
`git stash list` ：列出所有储藏  
`git stash apply <stash-name>` ：应用指定的储藏，如果留空则应用最近的储藏。注意，该命令会将未提交的文件自动提交，如果想完全还原之前的场景，则应使用--index选项。  
`git blame -L <aa,bb> <file>` ：查看指定文件从aa到bb行的历史修改  
`git bisect start` ：启动Git的提交对象二分查找  
`git bisect <good/bad>` ：表示当前版本为好/坏版本  
`git bisect reset` ：重置HEAD，**注意，在使用二分查找功能后必须使用该命令重置**
`git show-ref` ：查看所有引用的全名  

-------
###配置文件
/etc/gitconfig ：适用于系统中所有用户，由`git config --system`写入  
~/.gitconfig ：适用于当前用户，由`git config --global`写入  
./.git/config ：适用于当前项目  

注意，配置文件的优先级随覆盖范围降低而升高

-------
###Git的.git目录
HEAD ：指向当前被检出的分支  
config ：当前项目的配置文件  
description ：供GitWeb程序使用的描述文件  
hooks/ ：用于保存客户端或服务端的钩子脚本  
index ：保存暂存区信息  
info/ ：保存了一份不希望在 .gitignore 文件中管理的忽略模式 (ignored patterns) 的全局可执行文件  
objects/ ：存储对象，包括blob对象，tree对象，commit对象和tag对象  
refs/ ：存储分支信息  

-------
###使用Git底层命令完成版本流程
1.创建一个Git文件对象 ：`echo 'test content' | git hash-object -w --stdin`，其中-w保证写入该对象，--stdin使用标准输入流作为文件内容  
2.输出该Git文件对象内容 ：`git cat-file -p <SHA-1>`  
3.将单个文件加入暂存区 ：`git update-index --add --cacheinfo 100644 <SHA-1> <file>`,由于该文件原先并不在暂存区域中 ，必须传入 --add 参数;由于要添加的文件并不在当前目录下而是在数据库中，必须传入 --cacheinfo 参数。同时指定了文件模式，SHA-1 值和文件名。  
4.使用暂存区的文件生成tree对象 ：`git write-tree`  
5.生成commit对象并指向指定的tree对象 ：`echo 'first commit' | git commit-tree <SHA-1>`。也可以使用`git commit-tree <SHA-1> -p <parent-SHA-1>`指定父提交。  
6.生成master分支并将其指向特定的commit对象 ：`git update-ref refs/heads/master <SHA-1>`  
7.查看HEAD的值 ：`git symbolic-ref HEAD`  
8.设置HEAD的值 ：`git symbolic-ref HEAD <ref-file>`  

