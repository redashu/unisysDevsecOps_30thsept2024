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


### to deploy war file we need tomcat like app server 

### using multi stage dockerfile concept 

<img src="mst.png">

### building multistage image and do the contaioner create and test

```
docker build -t ashujava:tomcatapp1 . 
[+] Building 0.4s (15/15) FINISHED                                                                             docker:ashu-remote
 => [internal] load build definition from Dockerfile                                                                         0.0s
 => => transferring dockerfile: 585B                                                                                         0.0s
 => [internal] load metadata for docker.io/library/tomcat:latest                                                             0.0s
 => [internal] load metadata for docker.io/library/oraclelinux:8.4                                                           0.2s
 => [auth] library/oraclelinux:pull token for registry-1.docker.io                                                           0.0s
 => [internal] load .dockerignore                                                                                            0.0s
 => => transferring context: 2B                                                                                              0.0s
 => [warfilebuilder 1/7] FROM docker.io/library/oraclelinux:8.4@sha256:b81d5b0638bb67030b207d28586d0e714a811cc612396dbe3410  0.0s
 => [stage-1 1/2] FROM docker.io/library/tomcat:latest                                                                       0.1s
 => CACHED [warfilebuilder 2/7] RUN dnf install java-1.8.0-openjdk.x86_64  java-1.8.0-openjdk-devel.x86_64  maven git -y     0.0s
 => CACHED [warfilebuilder 3/7] RUN mkdir /ashu-java                                                                         0.0s
 => CACHED [warfilebuilder 4/7] WORKDIR /ashu-java                                                                           0.0s
 => CACHED [warfilebuilder 5/7] RUN git clone https://github.com/redashu/java-springboot.git                                 0.0s
 => CACHED [warfilebuilder 6/7] WORKDIR java-springboot                                                                      0.0s
 => CACHED [warfilebuilder 7/7] RUN mvn clean package                                                                        0.0s
 => [stage-1 2/2] COPY --from=WarfileBuilder /ashu-java/java-springboot/target/WebApp.war /usr/local/tomcat/webapps/         0.0s
 => exporting to image                                                                                                       0.0s
 => => exporting layers                                                                                                      0.0s
 => => writing image sha256:75a6c6a6cb88ae47272617aabf09e5808e32ee7425a6fc3e6ae9fa47f91cf848                                 0.0s
 => => naming to docker.io/library/ashujava:tomcatapp1                                                                       0.0s
[ashu@ip-172-31-29-58 java-springboot]$ docker run -itd --name ashuc1 -p 3001:8080 ashujava:tomcatapp1
daff30e90e76a21b70f4ec0162b05072c4fede7d116be0403016df956459461a
[ashu@ip-172-31-29-58 java-springboot]$ 

```

### pushing this image to container registry of azure 

```
docker images  | grep ashu 
ashujava                                           tomcatapp1      75a6c6a6cb88   19 minutes ago   462MB
ashujava                                           mvnb1           28f2cb1607ee   36 minutes ago   958MB
dockerashu/flaskday4                               appversion1     c343d8b22c17   5 days ago       315MB
registry.hub.docker.com/dockerashu/flaskday4       appversion1     c343d8b22c17   5 days ago       315MB
ashuflask                                          webappv1        dc988b9ac1d0   5 days ago       315MB
dockerashu/flaskday4                               appversion11    29524c8b653e   5 days ago       315MB
registry.hub.docker.com/dockerashu/flaskday4       appversion11    29524c8b653e   5 days ago       315MB
ashutoshh.azurecr.io/ashupython                    flaskappv1      0caa406fd31a   6 days ago       315MB
dockerashu/ashureponew                             appv1           4c2b7fb4d6cd   6 days ago       315MB
dockerashu/ashureponew                             latest          4c2b7fb4d6cd   6 days ago       315MB
dockerashu/unisysflask                             appversion1     4c2b7fb4d6cd   6 days ago       315MB
dockerashu/unisysflask                             latest          4c2b7fb4d6cd   6 days ago       315MB
registry.hub.docker.com/dockerashu/flaskday4       <none>          4c2b7fb4d6cd   6 days ago       315MB
ashu-flask                                         uniappv1        b35355eb36d0   8 days ago       137MB
ashualp                                            pycodev1        77893d4d694e   8 days ago       76.8MB
ashuflask                                          appv2           0baae1267e36   8 days ago       137MB
[ashu@ip-172-31-29-58 java-springboot]$ docker  tag  ashujava:tomcatapp1  ashutoshh.azurecr.io/ashujava:tomcatapp1
[ashu@ip-172-31-29-58 java-springboot]$ docker login ashutoshh.azurecr.io
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@ip-172-31-29-58 java-springboot]$ docker push ashutoshh.azurecr.io/ashujava:tomcatapp1
The push refers to repository [ashutoshh.azurecr.io/ashujava]
bb1ed3edbbba: Pushed 
5f70bf18a086: Pushed 
96b023633e9c: Pushed 

```

