---
title: jenkins搭建
date: 2017-01-22 13:20:57
tags: [自动化打包]
---
- 下载jenkins war包 [到这里下载最新的版本](https://jenkins.io/index.html)

- 下载之后 放在tomcat的webapps下，重启tomcat即可（注意这里是已经安装了tomcat，如果没有安装，请自行下载安装，这里不做介绍）

- 重启之后，在浏览器中访问 localhost:8080/jenkins

- 接下来就是初始化设置，注意第一次初始化会有密码在tomcat logs 中，复制粘贴就好。然后按照步骤即可，注意在提示插件的地方，尽量都安装上，这样我们在集成编译android项目就不用再安装了，比如git android 项目.

- 参数化构建
  > 可以定义参数 如 IS_JENKINS  
    然后在build.gradle中获取 如 "${System.properties['IS_JENKINS']}" 注意这里一般都是以字符串传过来的，逻辑中自己转换
- 构建
  - Invoke Gradle
  - Gradle Version gradle 2.14.1  注意这里可以在全局中自己配
  - Tasks clean build  
  - Root Build script   ${WORKSPACE}/app
  - Build File  ${WORKSPACE}/app/build.gradle

- 构建后操作
  - Archive the artifacts
    - `**/*.apk`  
  - 注意 build gradle 中 在buildTypes > release  
    ```
    applicationVariants.all { variant ->
               variant.outputs.each { output ->
                   def apk = output.outputFile
                   if (apk != null && apk.name.endsWith('.apk')) {
                       def fileName = ""
                       def isJenkins = "${System.properties['IS_JENKINS']}"
                       if ('true'==isJenkins) {
                           fileName += "jenkins_"
                       }
                       fileName += "DBForAndroidDemo_v${defaultConfig.versionName}_${variant.productFlavors[0].name}_"
                       fileName += getDate() + ".apk"

                       output.outputFile = new File(apk.parent, fileName)
                   }
               }

           }
    ```
  - 注意 一定要有productFlavors
    ```
    productFlavors {
       first{}
    }
    ```
- 遇到的问题
  - ##### groovy中字符串不能用equals，否则会一直为false #####
- 参考如下
  - http://blog.csdn.net/mabeijianxi/article/details/52680283
  - http://blog.csdn.net/jav_imba/article/details/51180822
  - http://blog.csdn.net/freewebsys/article/details/43758465
