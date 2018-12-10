---
title: "Thinkphp Setup"
date: 2018-12-10T19:30:36+08:00
draft: false
---

在 LEMP 安装配置完成的情况下

#### thinkphp

https://www.kancloud.cn/manual/thinkphp5/118006

> 三、Git安装  
 首先克隆下载应用项目仓库  
 git clone https://github.com/top-think/think tp5  
 然后切换到tp5目录下面，再克隆核心框架仓库：  
 git clone https://github.com/top-think/framework thinkphp  

```bash
git clone https://gitee.com/liu21st/thinkphp5 tp5
cd tp5
git clone https://gitee.com/liu21st/framework.git thinkphp
composer update
```
别忘了在 `tp5` 目录下执行 `composer update` 安装扩展包

#### composer

https://getcomposer.org/download/

就在普通用户的家目录 /home/anony/ 下执行

1. Command-line installation

	```bash
	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
	php -r "if (hash_file('sha384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
	php composer-setup.php
	php -r "unlink('composer-setup.php');"
	```

	执行完成之后发现 /home/anony/ 下多了一个 composer.phar 文件

2. https://getcomposer.org/doc/00-intro.md#globally

	```bash
	mv composer.phar /usr/local/bin/composer
	```

	全局可用

3. comoser package mirror

	https://laravel-china.org/composer

	全局配置

	```bash
	$ composer config -g repo.packagist composer https://packagist.laravel-china.org
	```

	还是以普通用户执行这条命令，
	后期composer的缓存目录的权限属于该用户

#### nginx 配置

https://gitee.com/royswale/nginx-config-tencent/blob/master/sites-available/default.backup.symfony

老朱亲自写的,最完美ThinkPHP Nginx 配置文件
http://www.thinkphp.cn/topic/34380.html

https://www.jianshu.com/p/018e5d0337a4

https://blog.csdn.net/voke_/article/details/79756886

http://www.thinkphp.cn/topic/40391.html

https://www.oliverdavies.uk/blog/nginx-redirects-with-query-string-arguments/


#### Apache 修改 public/.htaccess 即可

https://blog.csdn.net/Developersq/article/details/78386122

https://blog.csdn.net/lw545034502/article/details/79374611


### easywechat

在 tp5 目录下安装 easywechat

https://www.easywechat.com/docs/master/installation

```bash
which composer
composer require overtrue/wechat -vvv
```

报错，需要先安装 simplexml 扩展

https://www.limesurvey.org/es/comunidad/foros/installation-a-update-issues/113739-php-simplexml  
https://neilyamit.com/dev/simplexml-disabled.html

```bash
sudo apt install php7.2-xml
```

访问微信授权接口，报错

> ...wechat.log" could not be opened: failed to open stream: Permission denied

```bash
touch tp5/application/wechat.log
chmod a+rw wechat.log
```

https://www.easywechat.com/docs/master/installation

SDK 基础：EasyWeChat 的安装  
https://www.easywechat.com/lessons/1

easyWeChat 验证微信 token  
https://blog.csdn.net/frank_hanhan/article/details/80972192

YII2中服务器端验证总是失败..
https://github.com/overtrue/wechat/issues/1245

https://github.com/qiqizjl/think-wechat

服务器上安装的thinkphp版本是5.1.x的，这个文档是5.0的
https://www.kancloud.cn/manual/thinkphp5/118019


### 微信小程序商城构建全栈应用

全部文件>微信小程序商城构建全栈应用（下...>第9章微信登陆与令牌

8-14 开启路由完整匹配模式

https://coding.imooc.com/class/chapter/97.html#Anchor

关联预载入  
https://www.kancloud.cn/manual/thinkphp5/139045