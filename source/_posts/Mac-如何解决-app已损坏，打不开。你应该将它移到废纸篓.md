---
title: Mac ~ 如何解决 app已损坏，打不开。你应该将它移到废纸篓
date: 2017-12-11 16:40:20
tags: [Mac]
categories: [Mac]
---

> 如遇：「xxx.app已损坏,打不开.你应该将它移到废纸篓」，并非你安装的软件已损坏，而是Mac系统的安全设置问题，因为这些应用都是破解或者汉化的,那么解决方法就是临时改变Mac系统安全设置。
>
> 出现这个问题的解决方法：
>
> 修改系统配置：系统偏好设置 -> 安全性与隐私。修改为任何来源



如果没有这个选项的话,打开终端，执行以下对应命令

<!--more-->

- 显示"任何来源"选项


```bash
sudo spctl --master-disable
```

- 不显示"任何来源"选项


```bash
sudo spctl --master-enable
```

{% asset_img 1.png Mac安全性与隐私设置页面 %}