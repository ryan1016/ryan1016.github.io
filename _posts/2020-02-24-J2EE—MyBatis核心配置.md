---
title: Mybatis核心配置
layout: post
categories: J2EE
---




## 什么是SqlSessionFactory?

- SqlSessionFactory是MyBatis框架中十分重要的对象，它是单个数据库映射关系经过编译后的内存镜像，其主要作用是创建SqlSession。
- SqlSessionFactory对象的实例可以通过SqlSessionFactoryBuilder对象来构建，而SqlSessionFactoryBuilder则可以通过XML配置文件或一个预先定义好的Configuration实例构建出SqlSessionFactory的实例。

**<font color = "red">构建SqlSessionFactory</font>**

- 通过XML配置文件构建出的SqlSessionFactory实例现代码如下:

```java
InputStream inputStream = 
		Resources.getResourceAsStream("配置文件位置");
SqlSessionFactory sqlSessionFactory =
		new SqlSessionFactoryBuilder().build(inputStream);
```

- SqlSessionFactory对象是线程安全的，它一旦被创建，在整个应用执行期间都会存在。如果我们多次的创建同一个数据库的SqlSessionFactory，那么此数据库的资源将很容易被耗尽。为此，通常每一个数据库都会只对应一个SqlSessionFactory，所以在构建SqlSessionFactory实例时，建议使用单列模式。


## 什么是SqlSession?

- SqlSession是MyBatis框架中另一个重要的对象，它是应用程序与持久层之间执行交互操作的一个单线程对象，其主要作用是执行持久化操作。

**<font color = "red">SqlSession中的方法</font>**

1. 查询方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224150733400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

2. 插入方法

![插入方法](https://img-blog.csdnimg.cn/20200224150954131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

3. 其他方法

![其他方法](https://img-blog.csdnimg.cn/20200224151035367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

**<font color = "red">使用工具类创建SqlSession：</font>**

```java
public class MybatisUtils {
       private static SqlSessionFactory sqlSessionFactory = null;
       static {
           try {
    Reader reader = Resources.getResourceAsReader("mybatis-config.xml");
    sqlSessionFactory =  new SqlSessionFactoryBuilder().build(reader);
           } catch (Exception e) {
    e.printStackTrace();
           }
       }
       public static SqlSession getSession() {
            return sqlSessionFactory.openSession();
       }
 }

```

## 配置文件

 &emsp;&emsp;在MyBatis框架的核心配置文件中，<configuration>元素是配置文件的根元素，其他元素都要在<configuration>元素内配置。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224151546657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
**（一）<font color = "red">```<properties>```元素</font>**
&emsp;&emsp;properties是一个配置属性的元素，该元素通常用来将内部的配置外在化，即通过外部的配置来动态的替换内部定义的属性。例如，数据库的连接等属性，就可以通过典型的Java属性文件中的配置来替换，具体方式如下：

1. 编写db.properties

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224151638443.png)


2. 配置<properities … />属性

```xml
 <properties resource="db.properties" />
```
3. 修改配置文件中数据库连接信息

```xml

<dataSource type="POOLED">
	<!-- 数据库驱动 -->
	<property name="driver" value="${jdbc.driver}" />
	<!-- 连接数据库的url --><property name="url" value="${jdbc.url}" />
	<!-- 连接数据库的用户名 -->
	<property name="username" value="${jdbc.username}" />
	<!-- 连接数据库的密码 -->
	<property name="password" value="${jdbc.password}" />
</dataSource>
```


**（二）<font color = "red">```<setting>```元素</font>**

 &emsp;&emsp;settings元素主要用于改变MyBatis运行时的行为，例如开启二级缓存、开启延迟加载等。 

```xml
<!-- 设置 -->
<settings>
	<setting name="cacheEnabled" value="true" />
	<setting name="lazyLoadingEnabled" value="true" />
	<setting name="multipleResultSetsEnabled" value="true" />
	<setting name="useColumnLabel" value="true" />
	<setting name="useGeneratedKeys" value="false" />
	<setting name="autoMappingBehavior" value="PARTIAL" />
	...
</settings>

```

Tip:上述配置通常不需要开发人员去配置，读者作为了解即可。

**（三）<font color = "red">```<typeAliases>```元素</font>**

 &emsp;&emsp;typeAliases元素用于为配置文件中的Java类型设置一个简短的名字，即设置别名。别名的设置与XML配置相关，其使用的意义在于减少全限定类名的冗余。 

1. 使用<typeAliases>元素配置别名的方法如下：

```xml
<typeAliases>
	<typeAlias alias="user" type="com.ryan.po.User"/>
</typeAliases>

```

2. 当POJO类过多时，可以通过自动扫描包的形式自定义别名，具体如下：

```xml
<typeAliases>
	<package name="com.ryan.po"/>
</typeAliases>
```

   **MyBatis框架默认为许多常见的Java类型提供了相应的类型别名，如下表所示。**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224153410913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)
