---
title: Mybatis关联映射
layout: post
categories: J2EE
---


# 关联关系

&emsp;&emsp;**一对一**：在任意一方引入对方主键作为外键；在本类中定义对方类型的对象，如A类中定义B类类型的属性b，B类中定义A类类型的属性a。
```java
class A{
	B b;
}
class B{
	A a;
}
```

&emsp;&emsp;**一对多**：在“多”的一方，添加“一”的一方的主键作为外键；一个A类类型对应多个B类类型的情况，需要在A类中以集合的方式引入B类类型的对象，在B类中定义A类类型的属性a。
```java
class A{
	List<B> b;
}
class B{
	A a;
}
```

&emsp;&emsp;**多对多**：产生中间关系表，引入两张表的主键作为外键，两个主键成为联合主键或使用新的字段作为主键。在A类中定义B类类型的集合，在B类中定义A类类型的集合。
```java
class A{
	List<B> b;
}
class B{
	List<A> a;
}
```

## 一对一映射
&emsp;&emsp;在本系列动态SQL所讲解的*resultMap*元素中，包含了一个*association*子元素，MyBatis就是通过该元素来处理一对一关联关系的。

&emsp;&emsp;在*association*元素中，通常可以配置以下属性:
==property==：指定映射到的实体类对象属性，与表字段一一对应；
==column==：指定表中对应的字段；
==javaType==：指定映射到实体对象的类型；
==select==：指定引入嵌套查询的子SQL语句，该属性用于关联映射中的嵌套查询；
==*fetchType*==：指定在关联查询时是否启用延迟加载。该属性有lazy和eager两个属性值，默认值为lazy（即默认关联映射延迟加载）。

&emsp;&emsp;使用*association*元素进行一对一关联映射非常简单，只需要参考如下两种示例配置即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302095140699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

## 一对多映射
&emsp;&emsp;在本系列动态SQL所讲解的*resultMap*元素中，包含了一个*collection*子元素，MyBatis就是通过该元素来处理一对一关联关系的。

&emsp;&emsp;collection子元素的属性大部分与*association*元素相同，但其还包含一个特殊属性——*ofType* 。

==ofType==：ofType属性与javaType属性对应，它用于指定实体对象中集合类属性所包含的元素类型。

&emsp;&emsp;*collection* 元素的使用也非常简单，同样可以参考如下两种示例进行配置，具体代码如下:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302095808105.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

## 多对多映射

&emsp;&emsp;在MyBatis中，多对多的关联关系查询，同样可以使用前面介绍的*collection*元素进行处理（其用法和一对多关联关系查询语句用法基本相同）。

# 工程目录

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302111636292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302111651962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
# 测试代码

