---
title: "Install CDH 5.13.0 with RHEL 7"
date: 2017-11-13T11:32:26+01:00
draft: false 
---


```
sudo yum -y install epel-release
sudo yum -y install emacs
```


as root
```
ssh-keygen
```

```
cd ~/.ssh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

as root
emacs /etc/ssh/sshd_config

PermitRootLogin yes
PasswordAuthentication yes

service sshd restart


https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/v2v_guide/preparation_before_the_p2v_migration-enable_root_login_over_ssh

https://www.digitalocean.com/community/questions/ssh-failed-permission-denied-publickey-gssapi-keyex-gssapi-with-mic



Find the IP address, FQDN and shortname for each of nodes and add them to the /etc/hosts on all nodes in the format:
<IP address> <FQDN> <shortname>
To get shortname and FQDN:
>hostname && hostname -f

http://www.servermom.org/setup-fqdn-hostname-centos-server/

```
hostnamectl set-hostname host2.scray.org

```

Fora ll hosts:

set a password for root
passwd

Configure passwordless SSH for root user
```
ssh-copy-id -i ~/.ssh/id_rsa.pub root@host2
```



Edit the  ```/etc/hosts``` file and distribute it to all hosts

```
127.0.0.1   localhost
10.11.22.31 bdq-cassandra1.seeburger.de bdq-cassandra1
10.11.22.32 bdq-cassandra2.seeburger.de bdq-cassandra2
```

scp /etc/hosts host3:/etc/hosts


install Java 8
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```
rpm -i jdk-8u151-linux-x64.rpm 
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

Run the following commands at the manager's host.

```
wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
chmod u+x cloudera-manager-installer.bin
./cloudera-manager-installer.bin
```

```
host1.scray.org:7180
```

```
ssh -L 7180:localhost:7180 root@10.15.24.228
```
accept the licence
continue with
"Cloudera Enterprise
Cloudera Enterprise Trial"

press continue

Specify hosts for your CDH cluster installation. You can use patterns, e.g.
```
host[1-2]
```

stick with parcels and none for the rest. Just press continue.

At the fourth screen enter the root password

At screen 5 wait for the packages to be installed.

It says in details, but installation was succesful:
"Loaded plugins: fastestmirror
Error: No matching Packages to list"

* ### Cluster Setup

Next is the Cluster Setup
Choose the CDH 5 services that you want to install on your cluster.

So I chose Custom Services and chose the services mainly from Core with Impala + Spark. Make sure to check Include Cloudera Navigator.

choose HDFS, YARN, Zookepper

at the next screen I chose "All Hosts" for Datanode and default for the rest. Then continue.

At the next screen hit "Test connection" and continue.

At the next screen you have to review the cluster setup. Especially take care there is enough space in the data directories. E.g. if you have an own data partiotion for your linux installation you should change the /dfs/... to e.g. /home/dfs .

At step 5 it will start all the services. All should be green.

Finally "Finish". You get to the main screen and see your "Cluster 1" with all the services installed.

```
alternatives --list | fgrep java
```

Configuring a Custom Java Home Location
https://www.cloudera.com/documentation/enterprise/5-3-x/topics/cm_ig_java_home_location.html#cmig_topic_16

Select "All hosts" in the "Hosts" menu. 
Click the "Configuration" button in the top area.
Click the category "Advanced" in the left sidebar. 

In the Advanced category, click the Java Home Directory property.
Set the property to the custom location e.g. "/usr/java/jdk1.8.0_152/jre/"
Click Save Changes.
Go to the main page and restart all services by clicking "Restart" in the menu right to "Cluster 1".

The Cloudera Management service needs also to be restarted. "Stale configuration"

search for and check the box 
dfs.datanode.use.datanode.hostname
dfs.client.use.datanode.hostname

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
