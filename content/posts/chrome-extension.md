---
title: "Chrome Extension"
date: 2018-12-27T13:48:48+08:00
draft: true
---

### docs

https://developer.chrome.com/extensions/devguide

- [x] https://developer.chrome.com/extensions/getstarted

https://developer.chrome.com/extensions/background_pages  

https://developer.chrome.com/extensions/content_scripts  
https://developer.chrome.com/extensions/content_scripts#pi

https://developer.chrome.com/extensions/declarativeContent  
https://developer.chrome.com/extensions/pageAction  
https://developer.chrome.com/extensions/declare_permissions#host-permissions  
https://developer.chrome.com/extensions/activeTab  
https://developer.chrome.com/extensions/user_interface  
https://developer.chrome.com/extensions/match_patterns

https://developer.chrome.com/extensions/contextMenus  
https://developer.chrome.com/extensions/samples#search:contextmenus

https://developer.chrome.com/extensions/options  
https://developer.chrome.com/extensions/tabs


### cookie

Chrome 插件获取指定域下的Cookie，代码示例  
https://gist.github.com/neekey/6485525

Chrome Extension 筆記(07)針對 cookie 進行讀寫  
https://ithelp.ithome.com.tw/articles/10128585

Getting cookies in a google chrome extension  
https://stackoverflow.com/questions/5892176/getting-cookies-in-a-google-chrome-extension

Intercepting Cookies in a Chrome extension  
http://blob.tomerweller.com/cookie-interception-chrome-extension


### 监控网页发出的请求

开发chrome extension抓取页面数据，并传输到另一页面  
https://blog.csdn.net/m0_37729058/article/details/79485302

Chrome插件：拦截页面请求  
https://blog.csdn.net/hhmouse111/article/details/36901527

Look up all domain access requests of Firefox extensions before installation  
https://www.ghacks.net/2018/07/10/look-up-all-domain-access-requests-of-firefox-extensions-before-installation/


### read file

Reading files in a Chrome Extension  
https://dev.to/aussieguy/reading-files-in-a-chrome-extension--2c03

Open (Import) file in a chrome extension  
https://stackoverflow.com/questions/26884140/open-import-file-in-a-chrome-extension

### manipulate page

for example, autofill form input

```js
domElement.trigger(new Event('click'))
```

### parse page dom

https://stackoverflow.com/questions/19758028/chrome-extension-get-dom-content  
https://stackoverflow.com/a/40906564

> You don't have to use the message passing to obtain or modify DOM.

### xhr

https://developer.chrome.com/extensions/xhr#requesting-permission

> Instead, prefer safer APIs that do not run scripts:

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", "https://api.example.com/data.json", true);
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    // JSON.parse does not evaluate the attacker's scripts.
    var resp = JSON.parse(xhr.responseText);
  }
}
xhr.send();
```

### tab

https://stackoverflow.com/questions/16503879/chrome-extension-how-to-open-a-link-in-new-tab

### download

https://developer.chrome.com/extensions/downloads  
https://developer.chrome.com/extensions/samples#search:downloads  
Download Images  
Displays all webpage images and allows user to download  
https://developer.chrome.com/extensions/examples/extensions/download_images.zip


https://stackoverflow.com/questions/2153979/chrome-extension-how-to-save-a-file-on-disk  
https://stackoverflow.com/questions/4845215/making-a-chrome-extension-download-a-file/24162238

### publish

https://developer.chrome.com/extensions/hosting

### tutorial

Creating a Chrome extension in 2018: The good, the bad and the meh  
https://checklyhq.com/blog/2018/08/creating-a-chrome-extension-in-2018-the-good-the-bad-and-the-meh/

How to make a Chrome browser extension from scratch | Understanding Chrome extension anatomy  
https://medium.com/front-end-weekly/how-to-make-a-chrome-browser-extension-from-scratch-chrome-extension-development-basics-basic-ba1daee11123

How to Create a Gist Download Chrome Extension Using React  
https://www.telerik.com/blogs/how-to-create-a-gist-download-chrome-extension-using-react  
https://github.com/christiannwamba/gist-download-extension

> 这个非常好，和我的需求一致

```js
cd xxx
yarn
yarn build
yarn watch
```

### debug

1. chrome.tabs.executeScript 不指定 tab id 的话，回调函数接收不到返回值

2. chrome.contextMenus.create 的属性 onclick 报错的话，用 onClicked

    https://developer.chrome.com/extensions/contextMenus

    > A function that is called back when the menu item is clicked. Event pages cannot use this; instead, they should register a listener for contextMenus.onClicked.

	https://developer.chrome.com/extensions/contextMenus#event-onClicked

	```js
	chrome.contextMenus.onClicked.addListener(function callback)
	```

    https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/8504764/  
    https://stackoverflow.com/questions/13166293/about-chrome-tabs-executescript-id-details-callback  
	http://www.krasimirtsonev.com/blog/article/Chrome-Extension-run-JavaScript-in-the-context-of-the-current-page

### firefox

https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions