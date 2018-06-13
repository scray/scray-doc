---
title: "streamsets"
date: 2017-11-13T11:32:26+01:00
draft: false 
---



https://streamsets.com/documentation/datacollector/3.3.0/help/#Installation/Install_title.html




```
wget https://archives.streamsets.com/datacollector/3.3.0/csd/STREAMSETS-3.3.0.jar

mv ./STREAMSETS-3.3.0.jar /opt/cloudera/csd
chown cloudera-scm:cloudera-scm /opt/cloudera/csd/STREAMSETS*.jar
chmod 644 /opt/cloudera/csd/STREAMSETS*.jar
systemctl restart cloudera-scm-server
```



https://groups.google.com/a/streamsets.com/forum/#!topic/sdc-user/Cr6p0eBX0bc

In Cloudera Manager admin console I added the following url to the parcel settings: http://archives.streamsets.com/datacollector/latest/parcel/

Go to Parcels / Configuration and add the following URL to the parcel settings by clicking a (+) icon. Then save the settings

```
http://archives.streamsets.com/datacollector/3.3.0/parcel/
```

Now you can download, distribute and activate the parcel.

Note: If you are using a docker container or some local computer and you want to access services from your cluster the hosts need to be provided in the /etc/hosts.

Add a streamsets service to "Cluster 1"

Click "Streamsets" in "Cluster 1" and then "Data Clollectors Web UI"
or point your browser directly to ```http://host1.scray.org:18630/```

```
cd /root/scray/scray-example/pipelines/streamsets
curl -X POST -H "Content-Type: application/json" -d "@./CologneElevators.json" "http://streamsets:18630/rest/v1/pipeline/CologneElevators/import" \
-u admin:admin -H "X-Requested-By:myapp"
curl -X POST -H "Content-Type: application/json" -d "@./BahnFacilities.json" "http://streamsets:18630/rest/v1/pipeline/BahnFacilities/import" \
-u admin:admin -H "X-Requested-By:myapp"
```

* For the example: 

```
sudo su - hdfs
/usr/bin/hdfs dfs -mkdir hdfs://hdfs-namenode:8020/data
/usr/bin/hdfs dfs -chown sdc hdfs://hdfs-namenode:8020/data
/usr/bin/hdfs dfs -ls hdfs://hdfs-namenode:8020/
```


The provided pipelines require at minimum streamsets version 3.0.0

Post the pipelines to your streamsets using the API:

```
curl -X POST -H "Content-Type: application/json" -d "@./CologneElevators.json" "http://10.11.22.33:18630/rest/v1/pipeline/CologneElevators/import" -u admin:admin -H "X-Requested-By:myapp"
curl -X POST -H "Content-Type: application/json" -d "@./Bahn3-v1b.json" "http://10.11.22.33:18630/rest/v1/pipeline/Bahn3-v1b/import" -u admin:admin -H "X-Requested-By:myapp"
```

To backup the pipelines:

```
curl -X GET "http://streamsets:18630/rest/v1/pipeline/CologneElevators/export" -u admin:admin -H "X-Requested-By:myapp" \
> ./CologneElevators.json
curl -X GET "http://streamsets:18630/rest/v1/pipeline/BahnFacilities/export" -u admin:admin -H "X-Requested-By:myapp" \
> ./BahnFacilities.json
```

* To get the REST API click the questionmark (help) in the right upper corner and then RESTful API

* Some more examples using the API:

To get the number of avilable pipelines :
```
curl http://streamsets:18630/rest/v1/pipelines/count \
       -u admin:admin -H "X-Requested-By:myapp"
```

```
curl "http://streamsets:18630/rest/v1/pipeline/CologneElevators/status" \
       -u admin:admin -H "X-Requested-By:myapp"
```


Create a new pipeline. You need to hit refresh in Streamlines Pipeline view.

```
curl -X PUT "http://streamsets:18630/rest/v1/pipeline/tmp" -u admin:admin -H "X-Requested-By:myapp"
```

```
curl -X DELETE "http://streamsets:18630/rest/v1/pipeline/tmp" -u admin:admin -H "X-Requested-By:myapp"
```

```
```
For debugging use
```
nc -l localhost 5000
```
output should be e.g.
"bahn.equipment.type.ELEVATOR.all.state.ACTIVE.count" 1983 1513878120

```
```

```
```


https://www.cloudera.com/documentation/enterprise/5-8-x/topics/admin_hdfs_proxy_users.html

https://ask.streamsets.com/question/31/error-in-hadoop-fs-destination-user-sdc-is-not-allowed-to-impersonate-hadoop/

Use the cloudera manager and put the following sniplet


```
<property>
  <name>hadoop.proxyuser.sdc.hosts</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.sdc.groups</name>
  <value>*</value>
</property>
```

in
HDFS Service->Configuration->Service-Wide-Advanced->Cluster-wide Advanced Configuration Snippet (Safety Valve) for core-site.xml:

Then restart those services and they'll grab that entry from that configuration.

