---
title: 配置新服务器 ubuntu
date: 2017-01-18 09:41:06
tags: [ubuntu]
---
0. 挂载数据盘
  参照 [这里](http://jingyan.baidu.com/article/90808022d2e9a3fd91c80fe9.html)

  <!-- more -->

1. jdk
   > tar zxvf jdk.tar.gz -C /usr/lib/java
   > 配置path   
   > export JAVA_HOME=/usr/lib/java/jdk
   ```export PATH=$PATH:$JAVA_HOME/bin```
2. redis
   > $sudo apt-get update  
   > $sudo apt-get install redis-server  
   > $redis-server (启动redis)
   - 设置访问密码：
     > sudo vi /etc/redis/redis.conf  
     > requirepass 自己的密码  前边注释去掉  
     > sudo /etc/init.d/redis restart
3. mysql
   >sudo apt-get install mysql-server mysql-client

   远程链接通过 mysql worbench ssh链接  

   - 局域网访问
     > $sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf  
     > bind-address 127.0.0.1 改成 192.168.10.37  
     > $sudo /etc/init.d/mysql restart
4. tomcat
   - apt-get安装方式
     > apt-get install tomcat7  
     配置
     /var/lib/tomcat7下可以配置  
     如何设置tomcat user  

   - 目录直接解压安装的步骤：  
      - 解压到一个目录，注意修改tomcat的权限 chmod 777 /tomcat
      - 修改端口 server.xml
      > <Server port="8005" shutdown="SHUTDOWN" /> 如:9005  
        <Connector port="8080" ...> 8443这个不改   如:8090  
        <Connector port="8009" protocol="AJP/1.3" ..>  如:9009

5. nginx //TODO


6. Intelij Idea 部署远程tomcat
   - 通过sftp配置
   - tomcat catalina.sh 修改如下配置
     ```
      CATALINA_OPTS="-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=1099 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Djava.rmi.server.hostname=123.206.46.213"
      export CATALINA_OPTS
     ```
   - 项目名称的修改 target下web目录的名称
     > pom.xml中 build 标签下 finalName 修改为所需要的名称  
     > project structure >Artifacts name  
     > 然后clean rebuild



7.命令-.tar.gz格式  
  - 解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tar.gz
  - 压缩：[＊＊＊＊＊＊＊]$ tar zcvf FileName.tar.gz DirName


8.查看文件最新输出内容
  >tail -f catalina.out

9.apache2
>   sudo apt-get install apache2  
>  sudo apt-get install libapache2-mod-php


10.php7 + apache2 + mysql 安装 [参看这里](http://blog.csdn.net/emperor10juv/article/details/52705590)
