title: hexo  hexo-theme-next 搭建blog系统
author: TryCatch
date: 2017-05-16 11:29:35
tags:
---
##### 下载安装hexo

```
$ npm install -g hexo-cli
```
安装好hexo以后，在终端输入：
```
$ hexo
```
得到如下信息
```
➜  guoyoujin.github.io git:(develop) ✗ hexo
Usage: hexo <command>
Commands:
  clean     Removed generated files and cache.
  config    Get or set configurations.
  deploy    Deploy your website.
  generate  Generate static files.
  ----------------------部分省略
or you can check the docs: http://hexo.io/docs/
```
##### 初始化博客
```
// 建立一个博客文件夹，并初始化博客，guoyoujin.github.io为文件夹的名称，可以随便起名字
$ hexo init guoyoujin.github.io
// 进入博客文件夹，guoyoujin.github.io为文件夹的名称
$ cd guoyoujin.github.io
// node.js的命令，根据博客既定的dependencies配置安装所有的依赖包
$ npm install
```
##### hexo next them 使用手册
http://theme-next.iissnan.com/getting-started.html
##### 安装next主题
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
##### 启用主题
与所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 站点配置文件， 找到 theme 字段，并将其值更改为 next。
```
启用 NexT 主题
theme: next
```
到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。在切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存。
##### 验证主题
首先启动 Hexo 本地站点，并开启调试模式（即加上 --debug），整个命令是 hexo s --debug。 
```
$ hexo s --debug
```
在服务启动的过程，注意观察命令行输出是否有任何异常信息，如果你碰到问题，这些信息将帮助他人更好的定位错误。 当命令行输出中提示出：
```
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```
此时即可使用浏览器访问 http://localhost:4000，检查站点是否正确运。不出所料应该已经能看到样式了
##### 主题设定
选择 Scheme

Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。目前 NexT 支持三种 Scheme，他们是：

Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
Mist - Muse 的紧凑版本，整洁有序的单栏外观
Pisces - 双栏 Scheme，小家碧玉似的清新
Scheme 的切换通过更改 主题配置文件，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 # 去除即可。

选择 Pisces Scheme
```
#scheme: Muse
#scheme: Mist
scheme: Pisces
```
##### 设置语言

编辑 站点配置文件， 将 language 设置成你所需要的语言。建议明确设置你所需要的语言，例如选用简体中文，配置如下：
```
language: zh-Hans
```
##### hexo-admin配置
https://github.com/jaredly/hexo-admin
```
npm install --save hexo-admin
hexo server -d
open http://localhost:4000/admin/
```
密码设置，需要在根母目录下的/_config.yml文件里面加入如下代码
```
admin:
  username: guoyoujin
  password_hash: $2a$10$8f0CO288aEgpb0BQk0mAEOIDwPS.s6nl703xL6PLTVzM.758x8xsi
  secret: a secret something
```
注意上面的password_hash需要使用nodejs语语法生成
```
$ npm install bcrypt-nodejs --save
$ node
> const bcrypt = require('bcrypt-nodejs')
> bcrypt.hashSync('your password secret here!')
//=> '$2a$10$8f0CO288aEgpb0BQk0mAEOIDwPS.s6nl703xL6PLTVzM.758x8xsi'
```
把上面输出的值替换成password_hash的值即可

##### 开启baidu站点收录
链接地址: http://zhanzhang.baidu.com/site

添加站点: guoyoujin.github.io

进行验证:
![png](https://raw.githubusercontent.com/guoyoujin/guoyoujin.github.io/develop/images/baidu-site-verification-content.png)

注意上面的content的值有用，需要写入/them/next/_config.yml里面，如下：
```
baidu_site_verification: C5V7yJu5y2
```
完成以上步骤之后，部署一下，点击完成验证，即可验证成功

##### Hexo插件之百度主动提交链接
百度地址： https://tongji.baidu.com

前提
您得注册百度站长工具，然后在工具->网页抓取->链接提交里找到你的密匙。
![png](https://raw.githubusercontent.com/guoyoujin/guoyoujin.github.io/develop/images/baidu_url_submit_token.png)
首先，在Hexo根目录下，安装本插件：
```
npm install hexo-baidu-url-submit --save
```
在 them/next/_config.yml文件里面，填写以下数据
```
baidu_url_submit:
  count: 1 ## 提交最新的一个链接
  host: guoyoujin.github.io ## 在百度站长平台中注册的域名
  token: uSnj5Dx11111111 ## 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```
##### 百度统计   http://tongji.baidu.com/sc-web
在 them/next/_config.yml文件里面，填写以下数据
```
# Baidu Analytics ID
baidu_analytics: 658ef8640c9dcf118730ad9d90
```