### putting java cod to same repo but in a new branch 

```
ls
ashu-java-webapp  ashu-python-webapp  ashu_unisys_flaskMysql  java-springboot  unisys_devsecops
[ashu@ip-172-31-29-58 ashu-devsecops]$ cd unisys_devsecops/
[ashu@ip-172-31-29-58 unisys_devsecops]$ ls
Dockerfile  README.md  alpine.dockerfile  ashu.py  compose.yaml  static  templates
[ashu@ip-172-31-29-58 unisys_devsecops]$ git pull 
Already up to date.
[ashu@ip-172-31-29-58 unisys_devsecops]$ git branch 
* master
[ashu@ip-172-31-29-58 unisys_devsecops]$ git checkout -b springboot
Switched to a new branch 'springboot'
[ashu@ip-172-31-29-58 unisys_devsecops]$ git branch 
  master
* springboot
[ashu@ip-172-31-29-58 unisys_devsecops]$ ls
Dockerfile  README.md  alpine.dockerfile  ashu.py  compose.yaml  static  templates
[ashu@ip-172-31-29-58 unisys_devsecops]$ rm Dockerfile alpine.dockerfile  ashu.py  
[ashu@ip-172-31-29-58 unisys_devsecops]$ ls
README.md  compose.yaml  static  templates
[ashu@ip-172-31-29-58 unisys_devsecops]$ rm -rf static/ templates/
[ashu@ip-172-31-29-58 unisys_devsecops]$ ls
README.md  compose.yaml
[ashu@ip-172-31-29-58 unisys_devsecops]$ 

===>
[ashu@ip-172-31-29-58 unisys_devsecops]$ cd ..
[ashu@ip-172-31-29-58 ashu-devsecops]$ ls
ashu-java-webapp  ashu-python-webapp  ashu_unisys_flaskMysql  java-springboot  unisys_devsecops
[ashu@ip-172-31-29-58 ashu-devsecops]$ ls java-springboot/
Dockerfile  README.md  pom.xml  src
[ashu@ip-172-31-29-58 ashu-devsecops]$ cp -rf java-springboot/Dockerfile  unisys_devsecops/
[ashu@ip-172-31-29-58 ashu-devsecops]$ cp -rf java-springboot/pom.xml  unisys_devsecops/
[ashu@ip-172-31-29-58 ashu-devsecops]$ cp -rf java-springboot/src/ unisys_devsecops/
[ashu@ip-172-31-29-58 ashu-devsecops]$ cd unisys_devsecops/
[ashu@ip-172-31-29-58 unisys_devsecops]$ ls
Dockerfile  README.md  compose.yaml  pom.xml  src
[ashu@ip-172-31-29-58 unisys_devsecops]$ git branch 
  master
* springboot
```

### pushing the changes

```
git branch 
  master
* springboot
[ashu@ip-172-31-29-58 unisys_devsecops]$ git add .
[ashu@ip-172-31-29-58 unisys_devsecops]$ git commit  -m "spring app"
[springboot 27e68d6] spring app
 12 files changed, 159 insertions(+), 192 deletions(-)
 create mode 100644 .dockerignore
 create mode 100644 Dockerfile
 delete mode 100644 ashu.py
 create mode 100644 compose.yaml
 create mode 100644 pom.xml
 create mode 100644 src/main/webapp/WEB-INF/web.xml
 create mode 100644 src/main/webapp/css/jumbotron.css
 create mode 100644 src/main/webapp/index.jsp
 delete mode 100644 static/style.css
 delete mode 100644 templates/about.html
 delete mode 100644 templates/contact.html
 delete mode 100644 templates/index.html
[ashu@ip-172-31-29-58 unisys_devsecops]$ git push origin springboot 
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 4 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (14/14), 3.14 KiB | 3.14 MiB/s, done.
Total 14 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'springboot' on GitHub by visiting:
remote:      https://github.com/redashu/unisys_devsecops/pull/new/springboot
remote: 
To github.com:redashu/unisys_devsecops.git
 * [new branch]      springboot -> springboot

```

