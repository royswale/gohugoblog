---
title: "hexo image to qiniu"
date: 2018-12-06T15:44:44+08:00
draft: false
---
## image process and store

### upload markdown image to qiniu

[使用七牛为Hexo存储图片等资源](https://yuchen-lea.github.io/2016-01-21-use-qiniu-store-file-for-hexo/) 
[Hexo Next主题博客功能完善](https://www.jianshu.com/p/6f77c96b7eff) 
[使用七牛云储存博客图片](https://marvae.github.io/2017-12-01/qiqiu/) 
[hexo图片迁移至七牛](http://farwmarth.com/hexo%E5%9B%BE%E7%89%87%E8%BF%81%E7%A7%BB%E8%87%B3%E4%B8%83%E7%89%9B/) 
https://github.com/gyk001/hexo-qiniu-sync


### current store image locally

https://hexo.io/docs/asset-folders.html
https://lucaseo.github.io/2018/02/20/howtodo-20180220-1/#activate-asset-folder

modify hexo `_config.yml`

```bash
post_asset_folder: true
```

I use VSCode to write post with [Paste Image](https://github.com/mushanshitiancai/vscode-paste-image) extension

```bash
"pasteImage.path": "${currentFileNameWithoutExt}",
"pasteImage.insertPattern": "{% asset_img ${imageFileName} %}",
```

configure above in "User Settings" in order to compliant to hexo's post assets folder convention.
