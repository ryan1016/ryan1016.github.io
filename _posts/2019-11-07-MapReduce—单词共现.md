---
title: MapReduce——单词共现
layout: post
categories: BigData
---


## <center>MapReduce程序设计：单词共现</center>

### 准备数据
自己准备一段500词左右的英文文本。

### MapReduce程序设计
编写单词共现程序，窗口大小为5

**WholeFileInputFormat**


```java
import java.io.IOException;

import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.InputSplit;
import org.apache.hadoop.mapreduce.JobContext;
import org.apache.hadoop.mapreduce.RecordReader;
import org.apache.hadoop.mapreduce.TaskAttemptContext;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.FileSplit;
 
public class WholeFileInputFormat extends FileInputFormat<Text, Text> {
     
    @Override
    protected boolean isSplitable(JobContext context, Path filename) {
        return false;
    }
 
    @Override
    public RecordReader<Text, Text> createRecordReader(InputSplit split, TaskAttemptContext context) throws IOException, InterruptedException {
        return new WholeFileInputRecord((FileSplit) split, context.getConfiguration());
    }
 
}
```

**WholeFileInputRecord**


```java
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FSDataInputStream;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IOUtils;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.InputSplit;
import org.apache.hadoop.mapreduce.RecordReader;
import org.apache.hadoop.mapreduce.TaskAttemptContext;
import org.apache.hadoop.mapreduce.lib.input.FileSplit;
 
public class WholeFileInputRecord extends RecordReader<Text, Text> {
 
    private boolean progress = false;
    private Text key = new Text();
    private Text value = new Text();
    private Configuration conf;
    private FileSplit fileSplit;
    private FSDataInputStream fis;
 
    public WholeFileInputRecord(FileSplit fileSplit, Configuration conf) {
        this.fileSplit = fileSplit;
        this.conf = conf;
    }
 
    @Override
    public void initialize(InputSplit split, TaskAttemptContext context) throws IOException, InterruptedException {
        // TODO Auto-generated method stub
        Path file = fileSplit.getPath();
        FileSystem fs = file.getFileSystem(conf);
        fis = fs.open(file);
    }
 
    @Override
    public boolean nextKeyValue() throws IOException, InterruptedException {
        if (!progress) {
            byte[] content = new byte[(int) fileSplit.getLength()];
            Path file = fileSplit.getPath();
            key.set(file.getName());
            try {
                IOUtils.readFully(fis, content, 0, content.length);
                value.set(content);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                IOUtils.closeStream(fis);
            }
            progress = true;
            return true;
        }
        return false;
    }
 
    @Override
    public Text getCurrentKey() throws IOException, InterruptedException {
        return key;
    }
 
    @Override
    public Text getCurrentValue() throws IOException, InterruptedException {
        return value;
    }
 
    @Override
    public float getProgress() throws IOException, InterruptedException {
        return progress ? 1.0f : 0.0f;
    }
 
    @Override
    public void close() throws IOException {
    }
 
}
```

**WordConcurrnce**

```java
import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
 
public class WordConcurrnce {
 
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        if (args == null || args.length != 3) {
            throw new RuntimeException("请输入输入路径、输出路径和窗口大小");
        }
        Configuration conf = new Configuration();
        conf.setInt("windowSize", Integer.parseInt(args[2]));
        Job job = Job.getInstance(conf);
        job.setJobName("WordConcurrnce");
        job.setJarByClass(WordConcurrnce.class);
 
        job.setMapperClass(WordConcurrnceMap.class);
        job.setMapOutputKeyClass(WordPair.class);
        job.setMapOutputValueClass(IntWritable.class);
 
        job.setReducerClass(WordConcurrnceReduce.class);
        job.setOutputKeyClass(WordPair.class);
        job.setOutputValueClass(IntWritable.class);
 
        job.setInputFormatClass(WholeFileInputFormat.class);
        job.setOutputFormatClass(TextOutputFormat.class);
        FileInputFormat.addInputPaths(job, args[0]);
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
 
}
```

**WordConcurrnceMap**

```java
import java.io.IOException;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.Queue;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
 
public class WordConcurrnceMap extends Mapper<Text, Text, WordPair, IntWritable> {
 
    private int windowSize = 0;
    private static final String WORD_REGEX = "([a-zA-Z]{1,})";// 仅仅匹配由字母组成的简单英文单词
    private static final Pattern WORD_PATTERN = Pattern.compile(WORD_REGEX);// 用于识别英语单词(带连字符-)
    private Queue<String> queue = new LinkedList<String>();
    private static final IntWritable ONE = new IntWritable(1);
 
    protected void setup(Context context) throws java.io.IOException, InterruptedException {
        Configuration conf = context.getConfiguration();
        windowSize = conf.getInt("windowSize", 20);
    };
 
    protected void map(Text key, Text value, Context context) throws java.io.IOException, InterruptedException {
        System.out.println("value:" + value);
        System.out.println("windowSize:" + windowSize);
        Matcher matcher = WORD_PATTERN.matcher(value.toString());
        while (matcher.find()) {
            String word = matcher.group();
            queue.add(word);
            if (queue.size() >= windowSize) {
                wordPair(context);
            }
        }
        while (!(queue.size() <= 1)) {
            wordPair(context);
        }
    };
 
    private void wordPair(Context context) throws IOException, InterruptedException {
        Iterator<String> it = queue.iterator();
        String wordA = it.next();
        while (it.hasNext()) {
            String wordB = it.next();
            context.write(new WordPair(wordA, wordB), ONE);
        }
        queue.remove();
    }
 
}
```

**WordConcurrnceReduce**

```java
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.mapreduce.Reducer;
 
public class WordConcurrnceReduce extends Reducer<WordPair, IntWritable, WordPair, IntWritable> {
 
    private IntWritable wordSum = new IntWritable();
 
    protected void reduce(WordPair key, Iterable<IntWritable> values, Context context) throws java.io.IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        wordSum.set(sum);
        context.write(key, wordSum);
    };
 
}
```

**WordPair**

```java
import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;
 
import org.apache.hadoop.io.WritableComparable;
 
public class WordPair implements WritableComparable<WordPair> {
 
    private String wordA;
    private String wordB;
 
    public WordPair() {
    }
 
    public WordPair(String wordA, String wordB) {
        this.wordA = wordA;
        this.wordB = wordB;
    }
 
    public void write(DataOutput out) throws IOException {
        out.writeUTF(wordA);
        out.writeUTF(wordB);
    }
 
    public void readFields(DataInput in) throws IOException {
        wordA = in.readUTF();
        wordB = in.readUTF();
    }
 
    @Override
    public String toString() {
        return wordA + "," + wordB;
    }
 
    @Override
    public int hashCode() {
        return (wordA.hashCode() + wordB.hashCode()) * 9;
    }
 
    public int compareTo(WordPair o) {
        if (equals(o)) {
            return 0;
        }
        return (wordA + wordB).compareTo(o.wordA + o.wordB);
    }
 
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof WordPair) {
            return false;
        }
        WordPair w = (WordPair) obj;
        if (wordA.equals(w.wordA) && wordB.equals(w.wordB)) {
            return true;
        }
        if (wordA.equals(w.wordB) && wordB.equals(w.wordA)) {
            return true;
        }
        return false;
    }
 
}
```


  
  ## 运行

```powershell
hadoop jar WordOccurence.jar /input /output 
```
![运行结果](https://img-blog.csdnimg.cn/20200108212845826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDIyNDQ4,size_1,color_FFFFFF,t_70)




---
---
**欢迎查看我的CSDN博客：[Welcome To Ryan's Home](https://blog.csdn.net/qq_41422448)**

---
---