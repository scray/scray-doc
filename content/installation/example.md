---
title: "Example"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

/etc/hosts

```
127.0.0.1   localhost
10.14.0.61 host1.scray.org host1
10.14.0.62 host2.scray.org host2
```

https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7
sudo yum -y install git

Cloudera

Docker

Curl

Example auschecken

```
git clone https://github.com/scray/scray.git
cd scray/
git branch -a
git checkout feature/report-example
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

* ### Required for manual or to integrate with own installations

| |Version|
|---|---|
|Kafka| |
|Grafana| |
|Streamsets| |

```
```
