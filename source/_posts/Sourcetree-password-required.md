---
title: Sourcetree ~ 不停的提示 password required
date: 2015-03-25 15:26:53
tags: [Sourcetree]
categories: [开发工具]
---

<br/>

> 问题: Sourcetree 不停的让输入密码，报 password required

<br/>

按以下步骤可以解决:



1.在终端（Terminal）打开你的工程目录

2.输入以下命令

```bash
git config credential.helper store
```

3.拉取代码

```bash
git pull
```

4.输入用户名密码



