---
layout: post
title: Jekyll 环境配置
subtitle: 
categories: other
tags: [install]
---

## 简介
Jekyll是一个简单的静态网站生成器，用于生成个人，项目或组织的网站。 它由GitHub联合创始人汤姆·普雷斯顿·沃纳用Ruby编写，并根据MIT许可证发布。

Jekyll不使用数据库 ，用户通过编写Markdown、Textile或Liquid文件， 生成一个完整的静态网站，并且可以由Apache HTTP Server ， Nginx或其他Web服务器提供服务。 Jekyll是GitHub Pages的引擎。 

Jekyll非常灵活，可以与Bootstrap ， Semantic UI等前端框架结合使用。

Jekyll网站可以连接到基于云的CMS软件，例如CloudCannon，Forestry， Netlify或Siteleaf，使编辑者无需知道如何编程即可修改网站内容。

## Windows环境配置

### 下载和安装 Ruby
   直接下载Ruby+Devkit(x64)
   下载地址：https://rubyinstaller.org/downloads/
   下载完后，一路默认安装完成即可。
   cmd打开命令行工具，执行ruby -v命令，检测是否安装成功

```bash
C:\Users\mark>ruby -v
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x64-mingw-ucrt]
```

### 下载和安装RubyGems
   RubyGems是Ruby 的包管理器，可以类比为你电脑的下载中心。
   下载地址：https://rubygems.org/pages/download
   下载zip文件到本地，然后解压。
   cmd打开命令行工具，cd到解压目录，执行ruby setup.rb命令，进行安装：

```bash
ruby setup.rb
```

等待安装完成，执行`gem -v`命令，检测是否安装成功：

```bash
C:\Users\mark>gem -v
3.3.13
```

切换镜像源

```bash
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l
```

安装`Jekyll`

```bash
gem install jekyll
```

安装`Jekyll-paginate`

```bash
gem install jekyll-paginate
```

验证jekyll

```bash
C:\Users\mark>jekyll -v
jekyll 4.2.2
```

安装`Bundle`，用于自动安装其他所需的程序

```
gem install bundler
```

### 本地启动服务

在命令行中切换到你的网站仓库内，执行 `jekyll serve`

```bash
D:\mark\Jekyll\MrGreen-JekyllTheme>jekyll serve
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/resolver.rb:269:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'github-pages (~> 223)' in locally installed gems. (Bundler::GemNotFound)
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/resolver.rb:252:in `map!'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/resolver.rb:252:in `verify_gemfile_dependencies_are_found!'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/resolver.rb:48:in `start'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/resolver.rb:23:in `resolve'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/definition.rb:270:in `resolve'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/definition.rb:473:in `materialize'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/definition.rb:191:in `specs'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/definition.rb:239:in `specs_for'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/runtime.rb:18:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler.rb:151:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/plugin_manager.rb:52:in `require_from_bundler'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/exe/jekyll:11:in `<top (required)>'
        from C:/Ruby31-x64/bin/jekyll:32:in `load'
        from C:/Ruby31-x64/bin/jekyll:32:in `<main>'

### 上面报了个错，然后执行该命令去fix，`bundle install`，它会下载一些依赖。然后继续执行 jekyll serve
D:\mark\Jekyll\MrGreen-JekyllTheme>jekyll server
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/runtime.rb:309:in `check_for_activated_spec!': You have already activated i18n 1.10.0, but your Gemfile requires i18n 0.9.5. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/runtime.rb:25:in `block in setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/spec_set.rb:136:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/spec_set.rb:136:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/runtime.rb:24:in `map'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler/runtime.rb:24:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/bundler-2.3.13/lib/bundler.rb:151:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/plugin_manager.rb:52:in `require_from_bundler'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/exe/jekyll:11:in `<top (required)>'
        from C:/Ruby31-x64/bin/jekyll:25:in `load'
        from C:/Ruby31-x64/bin/jekyll:25:in `<main>'

## 发现还是报错了，但是明显错误少了很多。这次依次执行 bundle add webrick 和 bundle exec jekyll serve 去fix 这个错

D:\mark\Jekyll\MrGreen-JekyllTheme>bundle add webrick

[!] There was an error parsing `injected gems`: You cannot specify the same gem twice with different version requirements.
You specified: webrick (~> 1.7) and webrick (>= 0). Gem already added. Bundler cannot continue.

 #  from injected gems:1
 #  -------------------------------------------
 >  gem "webrick", ">= 0"
 #  -------------------------------------------
 
D:\mark\Jekyll\MrGreen-JekyllTheme>bundle exec jekyll serve
Configuration file: D:/mark/Jekyll/MrGreen-JekyllTheme/_config.yml
            Source: D:/mark/Jekyll/MrGreen-JekyllTheme
       Destination: D:/mark/Jekyll/MrGreen-JekyllTheme/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 23.498 seconds.
 Auto-regeneration: enabled for 'D:/mark/Jekyll/MrGreen-JekyllTheme'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

ok，服务启动成功，我们使用 http://127.0.0.1:4000/ 访问就好了 。
