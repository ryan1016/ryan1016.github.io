---
title: JSON数据交互和RESTful支持
layout: post
categories: J2EE
---


🌹🌹🌹今天我们来学习的是SpringMVC中的JSON的数据交互以及RESTful支持。

# 一、JSON数据交互
## （一）JSON概述
&emsp;&emsp;关于JSON的具体介绍，可以参考[JSON百度百科](https://baike.baidu.com/item/JSON/2462549?fr=aladdin)

1. **什么是JSON**
&emsp;&emsp;JSON（JavaScript Object Notation，JS对象标记）是一种轻量级的数据交换格式。它是基于JavaScript的一个子集，使用了C、C++、C#、Java、JavaScript、Perl、Python等其他语言的约定，采用完全独立于编程语言的文本格式来存储和表示数据。

2. **JSON的特点**
&emsp;&emsp;JSON与XML非常相似，都是用来存储数据的，并且都是基于纯文本的数据格式。与XML相比，JSON解析速度更快，占用空间更小，且易于阅读和编写，同时也易于机器解析和生成。

## （二）JSON数据转换
&emsp;&emsp;Spring提供了一个HttpMessageConverter<T>接口来实现浏览器与控制器类（Controller）之间的数据交互。该接口主要用于将请求信息中的数据转换为一个类型为T的对象，并将类型为T的对象绑定到请求方法的参数中，或者将对象转换为响应信息传递给浏览器显示。
&emsp;&emsp;HttpMessageConverter<T>接口有很多实现类，这些实现类可以对不同类型的数据进行信息转换。其中MappingJackson2HttpMessageConverter是Spring MVC默认处理JSON格式请求响应的实现类。该实现类利用Jackson开源包读写JSON数据，将Java对象转换为JSON对象和XML文档，同时也可以将JSON对象和XML文档转换为Java对象。
&emsp;&emsp;要使用MappingJackson2HttpMessageConverter对数据进行转换，就需要使用Jackson的开源包，开发时所需的开源包及其描述如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316130654386.png)
&emsp;&emsp;大家可以自行在Maven仓库中下载。
&emsp;&emsp;下载地址：[http://mvnrepository.com/artifact/com.fasterxml.jackson.core](http://mvnrepository.com/artifact/com.fasterxml.jackson.core)
&emsp;&emsp;但是如果因为网络原因下载速度太慢的话，也可以在我上传的资源里找相关的jar包。
&emsp;&emsp;**<font color = "red" size="4px">Tips:</font>**
1. 使用<bean>标签方式的JSON转换器配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316133404464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
2. 配置静态资源的访问方式
✨在springmvc-config.xml文件中使用<mvc:default-servlet-handler>
✨激活Tomcat默认的Servlet来处理静态文件访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316133627327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

# 二、RESTful支持

&emsp;&emsp;RESTful也称之为REST，是英文“Representational State Transfer”的简称。可以将他理解为一种软件架构风格或设计风格，而不是一个标准。
&emsp;&emsp;简单地说，RESTful风格就是把请求参数变成请求路径的一种风格。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316134047220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

# 三、测试代码运行结果
**JSON的数据交互**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316142608485.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316142622819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

**RESTful支持**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200316150119425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020031615024376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)


---
**<font color="red">最后的最后，欢迎查看我的CSDN博客：</font>[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448/article/details/104897041)**

---