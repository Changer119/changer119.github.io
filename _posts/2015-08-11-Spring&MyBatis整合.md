---
layout: post
title: Spring&MyBatis整合
categories: 研磨技术
---

本文介绍的Spring&MyBatis整合，是基于Maven和Intellij搭建的。

## 添加依赖

这里只列一个清单（主要包括了这些）

- Spring
- Mybatis
- DBCP
- Mysql-connector
- Junit
- Log4j

## 配置spring的上下文环境

**4步大法**

	<!-- step0 配置spring支持Annotation，以及自动扫描 -->
	<context:annotation-config/>
    <context:component-scan base-package="com.changer"/>

    <!-- step1 配置dataSource -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/fcjiangDB"/>
        <property name="username" value="root"/>
        <property  name="password" value="root"/>
        <!-- 连接池初始连接个数 1个 -->
        <property name="initialSize" value="1"/>
        <!-- 连接池允许的最大连接个数  50个 -->
        <property name="maxActive" value="50"/>
        <!-- 最大空闲值.当经过一个高峰时间后，连接池可以慢慢将已经用不到的连接慢慢释放一部分，一直减少到maxIdle为止 -->
        <property name="maxIdle" value="2"/>
        <!--  最小空闲值.当空闲的连接数少于阀值时，连接池就会预申请去一些连接，以免洪峰来时来不及申请 -->
        <property name="minIdle" value="1"/>
    </bean>

    <!-- step2 配置myBatis的sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
    </bean>

	<!-- step3 配置Mybatis自动扫描到Mapper文件 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="com.changer.data.persistence" />
    </bean>


## xml文件的拷贝

在工程中，spring-context.xml等xml文件一般放在/src/main/resources目录下。mvn install时，默认会将resources下的xml文件拷贝到编译后的classes目录下。如果要设置其它的xml文件也拷贝到classes目录下，需要修改pom的build节点。

	<build>
        <finalName>zd-hello2springmybatis</finalName>
        <!--默认会将与src/main/resources目录下的内容拷贝到classpath下-->
        <resources>
            <!-- 将src/main/java目录下的所有xml文件拷贝到classpath下 -->
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
  		 ...
  	</build>
  	
如果```<build>```下定义了```<resources>```标签，则默认的/src/main/resoureces也不会拷贝到classpath下。如果需要拷贝，就要明确的写出来。






	
	 
	

        









