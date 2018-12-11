---
title: "Git"
date: 2018-12-11T09:11:25+08:00
draft: false
---

- [ ] https://www.atlassian.com/git/tutorials

### submodule

#### 检出项目的同时也检出子模块

```bash
git clone git@github.com:royswale/gohugoblog.git --recursive
```
https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97

> 不过还有更简单一点的方式。 如果给 git clone 命令传递 --recursive 选项，它就会自动初始化并更新仓库中的每一个子模块。