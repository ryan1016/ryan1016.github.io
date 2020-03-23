---
title: Spring MVC的数据绑定
layout: post
categories: J2EE
---

# 一、spring MVC数据绑定的使用

- **<font color = "red">什么是数据绑定？</font>**
&emsp;&emsp;在执行程序时，Spring MVC会根据客户端请求参数的不同，将请求消息中的信息以一定的方式转换并绑定到控制器类的方法参数中。这种将请求消息数据与后台方法参数建立连接的过程就是Spring MVC中的数据绑定。

- **<font color = "red">如何完成数据绑定？</font>**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316084437564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
&emsp;&emsp;在数据绑定过程中，Spring MVC框架会通过数据绑定组件（DataBinder）将请求参数串的内容进行类型转换，然后将转换后的值赋给控制器类中方法的形参，这样后台方法就可以正确绑定并获取客户端请求携带的参数了。接下来，将通过一张数据流程图来介绍数据绑定的过程。
&emsp;&emsp;<font color = "red">①</font>Spring MVC将ServletRequest对象传递给DataBinder；
&emsp;&emsp;<font color = "red">②</font>将处理方法的入参对象传递给DataBinder；
&emsp;&emsp;<font color = "red">③</font>DataBinder调用ConversionService组件进行数据类型转换、数据格式化等工作，并将ServletRequest对象中的消息填充到参数对象中；
&emsp;&emsp;<font color = "red">④</font>调用Validator组件对已经绑定了请求消息数据的参数对象进行数据合法性校验；
&emsp;&emsp;<font color = "red">⑤</font>校验完成后会生成数据绑定结果BindingResult对象，Spring MVC会将BindingResult对象中的内容赋给处理方法的相应参数。



