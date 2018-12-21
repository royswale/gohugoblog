---
title: "Thinkphp Setup"
date: 2018-12-10T19:30:36+08:00
draft: false
---

https://royswale.github.io/posts/how-to-install-lemp-on-ubuntu-18-04/  

> 在 LEMP 安装配置完成的情况下  

### composer

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
	sudo mv composer.phar /usr/local/bin/composer
	```

	全局可用

3. comoser package mirror

	https://laravel-china.org/composer

	全局配置

	```bash
	$ composer config -g repo.packagist composer https://packagist.laravel-china.org
	```

	还是以普通用户 不加 sudo 执行这条命令，  
	所以composer的缓存目录 `/home/annoy/.config/composer/` 的权限属于该用户

### thinkphp

https://www.kancloud.cn/manual/thinkphp5/118006

> 三、Git安装  
 首先克隆下载应用项目仓库  
 git clone https://github.com/top-think/think tp5  
 然后切换到tp5目录下面，再克隆核心框架仓库：  
 git clone https://github.com/top-think/framework thinkphp  

```bash
cd ~
git clone https://gitee.com/liu21st/thinkphp5 tp5
cd tp5
git clone https://gitee.com/liu21st/framework.git thinkphp
composer update
```
别忘了在 `tp5` 目录下执行 `composer update` 安装扩展包


### nginx 配置

ThinkPHP5.1完全开发手册 -> 架构 -> URL访问 -> URL重写 -> [ Nginx ]
https://www.kancloud.cn/manual/thinkphp5_1/353955#URL_22


```bash
sudo vim /etc/nginx/sites-available/example.com

sudo nginx -t
sudo service nginx status
sudo service nginx restart
sudo service nginx status
```

```nginx
server {
	listen 80;
	root /home/anony/tp5/public;
	index index.php index.html index.htm;
	server_name example.com;

	location / {
		# try_files $uri $uri/ =404;
		# try_files $uri /index.php$is_args$args;
		if (!-e $request_filename) {
			rewrite  ^(.*)$  /index.php?s=/$1  last;
		}
	}

	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
	}

	location ~ /\.ht {
		deny all;
	}
}
```


其它一些关于nginx配置thinkphp的文章  
参考, 未使用

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

```apache
-  RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]
+  RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
```

#### HTTPS

把域名的SSL证书文件放到某个目录下

编辑 server block 配置文件 `/etc/nginx/sites-available/example.com`

```nginx
server {
	listen 80 default_server;
	server_name example.com;
	return 301 https://$host$request_uri;
}
server {
	listen 443 ssl default_server;
	server_name example.com;

	ssl on;
	ssl_certificate /etc/nginx/domainsslcert/example.com.crt;
	ssl_certificate_key /etc/nginx/domainsslcert/example.com.key;
	ssl_session_timeout 5m;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	ssl_ciphers replace-with-your-cipher;
	ssl_prefer_server_ciphers on;

	root /home/anony/tp5/public;
	index index.php index.html index.htm;

	location / {
		# try_files $uri $uri/ =404;
		# try_files $uri /index.php$is_args$args;
		if (!-e $request_filename) {
			rewrite ^(.*)$ /index.php?s=/$1 last;
		}
	}

	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
	}

	location ~ /\.ht {
		deny all;
	}
}
```

以上 ssl 配置部分参考腾讯云的文档

restart nginx

```bash
sudo service nginx status
sudo service nginx restart
sudo service nginx status
```

UFW

```bash
sudo ufw status
sudo ufw app list
sudo ufw allow 'Nginx HTTPS'
sudo ufw status
```

这时就可以用 https:// 访问了

### ThinkPHP 5.1.x 配置

ThinkPHP5.1完全开发手册  
https://www.kancloud.cn/manual/thinkphp5_1/353946

1. [请求 -> 参数绑定](https://www.kancloud.cn/manual/thinkphp5_1/353991)

	

	> 在使用路由定义的情况下 不建议使用顺序绑定  
	> URL参数方式改成顺序解析 'url_param_type'         => 1

	所以这个配置保留默认0, 参数绑定方式默认是按照变量名进行绑定


2. [路由](https://www.kancloud.cn/manual/thinkphp5_1/353960)

	> ThinkPHP5.1的路由定义更加对象化，并且默认开启路由（不能关闭），如果一个URL没有定义路由，则采用默认的PATH_INFO 模式访问URL

3. 系统常量

	thinkphp 5.1.x 系统常量用 `Env::get('runtime_path')` 来获取

	![2018-12-12-17-08-45-2018121217845](http://qiniu.xingtan.xyz/2018-12-12-17-08-45-2018121217845.png)

### easywechat

在 tp5 目录下安装 easywechat

https://www.easywechat.com/docs/master/installation

```bash
cd ~/tp5
which composer
composer require overtrue/wechat -vvv
```

报错，需要先安装 simplexml 扩展

https://www.limesurvey.org/es/comunidad/foros/installation-a-update-issues/113739-php-simplexml  
https://neilyamit.com/dev/simplexml-disabled.html

```bash
sudo apt install php7.2-xml
```

然后再次安装 easywechat

```bash
composer require overtrue/wechat -vvv
composer update
```

#### 使用 easywechat + thinkphp 来验证微信接口

```bash
> cd ~/tp5/
> cp -r application/index/ application/weixin
> cd application/weixin/controller/
> vi Index.php
```

https://www.easywechat.com/docs/master/official-account/tutorial  
https://www.easywechat.com/docs/master/official-account/configuration

easyWeChat 验证微信 token  
https://blog.csdn.net/frank_hanhan/article/details/80972192

```php
// ~/tp5/application/weixin/controller/Index.php
<?php
namespace app\weixin\controller;

