---
title: Anaconda安装Tensorflow详细教程
layout: post
categories: ML|DL
---



## 一、安装Anaconda

&emsp;&emsp;建议用迅雷下载或者用清华源下载Anaconda，否则速度会很慢。下载时注意版本。题主用的是Python3.7版本的。

![安装Anaconda](https://img-blog.csdnimg.cn/20200310180444118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)


&emsp;&emsp;下载好Anaconda后，按照提示一直点击Next即可。

&emsp;&emsp;<font color = "blue">安装过程中会让你选择是否将Anaconda添加到系统环境中去，此时可以勾选这个选项框，避免后期自己添加路径。</font>

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200310181010448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

## 二、创建tensorflow环境

1、打开Anaconda Prompt

![Anaconda Prompt](https://img-blog.csdnimg.cn/20200310181441431.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

2、为了加快其他库的安装速度，我们首先要替换清华源

&emsp;&emsp;在Anaconda Prompt里输入以下命令：
```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

```

3、在Anaconda内创建一个tensorflow环境

&emsp;&emsp;输入以下命令（创建一个python版本为3.7的环境，环境名为tf）：

```shell
conda create -n tf python=3.7
```
![tf](https://img-blog.csdnimg.cn/20200310182114587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;等待一会，出现Proceed（[y]/n）?果断输入y，进行安装，之后的工作就是静静地等着他没慢慢安装完咯。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200310182304220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)




## 三、安装tensorflow

1、启动tensorflw环境

&emsp;&emsp;在Anaconda Prompt中启动tensorflow环境：
```shell
activate tf
```

2、 安装tensorflow

&emsp;&emsp;安装cpu版本，此时直接输入conda install tensorflow即可，会默认安装最新版本，题主安装的就是2.1版本的。如果需要指定版本的话，在tensorflow后面输入版本号即可。
```shell
conda install tensorflow
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200310182733435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;安装gpu版本（指定版本号为1.8.0）
```shell
conda install tensorflow-gpu==1.8.0
```

**Tips:虽然conda和pip都可以安装python的依赖，但是建议只使用一种依赖，而且就目前情况来说，在Anaconda内使用conda安装时一种更加明智的选择。<font color = "red">切记：conda和pip不可混用！</font>**


3、测试tensorflow是否安装成功

&emsp;&emsp;进入tensorflow环境
```shell
activate tensorflow
```
- **方法一：在python里面导入tensorflow库，看是否会报错**

```shell
python
import tensorflow
```
&emsp;&emsp;如输入import tensorflow之后无任何提示消息（如图），即表明tensorflow安装成功

![检测tf](https://img-blog.csdnimg.cn/20200310183548877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

- **方法二：输出tensorflow的版本信息**

```shell
pip show tensorflow
```

&emsp;&emsp;如果能显示版本信息（如下图），则表明tensorflow安装成功了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200310183917942.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;最后，你就可以尽情享用tensorflow带给你的无穷魅力了。


---
**<font color="red">最后的最后，欢迎查看我的github博客：</font>[Welcome To Ryan's Home](https://www.github.io)**

---