---
title: Mac ~ 如何在Mac上搭建Web服务器(Apache)
date: 2019-06-21 19:51:26
tags: [web服务]
categories: [Mac]

---

局域网搭建 Web 服务器测试环境,因为Mac OS X 自带了 Apache 和 PHP 环境，我们只需要简单的启动它就行了。惊不惊喜,意不意外!😄

# 1.相关命令

a.启动 Apache 命令 :   `sudo apachectl start`
b.停止 Apache 命令 :   `sudo apachectl stop`
c.重启 Apache 命令 :   `sudo apachectl restart`

# 2.Apache相关信息

a.Apache服务器默认的web根目录在：`/Library/WebServer/Documents`
b.Apache的配置文件在：`/etc/apache2`

# 3.预览内容,请再浏览器输入以下两个任意地址:

a. [http://localhost/](http://localhost/)
b. [http://127.0.0.1/](http://127.0.0.1/)

{% asset_img MacServerDefaultPage.png Mac默认web页面 %}


# 4.如何查看自己编写的文件

将自己编写的html文件复制到 `/Library/WebServer/Documents` 文件夹下,无需重启服务,及时浏览内容.
{% asset_img DocumentsPath.png html路径 %}

浏览器访问 `index.html` 文件,浏览方式有三种:

1.`http://localhost/index.html`

2.`http://127.0.0.1/index.html`

3.`本机ip/index.html`

{% asset_img webPage.png 预览效果 %}