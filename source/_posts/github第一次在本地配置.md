---
title: github第一次在本地配置
date: 2017-01-17 17:14:11
tags: [github]
---
>cd ~/.ssh

看是否存在 没有就创建一个
>$ssh-keygen -t rsa -C  xxx@163.com

出现
>Generating public/private rsa key pair.
Enter file in which to save the key (/Users/wangliang/.ssh/id_rsa):

输入id_rsa, 出现
>Enter passphrase (empty for no passphrase):自己的密码

出现
>Enter same passphrase again: 再次输入
> $ ls
id_rsa		id_rsa.pub	 一个公钥 一个私钥

>$more id_rsa.pub
复制内容到github->头像->setting -> ssh key ->新增一个就行（title随便写）

>$ ssh -T git@github.com
  出现 Hi wangliang0209! You've successfully authenticated, but GitHub does not provide shell access.

到此表示配置成功了


>git init 初始化仓库
