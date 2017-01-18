---
title: Python 开发环境（Eclipse pyDev）
date: 2017-01-18 09:30:12
tags: [Python]
---
- 1 eclipse 点击 [去下载](https://www.eclipse.org/downloads/)

  <!-- more -->

- 2 启动Eclipse, 点击Help->Install New Software...   在弹出的对话框中，点Add 按钮。  Name中填:Pydev,  Location中填`http://pydev.org/updates`
       选择 PyDev 点击Next  然后accept协议 然后会弹出窗口 选择上之后确定就会安装了，不出意外就会成功了
- 3 配置pydev解释器
   > 在Eclipse菜单栏中，点击Windows ->Preferences.   
   > 在对话框中，点击pyDev->Interpreter - Python
   > 点击New Name 如我的 `Python2.7`  Location 我的在 /usr/bin/python2.7 确定即可完成
- 4 之后就可以建立项目了
