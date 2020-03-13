---
title: MapReduce关系代数运算——差
layout: post
categories: BigData
---

# MapReduce关系代数运算——差
关系沿用上一个选择运算的关系R和S，StudentR类也是一致的，本博文中就不赘述了。
## MapReduce程序设计
- **DifferenceMap**

```java
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.lib.input.FileSplit;
 
public class DifferenceMap extends Mapper<LongWritable, Text, Person, Text> {
 
    private Text relationName = new Text();
 
    protected void setup(Context context) throws java.io.IOException, InterruptedException {
        FileSplit fileSplit = (FileSplit) context.getInputSplit();
        relationName.set(fileSplit.getPath().getName());
    };
 
    protected void map(LongWritable key, Text value, Context context) throws java.io.IOException, InterruptedException {
        Person person = new Person(value.toString());
        context.write(person, relationName);
    };
 
}
```

- **DifferenceReduce**

```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;
 
public class DifferenceReduce extends Reducer<Person, Text, Person, NullWritable> {
 
    private String remove = "";
 
    protected void setup(Context context) throws java.io.IOException, InterruptedException {
        Configuration conf = context.getConfiguration();
        remove = conf.get("remove");
    };
 
    protected void reduce(Person key, Iterable<Text> values, Context context) throws java.io.IOException, InterruptedException {
        for (Text val : values) {
            if (remove.equals(val.toString())) {
                return;
            }
        }
        context.write(key, NullWritable.get());
    };
 
}
```

- **Difference**

```java
import java.io.IOException;
 
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
 
public class Difference {
 
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        if (args == null || args.length != 3) {
            throw new RuntimeException("请输入输入路径、输出路径和被减集合");
        }
        Configuration conf = new Configuration();
        conf.set("remove", args[2]);
        Job job = Job.getInstance(conf);
        job.setJobName("Difference");
        job.setJarByClass(Difference.class);
 
        job.setMapperClass(DifferenceMap.class);
        job.setMapOutputKeyClass(Person.class);
        job.setMapOutputValueClass(Text.class);
 
        job.setReducerClass(DifferenceReduce.class);
        job.setOutputKeyClass(Person.class);
        job.setOutputValueClass(NullWritable.class);
 
        FileInputFormat.addInputPaths(job, args[0]);
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
 
}
```


## 运行
像之前的WordCount一样将代码打包出来，生成Difference.jar文件。

```powershell
hadoop jar Difference.jar /input /output relationB
```





---
---
**欢迎查看我的CSDN博客：[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448)**

---
---