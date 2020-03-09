---
title: MyBatis和Spring的整合
layout: post
categories: J2EE
---


# 整合环境搭建
&emsp;&emsp;要实现MyBatis与Spring的整合，很明显需要这两个框架的JAR包，但是只使用这两个框架中所提供的JAR包是不够的，还需要其他的JAR包来配合使用，整合时所需准备的JAR包具体如下。

## 准备工作
- **Spring框架所需的JAR包**

![Spring-jar](https://img-blog.csdnimg.cn/2020030211373339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **MyBatis框架所需的JAR包：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302113830226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **MyBatis与Spring整合的中间JAR**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302114009956.png)
- **数据库驱动JAR（MySQL）**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302114024889.png)
- **数据源所需JAR（DBCP）**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302114050437.png)

## 操作步骤

- **创建项目，引入JAR包**

&emsp;&emsp;在Eclipse中，创建一个Web项目，将前面提到的JAR包添加到项目的lib目录中，并发布到类路径下。

- **编写db.properties**

```properties
jdbc.driver=com.mysql.cj.jdbc.Driverjdbc.
url=jdbc:mysql://localhost:3306/mybatisjdbc.
username=root.
password=root.
maxTotal=30jdbc.
maxIdle=10jdbc.
initialSize=5
```

- **编写Spring配置文件applicationContext.xml**

```xml
 <beans xmlns="http://www.springframework.org/schema/beans"
    <!--读取db.properties-->
    <context:property-placeholder location="classpath:db.properties"/>
    <!--配置数据源-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        	<property name="driverClassName" value="${jdbc.driver}" />
       	 <property name="url" value="${jdbc.url}" />
       	 <property name="username" value="${jdbc.username}" />
	 <property name="password" value="${jdbc.password}" />
	 <property name="maxTotal" value="${jdbc.maxTotal}" />
	 <property name="maxIdle" value="${jdbc.maxIdle}" />
	 <property name="initialSize" value="${jdbc.initialSize}" />
    </bean>
    <!--配置事务管理器-->
    <bean id="transactionManager"                 			   	 	class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        	<property name="dataSource" ref="dataSource" />
    </bean>	
    <!--开启事务注释-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
    <!--配置MyBatis工厂-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
                 <property name="dataSource" ref="dataSource" />
                 <property name="configLocation" value="classpath:mybatis-config.xml"/>
   </bean>
</beans>
```

- **编写MyBatis配置文件mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
…
<configuration> 
      <typeAliases>
             <package name="com.itheima.po" />
      </typeAliases> 
	    <mappers> 
                ...
	   </mappers>
</configuration>
```

- **引入log4j.properties**

```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.itheima=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

# 传统的DAO方式开发整合
&emsp;&emsp;采用传统DAO开发方式进行MyBatis与Spring框架的整合时，可以使用mybatis-spring包中所提供的SqlSessionTemplate类或SqlSessionDaoSupport类来实现。
- SqlSessionTemplate：是mybatis-spring的核心类，它负责管理MyBatis的SqlSession，调用MyBatis的SQL方法。当调用SQL方法时，SqlSessionTemplate将会保证使用的SqlSession和当前Spring的事务是相关的。它还管理SqlSession的生命周期，包含必要的关闭、提交和回滚操作。
- SqlSessionDaoSupport：是一个抽象支持类，它继承了DaoSupport类，主要是作为DAO的基类来使用。可以通过SqlSessionDaoSupport类的getSqlSession()方法来获取所需的SqlSession。


# Mapper接口方式的开发整合

&emsp;&emsp;在MyBatis+Spring的项目中，虽然使用传统的DAO开发方式可以实现所需功能，但是采用这种方式在实现类中会出现大量的重复代码，在方法中也需要指定映射文件中执行语句的id，并且不能保证编写时id的正确性（运行时才能知道）。为此，我们可以使用MyBatis提供的另外一种编程方式，即使用Mapper接口编程。
&emsp;&emsp;MapperFactoryBean是MyBatis-Spring团队提供的一个用于根据Mapper接口生成Mapper对象的类，该类在Spring配置文件中使用时可以配置以下参数：

- mapperInterface：用于指定接口；
- SqlSessionFactory：用于指定SqlSessionFactory；
- SqlSessionTemplate：用于指定SqlSessionTemplate。如果与SqlSessionFactory同时设定，则只会启用SqlSessionTemplate。

# 基于MapperFactoryBean的整合

**Tips1：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030212050774.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/2020030212062579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;在实际的项目中，DAO层会包含很多接口，如果每一个接口都在Spring配置文件中配置，不但会增加工作量，还会使得Spring配置文件非常臃肿。为此，可以采用自动扫描的形式来配置MyBatis中的映射器——采用MapperScannerConfigurer类。

