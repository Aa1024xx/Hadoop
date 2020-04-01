Big Data Area
> Data Warehousing<br/>
> Data Analysis<br/>
> Data Transformation<br/>
> Data Collection<br/>
> ...<br/>

eg. Facebook rare data -> analysis -> event recommendations/social network analysis -> better storage<br/>
eg. [Data Collection]selected information arrangement: homepage/album/events/no spam...<br/>

Hadoop in Industries:
> Google: Youtube Search Ranking<br/>
> Linkin: People You May Know<br/>
> Netflix: Movie Recommendation<br/>
> Twitter: Store and Process log files<br/>

### Outline ###
 + Introduction to Hadoop Ecosystem
 + MapReduce Project
 + Course Outline
 
 #### Introduction to Hadoop Ecosystem ####
  + What is Hadoop?
       - Apache Hadoop is an open source software **framework** for 
       **storage** and large scale **processing of data-sets** on **clusters of commodity** hardware.
  + Why is Hadoop?
       - Big Data --Break into pieces--> Piece of data --merge-->Final Result
       - advantage: to process problem(data) one computer cannot process, no requirement for specific computers(cheaper)
  + Apache Hadoop Ecosystem
       - **Ambari**: Provisioning, Managing and Monitoring Hadoop Clusters
       - **Flume**: Log Collector
       - **Sqoop**: Data Exchange
       - **Zookeeper**: Coordination
       - **Oozie**: Workflow
       - **Pig**: Scripting
       - **Mahout**: Machine Learning
       - **R Connectors**: statistics
       - **Hive**: SQL Query
       - **Hbase**: Columnar Store
       - **YARN Map Reduce v2**: Distributed Processing Framework
       - **HDFS**: Hadoop Distributed File System
 
 #### Pipeline ####
  Now the data is coming in, what is the first thing to do?<br/>
  -> Storage on multiple machines
  + HDFS
        - **Data Storage:** Data Split, Data Replication
        - All you care about is the path of file: /hdfs/input/doc1.txt (HDFS Data  Distribution)
  + MapReduce
        - **Data Processing:** How to leverage job, How do nodes communicate, How to deal with node failure
        - All you care about is input and output
  + HBase
        - HBase is an open source, non-relational, distributed database
        - Key-Value store (retrieve)
        
 /* Hive是数据仓库工具，Pig是大数据分析平台，为用户提供多种接口，	HDFS是Hadoop的分布式文件系统(Distributed File System ，HBase是一个分布式的、面向列的开源数据库。*/
  
  A simple question <br/>
  -> What if the input is a big file = 1T? <br/>
  -> Can we process it with one machine? <br/>
  -> No! Out of memory; too slow <br/>
  -> What should we do? <br/>
  -> Use multiple machines <br/>
  -> Issues may be caused by multiple machines? <br/>
  -> Node failure, data skew /* 某个key非常火，这台机器承载不过来) */ ,... <br/>
  -> MapReduce comes out!<br/>
  
  /*
  数据倾斜是一个在MapReduce中可能会出现的问题，共有以下几种解决方案 <br/>
  1. 增加jvm内存,这适用于第一种情况'唯一值非常少，极少数值有非常多的记录值(唯一值少于几千)',这种情况下,往往只能通过硬件的手段来进行调优,增加jvm内存可以显著的提高运行效率。
  2. 增加reduce的个数,这适用于第二种情况(唯一值比较多，这个字段的某些值有远远多于其他值的记录数，但是它的占比也小于百分之一或千分之一),我们知道,这种情况下,最容易造成的结果就是大量相同key被partition到一个分区,从而一个reduce执行了大量的工作,而如果我们增加了reduce的个数,这种情况相对来说会减轻很多,毕竟计算的节点多了,就算工作量还是不均匀的,那也要小很多。
  3. 自定义分区,这需要用户自己继承partition类,指定分区策略,这种方式效果比较显著。
  4. 重新设计key,有一种方案是在map阶段时给key加上一个随机数,有了随机数的key就不会被大量的分配到同一节点(小几率),待到reduce后再把随机数去掉即可。
  5. 使用combinner合并,combinner是在map阶段,reduce之前的一个中间阶段,在这个阶段可以选择性的把大量的相同key数据先进行一个合并,可以看做是local reduce,然后再交给reduce来处理,这样做的好处很多,即减轻了map端向reduce端发送的数据量(减轻了网络带宽),也减轻了map端和reduce端中间的shuffle阶段的数据拉取数量(本地化磁盘IO速率),推荐使用这种方法。
  */
  
  #### MapReduce ####
   + A programming model for processing and generating large data sets with a parallel, distributed algorithm on a cluster.
   + Input Data Computation(split/merge) -> MapReduce -> Output
   + Mapper
        - A stage: Splits into small words
        - [Implementation](https://hadoop.apache.org/docs/r2.6.2/api/org/apache/hadoop/mapreduce/Mapper.html): Simplify your work(read/write...)
   + Reducer
        - A stage: Combines data from Mapper
        - [Implementation](https://hadoop.apache.org/docs/r2.6.2/api/org/apache/hadoop/mapreduce/Reducer.html):Simplify your work
   + example<br/>
   
   <pre name = "code" class="java">
    // map 进新数据每次调用 || set up 调用一次
    // inputKey: offset
    // inputValue: line : I love big data
    // split into words -> lookup each word in sentiment library
    // outputKey: sentiment
    // outputValue: 1
    String[] words = value.toString().trim().split("\\s+"); //split by space
    for (String word : words) {
         if (emotionLibrary.containsKey(word.toLowerCase())) {
             String emotion = emotionLibrary.get(word.toLowerCase());
             context.write(new Text(emotion), new IntWritable(1));
         }
    }
    </pre> 
    
   <pre name = "code" class="java">
   // setup 建立 emotionLibrary
    public void setup(Context context) throws IOException{
        // String path = "xxx";
        String path = context.getConfiguration().get("dictionary");
        BufferedReader bufferedReader = new BufferedReader(new FileReader(path));
        String line = bufferedReader.readLine();
        while (line != null){
             //line: happy\tpositive
             String[] word_emotion = line.split("\t");
             emotionLibrary.put(word_emotion[0].toLowerCase(), word_emotion[1]);
             line = bufferedReader.readLine();
        }
        bufferedReader.close();
    }
   </pre> 
   
   + Shuffle (Iterators)
        - Transport
        - Sort
        - Map in parallel, Reduce in parallel
   
   <pre name = "code" class="java">
    public static class SentimentCollection extends Reducer<Text, IntWritable, Text, IntWritable>{
       @Override
       public void reduce(Text key, Iterable<IntWritable>values, Context context)
                throws IOException, InterruptedException {
                //inputKey: positive
                //inputValue: <1, 1, 1...>
                //sum(inputValue)
                int sum = 0;
                for (IntWritable value: values) {
                    sum += value.get();
                }
                
                context.write(key, new IntWritable(sum));
                }
    }
   </pre> 
        
  + Advantages of hadoop? Performance
        - **Throughout**: Run computation in parallel
        - **Scalability**: Store and distribute very large data sets across clusters of hundreds of inexpensive servers operating in parallel
        - **Reliability**: Even if individual nodes experience high rates of failure when running jobs on a large cluster, data is replicated across a cluster so that it can be recovered easily in the face of disk, node or rack failures.
        - **Cheap**: Cluster of inexpensive servers.
  
   + Install MapReduce on Docker
        - Mac user: 
        <pre name = "code">
        if 内存 >= 8GB :

            直接装 docker for mac ，之后参考 Mac 版本教程

        else:
    
            使用 digitalocean 服务器, ssh 连接上去操作
        </pre> 
        - Windows user:
         <pre name = "code">
         if 内存 >= 8GB 且 愿意安装双系统 :

            安装 ubuntu 双系统， 在ubuntu 下进行操作

         else :
    
            使用 digitalocean 服务器, ssh 连接上去操作 
         </pre>
