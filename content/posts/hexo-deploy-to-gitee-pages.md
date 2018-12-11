---
title: "hexo deploy to gitee pages"
date: 2018-12-06T13:42:02+08:00
draft: false
---

## deply hexo to gitee pages

### quick start

https://github.com/hexojs/hexo#installation 
https://github.com/hexojs/hexo#quick-start

```
npm i -g hexo-cli
hexo init blog
cd blog
hexo server
hexo new "a new blog"
hexo generate
```

create a **private** repo to hold your hexo raw data and configurations. 
`https://gitee.com/yourname/whatever.git`

### 码云Pages

[码云Pages 常见问题](https://gitee.com/help/articles/4136#article-header1)

> 如何创建一个首页访问地址不带二级目录的 pages，如ipvb.gitee.io？
> 你需要建立一个与自己个性地址同名的项目

create a **public** repo `https://gitee.com/yourname/yourname.git`

[码云Pages 一个小白的Pages搭建之旅](https://gitee.com/help/articles/4136#article-header2)

### depoly

[部署 Git](https://hexo.io/zh-cn/docs/deployment.html#Git)

```
npm install hexo-deployer-git --save
```

_config.yml

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://gitee.com/yourname/yourname.git
```

when ready to deploy

```
hexo generate
hexo deploy
```

go to `https://yourname.gitee.io/`