**（四）<font color = "red">```<typeHandler>```元素</font>**

 &emsp;&emsp;typeHandler的作用就是将预处理语句中传入的参数从javaType（Java类型）转换为jdbcType（JDBC类型），或者从数据库取出结果时将jdbcType转换为javaType。
<typeHandler>元素可以在配置文件中注册自定义的类型处理器，它的使用方式有两种。
1. 注册一个类的类型处理器

```xml
<typeHandlers> 
	<typeHandler handler="com.ryan.type.CustomtypeHandler" />
</typeHandlers>
```
2. 注册一个包中所有的类型处理器

```xml
<typeHandlers>
	<package name="com.ryan.type" />
</typeHandlers>
```
**（五）<font color = "red">```<objectFactory>```元素</font>**

 &emsp;&emsp;MyBatis中默认的ObjectFactory的作用是实例化目标类，它既可以通过默认构造方法实例化，也可以在参数映射存在的时候通过参数构造方法来实例化。通常使用默认的ObjectFactory即可。
 &emsp;&emsp;大部分场景下都不用配置和修改默认的ObjectFactory ，如果想覆盖ObjectFactory的默认行为，可以通过自定义ObjectFactory来实现，具体如下：
 
 1. 自定义一个对象工厂

```java
public class MyObjectFactory extends DefaultObjectFactory {
    private static final long serialVersionUID = -4114845625429965832L;
    public <T> T create(Class<T> type) {
return super.create(type);
    }
    public <T> T create(Class<T> type, List<Class<?>> constructorArgTypes, 
                 List<Object> constructorArgs) {
return super.create(type, constructorArgTypes, constructorArgs);
    }
    public void setProperties(Properties properties) {
super.setProperties(properties);
    }
    public <T> boolean isCollection(Class<T> type) {
return Collection.class.isAssignableFrom(type);
    }
}
```
 2. 在配置文件中使用```<objectFactory>```元素配置自定义的ObjectFactory
 
 ```xml
 <objectFactory type="com.itheima.factory.MyObjectFactory">
     <property name="name" value="MyObjectFactory"/>
</objectFactory>
 ```
**（六）<font color = "red">```<plugins>```元素</font>**

 &emsp;&emsp; MyBatis允许在已映射语句执行过程中的某一点进行拦截调用，这种拦截调用是通过插件来实现的。<plugins>元素的作用就是配置用户所开发的插件。
 &emsp;&emsp;  如果用户想要进行插件开发，必须要先了解其内部运行原理，因为在试图修改或重写已有方法的行为时，很可能会破坏MyBatis原有的核心模块。关于插件的使用，本书不做详细讲解，读者只需了解<plugins>元素的作用即可，有兴趣的读者可以查找官方文档等资料自行学习。

**（七）<font color = "red">```<enviroments>```元素</font>**

 &emsp;&emsp;environments元素用于对环境进行配置。MyBatis的环境配置实际上就是数据源的配置，我们可以通过<environments>元素配置多种数据源，即配置多种数据库。
