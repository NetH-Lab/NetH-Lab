---
title: Use Git to Manage Your Projects

summary: '[工具]Git使用方法总结'
authors:
- Minel Huang
date: “2021-03-18T00:00:00Z”
publishDate: "2021-03-18T00:00:00Z"

publication_types: ["0"]

tags: 
- Tools
- Git
featured: false
---

更新时间：2021-03-18

参考资料：
1. [git documents](https://git-scm.com/doc)
2. [git菜鸟教程](https://www.runoob.com/git/git-basic-operations.html)
# 基本操作
![](./1.jpg)
说明：
- workspace：工作区，即我们用vs code打开的工作区
- staging area：暂存区/缓存区
- local repository：版本库或本地仓库
- remote repository：远程仓库

本地操作命令：
git add: 将文件添加到staging area
git commit: 将文件存入本地仓库
git status: 查看本地仓库状态，显示有变更的文件
git diff: 比较暂存区和工作区的差异
远程操作：
git fetch: 从远程获得代码库
git pull: 下载远程代码并合并
git push: 上传远程代码并合并