### Creating pipeline from jenkinsfile 

```
pipeline {
    agent any 
    stages {
        // create stage 1 for fetching git repo info 
        stage('fetching git repo details'){
            // steps 
            steps {
                echo 'fetching git repo'
                git branch: 'springboot', url:'https://github.com/redashu/unisys_devsecops.git'
                sh 'ls'
            }
            
        }
        // creating second stage for doing SAST analysis for any Critical bugs
        stage('SAST using trivy for Critical vulns'){
            steps {
                echo 'using trivy to scan code pushed by developer'
                sh 'trivy  fs --scanners  vuln,secret,misconfig  .'
            }
        }

    }
}

```

### verify kubectl software in k8s client machine 

```
kubectl version --client 
Client Version: v1.31.0
Kustomize Version: v5.4.2
[ashu@ip-172-31-29-58 unisys_devsecops]$ 


```

### connecting with kubectl to azure kubernetes

```
kubectl get nodes   --kubeconfig /tmp/config 
NAME                                STATUS   ROLES    AGE     VERSION
aks-agentpool-28844960-vmss000002   Ready    <none>   5h44m   v1.29.8
aks-agentpool-28844960-vmss000003   Ready    <none>   5h44m   v1.29.8

```

## copy the creds to home directory 

```
[ashu@ip-172-31-29-58 unisys_devsecops]$ mkdir  ~/.kube
mkdir: cannot create directory ‘/home/ashu/.kube’: File exists
[ashu@ip-172-31-29-58 unisys_devsecops]$ 
[ashu@ip-172-31-29-58 unisys_devsecops]$ cp -v  /tmp/config   ~/.kube/
'/tmp/config' -> '/home/ashu/.kube/config'
[ashu@ip-172-31-29-58 unisys_devsecops]$ 
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl get nodes   
NAME                                STATUS   ROLES    AGE     VERSION
aks-agentpool-28844960-vmss000002   Ready    <none>   5h48m   v1.29.8
aks-agentpool-28844960-vmss000003   Ready    <none>   5h48m   v1.29.8
[ashu@ip-172-31-29-58 unisys_devsecops]$ 

```

### pod1 yaml 

```
apiVersion: v1 
kind: Pod
metadata:
  name: ashupod1  # name of pod
spec: 
  containers:
  - name: ashuc1 
    image: dockerashu/ashujava:tomcatdeploy5
    ports:
    - containerPort: 8080

```

### creating pod 

```
kubectl  create -f pod1.yaml 
pod/ashupod1 created
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl  get pods
NAME       READY   STATUS    RESTARTS   AGE
ashupod1   1/1     Running   0          15s
[ashu@ip-172-31-29-58 unisys_devsecops]$ 

```


## creating pods using deployment 

<img src="dep1.png">

```
kubectl create deployment  ashu-tomcat --image=dockerashu/ashujava:tomcatdeploy5 --port    8080  --dry-run=client -o yaml >deploy1.yaml

===>
kubectl create -f deploy1.yaml 
deployment.apps/ashu-tomcat created
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl   get deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-tomcat   2/2     2            2           5s
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl  get pods
NAME                           READY   STATUS    RESTARTS   AGE
ashu-tomcat-788d99b9b4-64t6g   1/1     Running   0          12s
ashu-tomcat-788d99b9b4-z9fxc   1/1     Running   0          12s
[ashu@ip-172-31-29-58 unisys_devsecops]$ 

```

### creating internal and external LB 

<img src="lb1.png">

### creating service 

```
 kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
ashu-tomcat        2/2     2            2           13m
bb-tomcat-deploy   2/2     2            2           10m
narayan-tomcat     2/2     2            2           3m4s
uday-tomcat        2/2     2            2           9m52s
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl expose deployment ashu-tomcat --type LoadBalancer --port 80 --target-port 8080    --name ashulb1  --dry-run=client -o yaml >service.yaml 
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl  create -f service.yaml 
service/ashulb1 created
[ashu@ip-172-31-29-58 unisys_devsecops]$ kubectl  get service
NAME         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
ashulb1      LoadBalancer   10.0.189.76   <pending>     80:31665/TCP   5s
kubernetes   ClusterIP      10.0.0.1      <none>        443/TCP        5d23h
[ashu@ip-172-31-29-58 unisys_devsecops]$ 

```