**com.ryan.po:**
<font color = "red">IdCard.java:</font>
```java
package com.ryan.po;
/**
 * 证件持久化类
 */
public class IdCard {
	private Integer id;
	private String code;
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getCode() {
		return code;
	}
	public void setCode(String code) {
		this.code = code;
	}
	@Override
	public String toString() {
		return "IdCard [id=" + id + ", code=" + code + "]";
	}
}

```
<font color = "red">.javaOrders:</font>
```java
package com.ryan.po;

import java.util.List;

/**
 * 订单持久化类
 */
public class Orders {
	private Integer id;    //订单id
	private String number;//订单编号
	//关联商品集合信息
	private List<Product> productList;

	
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getNumber() {
		return number;
	}
	public void setNumber(String number) {
		this.number = number;
	}
//	@Override
//	public String toString() {
//		return "Orders [id=" + id + ", number=" + number + "]";
//	}
	public List<Product> getProductList() {
		return productList;
	}
	public void setProductList(List<Product> productList) {
		this.productList = productList;
	}
	@Override
	public String toString() {
		return "Orders [id=" + id + ", number=" + number + ", productList=" + productList + "]";
	}
	
}

```
<font color = "red">Person.java:</font>
```java
package com.ryan.po;
/**
 * 个人持久化类
 */
public class Person {
	private Integer id;
	private String name;
	private Integer age;
	private String sex;
	private IdCard card;  //个人关联的证件
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
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	public IdCard getCard() {
		return card;
	}
	public void setCard(IdCard card) {
		this.card = card;
	}
	@Override
	public String toString() {
		return "Person [id=" + id + ", name=" + name + ", "
				+ "age=" + age + ", sex=" + sex + ", card=" + card + "]";
	}
}

```
<font color = "red">Product.java:</font>
```java
package com.ryan.po;
import java.util.List;
/**
 * 商品持久化类
 */
public class Product {
	private Integer id;  //商品id
	private String name; //商品名称
	private Double price;//商品单价
	private List<Orders> orders; //与订单的关联属性
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
	public Double getPrice() {
		return price;
	}
	public void setPrice(Double price) {
		this.price = price;
	}
	public List<Orders> getOrders() {
		return orders;
	}
	public void setOrders(List<Orders> orders) {
		this.orders = orders;
	}
	@Override
	public String toString() {
		return "Product [id=" + id + ", name=" + name 
				           + ", price=" + price + "]";
	}
}

```
<font color = "red">User.java:</font>
```java
package com.ryan.po;
import java.util.List;
/**
 * 用户持久化类
 */
public class User {
	private Integer id;                 // 用户编号
	private String username;           // 用户姓名
	private String address;            // 用户地址
	private List<Orders> ordersList; //用户关联的订单
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
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public List<Orders> getOrdersList() {
		return ordersList;
	}
	public void setOrdersList(List<Orders> ordersList) {
		this.ordersList = ordersList;
	}
	@Override
	public String toString() {
		return "User [id=" + id + ", username=" + username + ", address="
				+ address + ", ordersList=" + ordersList + "]";
	}
}

```
**com.ryan.utils:**
<font color = "red">MybatisUtils.java:</font>
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
    public static SqlSessionFactory sqlSessionFactory = null;
    // 初始化SqlSessionFactory对象
    static{
        try {
            //使用MyBatis提供的Resources类加载mybatis的配置文件
            Reader reader = Resources.getResourceAsReader("mybatis-config.xml");
            //构建sqlSession的工厂
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    // 获取SqlSession对象的静态方法
    public static SqlSession getSession(){
        return sqlSessionFactory.openSession();
    }
}

```
**com.ryan.test:**
<font color = "red">MybatisAssociatedTest.java</font>
```java
package com.ryan.test;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import com.ryan.po.Orders;
import com.ryan.po.Person;
import com.ryan.po.User;
import com.ryan.utils.MybatisUtils;
/**
 * Mybatis关联查询映射测试类
 */
public class MybatisAssociatedTest {
    /**
     * 嵌套查询
     */
    @Test
    public void findPersonByIdTest() {
        // 1、通过工具类生成SqlSession对象
        SqlSession session = MybatisUtils.getSession();
        // 2.使用MyBatis嵌套查询的方式查询id为1的人的信息
        Person person = session.selectOne("com.ryan.mapper." 
                                   + "PersonMapper.findPersonById", 1);
        // 3、输出查询结果信息
        System.out.println(person);
        // 4、关闭SqlSession
        session.close();
    }
    
    /**
     * 嵌套结果
     */
    @Test
    public void findPersonByIdTest2() {
        // 1、通过工具类生成SqlSession对象
        SqlSession session = MybatisUtils.getSession();
        // 2.使用MyBatis嵌套结果的方法查询id为1的人的信息
        Person person = session.selectOne("com.ryan.mapper." 
                                   + "PersonMapper.findPersonById2", 1);
        // 3、输出查询结果信息
        System.out.println(person);
        // 4、关闭SqlSession
        session.close();
    }
    
    /**
     * 一对多	
     */
    @Test
    public void findUserTest() {
        // 1、通过工具类生成SqlSession对象
        SqlSession session = MybatisUtils.getSession();
        // 2、查询id为1的用户信息
        User user = session.selectOne("com.ryan.mapper."
                                + "UserMapper.findUserWithOrders", 1);
        // 3、输出查询结果信息
        System.out.println(user);
        // 4、关闭SqlSession
        session.close();
    }

    /**
     * 多对多
     */
    @Test
    public void findOrdersTest(){
        // 1、通过工具类生成SqlSession对象
        SqlSession session = MybatisUtils.getSession();
        // 2、查询id为1的订单中的商品信息
        Orders orders = session.selectOne("com.ryan.mapper."
                               + "OrdersMapper.findOrdersWithPorduct", 1);
        // 3、输出查询结果信息
        System.out.println(orders);
        // 4、关闭SqlSession
        session.close();
    }


}

```
**com.ryan.mapper:**
<font color = "red">IdCardMapper.xml:</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.IdCardMapper">
  <!-- 根据id查询证件信息 -->
  <select id="findCodeById" parameterType="Integer" resultType="IdCard">
	  SELECT * from tb_idcard where id=#{id}
  </select>
</mapper>

```
<font color = "red">OrdersMapper:</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.OrdersMapper">
	<!-- 多对多嵌套查询：通过执行另外一条SQL映射语句来返回预期的特殊类型 -->
	<select id="findOrdersWithPorduct" parameterType="Integer" 
              resultMap="OrdersWithProductResult">
		select * from tb_orders WHERE id=#{id}	
	</select>
	<resultMap type="Orders" id="OrdersWithProductResult">
		<id property="id" column="id" />
		<result property="number" column="number" />
		<collection property="productList" column="id" ofType="Product" 
		     select="com.ryan.mapper.ProductMapper.findProductById">
		</collection>
	</resultMap>
	
	<!-- 多对多嵌套结果查询：查询某订单及其关联的商品详情 -->
	<select id="findOrdersWithPorduct2" parameterType="Integer" 
	         resultMap="OrdersWithPorductResult2">
	    select o.*,p.id as pid,p.name,p.price
	    from tb_orders o,tb_product p,tb_ordersitem  oi
	    WHERE oi.orders_id=o.id 
	    and oi.product_id=p.id 
	    and o.id=#{id}
	</select>
	<!-- 自定义手动映射类型 -->
	<resultMap type="Orders" id="OrdersWithPorductResult2">
	    <id property="id" column="id" />
	    <result property="number" column="number" />
	    <!-- 多对多关联映射：collection -->
	    <collection property="productList" ofType="Product">
	        <id property="id" column="pid" />
	        <result property="name" column="name" />
	        <result property="price" column="price" />
	    </collection>
	</resultMap>
	
</mapper>

```
<font color = "red">PersonMapper.xml:</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.PersonMapper">
	<!-- 嵌套查询：通过执行另外一条SQL映射语句来返回预期的特殊类型 -->
	<select id="findPersonById" parameterType="Integer" 
                                      resultMap="IdCardWithPersonResult">
		SELECT * from tb_person where id=#{id}
	</select>
	<resultMap type="Person" id="IdCardWithPersonResult">
		<id property="id" column="id" />
		<result property="name" column="name" />
		<result property="age" column="age" />
		<result property="sex" column="sex" />
		<!-- 一对一：association使用select属性引入另外一条SQL语句 -->
		<association property="card" column="card_id" javaType="IdCard"
			select="com.ryan.mapper.IdCardMapper.findCodeById" />
	</resultMap>
	
	<!-- 嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集 -->
	<select id="findPersonById2" parameterType="Integer" 
	                                   resultMap="IdCardWithPersonResult2">
	    SELECT p.*,idcard.code
	    from tb_person p,tb_idcard idcard
	    where p.card_id=idcard.id 
	    and p.id= #{id}
	</select>
	<resultMap type="Person" id="IdCardWithPersonResult2">
	    <id property="id" column="id" />
	    <result property="name" column="name" />
	    <result property="age" column="age" />
	    <result property="sex" column="sex" />
	    <association property="card" javaType="IdCard">
	        <id property="id" column="card_id" />
	        <result property="code" column="code" />
	    </association>
	</resultMap>
	
</mapper>

```
<font color = "red">ProductMapper.xml</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ryan.mapper.ProductMapper">
	<select id="findProductById" parameterType="Integer" 
                                       resultType="Product">
		SELECT * from tb_product where id IN(
		   SELECT product_id FROM tb_ordersitem  WHERE orders_id = #{id}
		)
	</select>
</mapper>

```
<font color = "red">UserMapper.xml:</font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace表示命名空间 -->
<mapper namespace="com.ryan.mapper.UserMapper">
	<!-- 一对多：查看某一用户及其关联的订单信息 
	      注意：当关联查询出的列名相同，则需要使用别名区分 -->
	<select id="findUserWithOrders" parameterType="Integer" 
						   resultMap="UserWithOrdersResult">
		SELECT u.*,o.id as orders_id,o.number 
		from tb_user u,tb_orders o 
		WHERE u.id=o.user_id 
         and u.id=#{id}
	</select>
	<resultMap type="User" id="UserWithOrdersResult">
		<id property="id" column="id"/>
		<result property="username" column="username"/>
		<result property="address" column="address"/>
		<!-- 一对多关联映射：collection 
			ofType表示属性集合中元素的类型，List<Orders>属性即Orders类 -->
		<collection property="ordersList" ofType="Orders">
			<id property="id" column="orders_id"/>
			<result property="number" column="number"/>
		</collection>
	</resultMap>
</mapper>

```

**src:**
<font color = "red">db.propertites:</font>
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```
<font color = "red">log4j.properties:</font>
```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# MyBatis logging configuration...
log4j.logger.com.ryan=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

```
<font color = "red">Mybatis-config.xml:</font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 引入数据库连接配置文件 -->
	<properties resource="db.properties" />
	
	<settings>
     <!-- 打开延迟加载的开关 -->  
    <setting name="lazyLoadingEnabled" value="true" />  
    <!-- 将积极加载改为消息加载，即按需加载 -->  
    <setting name="aggressiveLazyLoading" value="false"/>  
	</settings>
	
	<!--使用扫描包的形式定义别名 -->
	<typeAliases>
		<package name="com.ryan.po" />
	</typeAliases>
	<!--配置环境 ，默认的环境id为mysql -->
	<environments default="mysql">
		<!-- 配置id为mysql的数据库环境 -->
		<environment id="mysql">
			<!-- 使用JDBC的事务管理 -->
			<transactionManager type="JDBC" />
			<!--数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<!--配置Mapper的位置 -->
     <mappers>
         <mapper resource="com/ryan/mapper/IdCardMapper.xml" />
         <mapper resource="com/ryan/mapper/PersonMapper.xml" />
         <mapper resource="com/ryan/mapper/UserMapper.xml" />
         <mapper resource="com/ryan/mapper/OrdersMapper.xml" />
	 	 <mapper resource="com/ryan/mapper/ProductMapper.xml" />
     </mappers>
</configuration>
```


# 运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200302111024648.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)