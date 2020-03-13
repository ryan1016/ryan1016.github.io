---
title: Eclipse搭建Web开发环境
layout: post
categories: Web
---




## 问题
&emsp;&emsp;最近有学弟说想学web，看见他用HBulider在写，我就给他推荐了eclipse，所以决定出一个eclipse搭建Web开发环境的教程。 

## 一、查看eclipse的类型
&emsp;&emsp;如果本身的eclipse安装的是JavaEE版本的，那就不需要进行下面的步骤了，可以直接跳转到**第三步**。如果安装的是JavaSE版本的eclipse的话，那就可以参照以下步骤了。

- ### 查看Eclipse版本信息

1. Win+R，输入cmd，在命令行内输入`java -version`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210100909640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_16,color_FFFFFF,t_70)


2. 在eclipse的安装目录readme文件夹下，打开`readme_eclipse.html`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210101253507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

3. 打开eclipse，在`Help->About Eclipse IDE`内查看
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210101359309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

## 二、配置Eclipse

### （一）安装合适版本的Eclipse。
1. 进入[Eclipse](https://www.eclipse.org/downloads/)官网。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210100036148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)



2. 选择需要的版本（选择EE版本点击下载）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210100544860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

### （二）安装插件
&emsp;&emsp;根据自己的eclipse版本，选择适当的插件进行下载。查看eclipse版本信息有以下三种方式。

1. 安装。确定版本后就可以开始安装插件了，打开`Eclipse->Help->Install new SoftWare`,在work with里面输入：http://download.eclipse.org/releases/oxygen/
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210101934944.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


2. 选择 Web, XML, Java EE and OSGi Enterprise Development
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200210102320329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


3. 然后按照提示点击下一步进行操作，最后finish后，等待下载安装完成，然后重启eclipse即可。
![进度](https://img-blog.csdnimg.cn/20200210094814673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)



## 三、配置Tomcat
### （一）Tomcat下载安装
&emsp;&emsp;你可以根据你的系统下载对应的包(以下以Window系统为例，建议暂时不要下载Tomcat9版本)：

1. 进入[Tomcat](http://tomcat.apache.org/)官网。点击右侧的Tomcat8。
![Tomcat](https://img-blog.csdnimg.cn/20200313194853202.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

2. 下载自己需要的版本。以答主下载的tomcat8.5版本为例。
![下载](https://img-blog.csdnimg.cn/20200313195335661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

3. 解压。将压缩包解压到需要的路径，答主便解压在了D盘路径下。
![解压Tomcat](https://img-blog.csdnimg.cn/20200313195457978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

&emsp;&emsp;注意目录名不能有中文和空格。目录介绍如下：
- bin：二进制执行文件。里面最常用的文件是startup.bat，如果是 Linux 或 Mac 系统启动文件为 startup.sh。
- conf:配置目录。里面最核心的文件是server.xml。可以在里面改端口号等。默认端口号是8080，也就是说，此端口号不能被其他应用程序占用。
- lib：库文件。tomcat运行时需要的jar包所在的目录
- logs：日志
- temp：临时产生的文件，即缓存
- webapps：web的应用程序。web应用放置到此目录下浏览器可以直接访问
- work：编译以后的class文件。

### （二）Tomcat与Eclipse相关联
- 打开Eclipse，选择菜单栏Windows-->preferences
![Preference](https://img-blog.csdnimg.cn/20200313200159915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

- 弹出如下页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313200444537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- 上图中，点击"add"的添加按钮，弹出如下界面：
![选择版本](https://img-blog.csdnimg.cn/20200313200609278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- 在选项中，我们选择对应的 Tomcat 版本，接着点击 "Next"，选择 Tomcat 的安装目录，并选择我们安装的 Java 环境：
![安装](https://img-blog.csdnimg.cn/20200313200737167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- 最后Apply and Close。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313200816258.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


## 四、创建第一个Web程序
1. 选择 "File-->New-->Dynamic Web Project"，创建 TomcatTest 项目：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313201040912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

2. 输入项目名，匹配tomcat。
![Tomcat](https://img-blog.csdnimg.cn/20200313201344489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

3. 一直点击Next，最后一步一定要记得勾选`Generate web.xml deployment descriptor`（第一次创建可能需要多等一会）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020031320165449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

- 工程项目结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313201932310.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

上图中各个目录解析：
- deployment descriptor：部署的描述。
- Web App Libraries：自己加的包可以放在里面。
- build：放入编译之后的文件。
- WebContent:放进写入的页面。

4. 在WebContent文件夹下新建一个test.html文件。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313202142645.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

5. WebContent文件夹右键->HTML File
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313202221481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
6. 我们就可以看见新建的第一个html页面了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313202443758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

7. 编辑自己的页面（比如输出一个“Hello World！”），代码如下：
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<p>Hello World!</p>>
</body>
</html>
```

8. 运行之前更改一下浏览器（笔者用的是Firefox），不然是在eclipse内部的Browser运行的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313202800857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)
9. 在代码里面右键Run as->Run on Server
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200313202917459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)

10. 直接点击Finish就可以了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020031320294985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

11. 这样就可以看到我们的运行结果咯。
![运行结果](https://img-blog.csdnimg.cn/20200313203043712.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;然后你就可以尽情的享受Web的乐趣了，在这里，笔者推荐两个学习HTML CSS等入门的网站，想学习的可以进去看看哦。
- 菜鸟教程：[https://www.runoob.com/](https://www.runoob.com/)
- W3school：[https://www.w3school.com.cn/](https://www.w3school.com.cn/)




---
**<font color="red">最后的最后，欢迎查看我的github博客：</font>[Welcome To Ryan's Home](https://www.github.io)**

---