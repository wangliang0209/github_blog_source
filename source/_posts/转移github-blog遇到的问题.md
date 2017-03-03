---
title: 转移github blog遇到的问题
date: 2017-03-03 11:59:17
tags: [blog, github]
---
- 1. 从github上把github_blog_source下载下来
- 2. 按照Node  从[官网](https://nodejs.org/en/)下载即可
- 3. 安装hexo sudo npm install -g hexo
- 4. 初始化hexo hexo init  注意这样的话会该项目源代码中.git擦除掉 
     这里我采用的方案是把项目备份到另一个地方，然后比对copy过来，这样就没问题了。
- 5. 安装git 部署器  npm install hexo-deployer-git –save
- 6. 到此应该就没什么问题了。check 一下