---
title: Git ~ 忽略已经提交过一次文件的骚操作
date: 2015-03-12 21:24:29
tags: [Git]
categories: [开发工具]
---

> - 已经推送（push）过的文件，想在以后的提交时忽略此文件，即使本地已经修改过，而且不删除git远程库中相应文件
>
> - 本地开发中需要用的文件，不能提交到远程，但是还不能写入.gitignore文件

### 1. 添加忽略命令

```
git update-index --assume-unchanged fileName
```

### 2. 取消对应文件的忽略

```
git update-index –no-assume-unchanged  fileName
```

### 3. 列出所有被忽略的文件(两个任意一个即可)

```
git ls-files -v | grep '^h\ '

git ls-files -v | grep '^h\ ' | awk '{print $2}'
```

### 4. 忽略一个目录

如果要忽略一个目录，打开 git bash，cd到 目标目录下，执行

```
git update-index --assume-unchanged $(git ls-files | tr '\n' ' ')
```

### 5. 取消所有被忽略的文件

```
git ls-files -v | grep '^h' | awk '{print $2}' |xargs git update-index --no-assume-unchanged
```

