# 介绍

[![Language](https://img.shields.io/badge/Jekyll-Theme-blue)](https://github.com/juntong20XX/juntong20XX.github.io)
![license](https://img.shields.io/github/license/TMaize/tmaize-blog)

jekyll 主题 fork 自 [GitHub 地址](https://github.com/TMaize/tmaize-blog)。为了简介好看我删掉了原先大部分 ReadMe。文档查阅请看上游。

简洁纯净(主题资源请求<20KB)，未引入任何框架，秒开页面，支持自适应，支持全文检索，支持夜间模式。

你可以到[TMaize Blog](https://blog.tmaize.net/)查看主题效果 ，欢迎添加友链。

## 感谢

[JetBrains](https://www.jetbrains.com/?from=tmaize-blog) <font color="red">给上游</font>免费提供的开发工具[![JetBrains](./static/img/jetbrains.svg)](https://www.jetbrains.com/?from=tmaize-blog)

[夜间模式代码高亮配色](https://github.com/mgyongyosi/OneDarkJekyll)

# 项目配置

1. 如果使用自己的域名，`CNAME`文件里的内容请换成你自己的域名，然后 CNAME 解析到`用户名.github.com`

2. 如果使用 GitHub 的的域名，请删除`CNAME`文件，然后把你的项目修改为`用户名.github.io`

3. 修改`pages/about.md`中关于我的内容

4. 修改`_config.yml`文件，具体作用请参考注释

5. 清空`posts`和`_posts`目录下所有文件，注意是清空，不是删除这两个目录

6. 网站的 logo 和 favicon 放在了`static/img/`下，替换即可，大小无所谓，图片比例最好是 1:1

7. 如果你是把项目 fork 过去的，想要删除我的提交记录可以使用下面的命令

   ```
   git checkout --orphan temp
   git add . && git commit -m init
   git branch -D master
   git branch -m temp master
   git push --force
   ```

# 使用

文章放在`_posts`目录下，命名为`yyyy-MM-dd-xxxx-xxxx.md`，内容格式如下

```yaml
---
layout: mypost
title: 标题
categories: [分类1, 分类2]
---
文章内容，Markdown格式
```

文章资源放在`posts`目录，如文章文件名是`2019-05-01-theme-usage.md`，则该篇文章的资源需要放在`posts/2019/05/01`下，在文章使用时直接引用即可。当然了，写作的时候会提示资源不存在忽略即可

```md
![这是图片](xxx.png)

[xxx.zip 下载](xxx.zip)
```

## TODO

- 修改 _includes/script.html 
- 更新 pages/about.md
- 更新友情链接和留言
- 搞明白搜索
- （做梦）添加 i18n 支持

## 修改

- 在标题添加了 "新窗口打开" 的选项
