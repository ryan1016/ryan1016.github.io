---
title: Jupyter Notebook系列问题（一）
layout: post
categories: ML|DL
---

&emsp;&emsp;安装了Anaconda的设备，会一并安装了Jupyter。但是这个时候如果直接打开Jupyter Notebook，然后输入import tensorflow就会遇到一些小问题了。Jupyter Notebook会毫不客气地告诉你：`No module named ‘tensorflow’`
![No module](https://img-blog.csdnimg.cn/20200314231655808.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;**然后要怎么解决这个问题呢？**

- 首先我们进入到Anaconda Prompt
- 激活我们自己的tensorflow环境：```conda activate tf2```
![activate](https://img-blog.csdnimg.cn/20200314232632340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
- 安装ipython
```shell
conda install ipython
```

- 安装Jupyter
```shell
conda install jupyter
 ```

&emsp;&emsp;**接着，**
&emsp;&emsp;接着我们就会发现系统多了一个Jupyter Notebook了，如图：
![多的](https://img-blog.csdnimg.cn/20200314233304840.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)

&emsp;&emsp;**然后**
&emsp;&emsp;然后我们打开这个Jupyter Notebook，进去之后就可以成功导入tensorflow咯。

![导入成功](https://img-blog.csdnimg.cn/20200314233731891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_0)
&emsp;&emsp;**最后**
&emsp;&emsp;你就可以好好玩耍你的Jupyter Notebook了！




---
**<font color="red">最后的最后，欢迎查看我的github博客：</font>[Welcome To Ryan's Home](https://www.github.io)**

---