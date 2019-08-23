---
title: "Install CDH 5.13.0 with RHEL 7"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

https://www.cloudera.com/documentation/enterprise/6/6.3.html


```
sudo yum -y install epel-release
sudo yum -y install emacs
```

sudo su -
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
```
passwd
```

Configure passwordless SSH for root user
```
ssh-copy-id -i ~/.ssh/id_rsa.pub root@host2
```


https://www.cloudera.com/documentation/enterprise/6/6.3/topics/configure_network_names.html#configure_network_names


Edit the  ```/etc/hosts``` file and distribute it to all hosts

```
127.0.0.1   localhost
10.11.22.31 bdq-cassandra1.seeburger.de bdq-cassandra1
10.11.22.32 bdq-cassandra2.seeburger.de bdq-cassandra2
```

check
wget -qO- -T 1 -t 1 http://169.254.169.254/latest/meta-data/public-hostname && /bin/echo


scp /etc/hosts host3:/etc/hosts

step 3 of the cloudera guide


https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_cdh_disable_iptables.html#install_cdh_disable_iptables


Check, if there is a firewall
iptables -L

Only if the response does not say policy ACCEPT
Disable firewall

```
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld
```


https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_cdh_disable_selinux.html#install_cdh_disable_selinux

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
copy file to other hosts
scp /etc/selinux/config cloudera-2:/etc/selinux/config


NTP
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_cdh_enable_ntp.html#install_cdh_enable_ntp
```
yum -y install ntp
```

step 2, you can also add your local server
copy file to other hosts
scp /etc/ntp.conf cloudera-2:/etc/ntp.conf

step 3-6
```
sudo systemctl start ntpd
sudo systemctl enable ntpd
ntpdate -u seeburger.de
hwclock --systohc
date
```

if required set your timezone
```
sudo timedatectl set-timezone Europe/Berlin
timedatectl status # for debug
date
```


Set swappiness to 0 / Disable IPV6
Edit /etc/sysctl.conf

```
vm.swappiness=0 
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.all.disable_ipv6 = 1
```

copy to other hosts
```
 scp /etc/sysctl.conf cloudera-2:/etc/sysctl.conf
```

restart
```
shutdown -r now
```

yum -y install wget



* 





https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_cm_cdh.html

Configure a Repository for Cloudera Manager
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/configure_cm_repo.html#cm_repo

check for repo https://www.cloudera.com/documentation/enterprise/6/release-notes/topics/rg_cm_6_version_download.html

```
sudo wget https://archive.cloudera.com/cm6/6.3.0/redhat7/yum/cloudera-manager.repo  -P /etc/yum.repos.d/
sudo rpm --import https://archive.cloudera.com/cm6/6.3.0/redhat7/yum/RPM-GPG-KEY-cloudera
```

Install Java Development Kit
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/cdh_ig_jdk_installation.html#topic_29



install Java 8
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html?printOnly=1
https://www.digitalocean.com/community/tutorials/how-to-install-java-on-centos-and-fedora

 scp ./jdk-8u181-linux-x64.rpm cloudera-3:
```
rpm -i jdk-8u181-linux-x64.rpm
```

Step 3: Install Cloudera Manager Server
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_cm_server.html#install_cm_server
```
sudo yum -y install cloudera-manager-daemons cloudera-manager-agent cloudera-manager-server
sudo JAVA_HOME=/usr/java/jdk1.8.0_181-amd64 /opt/cloudera/cm-agent/bin/certmanager setup --configure-services
```

Install and Configure PostgreSQL for Cloudera Software
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/cm_ig_extrnl_pstgrs.html#cmig_topic_5_6

```
sudo -y yum install postgresql-server
sudo yum install python-pip
sudo pip install psycopg2==2.7.5 --ignore-installed

echo 'LC_ALL="en_US.UTF-8"' >> /etc/locale.conf
sudo su -l postgres -c "postgresql-setup initdb"

emacs /var/lib/pgsql/data/pg_hba.conf
emacs /var/lib/pgsql/data/postgresql.conf
sudo systemctl enable postgresql
sudo systemctl restart postgresql

sudo -u postgres psql

CREATE ROLE scm LOGIN PASSWORD '<password>';
CREATE DATABASE scm OWNER scm ENCODING 'UTF8';
CREATE ROLE amon LOGIN PASSWORD '<password>';
CREATE DATABASE amon OWNER amon ENCODING 'UTF8';
CREATE ROLE rman  LOGIN PASSWORD '<password>';
CREATE DATABASE rman OWNER rman ENCODING 'UTF8';
CREATE ROLE hue  LOGIN PASSWORD '<password>';
CREATE DATABASE hue  OWNER hue ENCODING 'UTF8';
CREATE ROLE metastore  LOGIN PASSWORD '<password>';
CREATE DATABASE metastore  OWNER metastore ENCODING 'UTF8';
CREATE ROLE sentry LOGIN PASSWORD '<password>';
CREATE DATABASE sentry  OWNER sentry ENCODING 'UTF8';
CREATE ROLE nav LOGIN PASSWORD '<password>';
CREATE DATABASE nav OWNER nav ENCODING 'UTF8';
CREATE ROLE navms LOGIN PASSWORD '<password>';
CREATE DATABASE navms OWNER navms ENCODING 'UTF8';
CREATE ROLE oozie LOGIN PASSWORD '<password>';
CREATE DATABASE oozie OWNER oozie ENCODING 'UTF8';

ALTER DATABASE metastore SET standard_conforming_strings=off;
ALTER DATABASE oozie SET standard_conforming_strings=off;
```

Step 5: Set up the Cloudera Manager Database
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/prepare_cm_database.html
```
sudo /opt/cloudera/cm/schema/scm_prepare_database.sh postgresql scm scm
```



We had problems to add a cluster later.  
```
emacs /etc/passwd
```
cloudera-scm:x:997:995:Cloudera Manager:/var/lib/cloudera-scm-server:/bin/bash

To run cloudera-scm-agent as cloudera-scm
```
emacs /etc/default/cloudera-scm-agent
```
and uncomment the line:
USER="cloudera-scm"






Step 6: Install CDH and Other Software
https://www.cloudera.com/documentation/enterprise/6/6.3/topics/install_software_cm_wizard.html#cm_installation_wizard

```
sudo systemctl start cloudera-scm-server
sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log
```

```
cloudera-1.research.dev.seeburger.de:7180
```



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

Accept JDK License : press continue

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

Setup Database
Set Database-Hostname to localhost:5432
Fill in the other fields.
Test connection.
Continue.

Command details
Change directories to some place with enough free space


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

sudo service cloudera-scm-agent stop

sudo service cloudera-scm-server stop
sudo service cloudera-scm-server status