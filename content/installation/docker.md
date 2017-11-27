---
title: "Docker"
date: 2017-11-13T11:32:26+01:00
draft: false 
---

https://askubuntu.com/questions/938700/how-do-i-install-docker-on-ubuntu-16-04-lts
	
Docker comes in two flavours: The Comunity Edition (CE) and the Enterprise Edition (EE). See this question for the differences. Just take Docker CE if you don't know which to take.
The Ubuntu installation instructions list all you need in detail, but in most cases it boils down to:
(1) Set up the docker repository

$ sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

(2) Install Docker CE

```
sudo apt-get update
sudo apt-get install docker-ce
```

(3) Verify the installation
```
sudo docker run hello-world
```



```
```

```
```

```
```

```
```


```
```

```
```

```
```

```

```
```

```
```

```
```

```
```

```
