---
title: "Kafka"
date: 2017-11-13T11:32:26+01:00
draft: false 
---


installing kafka
https://www.cloudera.com/documentation/kafka/1-2-x/topics/kafka_install.html

Parcel Locations
https://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_parcels.html#concept_vwq_421_yk__section_ug1_c3y_bm

Click the parcels icon. Then the "Configuration" button. Click a "+" and paste the following repository. Save the changes and the 2.1.1 Parcel should show up. Click "Download", then "Distribute" and "Activate".

Kafka different versions at:
https://www.cloudera.com/documentation/kafka/latest/topics/kafka_packaging.html

```
https://archive.cloudera.com/kafka/parcels/2.2.0/
```

Add a Kafka service to "Cluster 1" at the main page. Configure the service and select a host for the broker.

If you have problems, read
https://community.cloudera.com/t5/Cloudera-Manager-Installation/adding-a-Kafka-service-failed/m-p/40959


Continue .....

```
/usr/bin/kafka-topics --list --zookeeper bdq-cassandra5:2181
/usr/bin/kafka-topics --create --zookeeper bdq-cassandra5:2181 --replication-factor 1 --partitions 1 --topic facility
/usr/bin/kafka-console-producer --broker-list 10.11.22.34:9092 --topic aufzug
/usr/bin/kafka-console-consumer --zookeeper bdq-cassandra5:2181 --topic aufzug
```