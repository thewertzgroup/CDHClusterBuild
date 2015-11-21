# CDHClusterBuild

# 4 Data Nodes on RAID 5


## TestDFSIO:

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -write -nrFiles 10 -fileSize 1000 > TestDFSIO-4DataNodes-Write.out 2>&1

```
15/11/17 11:35:50 INFO fs.TestDFSIO: ----- TestDFSIO ----- : write
15/11/17 11:35:50 INFO fs.TestDFSIO:            Date & time: Tue Nov 17 11:35:50 MST 2015
15/11/17 11:35:50 INFO fs.TestDFSIO:        Number of files: 10
15/11/17 11:35:50 INFO fs.TestDFSIO: Total MBytes processed: 10000.0
15/11/17 11:35:50 INFO fs.TestDFSIO:      Throughput mb/sec: 59.95347610254443
15/11/17 11:35:50 INFO fs.TestDFSIO: Average IO rate mb/sec: 67.88632202148438
15/11/17 11:35:50 INFO fs.TestDFSIO:  IO rate std deviation: 23.334879865403757
15/11/17 11:35:50 INFO fs.TestDFSIO:     Test exec time sec: 169.912
15/11/17 11:35:50 INFO fs.TestDFSIO: 

real	2m53.325s
user	0m47.126s
sys	0m15.837s
```

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -read -nrFiles 10 -fileSize 1000 > TestDFSIO-4DataNodes-Read.out 2>&1

```
15/11/17 12:13:19 INFO fs.TestDFSIO: ----- TestDFSIO ----- : read
15/11/17 12:13:19 INFO fs.TestDFSIO:            Date & time: Tue Nov 17 12:13:19 MST 2015
15/11/17 12:13:19 INFO fs.TestDFSIO:        Number of files: 10
15/11/17 12:13:19 INFO fs.TestDFSIO: Total MBytes processed: 10000.0
15/11/17 12:13:19 INFO fs.TestDFSIO:      Throughput mb/sec: 110.19162323280185
15/11/17 12:13:19 INFO fs.TestDFSIO: Average IO rate mb/sec: 110.19415283203125
15/11/17 12:13:19 INFO fs.TestDFSIO:  IO rate std deviation: 0.5263097409693699
15/11/17 12:13:19 INFO fs.TestDFSIO:     Test exec time sec: 93.919
15/11/17 12:13:19 INFO fs.TestDFSIO: 

real	1m37.474s
user	0m35.650s
sys	0m18.246s
```

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -clean > TestDFSIO-4DataNodes-Clean.out 2>&1

```
15/11/17 12:15:50 INFO fs.TestDFSIO: TestDFSIO.1.7
15/11/17 12:15:50 INFO fs.TestDFSIO: nrFiles = 1
15/11/17 12:15:50 INFO fs.TestDFSIO: nrBytes (MB) = 1.0
15/11/17 12:15:50 INFO fs.TestDFSIO: bufferSize = 1000000
15/11/17 12:15:50 INFO fs.TestDFSIO: baseDir = /benchmarks/TestDFSIO
15/11/17 12:15:51 INFO fs.TestDFSIO: Cleaning up test files

real	0m2.832s
user	0m4.178s
sys	0m0.231s
```

## TeraGen:

[hdfs@cdh-test-edge ~]$ time yarn jar /opt/cloudera/parcels/CDH/jars/hadoop-examples.jar teragen 10000000000 /user/hduser/terasort-input >TeraGen-4DataNodes.out 2>&1

```
```

## TeraSort:

## TeraValidate:

# 6 Data Nodes on RAID 5

## TestDFSIO:

## TeraGen:

## TeraSort:

## TeraValidate:

# 4 Data Nodes on RAID 5 2 Data Nodes JBOD

## TestDFSIO:

## TeraGen:

## TeraSort:

## TeraValidate:

# 6 Data Nodes JBOD

## TestDFSIO:

## TeraGen:

## TeraSort:

## TeraValidate:
