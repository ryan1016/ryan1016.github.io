---
title: 基于注解的Spring MVC实例
layout: post
categories: J2EE
---



## 一、DispatcherServlet

&emsp;&emsp;DispatcherServlet的全名是org.springframework.web.servlet.DispatcherServlet，它在程序中充当着前端控制器的角色。在使用时，只需将其配置在项目的web.xml文件中，其配置代码如下：

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
          org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>

```
## 二、@Controller注解类型
**（一）@Controller注解的使用**

&emsp;&emsp;org.springframework.stereotype.Controller注解类型用于指示Spring类的实例是一个控制器，其注解形式为@Controller。该注解在使用时不需要再实现Controller接口，只需要将@Controller注解加入到控制器类上，然后通过Spring的扫描机制找到标注了该注解的控制器即可。
&emsp;&emsp;@Controller注解在控制器类中的使用示例如下：

```java
package com.ryan.controller;
import org.springframework.stereotype.Controller;
...
@Controller
public class FirstController{
     ...
}

```

&emsp;&emsp;为了保证Spring能够找到控制器类，还需要在Spring MVC的配置文件中添加相应的扫描配置信息，一个完整的配置文件示例如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context"
            xsi:schemaLocation="http://www.springframework.org/schema/beans
                                             http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
                                             http://www.springframework.org/schema/context 
                                             http://www.springframework.org/schema/context/spring-context-4.3.xsd">
	<context:component-scan base-package="com.itheima.controller" />
</beans> 
```

<font color = "red">1. 标注在方法上：作为请求处理方法在程序接收到对应的URL请求时被调用：</font>

```java
package com.ryan.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
...
@Controller
public class FirstController{
	@RequestMapping(value="/firstController")
	public ModelAndView handleRequest(HttpServletRequest request,
			HttpServletResponse response) {
                           ...
		return mav;
	}
}
```
<font color = "blue">此时，可以通过地址：http://localhost:8080/···/firstController访问该方法！</font>

<br/>

<font color = "red">2. 标注在类上：该类中的所有方法都将映射为相对于类级别的请求，表示该控制器所处理的所有请求都被映射到value属性值所指定的路径下。</font>
```java
package com.ryan.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
...
@Controller
@RequestMapping(value="/hello")
public class FirstController{
	@RequestMapping(value="/firstController")
	public ModelAndView handleRequest(HttpServletRequest request,
			HttpServletResponse response) {
                           ...
		return mav;
	}
}
```

<font color = "blue">由于在类上添加了@RequestMapping注解，并且其value属性值为“/hello”，所以上述代码方法的请求路径将变为：http://localhost:8080/···/hello/firstController。</font>


**（二）注解的属性**

&emsp;&emsp;@RequestMapping注解除了可以指定value属性外，还可以指定其他一些属性，如下表所示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200309145432597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
&emsp;&emsp;表中所有属性都是可选的，但其默认属性是value。当value是其唯一属性时，可以省略属性名。例如，下面两种标注的含义相同：@RequestMapping(value="/firstController")@RequestMapping("/firstController")

**（三）组合注解**

- @GetMapping：匹配GET方式的请求；
- @PostMapping：匹配POST方式的请求；
- @PutMapping：匹配PUT方式的请求；
- @DeleteMapping：匹配DELETE方式的请求；
- @PatchMapping：匹配PATCH方式的请求。

&emsp;&emsp;以@GetMapping为例，该组合注解是@RequestMapping(method = RequestMethod.GET)的缩写，它会将HTTP GET请求映射到特定的处理方法上。

- 在实际开发中，传统的@RequestMapping注解使用方式如下：

```java
@RequestMapping(value="/user/{id}",method=RequestMethod.GET) 
public String selectUserById(String id){
	 ... 
 }
```
- 使用GetMapping注解后的简化代码如下：

```java
@GetMapping(value="/user/{id}") 
public String selectUserById(String id){
	 ...
}
```


