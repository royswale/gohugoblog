---
title: "Debug"
date: 2019-01-21T14:17:02+08:00
draft: true
---


### xdebug + phpstorm

https://xdebug.org/docs/all_settings

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

> 这种方式不需要手动点击phpstorm中右上角的绿色虫子按钮开启调试  
> 只需要点击右上角的电话按钮开始监听调试连接请求  
> 其实是浏览器插件自己带上了XDEBUG_SESSION_START参数

#### 总结

[PHPStorm + Xdebug 配置使用教程](https://segmentfault.com/a/1190000011907425)

> PHPStorm使用PHP Remote Debug而不是PHP Web Application的话，就只需要在网址后面加上?XDEBUG_SESSION_START=PHPSTORM，然后就可以调试了，和浏览器没关系了，两者各有优劣，你的那种方法是需要依靠浏览器，浏览器是必须要安装插件，而使用Remote Debug的坏处在于需要加上参数，但却不依赖浏览器，不需要插件，手机App、微信都可以调试

> 第五步，PHP Web Application在phpstrom 2017.3版本是PHP Web Page