# 二、简单的数据绑定
&emsp;&emsp;根据客户端请求参数类型和个数的不同，我们将Spring MVC中的数据绑定主要分为简单数据绑定和复杂数据绑定，接下来的几个小节中，就对这两种类型数据绑定进行详细讲解。
- **<font color = "red">绑定默认数据类型</font>**
![默认·参数类型](https://img-blog.csdnimg.cn/20200316085704751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
&emsp;&emsp;接下来，以HttpServletRequest类型的使用为例，来演示默认数据类型绑定的使用：
&emsp;&emsp;<font color = "red">①</font> 在web.xml中配置Spring MVC的前端控制器等信息；
&emsp;&emsp;<font color = "red">②</font> 创建Spring MVC配置文件，并配置组件扫描器和视图解析器；
&emsp;&emsp;<font color = "red">③</font> 创建处理器类；
&emsp;&emsp;<font color = "red">④</font> 创建访问成功后的响应页面；
&emsp;&emsp;<font color = "red">⑤</font> 启动Web项目，访问http://localhost:8080/chapter13/selectUser?id=1。

- **<font color = "red">绑定简单数据类型</font>**
&emsp;&emsp;简单数据类型的绑定，就是指Java中几种基本数据类型的绑定，例如int、String、Double等类型。这里仍然以上一小节中的参数id为1的请求为例，来讲解简单数据类型的绑定。
&emsp;&emsp;将控制器类UserController中的selectUser()方法进行修改：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316090741657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
&emsp;&emsp;这里需要注意的是，有时候前端请求中参数名和后台控制器类方法中的形参名不一样，这就会导致后台无法正确绑定并接收到前端请求的参数。
&emsp;&emsp;针对上述提到的前端请求中参数名和后台控制器类方法中的形参名不一样的情况，可以考虑使用Spring MVC提供的@RequestParam注解类型来进行间接数据绑定。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316090907693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
&emsp;&emsp;假设请求地址为https://localhost:8080/chapter13/selectUser?user_id=1/,那么在后台selectUser()方法中的使用方法如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020031609102433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **<font color = "red">绑定POJO类型</font>**
&emsp;&emsp;在使用简单数据类型绑定时，可以很容易的根据具体需求来定义方法中的形参类型和个数，然而在实际应用中，客户端请求可能会传递多个不同类型的参数数据，如果还使用简单数据类型进行绑定，那么就需要手动编写多个不同类型的参数，这种操作显然比较繁琐。
&emsp;&emsp;针对多类型、多参数的请求，可以使用POJO类型进行数据绑定。POJO类型的数据绑定就是将所有关联的请求参数封装在一个POJO中，然后在方法中直接使用该POJO作为形参来完成数据绑定。
&emsp;&emsp;<font color = "red">①</font>创建用户类POJO，用来封装注册用户信息；
&emsp;&emsp;<font color = "red">②</font>在控制器中编写注册方法；
&emsp;&emsp;<font color = "red">③</font>创建用户注册页面；
&emsp;&emsp;<font color = "red">④</font>启动Web项目，访问https://localhost:8080/chapter13/toRegister；
&emsp;&emsp;<font color = "red">⑤</font>注册页面填写信息，并单击“注册”。
<font color = "red">**Tips：**</font>解决请求参数中的中文乱码问题
```xml
<filter>
        <filter-name>CharacterEncodingFilter</filter-name>		
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
                   <param-name>encoding</param-name>
                   <param-value>UTF-8</param-value>
        </init-param>
</filter>
<filter-mapping>
       <filter-name>CharacterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
</filter-mapping>
```

- **<font color = "red">绑定包装POJO</font>**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316093928134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
&emsp;&emsp;<font color = "red">①</font>创建订单包装POJO，来封装订单和用户信息；
&emsp;&emsp;<font color = "red">②</font>创建订单控制器类，在控制器中编写查询订单信息方法；
&emsp;&emsp;<font color = "red">③</font>创建订单查询页面；
&emsp;&emsp;<font color = "red">④</font>启动Web项目，访问http://localhost:8080/chapter13/tofindOrdersWithUser；
&emsp;&emsp;<font color = "red">⑤</font>查询页面填写查询信息。


- **<font color = "red">绑定自定义数据</font>**
&emsp;&emsp;一般情况下，使用基本数据类型和POJO类型的参数数据已经能够满足需求，然而有些特殊类型的参数是无法在后台进行直接转换的，但也有特殊数据类型无法直接进行数据绑定，必须先经过数据转换，例如日期数据。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316094611503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

# 三、复杂的数据绑定
&emsp;&emsp;在学习完前面小节讲解的简单数据绑定后，读者已经能够完成实际开发中多数的数据绑定问题，但仍可能遇到一些比较复杂的数据绑定问题，比如数组的绑定、集合的绑定，这在实际开发中也是十分常见的。接下来的两个小节中，将具体讲解一下数组绑定和集合绑定的使用。
- **<font color = "red">绑定数组</font>**
&emsp;&emsp;在实际开发时，可能会遇到前端请求需要传递到后台一个或多个相同名称参数的情况（如批量删除），此种情况采用前面讲解的简单数据绑定的方式显然是不合适的。
- **<font color = "red">绑定集合</font>**
&emsp;&emsp;在批量删除用户的操作中，前端请求传递的都是同名参数的用户id，只要在后台使用同一种数组类型的参数绑定接收，就可以在方法中通过循环数组参数的方式来完成删除操作。但如果是批量修改用户操作的话，前端请求传递过来的数据可能就会批量包含各种类型的数据，如Integer，String等。

# 四、运行结果
- **绑定默认数据**
![绑定默认数据](https://img-blog.csdnimg.cn/20200316105910205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316110032589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
- **绑定POJO类型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316123606759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

- **绑定包装POJO**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316113757628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316123638786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316113809546.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316123932211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)
- **绑定数组**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316124122138.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316124157681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316124212365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)


- **绑定集合**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020031612434512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316124401400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316124425495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)



---
**<font color="red">最后的最后，欢迎查看我的CSDN博客：</font>[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448/article/details/104891224)**

---