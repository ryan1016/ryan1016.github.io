---
title: MapReduce关系代数运算——交
layout: post
categories: 大数据平台与编程
---

# MapReduce关系代数运算——交
关系沿用上一个选择运算的关系R，并仿照R，再创建一个文件S。求R与S的交。StudentR类也是一致的，本博文中就不赘述了。
## MapReduce程序设计
- **IntersectionMap**

```java
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
 
public class IntersectionMap extends Mapper<LongWritable, Text, Person, IntWritable> {
 
    private static final IntWritable ONE = new IntWritable(1);
 
    protected void map(LongWritable key, Text value, Context context) throws java.io.IOException, InterruptedException {
        Person person = new Person(value.toString());
        context.write(person, ONE);
    };
 
}
```

- **IntersectionReduce**

```java
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.mapreduce.Reducer;
 
public class IntersectionReduce extends Reducer<Person, IntWritable, Person, NullWritable> {
    protected void reduce(Person key, Iterable<IntWritable> values, Context context) throws java.io.IOException, InterruptedException {
        int count = 0;
        for (IntWritable val : values) {
            count += val.get();
        }
        if (count == 2) {
            context.write(key, NullWritable.get());
        }
    };
}
```

- **Intersection**

```java
import java.io.IOException;
 
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
 
public class Intersection {
 
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        if (args == null || args.length != 2) {
            throw new RuntimeException("请输入输入路径、输出路径");
        }
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf);
        job.setJobName("Intersection");
        job.setJarByClass(Intersection.class);
         
        job.setMapperClass(IntersectionMap.class);
        job.setMapOutputKeyClass(Person.class);
        job.setMapOutputValueClass(IntWritable.class);
         
        job.setReducerClass(IntersectionReduce.class);
        job.setOutputKeyClass(Person.class);
        job.setOutputValueClass(NullWritable.class);
         
        FileInputFormat.addInputPaths(job, args[0]);
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
 
}
```

## 运行

```powershell
hadoop jar intersection.jar com.hadoop.mapreduce.Intersection /input /output
```


---
---
**欢迎查看我的CSDN博客：[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448)**

---
---