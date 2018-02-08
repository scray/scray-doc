---
title: "empty"
date: 2017-11-13T11:32:26+01:00
draft: false 
---
http://community.cloudera.com/t5/Advanced-Analytics-Apache-Spark/Spark2-service-is-not-on-the-Add-Service-list-on-cdh5-9-1/td-p/51085

https://www.cloudera.com/documentation/spark2/latest/topics/spark2_installing.html

https://community.cloudera.com/t5/Cloudera-Manager-Installation/Start-Spark-failed-caused-by-the-failure-of-starting-history/td-p/21515

sudo groupadd supergroup
usermod -a -G supergroup mapred
usermod -a -G supergroup hdfs
usermod -a -G supergroup spark

hdfs dfsadmin -safemode leave

https://www.cloudera.com/documentation/enterprise/latest/topics/cdh_ig_running_spark_apps.html#concept_qdr_p2p_hs

## execute job

#sudo su hdfs
cd /root/git/scray/scray-example/spark-job/facility-state-job/

export JAVA_HOME=/opt/java
export HADOOP_USER_NAME=root
export YARN_CONF_DIR=/root/git/scray/scray-example/spark-job/facility-state-job/conf

/usr/bin/spark2-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0 --master yarn --deploy-mode cluster --num-executors 5 --driver-memory 2048m --executor-memory 2048m --total-executor-cores 10 --executor-cores 10 --files ./conf/log4j.properties,./conf/facility-state-job.yaml --class org.scray.example.FacilityStateJob target/facility-state-job-1.0-SNAPSHOT-jar-with-dependencies.jar ${ARGUMENTS[@]} -m yarn


/usr/bin/spark2-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0 --master yarn --deploy-mode cluster --num-executors 5 --driver-memory 512m --executor-memory 512m --total-executor-cores 4 --executor-cores 4 --files ./conf/log4j.properties,./conf/facility-state-job.yaml --class org.scray.example.FacilityStateJob target/facility-state-job-1.0-SNAPSHOT-jar-with-dependencies.jar ${ARGUMENTS[@]} -m yarn


ssh -L 50070:host1.scray.org:50070 centos@10.15.24.228
ssh -L 50075:host1.scray.org:50075 centos@10.15.24.228

```
```

```
```

```
```

```
```


```
```

```
```

```
```

```

```
```

```
```

```
```

```
```

```
