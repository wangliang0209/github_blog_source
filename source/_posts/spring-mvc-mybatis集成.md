---
title: spring mvc mybatis集成
date: 2017-01-17 17:10:30
tags: [Java, server]
---
项目地址: https://github.com/wangliang0209/SpringMVCMybatisDemo/
#####1.首先看pom.xml文件 引入必须的jar

<!-- more -->

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.walker.maven.demo</groupId>
  <artifactId>web</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>web Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <properties>
  	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  	<spring.version>4.1.5.RELEASE</spring.version>
  	<mybatis.version>3.4.1</mybatis.version>
  	<mybatis-spring.version>1.3.0</mybatis-spring.version>
  	<mysql-connection.version>5.1.40</mysql-connection.version>
  </properties>
  <dependencies>
    <!-- spring begin -->  
	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-webmvc</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-jdbc</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-context</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-aop</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-core</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  

	<dependency>  
	    <groupId>org.springframework</groupId>  
	    <artifactId>spring-test</artifactId>  
	    <version>${spring.version}</version>  
	</dependency>  
	<!-- spring end -->  

	<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
	<dependency>
	    <groupId>org.mybatis</groupId>
	    <artifactId>mybatis</artifactId>
	    <version>${mybatis.version}</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
	<dependency>
	    <groupId>org.mybatis</groupId>
	    <artifactId>mybatis-spring</artifactId>
	    <version>${mybatis-spring.version}</version>
	</dependency>

	<!-- 数据源 -->  
    <dependency>  
        <groupId>com.alibaba</groupId>  
        <artifactId>druid</artifactId>  
        <version>1.0.12</version>  
    </dependency>

	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>${mysql-connection.version}</version>
	</dependency>

	<!-- AOP框架 -->
	<dependency>  
        <groupId>org.aspectj</groupId>  
        <artifactId>aspectjweaver</artifactId>  
        <version>1.8.4</version>  
    </dependency>

	<!-- https://mvnrepository.com/artifact/log4j/log4j -->
	<dependency>
	    <groupId>log4j</groupId>
	    <artifactId>log4j</artifactId>
	    <version>1.2.17</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
	<dependency>
	    <groupId>org.slf4j</groupId>
	    <artifactId>slf4j-api</artifactId>
	    <version>1.7.21</version>
	</dependency>

	<dependency>  
        <groupId>org.slf4j</groupId>  
        <artifactId>slf4j-log4j12</artifactId>  
        <version>1.7.21</version>  
        <scope>runtime</scope>  
    </dependency>

	<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
	<dependency>
	    <groupId>commons-fileupload</groupId>
	    <artifactId>commons-fileupload</artifactId>
	    <version>1.3.2</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
	<dependency>
	    <groupId>commons-io</groupId>
	    <artifactId>commons-io</artifactId>
	    <version>2.5</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/commons-codec/commons-codec -->
	<dependency>
	    <groupId>commons-codec</groupId>
	    <artifactId>commons-codec</artifactId>
	    <version>1.10</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/commons-dbcp/commons-dbcp -->
	<dependency>
	    <groupId>commons-dbcp</groupId>
	    <artifactId>commons-dbcp</artifactId>
	    <version>1.4</version>
	</dependency>

	<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
	<dependency>
	    <groupId>com.alibaba</groupId>
	    <artifactId>fastjson</artifactId>
	    <version>1.1.15</version>
	</dependency>

	<!-- 导入java ee jar 包 -->  
    <dependency>  
        <groupId>javax</groupId>  
        <artifactId>javaee-api</artifactId>  
        <version>7.0</version>  
    </dependency>

    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
    	<groupId>javax.servlet</groupId>
    	<artifactId>jstl</artifactId>
    	<version>1.2</version>
    </dependency>

    <!-- @Inject -->  
    <dependency>  
        <groupId>javax.inject</groupId>  
        <artifactId>javax.inject</artifactId>  
        <version>1</version>  
    </dependency>
  </dependencies>
  <build>
    <finalName>web</finalName>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>2.6</version>
            <configuration>
                <failOnMissingWebXml>false</failOnMissingWebXml>
            </configuration>
        </plugin>
    </plugins>
  </build>

</project>
```
#####2.spring核心配置文件引入
- 在web.xml添加该代码（```注意最好放在前边```）
```
<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->  
   <context-param>  
       <param-name>contextConfigLocation</param-name>  
       <param-value>classpath:spring.xml,classpath:spring-mybatis.xml</param-value>  
   </context-param>
```
这里spring.xml 和spring-mybatis.xml都放在src/main/resources中

- spring.xml 中加载配置文件及扫描注入Service
```
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans-4.1.xsd  
            http://www.springframework.org/schema/context  
            http://www.springframework.org/schema/context/spring-context-4.1.xsd">  

    <!--引入配置属性文件 -->  
    <context:property-placeholder location="classpath:config.properties" />  

    <!--自动扫描含有@Service将其注入为bean -->  
    <context:component-scan base-package="com.wl.service" />  
</beans>  
```

- spring-mybatis.xml 是集成mybatis的配置文件
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx"  
    xmlns:aop="http://www.springframework.org/schema/aop"  
    xsi:schemaLocation=" http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.1.xsd  
        http://www.springframework.org/schema/tx   
        http://www.springframework.org/schema/tx/spring-tx-4.1.xsd  
        http://www.springframework.org/schema/aop   
        http://www.springframework.org/schema/aop/spring-aop-4.1.xsd  
        ">  
    <!-- 配置数据源 使用的是Druid数据源 -->  
    <bean name="dataSource" class="com.alibaba.druid.pool.DruidDataSource"  
        init-method="init" destroy-method="close">  
        <property name="url" value="${jdbc.url}" />  
        <property name="username" value="${jdbc.username}" />  
        <property name="password" value="${jdbc.password}" />  
        <!-- 初始化连接大小 -->  
        <property name="initialSize" value="0" />  
        <!-- 连接池最大使用连接数量 -->  
        <property name="maxActive" value="20" />   
        <!-- 连接池最小空闲 -->  
        <property name="minIdle" value="0" />  
        <!-- 获取连接最大等待时间 -->  
        <property name="maxWait" value="60000" />  
        <property name="poolPreparedStatements" value="true" />  
        <property name="maxPoolPreparedStatementPerConnectionSize"  
            value="33" />  
        <!-- 用来检测有效sql -->  
        <property name="validationQuery" value="${validationQuery}" />  
        <property name="testOnBorrow" value="false" />  
        <property name="testOnReturn" value="false" />  
        <property name="testWhileIdle" value="true" />  
        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->  
        <property name="timeBetweenEvictionRunsMillis" value="60000" />  
        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->  
        <property name="minEvictableIdleTimeMillis" value="25200000" />  
        <!-- 打开removeAbandoned功能 -->  
        <property name="removeAbandoned" value="true" />  
        <!-- 1800秒，也就是30分钟 -->  
        <property name="removeAbandonedTimeout" value="1800" />  
        <!-- 关闭abanded连接时输出错误日志 -->  
        <property name="logAbandoned" value="true" />  
        <!-- 监控数据库 -->  
        <property name="filters" value="mergeStat" />  
    </bean>  
    <!-- myBatis文件 -->  
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
        <property name="dataSource" ref="dataSource" />  
        <!-- 自动扫描entity目录, 省掉Configuration.xml里的手工配置 -->  
        <property name="mapperLocations" value="classpath:com/wl/mapping/*.xml" />  
    </bean>  
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  
        <property name="basePackage" value="com.wl.dao" />  
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />  
    </bean>  
    <!-- 配置事务管理器 -->  
    <bean id="transactionManager"  
        class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
        <property name="dataSource" ref="dataSource" />  
    </bean>  
    <!-- 注解方式配置事物 -->  
    <!-- <tx:annotation-driven transaction-manager="transactionManager" /> -->  
    <!-- 拦截器方式配置事物 -->  
    <tx:advice id="transactionAdvice" transaction-manager="transactionManager">  
        <tx:attributes>  
            <tx:method name="insert*" propagation="REQUIRED" />  
            <tx:method name="update*" propagation="REQUIRED" />  
            <tx:method name="delete*" propagation="REQUIRED" />  

            <tx:method name="get*" propagation="SUPPORTS" read-only="true" />  
            <tx:method name="find*" propagation="SUPPORTS" read-only="true" />  
            <tx:method name="select*" propagation="SUPPORTS" read-only="true" />  

        </tx:attributes>  
    </tx:advice>  
    <!-- Spring aop事务管理 -->  
    <aop:config>  
        <aop:pointcut id="transactionPointcut"  
            expression="execution(* com.wl.service..*Impl.*(..))" />  
        <aop:advisor pointcut-ref="transactionPointcut"  
            advice-ref="transactionAdvice" />  
    </aop:config>  
</beans>  
```