&emsp;&emsp;MapperScannerConfigurer类在Spring配置文件中可以配置以下属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/202003021208326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;MapperScannerConfigurer的使用非常简单，只需要在Spring的配置文件中编写如下代码：
```xml
<!-- Mapper代理开发（基于MapperScannerConfigurer） --> 
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"> 
	<property name="basePackage" value="com.itheima.mapper" /> 
</bean>
```
&emsp;&emsp;通常情况下，MapperScannerConfigurer在使用时只需通过basePackage属性指定需要扫描的包即可，Spring会自动的通过包中的接口来生成映射器。这使得开发人员可以在编写很少代码的情况下，完成对映射器的配置，从而提高开发效率。

# 测试事务

&emsp;&emsp;在项目中，Service层既是处理业务的地方，又是管理数据库事务的地方。要对事务进行测试，首先需要创建Service层，并在Service层编写添加客户操作的代码；然后在添加操作的代码后，有意的添加一段异常代码（如int i = 1/0;）来模拟现实中的意外情况；最后编写测试方法，调用业务层的添加方法。这样，程序在执行到错误代码时就会出现异常。 
&emsp;&emsp;在没有事务管理的情况下，即使出现了异常，数据也会被存储到数据表中；如果添加了事务管理，并且事务管理的配置正确，那么在执行上述操作时，所添加的数据将不能够插入到数据表中。


# 工程目录
&emsp;&emsp;导入到JAR包较多，注意不要导错了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302132321107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302132338927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
# 测试代码
**com.ryan.dao:**
<font color = "red">CustomerDao.java：</font>
```java
package com.ryan.dao;
import com.ryan.po.Customer;
public interface CustomerDao {
	// 通过id查询客户
	public Customer findCustomerById(Integer id);
}

```

**com.ryan.dao.impl:**
<font color = "red">CustomerDaoImpl.java：</font>
```java
package com.ryan.dao.impl;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import com.ryan.dao.CustomerDao;
import com.ryan.po.Customer;
public class CustomerDaoImpl 
                      extends SqlSessionDaoSupport implements CustomerDao {
	// 通过id查询客户
	public Customer findCustomerById(Integer id) {
         	return this.getSqlSession().selectOne("com.ryan.po"
				      + ".CustomerMapper.findCustomerById", id);
	}
}

```

**com.ryan.mapper:**
<font color = "red">CustomerMapper.java：</font>
```java
package com.ryan.mapper;
import com.ryan.po.Customer;
public interface CustomerMapper {
	// 通过id查询客户
	public Customer findCustomerById(Integer id);
	
	// 添加客户
	public void addCustomer(Customer customer);

}

```

<font color = "red">CustomerMapper.xml：</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.CustomerMapper">
	<!--根据id查询客户信息 -->
	<select id="findCustomerById" parameterType="Integer"
		     resultType="customer">
		select * from t_customer where id = #{id}
	</select>
	
	<!--添加客户信息 -->
	<insert id="addCustomer" parameterType="customer">
	    insert into t_customer(username,jobs,phone)
	    values(#{username},#{jobs},#{phone})
	</insert>
	
</mapper>

```


**com.ryan.po:**
<font color = "red">Customer.java：</font>
```java
package com.ryan.po;
/**
 * 客户持久化类
 */
public class Customer {
	private Integer id;       // 主键id
	private String username; // 客户名称
	private String jobs;      // 职业
	private String phone;     // 电话
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getJobs() {
		return jobs;
	}
	public void setJobs(String jobs) {
		this.jobs = jobs;
	}
	public String getPhone() {
		return phone;
	}
	public void setPhone(String phone) {
		this.phone = phone;
	}
	@Override
	public String toString() {
		return "Customer [id=" + id + ", username=" + username + 
				       ", jobs=" + jobs + ", phone=" + phone + "]";
	}
}

```

<font color = "red">CustomerMapper.xml：</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.po.CustomerMapper">
	<!--根据id查询客户信息 -->
	<select id="findCustomerById" parameterType="Integer"
		     resultType="customer">
		select * from t_customer where id = #{id}
	</select>
</mapper>

```


**com.ryan.service:**
<font color = "red">CustomerService.java：</font>
```java
package com.ryan.service;
import com.ryan.po.Customer;
public interface CustomerService {
    public void addCustomer(Customer customer);
}

```


**com.ryan.service.impl:**
<font color = "red">CustomerServiceImpl.java：</font>
```java
package com.ryan.service.impl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.ryan.mapper.CustomerMapper;
import com.ryan.po.Customer;
import com.ryan.service.CustomerService;
@Service
//@Transactional
public class CustomerServiceImpl implements CustomerService {
	//注解注入CustomerMapper
	@Autowired
	private CustomerMapper customerMapper;
	//添加客户
	public void addCustomer(Customer customer) {
		this.customerMapper.addCustomer(customer);
	}
}

```


**com.ryan.test:**
<font color = "red">DaoTest.java：</font>
```java
package com.ryan.test;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import 
     org.springframework.context.support.ClassPathXmlApplicationContext;

import com.ryan.dao.CustomerDao;
import com.ryan.mapper.CustomerMapper;
import com.ryan.po.Customer;
/**
 * DAO测试类
 */
public class DaoTest {
	@Test
	public void findCustomerByIdDaoTest(){
		ApplicationContext act = 
		    new ClassPathXmlApplicationContext("applicationContext.xml");
          // 根据容器中Bean的id来获取指定的Bean
	     CustomerDao customerDao = 
                              (CustomerDao) act.getBean("customerDao");
//	     CustomerDao customerDao = act.getBean(CustomerDao.class);
		 Customer customer = customerDao.findCustomerById(1);
		 System.out.println(customer);
	}
	
	@Test
	public void findCustomerByIdMapperTest(){	
	    ApplicationContext act = 
	            new ClassPathXmlApplicationContext("applicationContext.xml");
	    CustomerMapper customerMapper = act.getBean(CustomerMapper.class);   
	    Customer customer = customerMapper.findCustomerById(1);
	    System.out.println(customer);
	}

}

```

<font color = "red">TransactionTest.java：</font>
```java
package com.ryan.test;
import org.springframework.context.ApplicationContext;
import 
     org.springframework.context.support.ClassPathXmlApplicationContext;

import com.ryan.po.Customer;
import com.ryan.service.CustomerService;
/**
 * 测试事务
 */
public class TransactionTest {
	public static void main(String[] args) {
		ApplicationContext act = 
             new ClassPathXmlApplicationContext("applicationContext.xml");
		CustomerService customerService = 
                                            act.getBean(CustomerService.class);
		Customer customer = new Customer();
		customer.setUsername("zhangsan");
		customer.setJobs("manager");
		customer.setPhone("13233334444");
		customerService.addCustomer(customer);
	}
}

```


**src:**
<font color = "red">applicationContext.xml：</font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx" 
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
    http://www.springframework.org/schema/tx 
    http://www.springframework.org/schema/tx/spring-tx-4.3.xsd
    http://www.springframework.org/schema/context 
    http://www.springframework.org/schema/context/spring-context-4.3.xsd
    http://www.springframework.org/schema/aop 
    http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
    <!--读取db.properties -->
    <context:property-placeholder location="classpath:db.properties"/>
    <!-- 配置数据源 -->
	<bean id="dataSource" 
            class="org.apache.commons.dbcp2.BasicDataSource">
        <!--数据库驱动 -->
        <property name="driverClassName" value="${jdbc.driver}" />
        <!--连接数据库的url -->
        <property name="url" value="${jdbc.url}" />
        <!--连接数据库的用户名 -->
        <property name="username" value="${jdbc.username}" />
        <!--连接数据库的密码 -->
        <property name="password" value="${jdbc.password}" />
        <!--最大连接数 -->
        <property name="maxTotal" value="${jdbc.maxTotal}" />
        <!--最大空闲连接  -->
        <property name="maxIdle" value="${jdbc.maxIdle}" />
        <!--初始化连接数  -->
        <property name="initialSize" value="${jdbc.initialSize}" />
	</bean>
	<!-- 事务管理器，依赖于数据源 --> 
	<bean id="transactionManager" class=
     "org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>	
    <!--开启事务注解 -->
	<tx:annotation-driven transaction-manager="transactionManager"/>
    <!--配置MyBatis工厂 -->
    <bean id="sqlSessionFactory" 
            class="org.mybatis.spring.SqlSessionFactoryBean">
         <!--注入数据源 -->
         <property name="dataSource" ref="dataSource" />
         <!--指定核心配置文件位置 -->
   		<property name="configLocation" value="classpath:mybatis-config.xml"/>
   </bean>
   
   <!--实例化Dao -->
	<bean id="customerDao" class="com.ryan.dao.impl.CustomerDaoImpl">
	<!-- 注入SqlSessionFactory对象实例-->
	     <property name="sqlSessionFactory" ref="sqlSessionFactory" />
	</bean>
	<!-- Mapper代理开发（基于MapperFactoryBean） -->
	<!-- <bean id="customerMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
	    <property name="mapperInterface" value="com.ryan.mapper.CustomerMapper" />
	    <property name="sqlSessionFactory" ref="sqlSessionFactory" />  
	</bean> -->
	<!-- Mapper代理开发（基于MapperScannerConfigurer） -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	     <property name="basePackage" value="com.ryan.mapper" />
	</bean>
	
	<!-- 开启扫描 --> 
	<context:component-scan base-package="com.ryan.service" />
	
</beans>

```

<font color = "red">db.properties：</font>
```java
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
jdbc.maxTotal=30
jdbc.maxIdle=10
jdbc.initialSize=5

```

<font color = "red">log4j.properties：</font>
```java
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.ryan=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

<font color = "red">mybatis-config.xml：</font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置别名 -->
    <typeAliases>
        <package name="com.ryan.po" />
    </typeAliases>
    <!--配置Mapper的位置 -->
	<mappers> 
       <mapper resource="com/ryan/po/CustomerMapper.xml" />
       <!-- Mapper接口开发方式 -->
	   <mapper resource="com/ryan/mapper/CustomerMapper.xml" />
       
	</mappers>
</configuration>

```


# 运行结果

**DaoTest.java:**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302154721399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

**TransactionTest.java:**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302154850525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)




---
---
**欢迎查看我的CSDN博客：[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448)**

---
---