**（四）请求处理方法的参数和返回类型**


- javax.servlet.ServletRequest / javax.servlet.http.HttpServletRequest
- javax.servlet.ServletResponse / javax.servlet.http.HttpServletResponse
- javax.servlet.http.HttpSession
- org.springframework.web.context.request.WebRequest或
- org.springframework.web.context.request.NativeWebRequest
- java.util.Locale
- java.util.TimeZone (Java 6+) / java.time.ZoneId (on Java 8)
- java.io.InputStream / java.io.Reader
- java.io.OutputStream / java.io.Writer
- org.springframework.http.HttpMethod
- java.security.Principal
- @PathVariable、@MatrixVariable、@RequestParam
@RequestHeader、@RequestBody、@RequestPart
@SessionAttribute、@RequestAttribute注解
- HttpEntity<?>
- java.util.Map / org.springframework.ui.Model /org.springframework.ui.ModelMap
- org.springframework.web.servlet.mvc.support.RedirectAttributes
- org.springframework.validation.Errors /org.springframework.validation.BindingResult
- org.springframework.web.bind.support.SessionStatus
- org.springframework.web.util.UriComponentsBuilder

## 三、@RequestMapping注解类型
&emsp;&emsp;Spring通过@Controller注解找到相应的控制器类后，还需要知道控制器内部对每一个请求是如何处理的，这就需要使用@RequestMapping注解类型，它用于映射一个请求或一个方法。使用时，可以标注在一个方法或一个类上。



## 四、ViewResolver

&emsp;&emsp;Spring MVC中的视图解析器负责解析视图。可以通过在配置文件中定义一个ViewResolver来配置视图解析器，其配置示例如下：

```xml
<bean id="viewResolver"    class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/jsp/" />
	<property name="suffix" value=".jsp" />
</bean>
```
&emsp;&emsp;在上述代码中，定义了一个视图解析器，并设置了视图的前缀和后缀属性。这样设置后，方法中所定义的view路径将可以简化。例如，入门案例中的逻辑视图名只需设置为“first”，而不再需要设置为“/jsp/first.jsp”，在访问时视图解析器会自动的增加前缀和后缀。

# 五、基于注解的Spring MVC应用

- **项目目录**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200309171936794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **源码展示**

**<font color = "red">src</font>**
**springmvc-config.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans
	   http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
	   http://www.springframework.org/schema/context 
  http://www.springframework.org/schema/context/spring-context-4.3.xsd">
	<!-- 指定需要扫描的包 -->
	<context:component-scan base-package="com.ryan.controller" />
	<!-- 定义视图解析器 -->
	<bean id="viewResolver" class=
    "org.springframework.web.servlet.view.InternalResourceViewResolver">
	     <!-- 设置前缀-->
	     <property name="prefix" value="/jsp/" />
	      <!-- 设置后缀 -->
	     <property name="suffix" value=".jsp" /> 
	</bean>
</beans>
```

**<font color = "red">com.ryan.controller</font>**

**FirstController.java**
```java
package com.ryan.controller;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
/**
 * 控制器类
 */
@Controller
//@RequestMapping(value="/hello")
public class FirstController{
	@RequestMapping(value="/first")
	public String handleRequest(HttpServletRequest request,
			HttpServletResponse response, Model model) throws Exception {
		// 向模型对象中添加数据
		model.addAttribute("msg", "基于注解的Spring MVC实例");
		// 返回视图页面
		return "first";
	}
}
```

**<font color = "red">jsp</font>**
**first.jsp**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
         "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>基于注解的Spring MVC</title>
	</head>
	<body>
		<p>123</p>
		${msg}
	</body>
</html>
```
**web.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>SpringMVC基于注解实例</display-name>
    <servlet>
    <servlet-name>springDispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

- **运行结果**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200309172455959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

**欢迎查看我的CSDN博客：[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448)**

