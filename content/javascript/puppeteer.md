---
title: "Puppeteer"
date: 2018-12-29T15:05:40+08:00
draft: true
---

### docs

https://github.com/GoogleChrome/puppeteer

#### api

https://github.com/GoogleChrome/puppeteer/blob/v1.11.0/docs/api.md#  
https://zhaoqize.github.io/puppeteer-api-zh_CN/#/

#### examples

https://github.com/GoogleChrome/puppeteer/tree/master/examples/

https://github.com/GoogleChromeLabs/puppeteer-examples

- [ ] https://github.com/checkly/puppeteer-examples

> 这个仓库中的里够多了，参考参考

https://puppeteersandbox.com/  
https://checklyhq.com/

https://github.com/emadehsan/thal  
https://medium.com/@e_mad_ehsan/getting-started-with-puppeteer-and-chrome-headless-for-web-scrapping-6bf5979dee3e

Google Puppeteer tutorial : 12 examples to play with  
https://www.aymen-loukil.com/en/blog-en/google-puppeteer-tutorial-with-examples/

### install

添加 chromium 镜像源  
https://github.com/cnpm/cnpmjs.org/issues/1246

```js
PUPPETEER_DOWNLOAD_HOST=https://storage.googleapis.com.cnpmjs.org npm i puppeteer
```

Puppeteer 基本概念介绍  
https://blog.yeeef.com/post/puppeteer-introduction/  
puppeteer环境部署问题小记  
https://gitissue.com/issues/5b11a749547b051cfd9e270c


https://pptr.dev/#?product=Puppeteer&version=v1.11.0&show=api-environment-variables

> PUPPETEER_DOWNLOAD_HOST - overwrite host part of URL that is used to download Chromium

![](http://qiniu.xingtan.xyz/install%20puppeteer.png)

![](http://qiniu.xingtan.xyz/puppeteer.png)

> 注意puppeteer 安装到全局，代码里找不到，把它复制到本地项目下 .\node_modules\puppeteer 
> C:\Users\Administrator\AppData\Roaming\npm\node_modules\puppeteer


测试一下 example

https://pptr.dev/

```js
// example.js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
//   await page.goto('https://example.com');
  await page.goto('https://google.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```

```bash
node example.js
```

测试通过，保存了网页的截图

> 复制到项目目录下之后，可以正常使用，但是发现项目下还没有package.json文件

![](http://qiniu.xingtan.xyz/puppeteer-install-local.png)

> 于是重新安装，比较快，可能是缓存的原因，chromium下载的很快

![](http://qiniu.xingtan.xyz/no-package-json-but-lock.png)

> 安装完成之后还是没有 package.json 生成了一个 package-lock.json
> 重新测试了一下 sample 成功

### login to website

https://stackoverflow.com/questions/50074799/how-to-login-in-puppeteer

### other

https://developers.google.com/web/tools/puppeteer/

https://pptr.dev/

Using Puppeteer for Easy Control Over Headless Chrome  
https://alligator.io/tooling/puppeteer/

https://adonisjs.com/docs/4.0/browser-tests

Getting Started Using Puppeteer & Headless Chrome for End-to-End Testing  
https://itnext.io/getting-started-using-puppeteer-headless-chrome-for-end-to-end-testing-8487718e4d97


### 也许可以用其它脚本工具来做自动化的网页操作

### selenium

Modern Web Automation With Python and Selenium  
https://realpython.com/modern-web-automation-with-python-and-selenium/  
Controlling the Web with Python  
https://towardsdatascience.com/controlling-the-web-with-python-6fceb22c5f08  
Chapter 11 – Web Scraping  
https://automatetheboringstuff.com/chapter11/


- [ ] [Python+Selenium自动化操作网页](http://blog.xulihang.me/python-selenium-website-automation/)

	> 本来想这种自动化操作可以用autoit，但autoit的语言风格我现在实在不适应


#### auto hot key

https://www.autohotkey.com/  
https://github.com/Lexikos/AutoHotkey_L/
https://www.autohotkey.com/docs/AutoHotkey.htm  
网页自动化初级教程  
https://www.autohotkey.com/boards/viewtopic.php?t=4311

[Library] Chrome.ahk - Automate Google Chrome using native AutoHotkey. No Selenium!  
https://www.autohotkey.com/boards/viewtopic.php?t=42890  
https://github.com/G33kDude/Chrome.ahk

https://sumtips.com/snippets/autohotkey/control-youtube-playback-in-chrome-with-autohotkey/

Automating Chrome with AutoHotkey: Setting InnerText, OuterHTML & Values  
https://the-automator.com/setting-text-in-chrome-with-autohotkey/  
Use AutoHotkey to Automate Chrome- Getting values  
https://www.youtube.com/watch?v=3vvvasxJjVw  
http://the-automator.com/web-scraping-with-autohotkey/


### 浏览器插件

Tampermonkey 脚本尝试与天马行空的想法  
https://testerhome.com/topics/15248

Chrome 自动化交互利器：用 tampermonkey 向页面注入自定义 Javascript  
https://my.oschina.net/leejun2005/blog/636155

用Tampermonkey实现自动化网页操作  
http://qinghua.github.io/tampermonkey/

> 这个不错哦

> 还是试一试我开发的 chrome 插件，看能不能搞定需求