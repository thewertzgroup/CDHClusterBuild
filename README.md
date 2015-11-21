# CDHClusterBuild

# 4 Data Nodes on RAID 5


## TestDFSIO:

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -read -nrFiles 10 -fileSize 1000 > TestDFSIO-4DataNodes-Read.out 2>&1

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -clean > TestDFSIO-4DataNodes-Clean.out 2>&1

[hdfs@cdh-test-edge ~]$ time hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-test-2.6.0-mr1-cdh5.4.8.jar TestDFSIO -write -nrFiles 10 -fileSize 1000 > TestDFSIO-4DataNodes-Write.out 2>&1

## TeraGen:

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
