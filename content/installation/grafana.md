---
title: "Grafana"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

Start a grafana container. Open a webbrowser. Login as admin/admin.

```
sudo docker run -d --name=grafana -p 3001:3000 grafana/grafana
```

Point your webbrowser to
http://host1.scray.org:3002
Attention use the full name, not localhost

Start a graphite container.
https://graphite.readthedocs.io/en/latest/install.html#docker
```
sudo docker run -d\
 --name graphite\
 --restart=always\
 -p 81:80\
 -p 2003-2004:2003-2004\
 -p 2023-2024:2023-2024\
 -p 8125:8125/udp\
 -p 8126:8126\
 graphiteapp/graphite-statsd
```

Paste the following commands in a bash und reload your browser.

```
cp -r ~/git/scray/scray-example/dashboards/grafana/ /tmp
cd /tmp/grafana
./create.sh
rm -rf /tmp/grafana
```

* .

```
curl -X POST  "http://admin:admin@localhost:3001/api/dashboards/db/bahn" -d "@./bahn.json" -H "Content-Type: application/json"
```

```
curl -X GET  "http://admin:admin@10.11.22.36:3001/api/search/"
```

get the dashboard part 
```
cat bahn.json | awk '{split($0,a,"dashboard\":"); print a[2]}' | sed 's/.\{1\}$//'
```

replace "id" and "title" by template arguments

add DashboardBegin.txt  DashboardEnd.txt

post an empty dashboard and get the id
```
curl -X POST  "http://admin:admin@localhost:3001/api/dashboards/db/" -d "@./Dasboard_null_.json" -H "Content-Type: application/json"
ID=$(curl -X GET  "http://admin:admin@localhost:3001/api/dashboards/db/null_" | awk '{split($0,a,"id\":"); print a[2]}' | cut -d',' -f1)
```

replace the template arguments for itle and id in  DashboardTemplate_Facilities.json
```
sed 's/title_/Bahn3/' Template_Dashboard_Facilities.json | sed "s/id_/${ID}/" > Dashboard_Facilities.json
```

create the datasource DEMO
```
curl -X POST  "http://admin:admin@localhost:3001/api/datasources" -d "@./Datasource_DEMO_graphite.json" -H "Content-Type: application/json"
```

Post the dashboard. It will replace the null_ dashboard, because it is using the same id.
```
curl -X POST  "http://admin:admin@localhost:3001/api/dashboards/db/" -d "@./Dashboard_Facilities.json" -H "Content-Type: application/json"
```




get a datasource with name DEMO
```
curl -X GET  "http://admin:admin@localhost:3001/api/datasources/name/DEMO" > ds1.json
```

```
WATCHCONT=$(sudo docker ps | fgrep grafana | cut -d' ' -f1)
echo $WATCHCONT
IP=$(sudo docker inspect $WATCHCONT | fgrep IPAddress | cut -d':' -f2 | cut -d'"' -f2)
echo $IP
#ssh root@$IP
sudo docker exec -i -t $WATCHCONT /bin/bash

```

```
getent hosts graphite-host | awk '{ print $1 }'
```

http://docs.grafana.org/http_api/dashboard/
https://stackoverflow.com/questions/31166932/create-grafana-dashboards-with-api

```
curl -X GET  "http://admin:admin@10.11.22.36:3001/api/dashboards/db/bahn" > bahn.json
```