#####3.配置spring mvc
- 首先在web.xml中配置如下
```
<servlet>  
    <servlet-name>dispatcher</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <init-param>  
        <param-name>contextConfigLocation</param-name>  
        <param-value>  
            classpath:spring-mvc.xml  
        </param-value>  
    </init-param>
    <load-on-startup>1</load-on-startup>  
  </servlet>  
  <!-- 设置dispatchservlet的匹配模式，通过把dispatchservlet映射到/，默认servlet会处理所有的请求，包括静态资源 -->  
  <servlet-mapping>  
    <servlet-name>dispatcher</servlet-name>  
    <url-pattern>/</url-pattern>  
  </servlet-mapping>
```
- spring-mvc.xml 配置文件（如果不设置init-param的话默认读取web-inf中dispatcher-servlet.xml文件）
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:mvc="http://www.springframework.org/schema/mvc"  
    xmlns:p="http://www.springframework.org/schema/p"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans.xsd  
            http://www.springframework.org/schema/mvc  
            http://www.springframework.org/schema/mvc/spring-mvc.xsd  
            http://www.springframework.org/schema/context  
            http://www.springframework.org/schema/context/spring-context.xsd"  
    default-lazy-init="true">  
    <mvc:annotation-driven>  
        <mvc:message-converters>  
            <bean class="com.wl.web.util.UTF8StringHttpMessageConverter" />  
        </mvc:message-converters>  
    </mvc:annotation-driven>  
    <!-- 通过mvc:resources设置静态资源，这样servlet就会处理这些静态资源，而不通过控制器 -->  
    <!-- 设置不过滤内容，比如:css,jquery,img 等资源文件 -->  
    <mvc:resources location="/*.html" mapping="/**.html" />  
    <mvc:resources location="/css/" mapping="/css/**" />  
    <mvc:resources location="/js/" mapping="/js/**" />  
    <mvc:resources location="/images/" mapping="/images/**" />  
    <mvc:resources location="/static/" mapping="/static/**" />  
    <!-- 添加注解驱动 -->  
    <mvc:annotation-driven />  
    <!-- 默认扫描的包路径 -->  
    <!--自动扫描含有@Service将其注入为bean -->  
    <context:component-scan base-package="com.wl.controller" />      <!-- mvc:view-controller可以在不需要Controller处理request的情况，转向到设置的View -->  
    <!-- 像下面这样设置，如果请求为/，则不通过controller，而直接解析为/index.jsp -->  
    <mvc:view-controller path="/" view-name="index" />  
    <bean class="org.springframework.web.servlet.view.UrlBasedViewResolver">  
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"></property>  
        <!-- 配置jsp路径前缀 -->  
        <property name="prefix" value="/"></property>  
        <!-- 配置URl后缀 -->  
        <property name="suffix" value=".jsp"></property>  
    </bean>  
</beans>
```

到此，配置文件已经基本结束

#####4.Mybatis代码生成（由于这部分比较麻烦，可以自己写工具生成默认的文件，包括 model dao mapping）
- model根据数据表结构定义
```
public class Account {
    private Integer id;

    private String account;

    private String password;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account == null ? null : account.trim();
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password == null ? null : password.trim();
    }
}
```
- dao代码
```
import com.wl.model.Account;
public interface AccountMapper {
    int deleteByPrimaryKey(Integer id);

    int insert(Account record);

    int insertSelective(Account record);

    Account selectByPrimaryKey(Integer id);

    Account selectByAccount(String account);

    Account selectByAccPwd(Account account);

    int updateByPrimaryKeySelective(Account record);

