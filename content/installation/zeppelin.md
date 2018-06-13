---
title: "Zeppelin"
date: 2017-11-13T11:32:26+01:00
draft: false 
---


https://zeppelin.apache.org/download.html

```
docker run -p 8080:8080 --rm --name zeppelin apache/zeppelin:0.7.3
```

```
docker run -p 8080:8080 --rm -v $PWD/logs:/logs -v $PWD/notebook:/notebook -e ZEPPELIN_LOG_DIR='/logs' -e ZEPPELIN_NOTEBOOK_DIR='/notebook' --name zeppelin apache/zeppelin:0.7.3
```

https://zeppelin.apache.org/docs/0.7.3/interpreter/jdbc.html

```
default.driver 	org.apache.hive.jdbc.HiveDriver
default.url 	jdbc:hive2://host1.scray.org:10000
default.user 	   hive1
default.password   y0ai0LkLut

Dependencies
Artifact 	
org.apache.hive:hive-jdbc:0.14.0 	
org.apache.hadoop:hadoop-common:2.6.0
```

https://hortonworks.com/tutorial/using-hive-with-orc-from-apache-spark/#getting-the-dataset


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
