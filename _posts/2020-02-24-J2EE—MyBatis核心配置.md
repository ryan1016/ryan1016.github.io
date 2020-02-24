---
title: Mybatis核心配置
layout: post
categories: J2EE
---



## 什么是Mybatis？
&emsp;&emsp;MyBatis（前身是iBatis）是一个支持普通SQL查询、存储过程以及高级映射的持久层框架。
&emsp;&emsp;MyBatis框架也被称之为ORM（Object/Relation Mapping，即对象关系映射）框架。所谓的ORM就是一种为了解决面向对象与关系型数据库中数据类型不匹配的技术，它通过描述Java对象与数据库表之间的映射关系，自动将Java应用程序中的对象持久化到关系型数据库的表中。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224123107896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
## MyBatis的使用

- 先去[https://github.com/mybatis/mybatis-3/releases](https://github.com/mybatis/mybatis-3/releases)下载合适版本的mybatis的jar包（通常用3.4.x及3.5.x的，笔者用的是3.5.4）。
- 应用程序中引入MyBatis的核心包和lib目录中的依赖包（后续会有文件目录截图）。

## MyBatis的工作原理
![mybatis工作原理](https://img-blog.csdnimg.cn/2020022412362612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


## MyBatis入门程序

- **目录树**

![目录树](https://img-blog.csdnimg.cn/20200224123920940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **Customer类**

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

- **MyBatisTest类：**

```java
package com.ryan.test;

import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import com.ryan.po.Customer;
/**
 * 入门程序测试类
 */
public class MybatisTest {
	/**
	 * 根据客户编号查询客户信息
	 */
	@Test
	public void findCustomerByIdTest() throws Exception {
		// 1、读取配置文件
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		// 2、根据配置文件构建SqlSessionFactory
		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		// 3、通过SqlSessionFactory创建SqlSession
		SqlSession sqlSession = sqlSessionFactory.openSession();
		// 4、SqlSession执行映射文件中定义的SQL，并返回映射结果
		Customer customer = sqlSession.selectOne("com.ryan.mapper.CustomerMapper.findCustomerById", 1);
		// 打印输出结果
		System.out.println(customer.toString());
		// 5、关闭SqlSession
		sqlSession.close();
	}
	
//	/**
//	 * 根据用户名称来模糊查询用户信息列表
//	 */
//	@Test
//	public void findCustomerByNameTest() throws Exception{	
//	    // 1、读取配置文件
//	    String resource = "mybatis-config.xml";
//	    InputStream inputStream = Resources.getResourceAsStream(resource);
//	    // 2、根据配置文件构建SqlSessionFactory
//	    SqlSessionFactory sqlSessionFactory = 
//	new SqlSessionFactoryBuilder().build(inputStream);
//	    // 3、通过SqlSessionFactory创建SqlSession
//	    SqlSession sqlSession = sqlSessionFactory.openSession();
//	    // 4、SqlSession执行映射文件中定义的SQL，并返回映射结果
//	    List<Customer> customers = sqlSession.selectList("com.ryan.mapper"
//					+ ".CustomerMapper.findCustomerByName", "j");
//	    for (Customer customer : customers) {
//	        //打印输出结果集
//	        System.out.println(customer);
//	    }
//	    // 5、关闭SqlSession
//	    sqlSession.close();
//	}
	
//	/**
//	 * 添加客户
//	 */
//	@Test
//	public void addCustomerTest() throws Exception{		
//	    // 1、读取配置文件
//	    String resource = "mybatis-config.xml";
//	    InputStream inputStream = Resources.getResourceAsStream(resource);
//	    // 2、根据配置文件构建SqlSessionFactory
//	    SqlSessionFactory sqlSessionFactory = 
//	    		new SqlSessionFactoryBuilder().build(inputStream);
//	    // 3、通过SqlSessionFactory创建SqlSession
//	    SqlSession sqlSession = sqlSessionFactory.openSession();
//	    // 4、SqlSession执行添加操作
//	    // 4.1创建Customer对象，并向对象中添加数据
//	    Customer customer = new Customer();
//	    customer.setUsername("rose");
//	    customer.setJobs("student");
//	    customer.setPhone("13333533092");
//	    // 4.2执行SqlSession的插入方法，返回的是SQL语句影响的行数
//		int rows = sqlSession.insert("com.ryan.mapper"
//					+ ".CustomerMapper.addCustomer", customer);
//	    // 4.3通过返回结果判断插入操作是否执行成功
//	    if(rows > 0){
//	        System.out.println("您成功插入了"+rows+"条数据！");
//	    }else{
//	        System.out.println("执行插入操作失败！！！");
//	    }
//	    // 4.4提交事务
//	    sqlSession.commit();
//	    // 5、关闭SqlSession
//	    sqlSession.close();
//	}
//
//	/**
//	 * 更新客户
//	 */
//	@Test
//	public void updateCustomerTest() throws Exception{		
//	    // 1、读取配置文件
//	    String resource = "mybatis-config.xml";
//	    InputStream inputStream = Resources.getResourceAsStream(resource);
//	    // 2、根据配置文件构建SqlSessionFactory
//	    SqlSessionFactory sqlSessionFactory = 
//	    		new SqlSessionFactoryBuilder().build(inputStream);
//	    // 3、通过SqlSessionFactory创建SqlSession
//	    SqlSession sqlSession = sqlSessionFactory.openSession();
//	    // 4、SqlSession执行更新操作
//	    // 4.1创建Customer对象，对对象中的数据进行模拟更新
//	    Customer customer = new Customer();
//	    customer.setId(4);
//	    customer.setUsername("rose");
//	    customer.setJobs("programmer");
//	    customer.setPhone("13311111111");
//	    // 4.2执行SqlSession的更新方法，返回的是SQL语句影响的行数
//	    int rows = sqlSession.update("com.ryan.mapper"
//	            + ".CustomerMapper.updateCustomer", customer);
//	    // 4.3通过返回结果判断更新操作是否执行成功
//	    if(rows > 0){
//	        System.out.println("您成功修改了"+rows+"条数据！");
//	    }else{
//	        System.out.println("执行修改操作失败！！！");
//	    }
//	    // 4.4提交事务
//	    sqlSession.commit();
//	    // 5、关闭SqlSession
//	    sqlSession.close();
//	}
//
//	/**
//	 * 删除客户
//	 */
//	@Test
//	public void deleteCustomerTest() throws Exception{		
//	    // 1、读取配置文件
//	    String resource = "mybatis-config.xml";
//	    InputStream inputStream = Resources.getResourceAsStream(resource);
//	    // 2、根据配置文件构建SqlSessionFactory
//	    SqlSessionFactory sqlSessionFactory = 
//	            new SqlSessionFactoryBuilder().build(inputStream);
//	    // 3、通过SqlSessionFactory创建SqlSession
//	    SqlSession sqlSession = sqlSessionFactory.openSession();
//	    // 4、SqlSession执行删除操作
//	    // 4.1执行SqlSession的删除方法，返回的是SQL语句影响的行数
//	    int rows = sqlSession.delete("com.ryan.mapper"
//	            + ".CustomerMapper.deleteCustomer", 4);
//	    // 4.2通过返回结果判断删除操作是否执行成功
//	    if(rows > 0){
//	        System.out.println("您成功删除了"+rows+"条数据！");
//	    }else{
//	        System.out.println("执行删除操作失败！！！");
//	    }
//	    // 4.3提交事务
//	    sqlSession.commit();
//	    // 5、关闭SqlSession
//	    sqlSession.close();
//	}

}

```

- ** CustomerMapper.xml **

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace表示命名空间 -->
<mapper namespace="com.ryan.mapper.CustomerMapper">
    <!--根据客户编号获取客户信息 -->
	<select id="findCustomerById" parameterType="Integer"
		resultType="com.ryan.po.Customer">
		select * from t_customer where id=${id}
	</select>
	
	<!--根据客户名模糊查询客户信息列表-->
	<select id="findCustomerByName" parameterType="String"
	    resultType="com.ryan.po.Customer">
	    <!-- select * from t_customer where username like '%${value}%' -->
	    select * from t_customer where username like concat('%',#{value},'%')
	</select>
	
	<!-- 添加客户信息 -->
	<insert id="addCustomer" parameterType="com.ryan.po.Customer">
	    insert into t_customer(username,jobs,phone)
	    values(#{username},#{jobs},#{phone})
	</insert>
	
	<!-- 更新客户信息 -->
	<update id="updateCustomer" parameterType="com.ryan.po.Customer">
	    update t_customer set
	    username=#{username},jobs=#{jobs},phone=#{phone}
	    where id=#{id}
	</update>
	
	<!-- 删除客户信息 -->
	<delete id="deleteCustomer" parameterType="Integer">
	    delete from t_customer where id=#{id}
	</delete>
</mapper>


```

- **mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--1.配置环境 ，默认的环境id为mysql-->
    <environments default="mysql">
        <!--1.2.配置id为mysql的数据库环境 -->
        <environment id="mysql">
            <!-- 使用JDBC的事务管理 -->
            <transactionManager type="JDBC" />
            <!--数据库连接池 -->
            <dataSource type="POOLED">
			  <property name="driver" value="com.mysql.cj.jdbc.Driver" />
			  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC" />
			  <property name="username" value="root" />
			  <property name="password" value="root" />
            </dataSource>
        </environment>
    </environments>
    <!--2.配置Mapper的位置 -->
    <mappers>
		<mapper resource="com/ryan/mapper/CustomerMapper.xml" />
    </mappers>
</configuration>


```

- **log4j.properties**

```python
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.ryan=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n


```

## 运行结果
- 按ID查询用户

![id查询](https://img-blog.csdnimg.cn/20200224124624140.png)

- 用户名模糊查询

![模糊查询](https://img-blog.csdnimg.cn/20200224124733366.png)

- 增加

![增加](https://img-blog.csdnimg.cn/20200224125219539.png)

- 删除

![删除](https://img-blog.csdnimg.cn/20200224125322446.png)

- 修改

![修改](https://img-blog.csdnimg.cn/20200224125422719.png)

- MyBatis的套路

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224125458315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

1. 读取配置文件

```java
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);

```

2. 根据配置文件构建SqlSessionFactory

```java
 SqlSessionFactory sqlSessionFactory = 
	    		new SqlSessionFactoryBuilder().build(inputStream);

```
3. 通过SqlSessionFactory创建SqlSession

```java
 SqlSession sqlSession = sqlSessionFactory.openSession();

```
4. 使用SqlSession对象操作数据库(以更新操作为例)

![更新](https://img-blog.csdnimg.cn/20200224125922582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
5. 关闭SqlSession

```java
sqlSession.close();

```


## 遇到的小问题

1. <font color = "red">java.lang.ClassNotFoundException: Cannot find class: com.mysql.cj.jdbc.Driver</font>

**原因：**
- 配置书写错误：配置文件value值引号内不能有空格，属性文件配置信息末尾不能有空格

（1）打开属性文件中com.mysql.jdbc.Driver后发现多了一个空格(如下我标出了），所以写属性文件时一定别多输入多余的空格了。

         jdbc.driverClassName=com.mysql.cj.jdbc.Driver(此处有空格)
         
（2）配置文件中的value值的" "号中前面或后面有空格，主要是一些框架的核心配置文件，比如mybatis的全局配置文件如下：

```xml
<property name="driver" value="（此处有空格） com.mysql.jdbc.Driver" />
<property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis" />
<property name="username" value="root" />
<property name="password" value="root" />
```
- 引包错误

没有引用jdbc驱动的jar包（一般来说不是这个问题，可以尝试把驱动换成更高级的，或许是低级驱动程序的问题）


欢迎访问我的博客：[Welcome To Ryan's Home](https://ryan1016.github.io)
