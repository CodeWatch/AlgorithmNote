##Greedy Strategy

本文是在阅读完OSChina对Pro Git的翻译版后整理的笔记和Cheatsheet。  
[OSChina Progit阅读地址](http://git.oschina.net/progit/)  
[Github Progit托管地址](https://github.com/progit/progit)

-------
###命令速记
`git config --global user.name [username]` ：指定提交用户名  
`git config --global user.email [email]` ：指定提交用邮箱  
`git config --global core.editor [editor]` ：指定编辑器  
`git config --global merge.tool [mergetool]` ：指定合并工具  
`git config --list` ：查询所有的Git配置信息  
`git config [setting option]` ：查询某配置信息，如user.name  
`git clone [target git repository]` ：从目标克隆指定仓库  
`git add [file/folder]` ：

-------
###配置文件
/etc/gitconfig ：适用于系统中所有用户，由`git config --system`写入  
~/.gitconfig ：适用于当前用户，由`git config --global`写入  
./.git/config ：适用于当前项目  

注意，配置文件的优先级随覆盖范围降低而升高

-------
###底层原理速记
[TODO]