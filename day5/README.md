# Understanding lifecyle of Devops -> Devsecops 

<img src="dev1.png">

## Java based app build and test using maven 

<img src="mvn1.png">

## ON vm lets build java code with maven 

```
git clone https://github.com/redashu/java-springboot.git
Cloning into 'java-springboot'...
remote: Enumerating objects: 33, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 33 (delta 3), reused 0 (delta 0), pack-reused 24 (from 1)
Receiving objects: 100% (33/33), 8.48 KiB | 8.48 MiB/s, done.
Resolving deltas: 100% (5/5), done.
[ashu@ip-172-31-29-58 ashu-devsecops]$ ls
ashu-java-webapp  ashu-python-webapp  ashu_unisys_flaskMysql  java-springboot  unisys_devsecops
[ashu@ip-172-31-29-58 ashu-devsecops]$ 

```

## building it 

### install jdk11 or later if not present 

```
yum install java-11* 

===> verify 
java --version 
openjdk 11.0.24 2024-07-16 LTS
OpenJDK Runtime Environment Corretto-11.0.24.8.1 (build 11.0.24+8-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.24.8.1 (build 11.0.24+8-LTS, mixed mode)
[ashu@ip-172-31-29-58 java-springboot]$ 

===> Installing maven 

yum install maven -y

===> verify 
ashu@ip-172-31-29-58 java-springboot]$ mvn -v
Apache Maven 3.8.4 (Red Hat 3.8.4-3.amzn2023.0.5)
Maven home: /usr/share/maven
Java version: 17.0.12, vendor: Amazon.com Inc., runtime: /usr/lib/jvm/java-17-amazon-corretto.x86_64
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "6.1.109-118.189.amzn2023.x86_64", arch: "amd64", family: "unix"
[ashu@ip-172-31-29-58 java-springboot]$ 

===> build it 

 mvn clean package
```

### doing in container 

```
docker build -t ashujava:mvnb1 . 

[ashu@ip-172-31-29-58 java-springboot]$ docker run -it --rm  ashujava:mvnb1  bash 
[root@e4e6f2a15ba9 java-springboot]# ls
Dockerfile  README.md  pom.xml  src  target
[root@e4e6f2a15ba9 java-springboot]# ls target/
WebApp  WebApp.war  maven-archiver
[root@e4e6f2a15ba9 java-springboot]# 

```
