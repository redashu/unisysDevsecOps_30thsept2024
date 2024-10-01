## DevsecOps 

### Revision 

<img src="rev1.png">

### Understanding app deployment / testing Infra 

<img src="infra1.png">

### app Incompatibiity was a really big issues 

<img src="infra2.png">

### introduing vm concept to run multiple apps on single physical infra 

<img src="vm1.png">

## Problems with Vm 

<img src="vm2.png">

## VM vs Containers 

<img src="cont1.png">

## Container tech is using Host OS to hanlde containers and apps running inside it 

<img src="cont2.png">

### Container runtimes 

<img src="cont3.png">


## Docker Understanding 

<img src="d1.png">

### Docker infra understanding 

<img src="d11.png">


## Note: to setup docker in RHEL linux 

```
yum install docker 
Last metadata expiration check: 1 day, 2:53:36 ago on Mon Sep 30 02:35:08 2024.
Package docker-25.0.6-1.amzn2023.0.2.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!

====>
[root@ip-172-31-28-115 ~]# systemctl start docker 
[root@ip-172-31-28-115 ~]# systemctl status  docker 

● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
     Active: active (running) since Tue 2024-10-01 04:33:39 UTC; 55min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
```

## Understanding docker client and server connect process

<img src="proc.png">


### checking docker client to server connection 

```
 docker  context  ls
NAME        DESCRIPTION                               DOCKER ENDPOINT               ERROR
default *   Current DOCKER_HOST based configuration   unix:///var/run/docker.sock   
[ashu@ip-172-31-29-58 ashu-devsecops]$ 


```

### creating remote context to connect docker client to remote server

```
docker context  create ashu-remote --docker  host="tcp://172.31.28.115:2375"

===>
 docker  context  ls
NAME          DESCRIPTION                               DOCKER ENDPOINT               ERROR
ashu-remote                                             tcp://172.31.28.115:2375      
default *     Current DOCKER_HOST based configuration   unix:///var/run/docker.sock  

===>>
 docker context use ashu-remote
ashu-remote
Current context is now "ashu-remote"

===>

docker  context  ls
NAME            DESCRIPTION                               DOCKER ENDPOINT               ERROR
ashu-remote *                                             tcp://172.31.28.115:2375      
default         Current DOCKER_HOST based configuration   unix:///var/run/docker.sock