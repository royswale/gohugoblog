---
title: "How to install LEMP on Ubuntu 18.04"
date: 2018-12-06T11:44:23+08:00
draft: false
---

## How to install LEMP on Ubuntu 18.04

主要是按照这个文章来的  
How To Install Linux, Nginx, MySQL, PHP (LEMP stack) on Ubuntu 18.04  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-ubuntu-18-04


### Initial setup

https://linuxconfig.org/allow-ssh-root-login-on-ubuntu-18-04-bionic-beaver-linux  
腾讯云的 ubuntu 18 默认不允许 root 登陆，可以用ubuntu登陆，我也不做修改  
而 digital ocean 的ubuntu 18 的droplet是可以root登陆ubuntu不可以登陆

```bash
ssh ubuntu@your_server_ip
```

> 用ubuntu用户名登陆，完成一些初始化操作

Initial Server Setup with Ubuntu 18.04  
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04

#### 创建新用户 赋予sudo权限

```bash
> sudo adduser anony
> sudo usermod -aG sudo anony
```

#### 开启 ufw

UFW Essentials: Common Firewall Rules and Commands
https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands
https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-18-04/


```bash
> sudo ufw status
Status: inactive
> sudo ufw app list
Available applications:
  OpenSSH
> sudo ufw allow OpenSSH
Rules updated
Rules updated (v6)
> sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)

```

#### SSH Key Authentication

> 这个未配置

How to Set Up SSH Keys on Ubuntu 18.04  
https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1804



### Nginx

用上面创建的新用户 `anony` 登陆，完成后续操作

#### install nginx

```bash
> sudo apt update
> sudo apt install nginx
> sudo service nginx status
```

#### enable http port 80

> It is recommended that you enable the most restrictive profile that will still allow the traffic you want. Since you haven't configured SSL for your server in this guide, you will only need to allow traffic on port 80.

注意  
How To Secure Nginx with Let's Encrypt on Ubuntu 18.04  
https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04

> ssl 和 HTTPS 以后再配置  
> 配置HTTPS证书的时候，别忘了 `sudo ufw allow 'Nginx HTTPS'`

```bash
> sudo ufw app list
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)

> sudo ufw allow 'Nginx HTTP'
Rule added
Rule added (v6)
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)

```

这时访问你的服务器 IP，就可以看到nginx的默认欢迎页面了


#### hide nginx version

How to Hide Nginx Server Version in Linux  
https://www.tecmint.com/hide-nginx-server-version-in-linux/

```bash
> sudo vi /etc/nginx/nginx.conf
# add or uncomment this line
server_tokens off;
> sudo nginx -t
> sudo service nginx status
> sudo service nginx restart
> sudo service nginx status
```

### MySQL

Step 2 – Installing MySQL to Manage Site Data  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-ubuntu-18-04#step-2-%E2%80%93-installing-mysql-to-manage-site-data

```bash
> sudo apt install mysql-server
> sudo mysql_secure_installation
VALIDATE PASSWORD PLUGIN
y
2
left option all answer y
```

> Note that in Ubuntu systems running MySQL 5.7 (and later versions), the root MySQL user is set to authenticate using the auth_socket plugin by default rather than with a password.

我没有修改这个默认方式，mysql 的 root 用户还是用 auth_socket

以上mysql安装过程是按照上面的 digitalocean 的文章来的，下面不是

```bash
> sudo mysql
```

create a database

```sql
mysql> create database tablename;
```

create a new user and grant privilege to that database

https://dev.mysql.com/doc/refman/5.7/en/adding-users.html

```sql
mysql> CREATE USER 'finley'@'localhost' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON tablename.* TO 'finley'@'localhost';
mysql> CREATE USER 'finley'@'%' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON tablename.* TO 'finley'@'%';
mysql> FLUSH PRIVILEGES;
mysql> show grants for 'finley'@'localhost';
mysql> show grants for 'finley'@'%';
mysql> exit;
```

> The 'finley'@'localhost' account is necessary if there is an anonymous-user account for localhost. Without the 'finley'@'localhost' account, that anonymous-user account takes precedence when finley connects from the local host and finley is treated as an anonymous user. The reason for this is that the anonymous-user account has a more specific Host column value than the 'finley'@'%' account and thus comes earlier in the user table sort order.

修改 mysql 配置文件的 bind-address

https://www.techrepublic.com/article/how-to-set-up-mysql-for-remote-access-on-ubuntu-server-16-04/ 
https://www.digitalocean.com/community/questions/how-do-i-open-port-3306-with-ufw

```bash
> sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
# bind-address = 127.0.0.1
bind-address = 0.0.0.0
> sudo service mysql status
> sudo service mysql restart
```

ufw (Ubuntu FireWall)

https://www.spritle.com/blogs/2015/02/25/how-to-enable-mysql-for-remote-access/

```bash
> sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp (OpenSSH)           ALLOW IN    Anywhere
80/tcp (Nginx HTTP)        ALLOW IN    Anywhere
22/tcp (OpenSSH (v6))      ALLOW IN    Anywhere (v6)
80/tcp (Nginx HTTP (v6))   ALLOW IN    Anywhere (v6)

```

test if can access 3306 port

```bash
> telnet your_server_ip 3306

Trying your_server_ip...
```

```bash
> sudo ufw allow 3306
Rule added
Rule added (v6)
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
3306                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
3306 (v6)                  ALLOW       Anywhere (v6)

```

```bash
> telnet your_server_ip 3306

Trying your_server_ip...
Connected to your_server_ip.
```

这时用 Navicat Premium 也可以连接上这个用户的数据库了

### PHP

Step 3 – Installing PHP and Configuring Nginx to Use the PHP Processor  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-ubuntu-18-04#step-3-%E2%80%93-installing-php-and-configuring-nginx-to-use-the-php-processor

1. install php

	```bash
	sudo apt update
	sudo apt install php-fpm php-mysql
	```

2. add new server block

	```bash
	sudo vi /etc/nginx/sites-available/example.com
	```

	```nginx
	server {
		listen 80;
		root /var/www/html;
		index index.php index.html index.htm index.nginx-debian.html;
		server_name example.com;

		location / {
			try_files $uri $uri/ =404;
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

	Enable your new server block by creating a symbolic link from your new server block configuration file (in the /etc/nginx/sites-available/ directory) to the /etc/nginx/sites-enabled/ directory:

	```bash
	sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
	```

	Then, unlink the default configuration file from the /sites-enabled/ directory:

	```bash
	sudo unlink /etc/nginx/sites-enabled/default
	```

	test your configuration and restart

	```bash
	sudo nginx -t
	sudo service nginx restart
	sudo service nginx status
	```

	最后用info.php测试一下

	```php
	<?php
	phpinfo();
	```

### HTTPS

把 SSL 证书文件放到某个目录  
配置 Nginx 的 server block 相关 https 配置

```bash
sudo nginx -t
sudo service nginx status
sudo service nginx restart
sudo service nginx status
```

别忘了防火墙允许https

```bash
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
3306                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
3306 (v6)                  ALLOW       Anywhere (v6)

> sudo ufw allow "Nginx HTTPS"
Rule added
Rule added (v6)
> sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
3306                       ALLOW       Anywhere
Nginx HTTPS                ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
3306 (v6)                  ALLOW       Anywhere (v6)
Nginx HTTPS (v6)           ALLOW       Anywhere (v6)
```

Bingo!