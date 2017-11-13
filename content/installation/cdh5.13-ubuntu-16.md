---
title: "Install CDH 5.13.0 on Ubuntu 16.04.2"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

Hosts

|IP|Hostname|
|---|---|
|10.0.0.1| node1|
|10.0.0.2| node2|


* ### Disable IPv6

    Edit ```/etc/sysctl.conf``` and add this values

    ```
    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    net.ipv6.conf.lo.disable_ipv6 = 1
    ```
    Reload configuration

    ```sudo sysctl -p```

* ### Configure hostnames

  Edit on 'node1' and 'node2' ```/etc/hosts```. 
  
  Add:
  ```
  10.0.0.1  node1
  10.0.0.1  node2
  ```

* ### Install Java 8
  ```
  sudo add-apt-repository ppa:webupd8team/java
  sudo apt-get update
  sudo apt-get install oracle-java8-installer
  sudo apt-get install oracle-java8-set-default
  ```
* Install Cloudera MANAGER on 'node1'

  Run:

    ```
    wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
    chmod u+x cloudera-manager-installer.bin
    sudo ./cloudera-manager-installer.bin
    ```
* Connect to WebUI

    + Host:   10.0.0.1
    + User:     admin
    + Password:     admin


* ### Update Java Home Directory in CDH Manager
    [Cloudere doc](https://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_java_home_location.html#cmig_topic_16)

* ### Add host2

    [Cloudera doc](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_mc_adding_hosts.html)
