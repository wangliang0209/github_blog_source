---
title: React Native 使用（一）
date: 2017-08-10 15:06:17
tags:
---

首先说下React Native是干嘛用的:  
1.HTML5、Android、IOS多端代码复用；  
2.实时热部署

**Mac下环境配置**  
参考官网[文档](http://reactnative.cn/docs/0.47/getting-started.html)  可参考其一步步安装即可

必需的软件  
1 `Homebrew`  Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件  
2 `Node` 
	> brew install node  
3 	`Yarn、React Native的命令行工具（react-native-cli)`  Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载。React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。  
   > npm install -g yarn react-native-cli  

安装完yarn后同理也要设置镜像源：  	
   > yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global 	

4 `Android Studio`  
  Android 开发必备 这里就不多说了
  最好配置下ANDROID_HOME PATH 等
  
5 `Nuclide`  


**测试安装**
--
> react-native init AwesomeProject
> cd AwesomeProject
> react-native run-android  

注意可以用Android Studio 打开android目录的项目
可以copy `local.properties` 到android目录下 这样就不用配置sdk.dir了

> react-native start  //启动react debug server

##Android 真机调试##

示例 App 直接部署到真机，红色界面报错，无法连接到 Debug Server。

如果是 5.0 或者以上机型，可通过 adb 反向代理端口，将 Mac 端口反向代理到测试机上。
> adb reverse tcp:8081 tcp:8081  //这样

至此，环境就算搭建起来了，运行hello world就出来了.   
 