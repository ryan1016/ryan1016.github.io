---
title: Ubutun下如何安装Mininet
layout: post
categories: SDN
---


# 一、安装Mininet
**1.安装git**

```powershell
sudo apt-get install git
```

**2.下载Mininet代码**

```powershell
git clone git://github.com/mininet/mininet
```

**3.安装**

```powershell
cd mininet
```

```powershell
cd util
```

```powershell
sudo ./install.sh -a
```
&emsp;&emsp;<font color = "red">安装时间可能会较长，耐心等待即可……</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/04f802d8b3884adb9838d4e124d78d35.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAUnlhbsK3R8K3S2luZw==,size_1,color_FFFFFF,t_70,g_se,x_16#pic_center)
&emsp;&emsp;安装成功后显示：Enjoy Mininet！
![在这里插入图片描述](https://img-blog.csdnimg.cn/4b2c5c40c7354aa2b0cd5d14cd2cdd0c.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAUnlhbsK3R8K3S2luZw==,size_1,color_FFFFFF,t_70,g_se,x_16#pic_center)

# 二、测试Mininet
**运行mininet**

```powershell
sudo mn
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ad5c86178154d40a42c86ee19c902cb.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAUnlhbsK3R8K3S2luZw==,size_1,color_FFFFFF,t_70,g_se,x_16#pic_center)
&emsp;&emsp;出现上图所示情况，则mininet安装成功！



---
**<font color="red">最后的最后，欢迎查看我的CSDN博客：</font>[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448?spm=1010.2135.3001.5343)**

---