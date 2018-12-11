---
title: "Hugo Setup"
date: 2018-12-10T19:34:01+08:00
draft: false
---

### install hugo
https://gohugo.io/getting-started/installing/#binary-cross-platfor

download hugo .exe and put it in PATH, for example, put hugo.exe in GOPATH/bin is ok.

### init a blog

https://gohugo.io/getting-started/quick-start/#step-2-create-a-new-site

```bash
hugo version
hugo new site blog
```

### add a theme

https://gohugo.io/getting-started/quick-start/#step-3-add-a-theme

```bash
cd blog
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
# Edit your config.toml configuration file
# and add the Ananke theme.
echo 'theme = "ananke"' >> config.toml
```

### add some content

https://gohugo.io/getting-started/quick-start/#step-4-add-some-content

```bash
hugo new posts/my-first-post.md
# Edit the newly created content file if you want.
# Now, start the Hugo server with drafts enabled:
hugo server -D
```

### customize

https://gohugo.io/getting-started/quick-start/#step-5-customize-the-theme

edit `config.toml`

```toml
# baseURL = "http://example.org/"
baseURL = "https://royswale.github.io/"
languageCode = "en-us"
# title = "My New Hugo Site"
title = "Royswale Code Garden"
theme="ananke"
```

### publish to github pages

https://gohugo.io/hosting-and-deployment/hosting-on-github/#github-user-or-organization-pages

https://gohugo.io/hosting-and-deployment/hosting-on-github/#step-by-step-instructions

1. Create a <YOUR-PROJECT> (e.g. blog) repository on GitHub. This repository will contain Hugo’s content and other source files.
2. Create a <USERNAME>.github.io GitHub repository. This is the repository that will contain the fully rendered version of your Hugo website.
3. git clone <YOUR-PROJECT-URL> && cd <YOUR-PROJECT>
4. Make your website work locally (hugo server or hugo server -t <YOURTHEME>) and open your browser to http://localhost:1313.
5. Once you are happy with the results:
6. Press Ctrl+C to kill the server
7. rm -rf public to completely remove the public directory
8. git submodule add -b master git@github.com:<USERNAME>/<USERNAME>.github.io.git public. This creates a git submodule. Now when you run the hugo command to build your site to public, the created public directory will have a different remote origin (i.e. hosted GitHub repository). You can automate some of these steps with the following script.

把 `blog` 目录用 `Git` 管理起来, [`https://github.com/royswale/gohugoblog`](https://github.com/royswale/gohugoblog)

https://pages.github.com/ 创建一个仓库 https://github.com/royswale/royswale.github.io

然后 `rm -rf public` 把开发时生成的目录删除

然后执行 `git submodule add -b master git@github.com:royswale/royswale.github.io.git public`

> submodule 的配置信息可以在 `blog/.git` 目录下的 `modules` 目录 和 `config` 文件看到，可以手动删除

#### deploy script

https://gohugo.io/hosting-and-deployment/hosting-on-github/#put-it-into-a-script

把部署脚本原样复制粘贴, 保存到 blog/deploy.sh  
部署之前执行 `hugo`  
然后执行 `./deploy.sh "commit message"`  
[https://royswale.github.io](https://royswale.github.io)

### draft

https://gohugo.io/getting-started/usage/#draft-future-and-expired-content

* Content with a future publishdate value
* Content with draft: true status
* Content with a past expirydate value

https://gohugo.io/commands/hugo_list_drafts

`hugo list drafts`

### 代码高亮

https://gohugo.io/content-management/syntax-highlighting/#example-highlight-shortcode  
https://gohugo.io/content-management/shortcodes/#highlight  

主要参考这个  
[**https://github.com/halogenica/beautifulhugo#syntax-highlighting**](https://github.com/halogenica/beautifulhugo#syntax-highlighting)

[**Pygments to Chroma Migration**](https://sheppard.in/2017/pygments-to-chroma-migration/)

[**Adding Custom Chroma Styles to Hugo Themes**](https://parsiya.net/blog/2018-04-15-adding-custom-chroma-styles-to-hugo-themes/)


#### Chroma

https://github.com/alecthomas/chroma

https://gohugo.io/content-management/syntax-highlighting/

> From Hugo 0.28, the default syntax hightlighter in Hugo is Chroma; it is built in Go and is really, really fast – and for the most important parts compatible with Pygments.

配置 config.toml

```toml
pygmentsCodefences = true
# pygmentsUseClasses = true
```

https://gohugo.io/content-management/syntax-highlighting/#generate-syntax-highlighter-css

#### Pygments

https://gohugo.io/getting-started/installing/#install-pygments-optional  
http://pygments.org/  
https://gohugo.io/content-management/syntax-highlighting/#highlight-with-pygments-classic

Run
```bash
pypi install Pygments
```

配置 config.toml

```toml
pygmentsUseClassic = true
```

#### Highlight.js

https://highlightjs.org/

https://www.bootcdn.cn/highlight.js/

Hugo中添加代码高亮支持  
http://note.qidong.name/2017/06/24/hugo-highlight/

Chroma 和 Pygments 都是服务端渲染的方式，都需要安装 python 包 Pygments?

但是他们可以高亮代码的某几行和设置代码起始行号，不知道highlight.js可不可以?

### in-depth with hugo theme

https://forestry.io/blog/up-and-running-with-hugo/

#### custom this ananke theme

copy from `themes/ananke/layouts/partials/site-footer.html` to `gohugoblog/layouts/partials/site-footer.html`  
append highlight.js code to the bottom

```html
<link href="https://cdn.bootcss.com/highlight.js/9.13.1/styles/solarized-dark.min.css" rel="stylesheet">
<script src="https://cdn.bootcss.com/highlight.js/9.13.1/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

copy from `themes/ananke/layouts/_default/single.html` to `gohugoblog/layouts/_default/single.html`  
remove the width 66.666% class `w-two-thirds-l` to make the main part wider.

```html
<!-- <main class="nested-copy-line-height lh-copy serif f4 nested-links nested-img mid-gray pr4-l w-two-thirds-l"> -->
<main class="nested-copy-line-height lh-copy serif f4 nested-links nested-img mid-gray pr4-l">
```

所以说，根目录下的模板文件可以覆盖主题的同名文件吗?

#### awesome themes

https://github.com/yscoder/hexo-theme-indigo

https://github.com/kaeyleo/jekyll-theme-H2O

https://github.com/theme-next/hexo-theme-next

### comment plugin

https://valine.js.org/

disqus?