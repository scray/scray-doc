---
title: "hdfs"
date: 2017-11-13T11:32:26+01:00
draft: false 
---


If you need to format the namenode
```
/usr/bin/hdfs namenode -format
```

stop hdfs
Data directory is local filesystem storage location for each datanode in your cluster. Search for "dfs.datanode.data.dir" property
/dfs/dn
delete files for all datanodes
rm -rf /dfs/dn/*
start hdfs service

```
/usr/bin/hdfs dfsadmin -safemode leave
```
