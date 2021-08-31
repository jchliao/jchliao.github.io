---
layout: post
title: Hello, jekyll
categories: jekyll
description: 在GitHub上部署jekyll
keywords: jekyll, Github
---

# jekyll在github上部署

主要参考 [GitHub Pages官方文档](https://docs.github.com/cn/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll) 的步骤进行部署，但是在过程中遇到了一些问题，现在记录下来。

## 安装Jekyll
在[Jekyll官文档](https://www.jekyll.com.cn/docs/installation/)上看到要先安装 Ruby 这个软件，macOS 上自带的 Ruby 属于系统软件使用时需要加 `sudo` 命令。
可以通过 Homebrew，安装最新版 Ruby。
```bash
brew install ruby
#顺便添加环境变量
export PATH=/usr/local/opt/ruby/bin:/usr/local/lib/ruby/gems/3.0.0/bin:$PATH
#安装 Jekyll:
gem install jekyll bundler
#安装完成后换源，节约生命
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```

按照 GitHub Pages 文档走，到修改 Gemfile：注释掉 `gem "jekyll"`，把 `#gem "github-pages"` 的#删掉，再添加`gem "webrick"` webrick插件是用来支持本地运行测试的。

换主题后提示layouts文件缺失，可以新建_layouts文件夹，把对应的文件模版添加进去。可以把默认的文件拷贝过来，
```bash
open $(bundle show minima)
```
