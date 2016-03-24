---
layout: post
title: Mybatis Getting Started
date: 2016-03-24
categories: Framework
tags: [Persistence framework ,SQL mapper framework]
description: 
---

# 1 Getting started

### (1) Installation
> To use MyBatis you just need to include the mybatis-x.x.x.jar file in the classpath.

> OR Maven

	<dependency>
	  <groupId>org.mybatis</groupId>
	  <artifactId>mybatis</artifactId>
	  <version>x.x.x</version>
	</dependency>

### (2) Building SqlSessionFactory 

- SqlSessionFactoryBuilder => SqlSessionFactory => SqlSession => MyBatis application instance. 

- SqlSessionFactoryBuilder can build a SqlSessionFactory instance from an XML configuration file, or from a custom prepared instance of the Configuration class.

> FROM XML

	String resource = "org/mybatis/example/mybatis-config.xml";
	InputStream inputStream = Resources.getResourceAsStream(resource);
	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
 
XML file contains:

components|detail
---|---
DataSource |  for acquiring database Connection instances	
TransactionManager |  for determining how transactions should be scoped and controlled. 	

Template:

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE configuration
	  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	  <environments default="development">
	    <environment id="development">
	      <transactionManager type="JDBC"/>
	      <dataSource type="POOLED">
	        <property name="driver" value="${driver}"/>
	        <property name="url" value="${url}"/>
	        <property name="username" value="${username}"/>
	        <property name="password" value="${password}"/>
	      </dataSource>
	    </environment>
	  </environments>
	  <mappers>
	    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
	  </mappers>
	</configuration>

Sample: /src/conf.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	    <environments default="development">
	        <environment id="development">
	            <transactionManager type="JDBC" />
	            <!-- Configuration -->
	            <dataSource type="POOLED">
	                <property name="driver" value="com.mysql.jdbc.Driver" />
	                <property name="url" value="jdbc:mysql://localhost:3306/mybatis" />
	                <property name="username" value="root" />
	                <property name="password" value="XDP" />
	            </dataSource>
	        </environment>
	    </environments>
	    
	</configuration>

>Without XML 

	DataSource dataSource = BlogDataSourceFactory.getBlogDataSource();
	TransactionFactory transactionFactory = new JdbcTransactionFactory();
	Environment environment = new Environment("development", transactionFactory, dataSource);
	Configuration configuration = new Configuration(environment);
	configuration.addMapper(BlogMapper.class);
	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);

# (3) Acquiring a SqlSession from SqlSessionFactory

	SqlSession session = sqlSessionFactory.openSession();
	try {
	  BlogMapper mapper = session.getMapper(BlogMapper.class);
	  Blog blog = mapper.selectBlog(101);
	} finally {
	  session.close();
	}	

# (4) Exploring Mapped SQL Statements

> /src/me.gacl.mapping/userMapper.xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="me.gacl.mapping.userMapper">
		<select id="getUser" parameterType="int" resultType="me.gacl.domain.User">
	        select * from users where id=#{id}
	    </select>
	</mapper>	

> /src/me.gacl.domain/User.class

	package me.gacl.domain;
	/**
	 * @author gacl
	 */
	public class User {
	    private int id;
	    private String name;
	    private int age;

	    public int getId() {
	        return id;
	    }

	    public void setId(int id) {
	        this.id = id;
	    }

	    public String getName() {
	        return name;
	    }

	    public void setName(String name) {
	        this.name = name;
	    }

	    public int getAge() {
	        return age;
	    }

	    public void setAge(int age) {
	        this.age = age;
	    }

	    @Override
	    public String toString() {
	        return "User [id=" + id + ", name=" + name + ", age=" + age + "]";
	    }
	}

>/src/conf.xml  register userMapper.xml 

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
	    <environments default="development">
	        <environment id="development">
	            <transactionManager type="JDBC" />
	            <!-- step up database -->
	            <dataSource type="POOLED">
	                <property name="driver" value="com.mysql.jdbc.Driver" />
	                <property name="url" value="jdbc:mysql://localhost:3306/mybatis" />
	                <property name="username" value="root" />
	                <property name="password" value="XDP" />
	            </dataSource>
	        </environment>
	    </environments>
	    
	    <mappers>
	        <mapper resource="me/gacl/mapping/userMapper.xml"/>
	    </mappers>
	    
	</configuration>

> /src/me.gacl.test/Test1.class  test

package me.gacl.test;

import java.io.IOException;
import java.io.InputStream;
import java.io.Reader;
import me.gacl.domain.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class Test1 {

    public static void main(String[] args) throws IOException {
        String resource = "conf.xml";
        InputStream is = Test1.class.getClassLoader().getResourceAsStream(resource);
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
        SqlSession session = sessionFactory.openSession();
        String statement = "me.gacl.mapping.userMapper.getUser";
        User user = session.selectOne(statement, 1);
        System.out.println(user);
    }
} 

