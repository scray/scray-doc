---
title: "Install CDH 5.13.0 with RHEL 7"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

```
ssh-keygen
cd ~/.ssh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

Configure passwordless SSH for root user
```
ssh-copy-id -i ~/.ssh/id_rsa.pub root@10.11.22.33
```

install Java 8
```
rpm -i jdk-8u151-linux-x64.rpm 
```

Find the IP address, FQDN and shortname for each of nodes and add them to the /etc/hosts on all nodes in the format:
<IP address> <FQDN> <shortname>
To get shortname and FQDN:
>hostname && hostname -f

Edit the  ```/etc/hosts``` file and distribute it to all hosts

```
127.0.0.1   localhost
10.11.22.31 bdq-cassandra1.seeburger.de bdq-cassandra1
10.11.22.32 bdq-cassandra2.seeburger.de bdq-cassandra2
```

* ### Set SELinux policy to diasabled.

Edit /etc/selinux/config
 ```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
#SELINUX=enforcing
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes
#     are protected. 
#     mls - Multi Level Security protection.
#SELINUXTYPE=targeted 
SELINUX=disabled
```

Disable firewall

```
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld
```

Set swappiness to 0 / Disable IPV6
Edit /etc/sysctl.conf

```
vm.swappiness=0 
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.all.disable_ipv6 = 1
```

restart
```
shutdown -r now
```

* 


/etc/passwd
cloudera-scm:x:997:995:Cloudera Manager:/var/lib/cloudera-scm-server:/bin/bash

To run cloudera-scm-agent as root
/etc/default/cloudera-scm-agent and uncommenting the line:
USER="cloudera-scm"


sudo service cloudera-scm-agent stop

sudo service cloudera-scm-server stop
sudo service cloudera-scm-server status


Ok, ready for the installation. Here are the steps.
1. Download and Run the Cloudera Manager Server Installer
Logon as root user on vmhost1. All of the installations are under root user.
Run the following commands.

```
wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
chmod u+x cloudera-manager-installer.bin
./cloudera-manager-installer.bin
```

choose parcels

Configuring a Custom Java Home Location
https://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_java_home_location.html#cmig_topic_16



* ### Cleanup a host

```
rm -rf /opt/cloudera
rm -rf /var/log/cloudera-scm-agent
rm -rf /tmp/*
rm -rf /run/cloudera-scm-agent
rm /var/lib/cloudera-scm-agent/cm_guid

yum -y remove cloudera-manager-daemons
yum -y remove cloudera-manager-agent

shutdown -r now
```


* Connect to WebUI

    + Host:   10.0.0.1
    + User:     admin
    + Password:     admin


* ### Update Java Home Directory in CDH Manager
    [Cloudere doc](https://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_java_home_location.html#cmig_topic_16)

* ### Add host2

    [Cloudera doc](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_mc_adding_hosts.html)
