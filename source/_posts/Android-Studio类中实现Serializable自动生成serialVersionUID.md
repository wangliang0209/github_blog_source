---
title: Android Studio类中实现Serializable自动生成serialVersionUID
date: 2017-01-18 10:03:16
tags: [android]
---
1. File -> Settings... -> Editor -> Inspections -> Serialization issues[在java类目下] -> Serializable class without 'serialVersionUID'（选中）
2. 进入实现了Serializable中的类，选中类名，Alt+Enter弹出提示，然后直接导入完成
