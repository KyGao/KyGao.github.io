---
title: Git 基础操作
tags: [Git,tools]
categories:
  - tools
description: 学到哪记到哪
date: 2020-03-14 13:12:43
---

### Git 安装配置
- 看看是否装成功
查看版本信息：`git --version` 
- 添加用户信息
  1. `git config --global user.name "KyGao"`
  2. `git config --global user.email "im_gky@outlook.com"`
  可以通过 `git config --list` 查看添加的信息有没有错

### Git 远程提交
1. 先在 github 创建一个仓库
2. 本地 `git init` 建立本地仓库
3. `git status`  `git add .`  `git commit -m "description"`三连进行本地git操作
4. 建立本地和远端的连接`git remote add origin https://github.com/***/***.git`
5. 每次在这个本地仓库都要：先 `git pull origin master`，如果报错就加上参数 `--allow-unrelated-histories` 强制拉取线上到本地合并。 然后 `git push -u origin master`，第一次以后不用 -u 参数

一些时常用到的操作

- 想在暂存区删除某个文件（不删除本地文件）：`git rm --cache <file>`

### Git出问题了
- 在github的仓库里有文件夹是灰色的，打不开
  **原因**：本地 .git 所在文件夹嵌套 .git 了
  **解决办法**：
  1. `git rm -r --cached "file path & name"`
  2. 本地文件夹找到内嵌的 .git，删除
  3. 重新add三连

