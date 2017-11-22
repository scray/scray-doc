---
title: "Conf"
date: 2017-11-13T11:32:26+01:00
draft: false 
---


installing kafka
https://www.cloudera.com/documentation/kafka/1-2-x/topics/kafka_install.html

Parcel Locations
https://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_parcels.html#concept_vwq_421_yk__section_ug1_c3y_bm

/usr/bin/kafka-topics --list --zookeeper bdq-cassandra5:2181
/usr/bin/kafka-topics --create --zookeeper bdq-cassandra5:2181 --replication-factor 1 --partitions 1 --topic facility
/usr/bin/kafka-console-producer --broker-list 10.11.22.34:9092 --topic aufzug
/usr/bin/kafka-console-consumer --zookeeper bdq-cassandra5:2181 --topic aufzug
  