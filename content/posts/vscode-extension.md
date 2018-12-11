---
title: "Vscode Extension"
date: 2018-12-11T09:56:48+08:00
draft: false
---

### image

VsCode插件：七牛图床工具，写文章更快一步  
https://imys.net/20160726/vscode-extension-qiniu-upload.html  
https://github.com/yscoder/vscode-qiniu-upload-image

https://github.com/mushanshitiancai/vscode-paste-image  
https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image

https://github.com/favers/vscode-qiniu-upload-image  
https://marketplace.visualstudio.com/items?itemName=favers.paste-image-to-qiniu

> 这个插件综合了1和2的功能，我用的是这个

实现一个 vscode 粘贴图片的插件  
https://www.njleonzhang.com/2018/08/14/vs-code-paste-image.html  
https://github.com/njleonzhang/vscode-extension-mardown-image-paste  
https://github.com/njleonzhang/electron-image-ipc-server

> 这个是用 electron 的方式获取剪切板的图片，另辟蹊径

### dev

1. [x] https://code.visualstudio.com/docs/extensions/overview
2. [ ] https://code.visualstudio.com/docs/extensions/example-hello-world

https://code.visualstudio.com/docs/extensions/samples  
https://github.com/Microsoft/vscode-extension-samples  
https://code.visualstudio.com/docs/extensions/developing-extensions

How to Make A Visual Studio Code Extension [to Generate Ducks]  
https://itnext.io/how-to-make-a-visual-studio-code-extension-77085dce7d82  
https://css-tricks.com/what-i-learned-by-building-my-own-vs-code-extension/

vscode编写插件详细过程  
http://www.cnblogs.com/caipeiyu/p/5507252.html


#### dev paste to qiniu

https://github.com/favers/vscode-qiniu-upload-image  
这个插件配置有点问题，fork之后修改一下  
https://github.com/royswale/vscode-paste-image-upload-qiniu

![vscode-extension.md-20181211114438](d:/Code/github.com/royswale/gohugoblog/content/posts/vscode-extension.md-20181211114438.png)

![2018-12-11-11-49-40-20181211114941](http://qiniu.xingtan.xyz/2018-12-11-11-49-40-20181211114941.png)


### VSCode User Settings

有关图片粘贴上传的插件配置

```json
	// https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image
	// "pasteImage.path": "${currentFileNameWithoutExt}",
	// "pasteImage.insertPattern": "${imageSyntaxPrefix}${imageFilePath}${imageSyntaxSuffix}",
	// when write hexo post and paste image use post_asset_folder = true
	// "pasteImage.insertPattern": "{% asset_img ${imageFileName} %}",

	// qiniu
	// 插件开关
    "qiniu.enable": true,
    // 一个有效的七牛 AccessKey 签名授权
    "qiniu.access_key": "cMbpnA9FGbRGbKIWxp-iBbJJY64vxng-UN_2xOVw",
    // 一个有效的七牛 SecretKey 签名授权
    "qiniu.secret_key": "8cZbLacvJXroMzOebxdmelNAHz-TkoBvuBB6vMp9",
    // 七牛图片上传空间
    "qiniu.bucket": "gohugoblog",
    // 七牛图片上传路径，参数化命名，暂时支持 ${fileName}、${mdFileName}、${date}、${dateTime}
    // 示例：
    //   ${fileName}-${date} -> picName-20160725.jpg
    //   ${mdFileName}-${dateTime} -> markdownName-20170412222810.jpg
    "qiniu.remotePath": "${mdFileName}-${dateTime}",
    // 七牛图床域名
	"qiniu.domain": "http://qiniu.xingtan.xyz",
	

	//
	// 有效的七牛 AccessKey 签名授权
    "pasteImageToQiniu.access_key": "cMbpnA9FGbRGbKIWxp-iBbJJY64vxng-UN_2xOVw",
    // 有效的七牛 SecretKey 签名授权
    "pasteImageToQiniu.secret_key": "8cZbLacvJXroMzOebxdmelNAHz-TkoBvuBB6vMp9",
    // 七牛图片上传空间
    "pasteImageToQiniu.bucket": "gohugoblog",
    // 七牛图片上传路径，参数化命名，暂时支持 ${fileName}、${mdFileName}、${date}、${dateTime}
    // 示例：
    //   ${fileName}-${date} -> picName-20160725.jpg
    //   ${mdFileName}-${dateTime} -> markdownName-20170412222810.jpg
    // "pasteImageToQiniu.remotePath": "${mdFileName}-${dateTime}",
    "pasteImageToQiniu.remotePath": "${fileName}-${dateTime}",
    // 七牛图床域名
    "pasteImageToQiniu.domain": "http://qiniu.xingtan.xyz",
    // 本地储存位置
    "pasteImageToQiniu.localPath":"./img",
```