use think\facade\Env;
use EasyWeChat\Factory;

class Index
{
	public function index()
	{
		$config = [
			'app_id'  => 'xxxxxxxxxxx',
			'secret'  => 'xxxxxxxxxxx',
			'token'   => 'xxxxxxxxxxx',
			'aes_key' => 'xxxxxxxxxxx',

			'response_type' => 'array',

			'log' => [
				'level' => 'debug',
				// 'file' => __DIR__.'/../../wechat.log',
				'file' => Env::get('runtime_path') .'wechat/wechat.log',
			],
		];
 
		$app = Factory::officialAccount($config);
		$response = $app->server->serve();
		$response->send(); // Laravel 里请使用：return $response;
	}
}
```

访问微信授权接口地址，报错，  

> ...wechat.log" could not be opened: failed to open stream: Permission denied

上面代码中配置的日志文件 `Env::get('runtime_path') .'wechat/wechat.log'` 不存在

```bash
> cd ~/tp5
> mkdir runtime/wechat
> touch runtime/wechat/wechat.log
> chmod a+rw runtime/wechat/wechat.log
```

> http://example.com/weixin/  
> 安全模式，验证成功

参考，未使用  
https://www.easywechat.com/docs/master/installation  
SDK 基础：EasyWeChat 的安装  
https://www.easywechat.com/lessons/1  
YII2中服务器端验证总是失败..  
https://github.com/overtrue/wechat/issues/1245  
https://github.com/qiqizjl/think-wechat

#### 模板消息

```php
// ~/tp5/application/weixin/controller/Index.php
<?php

namespace app\weixin\controller;

use think\facade\Env;
use EasyWeChat\Factory;

class Index
{
    protected $app = null;

    public function __construct()
    {
        $config = [
			'app_id'  => 'xxxxxxxxxxx',
			'secret'  => 'xxxxxxxxxxx',
			'token'   => 'xxxxxxxxxxx',
			'aes_key' => 'xxxxxxxxxxx',

            'response_type' => 'array',

            'log' => [
                'level' => 'debug',
                // 'file' => __DIR__.'/../../wechat.log',
                'file' => Env::get('runtime_path') . 'wechat/wechat.log',
            ],
        ];

        $this->app = Factory::officialAccount($config);
    }

    public function index()
    {
        // 接收 & 回复用户消息
        // https://www.easywechat.com/docs/master/official-account/tutorial
        $this->app->server->push(function ($message) {
            return "aha! Bro.";
        });

        $response = $this->app->server->serve();
        $response->send(); // Laravel 里请使用：return $response;
    }


//---------------------------------------------------
// comment out this function after test
    public function template()
    {
        // 模板消息
        // https://www.easywechat.com/docs/master/official-account/template_message
        $this->app->template_message->send([
            'touser' => 'the_user_open_id',
            'template_id' => 'the_template_id',
            'url' => 'https://press-to-go-there.com/your/package/info',
            'data' => [
                'first' => '你的包裹更新啦',
                'keyword1' => '43454251460',
                'keyword2' => '快件到达 【北京三里屯顺丰营业点】',
                'keyword3' => '2018-12-19 06:30',
                'remark' => '注意接听电话啊',
            ],
        ]);
    }

//    访问 /weixin/index/template，没有收到服务号发送的模板消息
//    报错如下
//    Use of undefined constant CURLOPT_IPRESOLVE - assumed 'CURLOPT_IPRESOLVE' (this will throw an Error in a future version of PHP)
//    https://github.com/overtrue/wechat/issues/1178
//    用以下的代码测试以下有没有按照 php curl 扩展
    /*public function curl()
    {
        var_dump(curl_version());
    }*/
//    Call to undefined function curl_version()
//    没有安装
//    sudo apt search php7.2-curl
//    sudo apt install php7.2-curl
//    sudo service nginx status
//    sudo service nginx restart
//    sudo service nginx status
//    安装完成
//      再次访问 /weixin/index/curl
//      array(9) { ["version_number"]=> int(473600) ......
//      ok，打印出了curl版本号等信息
//    再次访问 /weixin/index/template
//    网页一片空白，说明没有报错
//    微信上收到 服务号 发来的模板消息，大功告成！
//---------------------------------------------------
}
```

微信上收到 服务号 发来的模板消息，大功告成！

![](http://qiniu.xingtan.xyz/weixin-template-message.png)

测试成功后，就可以把上面的template代码注释了，  
因为实际运行时，需要用定时任务，按条件自动发送模板消息  

> - 比如，快递单号和用户的openid(加密一下？)关联关系存储在数据库中，  
	cron脚本定时查询快递接口，如果有更新，就给相关联的用户发送模板消息

> - 用户从菜单中进入开发的网页，网页中调用微信的jssdk，获取用户相关信息，  
	用户用表单提交复制粘贴的快递单号，关联关系建立起来了

> - cron 脚本(nodejs 定时任务？) 请求快递接口，所有未签收的快递单号，  
	如果有更新，就给相关联的用户发送模板消息  
	缓存？队列？  
	最终的发送调用还是要通过上面的template中的php代码？  
	easywechat还没有nodejs支持，可以自己写发送模板消息的nodejs中间件嘛！  
	https://github.com/51ding/koa-easywechat

> - 缓存，  
	快递单号和用户openid的关联关系存到数据库，同时缓存在redis里面，  
	快递单号最新的状态也缓存到redis，以及快递单号是否已经签收的标记，签收后就不再查询快递接口了。  
	
> - 队列，  
	定时任务(nodejs)抓取快递接口，如果该单号的最新状态和缓存之中的不一致，就需要发送模板消息，  
	把这个要发送模板消息的任务和相关数据，放进队列，  
	php再从队列中取出任务，一个一个发送出去

参考，未使用

easywechat的使用(laravel + easywechat 开发微信公众号(原创))  
https://cloud.tencent.com/developer/article/1326726

> 这个文章的模板消息部分提醒了我，用户的openid是存错在数据库里的，发送模板消息是主动发送的  
	疑问，用户的openid是怎么存储到数据库的呢，怎么第一次拿到用户的openid呢，  
	用户关注公众号这个动作能拿到openid吗？  
	还是需要用户关注以后，主动与公众号交互呢？  
	比如发送消息给公众号，或者从公众号的菜单进入微信网页(自己开发，调用jssdk)，然后拿到openid呢？

easywechat发送模板消息频繁超时小记  
https://www.luyuqiang.com/easywechat-curl-timeout  
使用EasyWechat开发java微信公众平台应用（二）——发送不同类型的消息  
http://www.voidcn.com/article/p-ketxjrgu-sw.html  
给你的网站加上公众号消息模板异常提醒吧  
https://hanc.cc/index.php/archives/155/  
微信调试，各种WebView样式调试、手机浏览器的页面真机调试。便捷的远程调试手机页面、抓包工具，支持：HTTP/HTTPS，无需USB连接设备。
https://github.com/wuchangming/spy-debugger


### 微信开发

#### 微信小程序商城构建全栈应用

百度云>全部文件>微信小程序商城构建全栈应用（下...>第9章微信登陆与令牌

https://coding.imooc.com/class/chapter/97.html#Anchor

#### 开发者工具

公众平台测试帐号  
[微信公众平台接口测试帐号申请](https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login)

在线接口调试工具
![2018-12-12-17-03-10-2018121217311](http://qiniu.xingtan.xyz/2018-12-12-17-03-10-2018121217311.png)

[微信web开发者工具](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1455784140)

[全新的 微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html)

> 为了帮助开发者简单和高效地开发和调试微信小程序，我们在原有的公众号网页调试工具的基础上，推出了全新的 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，集成了公众号网页调试和小程序调试两种开发模式。


### 问题

#### xdebug

查看phpinfo的输出信息，下载正确的xdebug版本，  
放到php\ext目录下，修改php.ini

```ini
[Xdebug]
zend_extension = D:\Program Files\xampp\php\ext\php_xdebug-2.5.5-5.6-vc11.dll
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
```

然后在phpstorm中配置调试服务器  
或者在VSCode中xdebug extension来调试

#### guzzlehttp

```bash
cd ~/tp5
composer require guzzlehttp/guzzle
```

```php
// ~/tp5/application/index/controller/Index.php
<?php
namespace app\index\controller;

