---
title: "Mirrors Config"
date: 2018-12-17T13:53:59+08:00
draft: true
---

### pip

### composer

### npm

### golang

#### 解决 go get 问题

1. socks5 给 git 代理

	git设置socks代理  
	https://www.jianshu.com/p/d82e9ed1c09c

	设置

	```bash
	git config --global http.proxy 'socks5://127.0.0.1:1080'
	git config --global https.proxy 'socks5://127.0.0.1:1080'
	```

	取消设置

	```bash
	git config --global --unset http.proxy
	git config --global --unset https.proxy
	```

2. socks5 给 http https 代理

	go-get 利用 socks5 代理翻墙下载  
	https://ybilly.com/2018/07/03/go-get%E5%88%A9%E7%94%A8ss%E7%9B%B4%E6%8E%A5%E7%BF%BB%E5%A2%99/

	创建一个 goget.bat 文件，以后用 goget.bat 代替 go get 命令，来安装依赖

	```bat
	@echo off
	set http_proxy=socks5://127.0.0.1:1080
	set https_proxy=socks5://127.0.0.1:1080
	go get -u -v %*
	echo ...
	pause
	```

	我把 goget.bat 放在了 GOPATH/bin 目录，打开 cmd 运行 goget.bat，不是 git-bash

完美解决 😆
只用 socks5 就可以了

### linux

http://mirrors.ustc.edu.cn/  
https://mirrors.ustc.edu.cn/help/  
http://mirrors.ustc.edu.cn/help/debian.html

小技巧

> 使用 HTTPS 可以有效避免国内运营商的缓存劫持，但需要事先安装 apt-transport-https (Debian Buster 及以上版本不需要)。

另外，也可以使用 snullp 大叔开发的 [配置生成器](https://mirrors.ustc.edu.cn/repogen/) 。