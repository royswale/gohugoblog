---
title: "Mongodb"
date: 2019-01-25T16:14:48+08:00
draft: true
---

### database as a service

https://mlab.com

https://cloud.mongodb.com

### client

https://studio3t.com/

> 安装试用过了

![mongodb-2019128113138](http://qiniu.xingtan.xyz/mongodb-2019128113138.png)

> 配置 proxy

此教程并非真正破解，而是通过重置studio 3t的试用时间解决的。每次开机重启脚本重置试用时间  
https://blog.csdn.net/justweb/article/details/83619685

```bat
@echo off
ECHO 重置Studio 3T的使用日期......
FOR /f "tokens=1,2,* " %%i IN ('reg query "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\3t\mongochef\enterprise" ^| find /V "installation" ^| find /V "HKEY"') DO ECHO yes | reg add "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\3t\mongochef\enterprise" /v %%i /t REG_SZ /d ""
ECHO 重置完成, 按任意键退出......
pause>nul
exit
```

将文件studio3t.bat文件移动到如下路径中 `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`

https://www.mongodb.com/products/compass