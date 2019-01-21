---
title: "Debug"
date: 2019-01-21T14:17:02+08:00
draft: true
---


### xdebug + phpstorm

#### 方法#1 单个api接口

1. install xdebug extension

	```ini
	[XDebug]
	zend_extension="\path\to\php_xdebug.dll"
	xdebug.remote_enable = 1
	xdebug.remote_autostart = 1
	```
2. phpstorm setting

	Run -> Edit Configurations -> Add New Configuration -> PHP Web Page
	
	```
	Server		Start URL		Browser
	```
	
	-> Servers -> Add

	```
	Host 		Port 	Debugger
	xxx.test 	80 		Xdebug
	```

3. set up breakpoints and click the bug icon to start Debug

#### 方法#2 对整个项目有效，网页跳转都行

https://www.jetbrains.com/help/phpstorm/debugging-with-phpstorm-ultimate-guide.html  
https://www.jetbrains.com/help/phpstorm/zero-configuration-debugging.html  
https://www.jetbrains.com/help/phpstorm/browser-debugging-extensions.html

1. install xdebug extension

	```ini
	[XDebug]
	zend_extension="F:\yyk@zhitu\software\phpStudy\PHPTutorial\php\php-5.6.27-nts\ext\php_xdebug-2.5.5-5.6-vc11-nts.dll"
	xdebug.remote_enable = 1
	xdebug.remote_autostart = 1
	; xdebug.idekey = PHPSTORM
	```

2. chrome extension

	[xdebug-helper](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc/related?hl=en-US)

	> switch to "debug" mode at your target host

3. Enable listening for PHP Debug Connections, i.e. click the green phone icon.

	![](http://qiniu.xingtan.xyz/settings-php-debug.png)

4. 还是需要配置方法#1的server和php web page这2项