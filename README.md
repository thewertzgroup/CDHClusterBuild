# CDHClusterBuild

## Passwordless SSH Configuration

```
[foo@FOO ~]# ssh-keygen -t rsa
```
```
[foo@FOO ~]# ssh bar@BAR mkdir -p .ssh
```
```
[foo@FOO ~]# cat .ssh/id_rsa.pub | ssh bar@BAR 'cat >> .ssh/authorized_keys'
bar@BAR's password:
```
```
[foo@FOO ~]# ssh bar@BAR
```

## OS Configuration

- Edit networking
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```
- Sync /etc/hosts
```
vi /etc/hosts
```
- Disable selinux
```
vi /etc/selinux/config

SELINUX=disabled
```
- Set swappiness = 10
```
Cloudera recommends setting /proc/sys/vm/swappiness to at most 10. Current setting is 60. Use the sysctl command to change this setting at runtime and edit /etc/sysctl.conf for this setting to be saved after a reboot. You may continue with installation, but you may run into issues with Cloudera Manager reporting that your hosts are unhealthy because they are swapping. The following hosts are affected: 

http://askubuntu.com/questions/103915/how-do-i-configure-swappiness

sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness

vi /etc/sysctl.conf

# Set swappiness to Cloudera recommendation
vm.swappiness=10
```
- Disable Transparent Huge Pages
```
Transparent Huge Pages is enabled and can cause significant performance problems. Kernel with release 'CentOS release 6.7 (Final)' and version '2.6.32-573.8.1.el6.x86_64' has enabled set to '[always] madvise never' and defrag set to '[always] madvise never'. Run "echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag" to disable this, then add the same command to an init script such as /etc/rc.local so it will be set upon system reboot. Alternatively, upgrade to RHEL 6.5 or later, which does not have this bug. The following hosts are affected: 

echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag
echo "echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag" >> /etc/rc.local
```
- Disable iptables
```
service iptables stop
chkconfig iptables off
chkconfig iptables --list

service ip6tables stop
chkconfig ip6tables off
chkconfig ip6tables --list
```
- Update OS
```
yum update
```
- Sync network time
```
http://www.cyberciti.biz/faq/howto-install-ntp-to-synchronize-server-clock/

yum install ntp ntpdate ntp-doc
chkconfig ntpd on
ntpdate pool.ntp.org
/etc/init.d/ntpd start
```
# 6 Data Nodes w/ 3 120GB JBOD

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

[hdfs@cdh-test-edge ~]$ time yarn jar /opt/cloudera/parcels/CDH/jars/hadoop-examples.jar teragen -D mapreduce.job.maps=48 -D mapreduce.job.reduces=48 -D dfs.replication=1 5000000000 /user/hduser/terasort-input >TeraGen-4DataNodes.out 2>&1

![](https://github.com/thewertzgroup/CDHClusterBuild/blob/master/images/TeraGen-4DataNodes-RAID5-1GbE.png)

```
real	116m29.535s
user	0m19.114s
sys	0m2.171s
```

## TeraSort:

[hdfs@cdh-test-edge ~]$ time yarn jar /opt/cloudera/parcels/CDH/jars/hadoop-examples.jar terasort /user/hduser/terasort-input /user/hduser/terasort-output >TeraSort-4DataNodes.out 2>&1

![](https://github.com/thewertzgroup/CDHClusterBuild/blob/master/images/TeraSort-4DataNodes-RAID5-1GbE.png)

```
```

## TeraValidate:

