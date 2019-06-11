# Hadoop Book - Chapter 1
Solution to weather forecast data. This solution uses small sample data 
from National climatic data center. Complete data is available (here)[ftp://ftp.ncdc.noaa.gov/pub/data/noaa/]
for download.

# How to run this application

* Build the application `mvn clean install`. This should create a jar file `weather-sample-1.0-SNAPSHOT.jar` in target dir.
* Start Hadoop docker image. 
** Pull the image (if you don't have already) : `docker pull sequenceiq/hadoop-docker`
** Run the container by passing target directory as volume to docker image.

sample: `docker run -v=<local target folder location>:<location where target to be placed inside container> -it <container id or name> /etc/bootstrap.sh -bash`
command: `docker run -v=$HOME/data/:/etc/hadoop/ -it 5c /etc/bootstrap.sh -bash`

Note: Docker doesnot allow to use actual full path for local folder location.  Hence `$HOME/data/target`.
Reference: https://stackoverflow.com/questions/40213524/using-absolute-path-with-docker-run-command-not-working

The above command will start the shell.


* To check whether the volume is properly mounted: `cd etc/hadoop/`. Make sure your files are present.
* Run the command `cd $HADOOP_PREFIX` in bash.
* Make a hadoop file system input directory.  `bin/hadoop fs -mkdir -p /etc/hadoop/input`
* Move the input file to created input directory. 
`bin/hadoop fs -put /etc/hadoop/target/classes/input.txt  /etc/hadoop/input`
* To check whether input file is placed correctly: `bin/hadoop fs -ls /etc/hadoop/input`

* Run the application 
`bin/hadoop jar /etc/hadoop/target/weather-sample-1.0-SNAPSHOT.jar /etc/hadoop/input/input.txt  /etc/hadoop/target/classes/output`

* To copy the output to local file system from hadoop fs:  `hadoop fs –copyToLocal < new file name>`

Notes: This project uses java 1.7 as docker image has this version. If you build your application using different version of java
and run the app, you will get following error.
Exception in thread "main" java.lang.UnsupportedClassVersionError: Unsupported major.minor version 52.0

# Sample log

bash-4.1# bin/hadoop jar /etc/hadoop/target/weather-sample-1.0-SNAPSHOT.jar /etc
/hadoop/input/input.txt  /etc/hadoop/target/classes/output
19/05/20 06:47:09 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0
:8032
19/05/20 06:47:11 WARN mapreduce.JobResourceUploader: Hadoop command-line option
 parsing not performed. Implement the Tool interface and execute your applicatio
n with ToolRunner to remedy this.
19/05/20 06:47:13 INFO input.FileInputFormat: Total input paths to process : 1
19/05/20 06:47:14 INFO mapreduce.JobSubmitter: number of splits:1
19/05/20 06:47:16 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_15
58346829398_0003
19/05/20 06:47:17 INFO impl.YarnClientImpl: Submitted application application_15
58346829398_0003
19/05/20 06:47:18 INFO mapreduce.Job: The url to track the job: http://fd798235a
8f4:8088/proxy/application_1558346829398_0003/
19/05/20 06:47:18 INFO mapreduce.Job: Running job: job_1558346829398_0003
19/05/20 06:48:32 INFO mapreduce.Job: Job job_1558346829398_0003 running in uber
 mode : false
19/05/20 06:48:32 INFO mapreduce.Job:  map 0% reduce 0%
19/05/20 06:49:11 INFO mapreduce.Job:  map 100% reduce 0%
19/05/20 06:49:39 INFO mapreduce.Job:  map 100% reduce 100%
19/05/20 06:49:41 INFO mapreduce.Job: Job job_1558346829398_0003 completed succe
ssfully
19/05/20 06:49:41 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=6
                FILE: Number of bytes written=229583
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=649
                HDFS: Number of bytes written=0
                HDFS: Number of read operations=6
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters
                Launched map tasks=1
                Launched reduce tasks=1
                Data-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=34350
                Total time spent by all reduces in occupied slots (ms)=24447
                Total time spent by all map tasks (ms)=34350
                Total time spent by all reduce tasks (ms)=24447
                Total vcore-seconds taken by all map tasks=34350
                Total vcore-seconds taken by all reduce tasks=24447
                Total megabyte-seconds taken by all reduce tasks=25033728
        Map-ReduMap input records=5
                Map output records=0
                Map output materialized bytes=6
                Combine input records=0
                Reduce input groups=0s=0
                Reduce input records=0
                Reduce output records=0
                Shuffled Maps =10
                Merged Map outputs=1