&emsp;&emsp; 使用environments元素进行环境配置的示例如下：

```xml
<environments default="development">
	<environment id="development">
		<transactionManager type="JDBC" />
      	<dataSource type="POOLED">
             <property name="driver" value="${jdbc.driver}" />
             <property name="url" value="${jdbc.url}" />
             <property name="username" value="${jdbc.username}" />
             <property name="password" value="${jdbc.password}" />
 		</dataSource>
    </environment>
            ...
</environments>
```

<font color = "red">Tips：</font>如果项目中使用的是Spring+ MyBatis，则没有必要在MyBatis中配置事务管理器，因为实际开发中，会使用Spring自带的管理器来实现事务管理。

**（八）<font color = "red">```<mappers>```元素</font>**

 &emsp;&emsp; mappers元素用于指定MyBatis映射文件的位置，一般可以使用以下4种方法引入映射器文件，具体如下。
 
 1. 使用类路径引入

```xml
<mappers>
    <mapper resource="com/ryan/mapper/UserMapper.xml"/>
</mappers>
```
 2. 使用本地文件路径引入

```xml
<mappers>
    <mapper url="file:///D:/com/ryan/mapper/UserMapper.xml"/>
</mappers>
```
 3. 使用接口引入

```xml
<mappers>
    <mapper class="com.ryan.mapper.UserMapper"/>
</mappers>
```
 4. 使用包名引入

```xml
<mappers>
    <package name="com.ryan.mapper"/>
</mappers>

```

## 映射文件
 &emsp;&emsp;在映射文件中，<mapper>元素是映射文件的根元素，其他元素都是它的子元素。 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224155359145.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
**（一）<font color = "red">```<select>```元素</font>**

&emsp;&emsp; select元素用来映射查询语句，它可以帮助我们从数据库中读取出数据，并组装数据给业务开发人员。

```xml
<select id="findCustomerById" parameterType="Integer">
	resultType="com.ryan.po.Customer">
	select * from t_customer where id = #{id}
</select>
```
**select元素常用属性**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224155912320.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)
**（二）<font color = "red">```<insert>```元素</font>**

&emsp;&emsp; insert元素用于映射插入语句，在执行完元素中定义的SQL语句后，会返回一个表示插入记录数的整数。

```xml
<insert
      id="addCustomer"
      parameterType="com.itheima.po.Customer"
      flushCache="true"
      statementType="PREPARED"
      keyProperty=""
      keyColumn=""
      useGeneratedKeys=""
      timeout="20">
```

**insert常用属性**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224160106728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)

**（三）<font color = "red">```<update>```元素和```<delete>```元素</font>**

&emsp;&emsp;update和delete元素的使用比较简单，它们的属性配置也基本相同。

1. update和delete常用属性如下：

```xml
<update
      id="updateCustomer"
      parameterType="com.itheima.po.Customer"
      flushCache="true"
      statementType="PREPARED"
      timeout="20">
<delete
      id="deleteCustomer"
      parameterType="com.itheima.po.Customer"
      flushCache="true"
      statementType="PREPARED"
      timeout="20">
```

2. update和delete元素使用示例：

```xml
<update id="updateCustomer" parameterType="com.itheima.po.Customer">
       update t_customer 
       set username=#{username},jobs=#{jobs},phone=#{phone}
       where id=#{id}
</update>

<delete id="deleteCustomer" parameterType="Integer">
        delete from t_customer where id=#{id}
</delete>
```

**（四）<font color = "red">```<sql>```元素</font>**

&emsp;&emsp;<sql>元素的作用就是定义可重用的SQL代码片段，然后在其他语句中引用这一代码片段。

1. 定义一个包含id、username、jobs和phone字段的代码片段如下：

```xml
<sql id="customerColumns">id,username,jobs,phone</sql>
```
2. 上述代码片段可以包含在其他语句中使用，具体如下：

```xml
<select id="findCustomerById" parameterType="Integer"
            resultType="com.itheima.po.Customer">
    select <include refid="customerColumns"/>
    from t_customer 
    where id = #{id}
</select>
```

