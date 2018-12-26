---
title: "Git"
date: 2018-12-11T09:11:25+08:00
draft: false
---

- [ ] https://www.atlassian.com/git/tutorials

### submodule

#### 检出项目的同时也检出子模块

```bash
git clone git@github.com:royswale/gohugoblog.git --recursive
```
https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97

> 不过还有更简单一点的方式。 如果给 git clone 命令传递 --recursive 选项，它就会自动初始化并更新仓库中的每一个子模块。

### git detached

![git-2018121492213](http://qiniu.xingtan.xyz/git-2018121492213.png)

在GitKraken中从匿名分支切换到master分支，匿名分支丢失了  
是我误操作的

![git-2018121493742](http://qiniu.xingtan.xyz/git-2018121493742.png)

执行这个
```bash
git checkout 1f4fc55
```
可以回到之前的`HEAD游离`状态

[图解Git - HEAD标识处于分离状态时的提交操作](https://marklodato.github.io/visual-git-guide/index-zh-cn.html#detached)  
[Git HEAD detached from XXX (git HEAD 游离) 解决办法](https://blog.csdn.net/u011240877/article/details/76273335)  
这个文章的解救方法就是：  
在当前游离状态下新建分支temp，提交改动  
回到master分支，合并temp，解决冲突，删除temp

```bash
# git branch temp
# git checkout temp
git checkout -b temp
git status
git checkout master
git merge temp
# 用 GitKraken 解决冲突
git commit -m "resolve conflict"
git push -u origin master
git branch -d temp
```

按照上述方法解决了，  
我是在使用 gohugo 的 submodule 之 public 下遇到的这个问题。  
看图 GitKraken 可以更清晰是怎么回事

![git-20181214102326](http://qiniu.xingtan.xyz/git-20181214102326.png)


### config

1. 全局或在当前项目配置 name 和 email

	```bash
	git config --list --global
	git config --global user.name yourname
	# git config --global user.name "firstname lastname"
	git config --global user.email yourname@yahoo.com
	```

### ignore

1. IntelliJ IDE 生成的 .idea 目录在 .gitignore 文件中添加之后还是被跟踪的问题

	[idea .gitignore(git文件忽略)](https://www.jianshu.com/p/9ff3920d7a63)

	> 发现.idea文件夹下的文件还有变更被提交，这是因为在使用gitignore之前，此文件就以及被跟踪了，这样的话需要移除跟踪，如下命令:

	```bash
	git rm --cached --force -r .idea
	```