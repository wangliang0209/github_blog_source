---
title: Maven 增加Mybatis generator 插件
date: 2017-01-18 09:44:49
tags: [maven mybatis]
---
- ##### 在pmd.xml中增加代码
  ```
  <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.2</version>
        <configuration>
          <configurationFile>src/main/resources/mybatis-generator/generatorConfig.xml</configurationFile>
          <verbose>true</verbose>
          <overwrite>true</overwrite>
        </configuration>
        <executions>
          <execution>
            <id>Generate MyBatis Artifacts</id>
            <goals>
              <goal>generate</goal>
            </goals>
          </execution>
        </executions>
        <dependencies>
          <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.2</version>
          </dependency>
        </dependencies>
      </plugin>
  ```
- ##### 增加generatorConfig.xml文件  
  在上面对应目录增加该文件，内容如下
  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
  <generatorConfiguration>
    <classPathEntry location="/home/wangliang/tool/generator/mysql-connector-java-5.1.40.jar" />
    <context id="apimanager" targetRuntime="MyBatis3">
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
            <property name="prefix" value="com.wl.api"/>
        </commentGenerator>
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/db_tech" userId="root"
                        password="root"/>
        <javaModelGenerator targetPackage="com.wl.api.model"
                            targetProject="/home/wangliang/workspace1/ApiManager/src/main/java"/>
        <sqlMapGenerator targetPackage="com.wl.api.mapping"
                         targetProject="/home/wangliang/workspace1/ApiManager/src/main/java/"/>
        <javaClientGenerator targetPackage="com.wl.api.dao"
                             targetProject="/home/wangliang/workspace1/ApiManager/src/main/java"
                             type="XMLMAPPER"/>
        <table schema="taivexpay" tableName="tb_account"
               domainObjectName="Account"
                       enableCountByExample="false" enableUpdateByExample="false"
                       enableDeleteByExample="false" enableSelectByExample="false"
                       selectByExampleQueryId="false"></table>
    </context>
  </generatorConfiguration>
  ```
- ##### 导入后即可 ``` 这里我遇到一直卡死到downloading 关掉重启就好了```
- 然后命令行执行
  > mybatis-generator:generate
- 如果成功就会自动生成了