**（五）<font color = "red">```<resultMap>```元素</font>**

&emsp;&emsp;resultMap>元素表示结果映射集，是MyBatis中最重要也是最强大的元素。它的主要作用是定义映射规则、级联的更新以及定义类型转化器等。

&emsp;&emsp;resultMap元素中包含了一些子元素，它的元素结构如下所示：
```xml
<resultMap type="" id="">
       <constructor>    <!-- 类在实例化时,用来注入结果到构造方法中-->
             <idArg/>      <!-- ID参数;标记结果作为ID-->
             <arg/>          <!-- 注入到构造方法的一个普通结果-->
       </constructor>  
       <id/>                 <!-- 用于表示哪个列是主键-->
       <result/>           <!-- 注入到字段或JavaBean属性的普通结果-->
       <association property="" />        <!-- 用于一对一关联 -->
       <collection property="" />          <!-- 用于一对多关联 -->
       <discriminator javaType="">      <!-- 使用结果值来决定使用哪个结果映射-->
            <case value="" />                   <!-- 基于某些值的结果映射 -->
       </discriminator>	
</resultMap>
```

## 工程目录
![工程目录](https://img-blog.csdnimg.cn/20200301145537264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

## 代码
**MyObjectionFactory:**
```java
package com.ryan.factory;

import java.util.Collection;
import java.util.List;
import java.util.Properties;

import org.apache.ibatis.reflection.factory.DefaultObjectFactory;

//自定义工厂类
public class MyObjectFactory extends DefaultObjectFactory {
	private static final long serialVersionUID = -4114845625429965832L;
	public <T> T create(Class<T> type) {
		return super.create(type);
	}
	public <T> T create(Class<T> type, List<Class<?>> constructorArgTypes, 
                                           List<Object> constructorArgs) {
		return super.create(type, constructorArgTypes, constructorArgs);
	}
	public void setProperties(Properties properties) {
		super.setProperties(properties);
	}
	public <T> boolean isCollection(Class<T> type) {
		return Collection.class.isAssignableFrom(type);
	}
}
```

**Custorm：**

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
**User:**
```java
package com.ryan.po;
public class User {
	private Integer id;
	private String name;
	private Integer age;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", age=" + age + "]";
	}	
}

```

**MybatisUtils:**
```java
package com.ryan.utils;
import java.io.Reader;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
/**
 * 工具类
 */
public class MybatisUtils {
	private static SqlSessionFactory sqlSessionFactory = null;
	// 初始化SqlSessionFactory对象
	static {
		try {
			// 使用MyBatis提供的Resources类加载MyBatis的配置文件
			Reader reader = 
					Resources.getResourceAsReader("mybatis-config.xml");
			// 构建SqlSessionFactory工厂
			sqlSessionFactory = 
					new SqlSessionFactoryBuilder().build(reader);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	// 获取SqlSession对象的静态方法
	public static SqlSession getSession() {
		return sqlSessionFactory.openSession();
	}
}

```

**MybatisTest:**
```java
package com.ryan.test;
import java.util.List;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import com.ryan.po.Customer;
import com.ryan.po.User;
import com.ryan.utils.MybatisUtils;
/**
 * 入门程序测试类
 */
public class MybatisTest {
	/**
	 * 1.根据客户编号查询客户信息
	 */
	@Test
	public void findCustomerByIdTest() {
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		// SqlSession执行映射文件中定义的SQL，并返回映射结果
		Customer customer = sqlSession.selectOne("com.ryan.mapper" 
		                 + ".CustomerMapper.findCustomerById", 1);
		// 打印输出结果
		System.out.println(customer.toString());
		// 关闭SqlSession
		sqlSession.close();
	}
	/**
	 * 2.根据用户名称来模糊查询用户信息列表
	 */
	@Test
	public void findCustomerByNameTest() {		
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		// SqlSession执行映射文件中定义的SQL，并返回映射结果
		List<Customer> customers = sqlSession.selectList("com.ryan.mapper"
				+ ".CustomerMapper.findCustomerByName", "j");
		for (Customer customer : customers) {
			//打印输出结果集
			System.out.println(customer);					
		}
		// 关闭SqlSession
		sqlSession.close();
	}
	/**
	 * 3.添加客户
	 */
	@Test
	public void addCustomerTest(){
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		Customer customer = new Customer();
		customer.setUsername("rose");
		customer.setJobs("student");
		customer.setPhone("13333533092");
		// 使用主键自助增长的添加方法
//		int rows = sqlSession.insert("com.ryan.mapper."
//				+ "CustomerMapper.addCustomer", customer);
		// 使用自定义主键值的添加方法
		int rows = sqlSession.insert("com.ryan.mapper."
				+ "CustomerMapper.insertCustomer", customer);
		// 输出插入数据的主键id值
		System.out.println(customer.getId());		
		if(rows > 0){
			System.out.println("您成功插入了"+rows+"条数据！");
		}else{
			System.out.println("执行插入操作失败！！！");
		}
		sqlSession.commit();
		sqlSession.close();
	}
	/**
	 * 4.更新客户
	 */
	@Test
	public void updateCustomerTest() throws Exception{		
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		// SqlSession执行更新操作
		// 创建Customer对象，并向对象中添加数据
		Customer customer = new Customer();
		customer.setId(4);
		customer.setUsername("rose");
		customer.setJobs("programmer");
		customer.setPhone("13311111111");
		// 执行sqlSession的更新方法，返回的是SQL语句影响的行数
		int rows = sqlSession.update("com.ryan.mapper"
				+ ".CustomerMapper.updateCustomer", customer);
		// 通过返回结果判断更新操作是否执行成功
		if(rows > 0){
			System.out.println("您成功修改了"+rows+"条数据！");
		}else{
			System.out.println("执行修改操作失败！！！");
		}
		// 提交事务
		sqlSession.commit();
		// 关闭SqlSession
		sqlSession.close();
	}
	/**
	 * 5.删除客户
	 */
	@Test
	public void deleteCustomerTest() {		
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		// SqlSession执行删除操作
		// 执行SqlSession的删除方法，返回的是SQL语句影响的行数
		int rows = sqlSession.delete("com.ryan.mapper"
				+ ".CustomerMapper.deleteCustomer", 4);
		// 通过返回结果判断删除操作是否执行成功
		if(rows > 0){
			System.out.println("您成功删除了"+rows+"条数据！");
		}else{
			System.out.println("执行删除操作失败！！！");
		}
		// 提交事务
		sqlSession.commit();
		// 关闭SqlSession
		sqlSession.close();
	}
	
	@Test
	public void findAllUserTest() {
		// 获取SqlSession
		SqlSession sqlSession = MybatisUtils.getSession();
		// SqlSession执行映射文件中定义的SQL，并返回映射结果
		List<User> list = sqlSession.selectList("com.ryan.mapper.UserMapper.findAllUser");
		for (User user : list) {
			System.out.println(user);
		}
		// 关闭SqlSession
		sqlSession.close();
	}
}
```

**CustomerMapper.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
   PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace表示命名空间 -->
<mapper namespace="com.ryan.mapper.CustomerMapper">
	<!--1.根据客户编号获取客户信息 -->
<!-- 	<select id="findCustomerById" parameterType="Integer" -->
<!-- 		resultType="com.ryan.po.Customer"> -->
<!-- 		select * from t_customer where id = #{id} -->
<!-- 	</select> -->
	<!--2.根据客户名模糊查询客户信息列表 -->
	<select id="findCustomerByName" parameterType="String"
		resultType="com.ryan.po.Customer">
		select * from t_customer where username like '%${value}%'
	</select>
	<!-- 3.添加客户信息 -->
	<insert id="addCustomer" parameterType="com.ryan.po.Customer" 
	        keyProperty="id" useGeneratedKeys="true">	        
		insert into t_customer(username,jobs,phone)
		values(#{username},#{jobs},#{phone})
	</insert>
    <!-- 对于不支持自动生成主键的数据库，或取消自主增长规则的数据库可以自定义主键生成规则 -->
	<insert id="insertCustomer" parameterType="com.ryan.po.Customer">
	    <selectKey keyProperty="id" resultType="Integer" order="BEFORE">
	    select if(max(id) is null, 1, max(id) +1) as newId from t_customer
	    </selectKey>	        
		insert into t_customer(id,username,jobs,phone)
		values(#{id},#{username},#{jobs},#{phone})
	</insert>
	<!-- 4.更新客户信息 -->
	<update id="updateCustomer" parameterType="com.ryan.po.Customer">
		update t_customer 
		set username=#{username},jobs=#{jobs},phone=#{phone}
		where id=#{id}
	</update>
	<!-- 5.删除客户信息 -->
	<delete id="deleteCustomer" parameterType="Integer">
		delete from t_customer where id=#{id}
	</delete>

    <!--定义代码片段 -->
<!-- 	<sql id="customerColumns">id,username,jobs,phone</sql> -->
<!-- 	<select id="findCustomerById" parameterType="Integer" -->
<!-- 		resultType="com.ryan.po.Customer"> -->
<!-- 		select <include refid="customerColumns"/> -->
<!-- 		from t_customer  -->
<!-- 		where id = #{id} -->
<!-- 	</select> -->

    <!--定义表的前缀名 -->
	<sql id="tablename">
		${prefix}customer
	</sql>
    <!--定义要查询的表 -->
	<sql id="someinclude">
		from
		<include refid="${include_target}" />		
	</sql>
    <!--定义查询列 -->
	<sql id="customerColumns">
	   id,username,jobs,phone
	</sql>
    <!--根据id查询客户信息 -->
	<select id="findCustomerById" parameterType="Integer" 
	        resultType="com.ryan.po.Customer">
		select 
		<include refid="customerColumns"/>
		<include refid="someinclude">
			<property name="prefix" value="t_" />
			<property name="include_target" value="tablename" />
		</include>
		where id = #{id}
	</select>

</mapper>
```

**UserMapper.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
   PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.UserMapper">
    <resultMap type="com.ryan.po.User" id="resultMap">
      <id property="id" column="t_id"/>
      <result property="name" column="t_name"/>
      <result property="age" column="t_age"/>    
    </resultMap>
	<select id="findAllUser" resultMap="resultMap">
		select * from t_user
	</select>
</mapper>
```

**MybatisMapper.xml**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="db.properties" />
	<!-- 定义别名 -->
	<typeAliases>
		<!-- <typeAlias alias="user" type="com.ryan.po.User" /> -->
		<package name="com.ryan.po" />
	</typeAliases>
	
	<!-- 配置自定义工厂 -->
	<objectFactory type="com.ryan.factory.MyObjectFactory">
     <property name="name" value="MyObjectFactory"/>
	</objectFactory>

	<!--1.配置环境 ，默认的环境id为mysql -->
	<environments default="mysql">
		<!--1.2.配置id为mysql的数据库环境 -->
		<environment id="mysql">
			<!-- 使用JDBC的事务管理 -->
			<transactionManager type="JDBC" />
			<!--数据库连接池 -->
			<dataSource type="POOLED">
				<!-- 数据库驱动 -->
				<property name="driver" value="${jdbc.driver}" />
				<!-- 连接数据库的url -->
				<property name="url" value="${jdbc.url}" />
				<!-- 连接数据库的用户名 -->
				<property name="username" value="${jdbc.username}" />
				<!-- 连接数据库的密码 -->
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>

	<!--2.配置Mapper的位置 -->
	<mappers>
		<mapper resource="com/ryan/mapper/CustomerMapper.xml" />
		<mapper resource="com/ryan/mapper/UserMapper.xml" />
	</mappers>
</configuration>
```

**db.properties:**
```propertites
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```
**log4j.propertites:**
```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```



## 运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301145850737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301150125614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)