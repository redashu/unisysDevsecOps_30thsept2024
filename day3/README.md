## Revision 

<img src="rev1.png">

## for Flask and Mysql app we are creating a new repo  and cloning it to dev server

```
git clone git@github.com:redashu/ashu_unisys_flaskMysql.git
Cloning into 'ashu_unisys_flaskMysql'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (3/3), done.

[ashu@ip-172-31-29-58 ashu-devsecops]$ ls
ashu-java-webapp  ashu-python-webapp  ashu_unisys_flaskMysql  unisys_devsecops
[ashu@ip-172-31-29-58 ashu-devsecops]$ 



```

### after cloning all structure look like 

```
ls
ashu-java-webapp  ashu-python-webapp  ashu_unisys_flaskMysql  unisys_devsecops
[ashu@ip-172-31-29-58 ashu-devsecops]$ 
[ashu@ip-172-31-29-58 ashu-devsecops]$ ls  ashu_unisys_flaskMysql/
Dockerfile  README.md  app.py  compose.yaml  requirements.txt  templates
[ashu@ip-172-31-29-58 ashu-devsecops]$ 
[ashu@ip-172-31-29-58 ashu-devsecops]$ 
[ashu@ip-172-31-29-58 ashu-devsecops]$ tree ashu_unisys_flaskMysql/
ashu_unisys_flaskMysql/
├── Dockerfile
├── README.md
├── app.py
├── compose.yaml
├── requirements.txt
└── templates
    ├── index.html
    └── success.html

1 directory, 7 files

```
