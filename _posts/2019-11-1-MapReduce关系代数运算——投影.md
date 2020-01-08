---
title: MapReduce关系代数运算——投影
layout: post
categories: 大数据平台与编程
---


# MapReduce关系代数运算——投影
关系沿用上一个选择运算的关系R，StudentR类也是一致的，本博文中就不赘述了。
## MapReduce程序设计

- **Projection**

```java

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;

import java.io.IOException;

public class Projection {
    public static class ProjectionMap extends Mapper<LongWritable, Text, Text, NullWritable> {
        private int col;
        @Override
        protected void setup(Context context) throws IOException, InterruptedException {
            col = context.getConfiguration().getInt("col", 0);
        }

        @Override
        protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
            StudentR record = new StudentR(value.toString());
            context.write(new Text(record.getCol(col)), NullWritable.get());
        }
    }

    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        Configuration conf = new Configuration();
        conf.setInt("col", Integer.parseInt(args[2]));
        Job projectionJob = Job.getInstance(conf, "ProgectionJob");

        projectionJob.setJarByClass(Projection.class);
        projectionJob.setMapperClass(ProjectionMap.class);
        projectionJob.setMapOutputKeyClass(Text.class);
        projectionJob.setMapOutputValueClass(NullWritable.class);
        projectionJob.setNumReduceTasks(0);

        projectionJob.setInputFormatClass(TextInputFormat.class);
        projectionJob.setOutputFormatClass(TextOutputFormat.class);

        FileInputFormat.addInputPath(projectionJob, new Path(args[0]));
        FileOutputFormat.setOutputPath(projectionJob, new Path(args[1]));

        projectionJob.waitForCompletion(true);
        System.out.println("finished!");

    }
}

```


## 运行
像之前的WordCount一样将代码打包出来，生成Projection.jar文件，但是在终端运行的时候，需要注意的是不只是输入两个路径，而应该输入选择的条件。如本例中，我们要输出学生成绩细腻些，则应该选定第四列grade
字段：

```powershell
hadoop jar Projection.jar /input /output 4
```
查看输出结果：
![输出结果](https://img-blog.csdnimg.cn/20200108184409624.png)
