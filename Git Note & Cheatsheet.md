##Git Note & Cheatsheet

本文是在阅读完OSChina对Pro Git的翻译版后整理的笔记和Cheatsheet。  
[OSChina Progit阅读地址](http://git.oschina.net/progit/)  
[Github Progit托管地址](https://github.com/progit/progit)

-------
###命令速记
`git config --global user.name <username>` ：指定提交用户名  
`git config --global user.email <e-mail>` ：指定提交用邮箱  
`git config --global core.editor <editor>` ：指定编辑器  
`git config --global merge.tool <mergetool>` ：指定合并工具  
`git config --list` ：查询所有的Git配置信息  
`git config <setting option>` ：查询某配置信息，如user.name  
`git clone <git repository>` ：从目标克隆指定仓库  
`git init` ：Git仓库初始化  
`git add <file/folder>` ：将指定文件加入暂存区  
`git status` ：查看暂存区状态  
`git diff` ：比较**工作目录**和**暂存区快照**的差异  
`git diff --staged` ：比较**上次提交的快照**和**暂存区快照**的差异  
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
`git remote -v` ：查看所有远程仓库名称和对应的url  
`git remote add <remote-name> <git repository>` ：添加远程仓库  
`git remote show <remote-name>` ：显示远程仓库详细信息  
`git remote rename <old-remote-name> <new-remote-name>` ：变更远程仓库名称  
`git remote rm <remote-name>` ：移除指定远程仓库  
`git fetch <remote-name>` ：抓取指定远程仓库到本地  
`git pull <remote-name>` ：git fetch + git merge  
`git push <remote-name> <branch-name>` ：将当前分支推送到指定仓库中指定名称分支  


-------
###配置文件
/etc/gitconfig ：适用于系统中所有用户，由`git config --system`写入  
~/.gitconfig ：适用于当前用户，由`git config --global`写入  
./.git/config ：适用于当前项目  

注意，配置文件的优先级随覆盖范围降低而升高

-------
###底层原理速记
[TODO]