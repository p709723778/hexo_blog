---
title: SVN ~ Status字段含义小记
date: 2013-09-10 16:18:10
tags: [SVN]
categories: [SVN]
---

执行SVN up和svn merge等命令出现在首位置的各字母含义如下：

| 字符 |                     含义                      |
| :--: | :-------------------------------------------: |
|  A   |                     新增                      |
|  C   |                     冲突                      |
|  D   |                     删除                      |
|  G   |                     合并                      |
|  I   |                     忽略                      |
|  M   |                     改变                      |
|  R   |                     替换                      |
|  X   |       未纳入版本控制，但被外部定义所用        |
|  ?   |                未纳入版本控制                 |
|  !   | 该项目已遗失 (被非 svn 命令所删除) 或是不完整 |
|  ~   |     版本控制下的项目与其它类型的项目重名      |

