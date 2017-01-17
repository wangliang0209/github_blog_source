---
title: github 静态博客创建（hexo）
date: 2017-01-03 11:42:53
tags:
---
#### 本文只是我自己的博客创建步骤，仅供参考，我是基于hexo jackman主题创建的。####
- 1.[下载node](https://nodejs.org/en/) ，解压后配置path到bin目录下

<!--more-->

- 2.运行npm_install.sh，[点击这里下载](https://wangliang0209.github.io/npm_install.sh)，注意这个文件如果不是可执行文件运行如下命令
  ```
    chmod +x npm_install.sh
  ```

- 3.安装hexo  

    ```
    npm install -g hexo-cli
    ```

    如果不出问题应该就安装成功了，下面我们开始初始化我们的项目
- 4.初始化项目

    ```
    $ hexo init 文件夹名称  
    $ cd <folder>  
    $ npm install
    ```

- 5.开始写博客
    创建博客

    ```
    hexo new '我的第一篇博客'
    ```

    然后就生成一个md的文件，用atom等markdown编辑器编辑内容即可。
    然后运行

    ```
    hexo g  
    hexo s
    ```

    在浏览器中打开http://localhost:4000/  就会出现网站内容了。
- 6.部署到远程网站上（github）    
    停止server运行 即`Ctrl + C` 命令即可
    然后安装插件

    ```
    npm install hexo-deployer-git --save
    ```

    安装完成之后修改_config.yml 修改deploy下面几处  

    ```
    deploy:  
    type: git  
    repo: git@github.com:SunCrazy/suncrazy.github.io.git `自己的git地址`  
    branch: master
    ```

    修改后保存

    ```
    > hexo deploy
    ```

- 7.到此博客算是部署成功了    
