---
title: android studio（Intellij）设置save actions
date: 2017-01-17 17:04:45
tags: [android, Intellij]
---
- 如果是android studio 可能需要下载，然后install from local， 步骤如下：
   - 首先， 到 [这里](https://plugins.jetbrains.com/plugin/7642) 下载最新版本的jar
   - 然后，打开android studio File > Settings > Plugins > Install plugin from disk.. 选择下载好的jar文件
   - 最后，重启android studio，在 File > Settings > Other Settings > Save Actions 中即可设置（无特殊需求，默认状态即可）

<!-- more -->

-  如果是Intellij idea IDE的话，设置起来就简单了，执行在线安装插件流程
   - 首先，打开 android studio 按照下边导航执行
     > File > Settings > Plugins > Browse repositories... > Search 'Save Actions'

  - 最后，重启android studio，在 File > Settings > Other Settings > Save Actions 中即可设置（无特殊需求，默认状态即可）


- 设置完成后，随便打开一个java文件，编辑后 Ctrl + S，看效果即可。
  #####注意这里一行内容太多的话，只会粗暴的回行一次，建议大家保持良好的编码习惯，规避一行内容过长的问题。#####


- 如果还不懂可参考 [这里](https://github.com/dubreuia/intellij-plugin-save-actions)
