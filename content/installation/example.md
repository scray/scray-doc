---
title: "Example"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

In this IOT example we will poll the state of elevators and escalators from a [open data source](https://www.mcloud.de/web/guest/suche/-/results/detail/aufzugsdaten?_mysearchportlet_backURL=https%3A%2F%2Fwww.mcloud.de%2Fweb%2Fguest%2Fsuche%2F-%2Fresults%2FsearchAction%3F_mysearchportlet_query%3DAufzug%26_mysearchportlet_page%3D0&_mysearchportlet_query=Aufzug) using the API of Deutsche Bahn every 20s. We are developing a [Spark](https://spark.apache.org/) aggregation job counting elevators which are at maintenance.


[Cloudera Manager](http://blog.cloudera.com/blog/2013/07/how-does-cloudera-manager-work/)

[Grafana](https://grafana.com/)

[Kafka](https://kafka.apache.org/)

[StreamSets Data Collector](https://streamsets.com/products/sdc/)


|Service |Version| Cloudera | Docker |
|---|---|---|---|
|Kafka| 0.10.0 | 2.1.1 | |
|Grafana|min. 4.6.2 | | x |
|Graphite| 1.1 | | x |
|Streamsets|min. 3.0.0.0 | | |

Different services are distributed over several hosts of a cluster. For easy installion and operation we start with creating a 
```/etc/hosts``` which we will distribute to all the hosts.

```
127.0.0.1   localhost
10.14.0.61 host1.scray.org host1 streamsets hdfs-namenode yarn.resourcemanager kafka-broker-1 graphite-host
10.14.0.62 host2.scray.org host2
```

We can use physical or virtual machines. They can be accessed directly or by tunnels.


https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7

```
sudo yum -y install git
wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
yum -y install apache-maven
```

https://stackoverflow.com/questions/7532928/how-do-i-install-maven-with-yum

Cloudera

Docker

Curl

Example auschecken

```
git clone https://github.com/scray/scray.git
cd scray/
git branch -a
git checkout feature/report-example
mvn install -DskipTests
```

Streamsets

Grafana
```
ssh -L 3002:localhost:3001 centos@10.15.24.228
```

```
WATCHCONT=$(sudo docker ps | fgrep grafana | cut -d' ' -f1)
IP=$(sudo docker inspect $WATCHCONT | fgrep IPAddress | cut -d':' -f2 | cut -d'"' -f2)
sudo docker exec -i -t $WATCHCONT /bin/bash
```

Job
adapt memory for yarn in Cloudera Manager


* ### Required for manual or to integrate with own installations

| |Version|
|---|---|
|Kafka| |
|Grafana| |
|Streamsets| |

```
```
