---
title: '搭建学术型博客系统'
date: 2023-08-30
permalink: /posts/2023/08/blog-post-mianfei xueshu blog dajian/
tags:
  - 网站
  - 免费
  - 博客
  - 原创
  - 静态
---

域名免费、空间免费、模板免费，可通过第三方服务接口可实现更多功能：线上聊天预约、站点留言功能、访客记录、访客分布动态地球仪...

使用到的免费资源
======

模板由[ academicpages](https://github.com/academicpages/academicpages.github.io) 提供，该模板是从迈克尔·罗斯（Michael Rose）创建的 [Minimal Mistakes Jekyll Theme](https://mmistakes.github.io/minimal-mistakes/)  Fork 而来的，然后扩展到支持学者拥有的内容类型：出版物，演讲，教学，作品集，博客文章和动态生成的简历；空间和域名由 GitHub Pages 提供，[GitHub pages](https://pages.github.com) 是一项免费服务，其中网站从存储在 GitHub 存储库中的代码和数据构建和托管，并在对存储库进行新提交时自动更新。

主要过程
======
前提是拥有 github 账号：
1. 参照模板的说明，将项目 fork 到自己这边
2. 在 setting 里边重命名这个项目（得以你的 github username 为前缀才行，如我的是：lygwys.github.io）
3. 修改配置文件 _config.yml 中的 url 和 repository
4. 等一会儿就能在 url 看到东西了，是默认的一些页面
5. 可以根据 guide，去 _data/navigation.yml 设置顶部菜单，并且对每个栏目的内容进行修改

本地运行
======
1. 所需软件及版本参考
- Ruby 3.1.2-1
###### [下载 Ruby+Devkit 3.1.2-1](https://rubyinstaller.org/downloads/archives/)
可以自定义安装路径，最后的界面键入“3”回车以继续完成剩余的安装
- gem install bundler -v 2.5.8
- gem install jekyll -v 4.3.3
2. cmd 进入博客目录
3. 安装依赖 bundle install
4. 本地运行 bundle exec jekyll serve
###### 如果运行不成功，可删除 Gemfile.lock 文件，重新运行 bundle install
5. 访问地址 [http://127.0.0.1:4000/](http://127.0.0.1:4000/)


这儿有一篇[可能是最全面的 github pages 搭建个人博客教程](https://lemonchann.github.io/create_blog_with_github_pages/)，推荐阅读。
