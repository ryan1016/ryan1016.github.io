---
title: Spring MVC入门
layout: post
categories: J2EE
---



# 一、Spring MVC概述

**<font color = "red">  1. 什么是Spring MVC</font>**

&emsp;&emsp;Spring MVC是Spring提供的一个实现了Web MVC设计模式的轻量级Web框架。它与Struts2框架一样，都属于MVC框架，但其使用和性能等方面比Struts2更加优异。

 **<font color = "red">2. Spring MVC的特点</font>**

- 是Spring框架的一部分，可以方便的利用Spring所提供的其他功能。
- 灵活性强，易于与其他框架集成。
- 提供了一个前端控制器DispatcherServlet，使开发人员无需额外开发控制器对象。
- 可自动绑定用户输入，并能正确的转换数据类型。
- 内置了常见的校验器，可以校验用户输入。如果校验不能通过，那么就会重定向到输入表单。
- 支持国际化。可以根据用户区域显示多国语言。
- 支持多种视图技术。它支持JSP、Velocity和FreeMarker等视图技术。
- 使用基于XML的配置文件，在编辑后，不需要重新编译应用程序。

# 二、第一个Spring MVC应用

<font color = "red">  **1. 创建Dynamic Web项目，在项目的lib目录中添加运行SpringMVC的jar包</font>**

&emsp;&emsp;项目中添加了Spring的4个核心JAR包、commons-logging的JAR以及两个web相关的JAR（可以在Spring解压文件夹的libs目录中找到），这两个web相关的JAR包就是Spring MVC框架所需的JAR包。
![添加jar包](https://img-blog.csdnimg.cn/20200309101501653.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


**<font color = "red">  2. 在web.xml中，配置Spring MVC的前端控制器DispatcherServlet</font>**
![配置web.xml](https://img-blog.csdnimg.cn/20200309101854135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


**<font color = "red">  3. 在src目录下，创建一个com.ryan.controller包，并在包中创建控制器类FirstController，该类需要实现Controller接口，编辑后如下所示。</font>**

![控制器](https://img-blog.csdnimg.cn/20200309102034466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

**<font color = "red">  4. 在src目录下，创建配置文件springmvc-config.xml，并在文件中配置控制器信息。</font>**

![springmvc-xml](https://img-blog.csdnimg.cn/20200309102213827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


**<font color = "red">  5. 在webContent目录下，创建jsp文件。</font>**


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200309102434295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)



**<font color = "red">  6. 运行结果</font>**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200309135540188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


<font color = "blue">注意：运行项目是一定是输入控制类的bean设置的name，直接运行jsp文件是无法获取到数据的。</font>