    int updateByPrimaryKey(Account record);
}
```
- mapping代码
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.wl.dao.AccountMapper" >
  <resultMap id="BaseResultMap" type="com.wl.model.Account" >
    <id column="_id" property="id" jdbcType="INTEGER" />
    <result column="account" property="account" jdbcType="VARCHAR" />
    <result column="password" property="password" jdbcType="VARCHAR" />
  </resultMap>
  <sql id="Base_Column_List" >
    _id, account, password
  </sql>
  <select id="selectByPrimaryKey" resultMap="BaseResultMap" parameterType="java.lang.Integer" >
    select
    <include refid="Base_Column_List" />
    from tb_account
    where _id = #{id,jdbcType=INTEGER}
  </select>
  <select id="selectByAccount" resultMap="BaseResultMap" parameterType="java.lang.String">
    select
    <include refid="Base_Column_List" />
    from tb_account
    where account = #{account,jdbcType=VARCHAR}
  </select>
  <select id="selectByAccPwd" resultMap="BaseResultMap" parameterType="com.wl.model.Account">
  	select
    <include refid="Base_Column_List" />
    from tb_account
    where account = #{account,jdbcType=VARCHAR} AND password = #{password, jdbcType=VARCHAR}
  </select>
  <delete id="deleteByPrimaryKey" parameterType="java.lang.Integer" >
    delete from tb_account
    where _id = #{id,jdbcType=INTEGER}
  </delete>
  <insert id="insert" parameterType="com.wl.model.Account" >
    insert into tb_account (_id, account, password
      )
    values (#{id,jdbcType=INTEGER}, #{account,jdbcType=VARCHAR}, #{password,jdbcType=VARCHAR}
      )
  </insert>
  <insert id="insertSelective" parameterType="com.wl.model.Account" >
    insert into tb_account
    <trim prefix="(" suffix=")" suffixOverrides="," >
      <if test="id != null" >
        _id,
      </if>
      <if test="account != null" >
        account,
      </if>
      <if test="password != null" >
        password,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides="," >
      <if test="id != null" >
        #{id,jdbcType=INTEGER},
      </if>
      <if test="account != null" >
        #{account,jdbcType=VARCHAR},
      </if>
      <if test="password != null" >
        #{password,jdbcType=VARCHAR},
      </if>
    </trim>
  </insert>
  <update id="updateByPrimaryKeySelective" parameterType="com.wl.model.Account" >
    update tb_account
    <set >
      <if test="account != null" >
        account = #{account,jdbcType=VARCHAR},
      </if>
      <if test="password != null" >
        password = #{password,jdbcType=VARCHAR},
      </if>
    </set>
    where _id = #{id,jdbcType=INTEGER}
  </update>
  <update id="updateByPrimaryKey" parameterType="com.wl.model.Account" >
    update tb_account
    set account = #{account,jdbcType=VARCHAR},
      password = #{password,jdbcType=VARCHAR}
    where _id = #{id,jdbcType=INTEGER}
  </update>
</mapper>
```

#####5.service代码
- 定义接口
```
  package com.wl.service;
import com.wl.model.Account;
public interface RegService {

	Account getAccountByAccount(String account);

	Account getAccountByAccPwd(String account, String pwd);

	int regAccount(Account account);
}
```
- 具体实现
```
@Service("regService")
public class RegServiceImpl implements RegService {
	@Autowired
	private AccountMapper accountMapper;
	public Account getAccountByAccount(String account) {
		return accountMapper.selectByAccount(account);
	}
	public Account getAccountByAccPwd(String account, String pwd) {
		Account acc = new Account();
		acc.setAccount(account);
		acc.setPassword(pwd);
		return accountMapper.selectByAccPwd(acc);
	}
	/**
	 * @return -2 account exist.  > 0 succ.
	 */
	public int regAccount(Account account) {
		int ret = -1;
		Account acc = accountMapper.selectByAccount(account.getAccount());
		if(acc != null) {
			account.setId(acc.getId());
			ret = -2;
		} else {
			accountMapper.insert(account);
			return accountMapper.selectByAccount(account.getAccount()).getId();
		}
		return ret;
	}
}
```