use GuzzleHttp\Client;

class Index
{
	// ...

	public function http()
	{
		$client = new Client();
		$res = $client->request('GET', 'https://api.github.com/repos/guzzle/guzzle');
		return $res->getBody();
	}
}
```

访问 http://example.com/index/index/http 报错

> cURL error 60: SSL certificate problem: unable to get local issuer certificate (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)

> cURL error 35: OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to api.github.com:443 (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)

> cURL error 56: OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 0 (see http://curl.haxx.se/libcurl/c/libcurl-errors.html)

XAMPP - WAMPP PHP - SSL certificate error: unable to get local issuer certificate  https://ourcodeworld.com/articles/read/196/xampp-wampp-php-ssl-certificate-error-unable-to-get-local-issuer-certificate

https://medium.com/@badruayomaya/laravel-paystack-curl-error-e21ffbe9b384  
https://github.com/yabacon/paystack-php/wiki/cURL-error-60:-SSL-certificate-problem:-unable-to-get-local-issuer-certificate-(see-http:--curl.haxx.se-libcurl-c-libcurl-errors.html)  
https://stackoverflow.com/a/49071524

1. download https://curl.haxx.se/ca/cacert.pem  
2. move it to php ext directory or somewhere else  
3. edit php.ini

	```ini
	[curl]
	; A default value for the CURLOPT_CAINFO option. This is required to be an
	; absolute path.
	curl.cainfo = "absolute\path\to\cacert.pem"
	```

4. restart web server

使用Guzzle进行API测试  
https://php1024.com/posts/15.htm

#### curl

https://github.com/php-mod/curl

> 用这个包，没有上面guzzle遇到的问题，无需配置cacert.pem

```bash
composer require curl/curl
```

```php
$curl = new Curl\Curl();
$curl->get('http://www.example.com/');
```

#### ThinkPHP 返回格式 html => json

[响应->响应输出](https://www.kancloud.cn/manual/thinkphp5_1/353994)  
[响应->响应参数](https://www.kancloud.cn/manual/thinkphp5_1/353995)
```php
// 配置文件 ~/tp5/config/app.php
// 默认输出类型
'default_return_type'    => 'json',
// 访问的输出结果就变成了JSON字符串
```
或者这样设置header
```php
// ~/tp5/application/index/controller/Index.php
<?php
namespace app\index\controller;

use GuzzleHttp\Client;

class Index
{
	// ...

	public function http()
	{
		$client = new Client();
		$res = $client->request('GET', 'https://api.github.com/repos/guzzle/guzzle');
		// return $res->getBody();
		return response()->data($res->getBody())
			->header('Content-Type', 'text/json; charset=utf-8');
	}
}
```


### Chroma

还是用这个来高亮代码比较好，方便设置行号，高亮指定行

https://github.com/alecthomas/chroma