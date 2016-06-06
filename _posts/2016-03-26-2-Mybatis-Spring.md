---
layout: post
title: Mybatis & Spring
date: 2016-03-24
weather: sunny
categories: Java Framework
tags: [Persistence framework ,SQL mapper framework]
description: 
---

# Generate Project

	mvn archetype:generate -DgroupId=me.gacl -DartifactId=spring4-mybatis3 -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

---

# Create three folder
- src/main/java
- src/test/resources
- src/test/java


---

# create sample db

	Create DATABASE spring4_mybatis3;
	USE spring4_mybatis3;

	DROP TABLE IF EXISTS t_user;
	CREATE TABLE t_user (
	  user_id char(32) NOT NULL,
	  user_name varchar(30) DEFAULT NULL,
	  user_birthday date DEFAULT NULL,
	  user_salary double DEFAULT NULL,
	  PRIMARY KEY (user_id)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

---

# Use generator to create entity class, mapping xml and dao class	

add plugin definition in pom.xml

	<dependency>
	    <groupId>org.mybatis.generator</groupId>
	    <artifactId>mybatis-generator</artifactId>
	    <version>1.3.2</version>
	</dependency>




create src/main/resources/generatorConfig.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
	<generatorConfiguration>
	    <!-- DB Driver -->
	    <classPathEntry location="E:\repository\mysql\mysql-connector-java\5.1.34\mysql-connector-java-5.1.34.jar" /> 
	    <!-- <classPathEntry location="C:\oracle\product\10.2.0\db_1\jdbc\lib\ojdbc14.jar" />-->
	    <context id="DB2Tables" targetRuntime="MyBatis3">
	        <commentGenerator>
	            <property name="suppressAllComments" value="true" />
	        </commentGenerator>
	        <!-- DB URL, user name, pwd-->
	         <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost:3306/spring4_mybatis3" userId="root" password="XDP"> 
	        <!--<jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver" connectionURL="jdbc:oracle:thin:@localhost:1521:orcl" userId="msa" password="msa">-->
	        </jdbcConnection>
	        <javaTypeResolver>
	            <property name="forceBigDecimals" value="false" />
	        </javaTypeResolver>
	        <!-- entity class path => me.gacl.domain-->
	        <javaModelGenerator targetPackage="me.gacl.domain" targetProject="C:\Users\gacl\spring4-mybatis3\src\main\java">
	            <property name="enableSubPackages" value="true" />
	            <property name="trimStrings" value="true" />
	        </javaModelGenerator>
	        <!-- SQL mapping path => me.gacl.mapping-->
	        <sqlMapGenerator targetPackage="me.gacl.mapping" targetProject="C:\Users\gacl\spring4-mybatis3\src\main\java">
	            <property name="enableSubPackages" value="true" />
	        </sqlMapGenerator>
	        <!-- DAO class path => me.gacl.dao-->
	        <javaClientGenerator type="XMLMAPPER" targetPackage="me.gacl.dao" targetProject="C:\Users\gacl\spring4-mybatis3\src\main\java">
	            <property name="enableSubPackages" value="true" />
	        </javaClientGenerator>
	        <!-- tableName & domainObjectName -->
	        <table tableName="t_user" domainObjectName="User" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" />
	    </context>
	</generatorConfiguration>

run generator

---

# MyBatis-Spring

- Get dependencies info from http://search.maven.org/

- Dependency list: (1)spring-core (2)spring-context (3)spring-tx (4)spring-jdbc (5)spring-test (7)spring-web (8)aspectjweaver (9)mybatis (10)mybatis-spring (11)javax.servlet-api (12)javax.servlet.jsp-api (13)jstl (14)mysql-connector-java (15)druid (16)junit

---

# Create src/main/resources/dbconfig.properties

	driverClassName=com.mysql.jdbc.Driver
	validationQuery=SELECT 1
	jdbc_url=jdbc:mysql://localhost:3306/spring4_mybatis3?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
	jdbc_username=root
	jdbc_password=XDP

---

# Edit src/main/resources/spring.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	    xsi:schemaLocation="
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd">

	    <context:property-placeholder location="classpath:dbconfig.properties" />
	    <context:component-scan base-package="me.gacl.service" />
	</beans>

---

# Edit src/main/resources/spring-mybatis.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
	http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	">
	    <bean name="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
	        <property name="url" value="${jdbc_url}" />
	        <property name="username" value="${jdbc_username}" />
	        <property name="password" value="${jdbc_password}" />
	        <property name="initialSize" value="0" />
	        <property name="maxActive" value="20" />
	        <property name="maxIdle" value="20" />
	        <property name="minIdle" value="0" />
	        <property name="maxWait" value="60000" />
	        <property name="validationQuery" value="${validationQuery}" />
	        <property name="testOnBorrow" value="false" />
	        <property name="testOnReturn" value="false" />
	        <property name="testWhileIdle" value="true" />
	        <property name="timeBetweenEvictionRunsMillis" value="60000" />
	        <property name="minEvictableIdleTimeMillis" value="25200000" />
	        <property name="removeAbandoned" value="true" />
	        <property name="removeAbandonedTimeout" value="1800" />
	        <property name="logAbandoned" value="true" />
	        <property name="filters" value="mergeStat" />
	    </bean>
	    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	        <property name="dataSource" ref="dataSource" />
	        <property name="mapperLocations" value="classpath:me/gacl/mapping/*.xml" />
	    </bean>
	    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	        <property name="basePackage" value="me.gacl.dao" />
	        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
	    </bean>
	    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	        <property name="dataSource" ref="dataSource" />
	    </bean>
	    <tx:advice id="transactionAdvice" transaction-manager="transactionManager">
	        <tx:attributes>
	            <tx:method name="add*" propagation="REQUIRED" />
	            <tx:method name="append*" propagation="REQUIRED" />
	            <tx:method name="insert*" propagation="REQUIRED" />
	            <tx:method name="save*" propagation="REQUIRED" />
	            <tx:method name="update*" propagation="REQUIRED" />
	            <tx:method name="modify*" propagation="REQUIRED" />
	            <tx:method name="edit*" propagation="REQUIRED" />
	            <tx:method name="delete*" propagation="REQUIRED" />
	            <tx:method name="remove*" propagation="REQUIRED" />
	            <tx:method name="repair" propagation="REQUIRED" />
	            <tx:method name="delAndRepair" propagation="REQUIRED" />
	            <tx:method name="get*" propagation="SUPPORTS" />
	            <tx:method name="find*" propagation="SUPPORTS" />
	            <tx:method name="load*" propagation="SUPPORTS" />
	            <tx:method name="search*" propagation="SUPPORTS" />
	            <tx:method name="datagrid*" propagation="SUPPORTS" />
	            <tx:method name="*" propagation="SUPPORTS" />
	        </tx:attributes>
	    </tx:advice>
	    <aop:config>
	        <aop:pointcut id="transactionPointcut" expression="execution(* me.gacl.service..*Impl.*(..))" />
	        <aop:advisor pointcut-ref="transactionPointcut" advice-ref="transactionAdvice" />
	    </aop:config>
	    <bean id="druid-stat-interceptor" class="com.alibaba.druid.support.spring.stat.DruidStatInterceptor">
	    </bean>
	    <bean id="druid-stat-pointcut" class="org.springframework.aop.support.JdkRegexpMethodPointcut" scope="prototype">
	        <property name="patterns">
	            <list>
	                <value>me.gacl.service.*</value>
	            </list>
	        </property>
	    </bean>
	    <aop:config>
	        <aop:advisor advice-ref="druid-stat-interceptor" pointcut-ref="druid-stat-pointcut" />
	    </aop:config>

	</beans>

---

# Unit test & Web test