#####6.到此可以测试了。注意这里没写单元测试代码了，数据库请自行建立
- 在src/test/java 建立同包名的业务，如com.wl.service,创建测试类TestRegService
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"classpath:spring.xml", "classpath:spring-mybatis.xml"})
public class TestRegService {
	private static final Logger LOGGER = Logger.getLogger(TestRegService.class);
	@Autowired
	private RegService regService;
	@Test
	public void testGetAccountByAccount() {
		Account account = regService.getAccountByAccount("test");
		if(account == null) {
			System.out.println("testGetAccountByAccount >> test not exist.");
		} else {
			System.out.println("testGetAccountByAccount >> " + JSON.toJSONString(account));
		}
	}
	@Test
	public void testGetAccountByAccPwd() {
		Account account = regService.getAccountByAccPwd("test", "123456");
		if(account == null) {
			System.out.println("testGetAccountByAccPwd >> test not exist.");
		} else {
			System.out.println("testGetAccountByAccPwd >> " + JSON.toJSONString(account));
		}
	}
	@Test
	public void testRegAccount() {
		Account account = new Account();
		account.setAccount("test");
		account.setPassword("123456");
		int ret = regService.regAccount(account);
		System.out.println("testRegAccount >> " + ret);
	}
}
```
单元测试很重要，也可以根据业务方法自行生成的，具体的自己写吧。

#####7.控制层集成 参考RegController
```
@Controller
@RequestMapping("/reg")
public class RegController {

	@Autowired
	private RegService regService;

	@RequestMapping(method=RequestMethod.POST)
	private @ResponseBody String reg(@RequestParam(value="account") String account,
						@RequestParam(value="password") String password) {
		if(account == null || "".equals(account)){
			return ResponseUtils.generFailedJsonStr(-1, "account is null");
		} else if(password == null || "".equals(password)) {
			return ResponseUtils.generFailedJsonStr(-1, "pwd is null");
		} else {
			Account acc = new Account();
			acc.setAccount(account);
			acc.setPassword(password);
			int ret = regService.regAccount(acc);
			if(ret > 0) {
				JSONObject data = new JSONObject();
				data.put("uid", ret);
				return ResponseUtils.generSuccJsonStr(data);
			} else {
				return ResponseUtils.generFailedJsonStr(ret, "reg failed.");
			}
		}
	}
}
```
到此，集成应该算完成了。
#####8.遇到的问题
- Validation慢的问题，解决方案就是把Validation中build中web js 相关都勾掉
- mybatis配置文件不提示的问题
  ```
  <?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  ```
然后步入正题，当这个自动提示不管用的时候，如何自己设置让其可以自动提示。
首先，按着ctrl键点击上方的这个头，[http://mybatis.org/dtd/mybatis-3-mapper.dtd](http://mybatis.org/dtd/mybatis-3-mapper.dtd)，这一部分，就会让你下载这个dtd到本机上，找个专用文件夹存好。如果下载不了就说明这个url有问题，那就去往上找把。
下载完成后，打开window–>Preferences–>XML–>XML catalog。自己用搜索框直接搜索xml也可以找到。然后点击add，Location地方找到你下载好的dtd文件，key type用public-Key，key复制xml头上的-//mybatis.org//DTD Mapper 3.0//EN这一部分，然后确定保存，再返回xml文件，你就会发现自动提示能用啦！
这个地方我的猜测是xml头上有个PUBLIC，所以才选择的public-key。还是不知道这个猜测对不对，期待有兴趣的朋友或者大神给解个惑。其他的xml配置，我感觉就是重复这个步骤就ok了吧~
- #####cannot change version Dynamic web module to 3.0
   1.修改org.eclipse.jdt.core.prefs 中1.5 到1.8（根据自己的jdk设置）
   2.修改org.eclipse.wst.common.project.facet.core.xml
      - <installed facet="jst.web" version="3.0"/> //从2.3改为 3.0
      - <installed facet="java" version="1.8"/>  //改为自己对应的版本
   3.注意这里一定要在Navigator中修改，否则可能会没效果。
- ##### dynamic web module 3.0 requires 1.6
  在pom.xml中添加
  ```
  <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-compiler-plugin</artifactId>  
            <version>3.0</version>  
            <configuration>  
                <source>1.8</source>  
                <target>1.8</target>  
            </configuration>  
        </plugin>
  ```
  然后，maven update project 即可。
