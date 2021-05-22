# 基于Docker安装常用软件

本实验介绍如何基于Docker安装常用的软件，具体包括：

- Ubuntu
- Cetnos
- Nginx
- Node.js
- PHP
- MySQL
- Tomcat
- Redis
- MongoDB
- Apache

在安装Ubuntu的过程中，会详细介绍如何在 Docker hub 上查找所需镜像，其余软件安装时不再介绍此过程。

## 安装 Ubuntu

Ubuntu 是基于 Debian 的 Linux 操作系统。

### 查看可用的 Ubuntu 版本

使用浏览器访问[Docker Hub](https://hub.docker.com/) ，在搜索框中输入Ubuntu进行搜索。

默认是按照 Most Popular排序的，也就是按照下载量排序的。一般来说，下载量越高的镜像，质量也越好。

![image-20210416145607546](\img\Docker基于Docker安装常用软件\image-20210416145607546.png)

可以看到第一条ubuntu的下载量达到了1000万次以上，选择该镜像作为我们要下载的镜像。

单击该镜像图标，打开详细介绍的页面：

![image-20210416145736280](\img\Docker基于Docker安装常用软件\image-20210416145736280.png)

单击拷贝图标可以直接拷贝 docker pull 语句。

Description给出了本镜像的详细介绍信息，Reviewers为用户的评论，Tags为不同版本的镜像。

![image-20210416145821498](\img\Docker基于Docker安装常用软件\image-20210416145821498.png)

点击tags后可以看到不同标识的Ubuntu镜像，默认是按照latest排序的。实际应用中，可根据自己的需求，选择相应标识的镜像。这里我们选择 ubuntu:latest 这个镜像。

### 拉取镜像

使用从网页上拷贝的下载命令下载最新版镜像：

```
[root@localhost ~]# clear
[root@localhost ~]# docker pull ubuntu:latest
latest: Pulling from library/ubuntu
a70d879fa598: Pulling fs layer 
c4394a92d1f8: Downloading 
10e6159c56c0: Downloading 
latest: Pulling from library/ubuntu
a70d879fa598: Pull complete 
c4394a92d1f8: Pull complete 
10e6159c56c0: Pull complete 
Digest: sha256:3c9c713e0979e9bd6061ed52ac1e9e1f246c9495aa063619d9d695fb8039aa1f
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
[root@localhost ~]# 
```

![image-20210416145914785](\img\Docker基于Docker安装常用软件\image-20210416145914785.png)

### 查看本地镜像

使用 docker images 查看是否已安装 ubuntu

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    26b77e58432b   13 days ago   72.9MB
[root@localhost ~]# 
```

在上图中可以看到我们已经安装了 ubuntu 镜像。

![image-20210416145938879](\img\Docker基于Docker安装常用软件\image-20210416145938879.png)

### 运行容器

使用 docker run 运行 Ubuntu 容器

```
[root@localhost ~]# docker run -itd --name ubuntu-test --hostname ubuntu-test ubuntu
6f836cc50671a9684b4873b863eb15c16f80e52f2c891b121a6a506065302699
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
6f836cc50671   ubuntu    "/bin/bash"   12 seconds ago   Up 11 seconds             ubuntu-test
[root@localhost ~]# docker exec -it ubuntu-test /bin/bash
root@ubuntu-test:/# cat /etc/issue 
Ubuntu 20.04.2 LTS \n \l

root@ubuntu-test:/# 
```



![image-20210416150058737](\img\Docker基于Docker安装常用软件\image-20210416150058737.png)

可以看到 Ubuntu 容器已成功启动。使用 docker exec 进入容器后，可以看到当前 Ubuntu 的版本号为 20.04.2 。

## Docker 安装 CentOS

CentOS（Community Enterprise Operating System）是 Linux 发行版之一，它是来自于 Red Hat Enterprise Linux(RHEL) 依照开放源代码规定发布的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以 CentOS 替代商业版的 Red Hat Enterprise Linux 使用。

### 拉取镜像

拉取指定版本的 CentOS 镜像，这里我们安装指定版本为例(centos7):

```
[root@localhost ~]# docker pull centos:centos7
centos7: Pulling from library/centos
2d473b07cdd5: Pulling fs layer 
centos7: Pulling from library/centos
2d473b07cdd5: Pull complete 
Digest: sha256:0f4ec88e21daf75124b8a9e5ca03c37a5e937e0e108a255d890492430789b60e
Status: Downloaded newer image for centos:centos7
docker.io/library/centos:centos7
[root@localhost ~]# 
```

![image-20210416150247030](\img\Docker基于Docker安装常用软件\image-20210416150247030.png)

### 查看本地镜像

使用 docker images 查看是否已安装centos7

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
[root@localhost ~]# 
```

![image-20210416150315948](\img\Docker基于Docker安装常用软件\image-20210416150315948.png)

在上图中可以看到我们已经安装了 centos7 镜像。

### 运行容器

运行 centos 容器

```
[root@localhost ~]# docker run -itd --name centos-test --hostname centos-test centos:centos7
c5f84a55de5ec81f1313b845290aa011fc0d113c173490fc86836e3583eb33fe
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE            COMMAND       CREATED         STATUS         PORTS     NAMES
c5f84a55de5e   centos:centos7   "/bin/bash"   8 seconds ago   Up 7 seconds             centos-test
6f836cc50671   ubuntu           "/bin/bash"   3 minutes ago   Up 3 minutes             ubuntu-test
[root@localhost ~]# docker exec -it centos-test /bin/bash
[root@centos-test /]# 
```

![image-20210416150405071](\img\Docker基于Docker安装常用软件\image-20210416150405071.png)

## Docker 安装 Nginx

Nginx 是一个高性能的 HTTP 和反向代理 web 服务器，同时也提供了 IMAP/POP3/SMTP 服务 。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost ~]# docker pull nginx:latest
latest: Pulling from library/nginx
f7ec5a41d630: Pull complete 
aa1efa14b3bf: Pull complete 
b78b95af9b17: Pull complete 
c7d6bca2b8dc: Pull complete 
cf16cd8e71e0: Pull complete 
0241c68333ef: Pull complete 
Digest: sha256:75a55d33ecc73c2a242450a9f1cc858499d468f077ea942867e662c247b5e412
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
[root@localhost ~]# 
```

![image-20210416150453415](\img\Docker基于Docker安装常用软件\image-20210416150453415.png)

### 查看本地镜像

使用以下命令来查看是否已安装了 nginx：

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
[root@localhost ~]# 
```

![image-20210416150537676](\img\Docker基于Docker安装常用软件\image-20210416150537676.png)

在上图中可以看到我们已经安装了最新版本（latest）的 nginx 镜像。

### 运行容器

安装完成后，我们可以使用以下命令来运行 nginx 容器：

```
[root@localhost ~]# docker run --name nginx-test --hostname nginx-test -p 8080:80 -d nginx
f038b19a17d221dfa266132d7f14d1420bb0397a8e6337705e6410f8cf80c0cd
[root@localhost ~]# 
```

![image-20210416150607092](\img\Docker基于Docker安装常用软件\image-20210416150607092.png)

参数说明：

- --name nginx-test：容器名称。
- -p 8080:80： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口。
- -d nginx： 设置容器在在后台一直运行。

### 测试容器

打开浏览器，输入地址：IP:8080，测试 nginx 服务。

![image-20210416150728490](\img\Docker基于Docker安装常用软件\image-20210416150728490.png)

可见，nginx已正确安装。

## Docker 安装 Node.js

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，是一个让 JavaScript 运行在服务端的开发平台。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost ~]# docker pull node:latest
latest: Pulling from library/node
latest: Pulling from library/node
76b8ef87096f: Pull complete 
2e2bafe8a0f4: Pull complete 
b53ce1fd2746: Pull complete 
84a8c1bd5887: Pull complete 
7a803dc0b40f: Pull complete 
b800e94e7303: Pull complete 
a71a5fdd1ae1: Pull complete 
b308d1251a84: Pull complete 
80886c3e6f4a: Pull complete 
Digest: sha256:6cbc150709d59d2667f5d34cbf03fb4594dc8b34acb8872f9ab27ba915b28b56
Status: Downloaded newer image for node:latest
docker.io/library/node:latest
[root@localhost ~]# 
```

![image-20210416150858604](\img\Docker基于Docker安装常用软件\image-20210416150858604.png)

### 查看本地镜像

使用 docker images 查看是否已安装

```
[root@localhost ~]# docker images 
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
node         latest    d2850632b602   5 days ago     936MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
[root@localhost ~]# 
```

![image-20210416150922549](\img\Docker基于Docker安装常用软件\image-20210416150922549.png)

在上图中可以看到我们已经安装了最新版本（latest）的  镜像。

### 运行容器

```
[root@localhost ~]# docker run -itd --name node-test --hostname --node-test node
5adedf96f114bd90c616e68bf9623fccfa1b698582ade9b8c839de7ed27f2fb5
[root@localhost ~]# docker exec -it node-test /bin/bash
root@--node-test:/# node -v
v15.14.0
root@--node-test:/# exit
exit
[root@localhost ~]# 
```

![image-20210416151007967](\img\Docker基于Docker安装常用软件\image-20210416151007967.png)

通过在容器中运行 node -v 命令，可以看到 node 已正确安装。

## Docker 安装 PHP

PHP 即：超文本预处理器，是一种通用开源脚本语言。PHP是在服务器端执行的脚本语言，与C语言类似，是常用的网站编程语言。 PHP 独特的语法混合了 C 、 Java 、 Perl 以及 PHP 自创的语法。利于学习，使用广泛，主要适用于Web开发领域。

### 拉取镜像

这里我们的版本为：5.6-fpm：

```
[root@localhost ~]# docker pull php:5.6-fpm
5.6-fpm: Pulling from library/php
5e6ec7f28fb7: Downloading 
cf165947b5b7: Download complete 
7bd37682846d: Downloading [========================>                          ]  32.41MB/67.44MB
99daf8e838e1: Downloading 
f8628c9f032f: Download complete 
50ff925cdfa2: Download complete 
6ab76f312877: Download complete 
28ea94b4dd82: Download complete 
a6dbb35d45d2: Download complete 
98b901ec9e8d: Download complete 
5.6-fpm: Pulling from library/php
5e6ec7f28fb7: Pull complete 
cf165947b5b7: Pull complete 
7bd37682846d: Pull complete 
99daf8e838e1: Pull complete 
f8628c9f032f: Pull complete 
50ff925cdfa2: Pull complete 
6ab76f312877: Pull complete 
28ea94b4dd82: Pull complete 
a6dbb35d45d2: Pull complete 
98b901ec9e8d: Pull complete 
Digest: sha256:4f070f1b7b93cc5ab364839b79a5b26f38d5f89461f7bc0cd4bab4a3ad7d67d7
Status: Downloaded newer image for php:5.6-fpm
docker.io/library/php:5.6-fpm
[root@localhost ~]# 
```

![image-20210416154722500](\img\Docker基于Docker安装常用软件\image-20210416154722500.png)

### 查看本地镜像

使用 docker images 查看是否已安装 php 镜像

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
node         latest    d2850632b602   6 days ago     936MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
[root@localhost ~]# 

```

![image-20210416154743825](\img\Docker基于Docker安装常用软件\image-20210416154743825.png)

在上图中可以看到我们已经安装了 php 镜像。

### 运行容器

启动 PHP：

```
[root@localhost ~]# docker run --name myphp-fpm -v ~/nginx/www:/www -d php:5.6-fpm
737d77753a76a1ec1356f5e08a7a47bd4c407d5fb74069b388d4fcb08b79579b
[root@localhost ~]# 
```

![image-20210416155909839](\img\Docker基于Docker安装常用软件\image-20210416155909839.png)

命令说明：

- --name myphp-fpm : 将容器命名为 myphp-fpm。
- -v ~/nginx/www:/www : 将主机中项目的目录 www 挂载到容器的 /www

创建 ~/nginx/conf/conf.d 目录，并在该目录下创建test-php.conf文件，文件内容如下：

```
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm index.php;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ \.php$ {
        fastcgi_pass   php:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /www/$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

配置文件说明：

- php:9000: 表示 php-fpm 服务的 URL，下面我们会具体说明。
- /www/: 是 myphp-fpm 中 php 文件的存储路径，映射到本地的 ~/nginx/www 目录。

上述操作过程如下：

```
[root@localhost ~]# mkdir -p ~/nginx/conf/conf.d
[root@localhost ~]# vi ~/nginx/conf/conf.d/php-test.conf 
[root@localhost ~]# cat ~/nginx/conf/conf.d/php-test.conf 
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm index.php;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ \.php$ {
        fastcgi_pass   php:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /www/$fastcgi_script_name;
        include        fastcgi_params;
    }
}
[root@localhost ~]# 
```

![image-20210416155942454](\img\Docker基于Docker安装常用软件\image-20210416155942454.png)

启动 nginx：

```
[root@localhost ~]# docker run --name php-nginx -p 8083:80 -d -v ~/nginx/www:/usr/share/nginx/html:ro -v ~/nginx/conf/conf.d:/etc/nginx/conf.d:ro --link myphp-fpm:php nginx
f72862ef311b72e4f38f382416949a1ae20ea593cc044df72492543e4b97398d
[root@localhost ~]# 
```

![image-20210416160015586](\img\Docker基于Docker安装常用软件\image-20210416160015586.png)

- -p 8083:80: 端口映射，把 nginx 中的 80 映射到本地的 8083 端口。
- ~/nginx/www: 是本地 html 文件的存储目录，/usr/share/nginx/html 是容器内 html 文件的存储目录。
- ~/nginx/conf/conf.d: 是本地 nginx 配置文件的存储目录，/etc/nginx/conf.d 是容器内 nginx 配置文件的存储目录。
- --link myphp-fpm:php: 把 myphp-fpm 的网络并入 nginx，并通过修改 nginx 的 /etc/hosts，把域名 php 映射成 127.0.0.1，让 nginx 通过 php:9000 访问 php-fpm。

接下来我们在 ~/nginx/www 目录下创建 index.php，代码如下：

```
[root@localhost ~]# vi ~/nginx/www/index.php 
[root@localhost ~]# cat ~/nginx/www/index.php
<?php
echo phpinfo();
?>
[root@localhost ~]# 
```

![image-20210416161721757](\img\Docker基于Docker安装常用软件\image-20210416161721757.png)

浏览器打开IP:8083以看到，已正确输出 php 的相关配置信息。

![image-20210416161747251](\img\Docker基于Docker安装常用软件\image-20210416161747251.png)

可以看到，已正确输出 php 的相关配置信息。

## Docker 安装 MySQL

MySQL 是世界上最受欢迎的开源数据库。凭借其可靠性、易用性和性能，MySQL 已成为 Web 应用程序的数据库优先选择。

### 拉取镜像

这里我们拉取官方的最新版本的 mysql 镜像：

```
[root@localhost ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
f7ec5a41d630: Already exists 
9444bb562699: Pull complete 
6a4207b96940: Pull complete 
181cefd361ce: Pull complete 
8a2090759d8a: Pull complete 
15f235e0d7ee: Pull complete 
d870539cd9db: Pull complete 
5726073179b6: Pull complete 
eadfac8b2520: Pull complete 
f5936a8c3f2b: Pull complete 
cca8ee89e625: Pull complete 
6c79df02586a: Pull complete 
Digest: sha256:6e0014cdd88092545557dee5e9eb7e1a3c84c9a14ad2418d5f2231e930967a38
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
[root@localhost ~]# 
```

![image-20210416161948489](\img\Docker基于Docker安装常用软件\image-20210416161948489.png)

### 查看本地镜像

使用 docker images 查看是否已安装 mysql 镜像

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
[root@localhost ~]# 
```

![image-20210416162013939](\img\Docker基于Docker安装常用软件\image-20210416162013939.png)

在上图中可以看到我们已经安装了最新版本（latest）的 mysql 镜像。

### 运行容器

安装完成后，可以使用以下命令来运行 mysql 容器：

```
[root@localhost ~]# docker run -itd --name mysql-test --hostname mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
8a92857ce1b3f56fc36e505f6583d96fd076498bbc67e47961f635fd8a501e73
[root@localhost ~]# 
```

![image-20210416162122969](\img\Docker基于Docker安装常用软件\image-20210416162122969.png)

参数说明：

- -p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
- MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。

通过 docker ps 命令查看是否安装成功：

```
[root@localhost ~]# docker ps -l
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                               NAMES
8a92857ce1b3   mysql     "docker-entrypoint.s…"   27 seconds ago   Up 26 seconds   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
[root@localhost ~]# 
```

本机可以通过 root 和密码 123456 访问 MySQL 服务。

```
[root@localhost ~]# docker exec -it mysql-test /bin/bash
root@mysql-test:/# mysql -h localhost -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.23 MySQL Community Server - GPL

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> 
```

![image-20210416162252069](\img\Docker基于Docker安装常用软件\image-20210416162252069.png)

由上图可知，MySQL已正确安装。

注意： 使用 quit 命令可退出 MySQL 。

## Docker 安装 Tomcat

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

### 拉取镜像

这里我们拉取官方的最新版本的 tomcat 镜像：

```
[root@localhost ~]# docker pull tomcat
Using default tag: latest
latest: Pulling from library/tomcat
bd8f6a7501cc: Pull complete 
44718e6d535d: Pull complete 
efe9738af0cb: Pull complete 
f37aabde37b8: Pull complete 
b87fc504233c: Pull complete 
8bf93eef8c9e: Pull complete 
a62c27841e77: Pull complete 
3b23560b24c9: Pull complete 
168537fce8fb: Pull complete 
6643b79f9364: Pull complete 
Digest: sha256:a655be865e9f62d6d2ed3823c7382a2d77d0a034eb17714bbf2a514c3f620717
Status: Downloaded newer image for tomcat:latest
docker.io/library/tomcat:latest
[root@localhost ~]# 
```

![image-20210416165556278](\img\Docker基于Docker安装常用软件\image-20210416165556278.png)

### 查看本地镜像

使用 docker images 查看 tomcat 是否已安装

```
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
tomcat       latest    bd431ca8553c   5 days ago     667MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
[root@localhost ~]# 
```

![image-20210416165814266](\img\Docker基于Docker安装常用软件\image-20210416165814266.png)

在上图中可以看到我们已经安装了最新版本（latest）的 tomcat 镜像。

### 运行容器

首先进入目录：/mnt/docker/tomcat/myapp/，在该目录下创建文件：index.html，在该文件中填入如下内容：


```
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>
```

然后启动tomcat，并挂载 /mnt/docker/tomcat/myapp/ 到 tomcat 的 webapps 目录。

```
[root@localhost ~]# cd /mnt/docker/tomcat/myapp/
[root@localhost myapp]# vi index.html
[root@localhost myapp]# cat index.html 
<html>
<body>
<h2>Hello World!</h2>
</body>
</html>
[root@localhost myapp]# docker run --name tomcat-test --hostname tomcat-test -p 8080:8080 -v /mnt/docker/tomcat/myapp:/usr/local/tomcat/webapps/myapp -d tomcat  
90d7b61021e89e222bd7eca2c62bdaa8d94cab70028b62bc6983092956756537
[root@localhost myapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND             CREATED         STATUS         PORTS                    NAMES
90d7b61021e8   tomcat    "catalina.sh run"   8 seconds ago   Up 7 seconds   0.0.0.0:8080->8080/tcp   tomcat-test
[root@localhost myapp]# 

```

![image-20210416170242693](\img\Docker基于Docker安装常用软件\image-20210416170242693.png)

通过 docker ps 命令可以看到 tomcat 容器已顺利启动。

打开 chrome 浏览器， 访问地址： IP:8080/myapp，可打开刚才编辑的 index.html 。

效果如下：

![image-20210416170307135](\img\Docker基于Docker安装常用软件\image-20210416170307135.png)

可见， tomcat 已正确配置。

## Docker 安装 Python

Python是一种跨平台的计算机程序设计语言。是一种面向对象的动态类型语言，最初被设计用于编写自动化脚本(shell)，随着版本的不断更新和语言新功能的添加，越多被用于独立的、大型项目的开发。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost myapp]# docker pull python:3.6.5
3.6.5: Pulling from library/python
0bd44ff9c2cf: Pulling fs layer 
047670ddbd2a: Downloading 
ea7d5dc89438: Downloading 
ae7ad5906a75: Downloading 
0f2ddfdfc7d1: Downloading 
d055f4d7ae62: Waiting 
c501289d05b9: Downloading 
211aaca0a156: Waiting 
a2d4f20d1579: Waiting 
3.6.5: Pulling from library/python
0bd44ff9c2cf: Pull complete 
047670ddbd2a: Pull complete 
ea7d5dc89438: Pull complete 
ae7ad5906a75: Pull complete 
0f2ddfdfc7d1: Pull complete 
d055f4d7ae62: Pull complete 
c501289d05b9: Pull complete 
211aaca0a156: Pull complete 
a2d4f20d1579: Pull complete 
Digest: sha256:c49ab7d5121521de57653c7209e68102d057ed77aff9859e8a9603b36105911a
Status: Downloaded newer image for python:3.6.5
docker.io/library/python:3.6.5
[root@localhost myapp]# 
```

![image-20210416170628127](\img\Docker基于Docker安装常用软件\image-20210416170628127.png)

### 查看本地镜像

使用 docker images 查看 python 镜像是否已安装

```
[root@localhost myapp]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
tomcat       latest    bd431ca8553c   5 days ago     667MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
python       3.6.5     9a58cce9b09f   2 years ago    912MB
[root@localhost myapp]# 
```

![image-20210416170651860](\img\Docker基于Docker安装常用软件\image-20210416170651860.png)

在上图中可以看到我们已经安装了最新版本（latest）的 python 镜像。

### 运行容器

首先创建目录：/mnt/docker/python/myapp，在该目录下创建一个Python程序文件：hello_world.py。

hello_world.py的文件内容如下：


```
#!/usr/bin/python

print("Hello, World!")
```


```
[root@localhost ~]# mkdir -p /mnt/docker/python/myapp
[root@localhost ~]# cd /mnt/docker/python/myapp
[root@localhost myapp]# vi hello_world.py 
[root@localhost myapp]# cat hello_world.py 
#!/usr/bin/python

print("Hello, World!")
[root@localhost myapp]# 
```

![image-20210416170903591](\img\Docker基于Docker安装常用软件\image-20210416170903591.png)

然后启动 Python 容器并运行该文件，

```
[root@localhost myapp]# docker run -v $PWD:/usr/src/myapp -w /usr/src/myapp python:3.6.5 python hello_world.py
Hello, World!
[root@localhost myapp]# 
```

![image-20210416170929110](\img\Docker基于Docker安装常用软件\image-20210416170929110.png)

命令说明：
- -v $PWD:/usr/src/myapp: 将主机中当前目录挂载到容器的 /usr/src/myapp。
- -w /usr/src/myapp: 指定容器的 /usr/src/myapp 目录为工作目录。
- python hello_world.py: 使用容器的 python 命令来执行工作目录中的 hello_world.py 文件。

可以看到，程序已正确执行，说明 Python 已正确配置。

## Docker 安装 Redis

Redis 是一个开源的使用 ANSI C 语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value 的 NoSQL 数据库，并提供多种语言的 API。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost myapp]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
f7ec5a41d630: Already exists 
a36224ca8bbd: Pull complete 
7630ad34dcb2: Pull complete 
dd0ea236b03b: Pull complete 
ed6ed4f2f5a6: Pull complete 
8788804112c6: Pull complete 
Digest: sha256:08e282682a708eb7f51b473516be222fff0251cdee5ef8f99f4441a795c335b6
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
[root@localhost myapp]# 
```

![image-20210416171027180](\img\Docker基于Docker安装常用软件\image-20210416171027180.png)

### 查看本地镜像

使用 docker images 查看 reids 是否已安装

```
[root@localhost myapp]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
tomcat       latest    bd431ca8553c   5 days ago     667MB
redis        latest    de974760ddb2   5 days ago     105MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
python       3.6.5     9a58cce9b09f   2 years ago    912MB
[root@localhost myapp]# 
```

![image-20210416171109682](\img\Docker基于Docker安装常用软件\image-20210416171109682.png)

在上图中可以看到我们已经安装了最新版本（latest）的 redis 镜像。

### 运行容器

安装完成后，我们可以使用以下命令来运行 redis 容器：

```
[root@localhost myapp]# docker run -itd --name redis-test --hostname redis-test -p 6379:6379 redis
8584b00b464a371636d2c1cf7400bf4d2904f08f1f6bc1e390ea9d5b049bc005
[root@localhost myapp]# 
```

![image-20210416171200447](\img\Docker基于Docker安装常用软件\image-20210416171200447.png)

参数说明：
- -p 6379:6379：映射容器服务的 6379 端口到宿主机的 6379 端口。外部可以直接通过宿主机ip:6379 访问到 Redis 的服务。

最后，可以通过 docker ps 命令查看容器的运行信息，通过 redis-cli 连接测试使用 redis 服务。

```
[root@localhost myapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                    NAMES
8584b00b464a   redis     "docker-entrypoint.s…"   28 seconds ago   Up 26 seconds   0.0.0.0:6379->6379/tcp   redis-test
[root@localhost myapp]# docker exec -it redis-test /bin/bash
root@redis-test:/data# redis-cli
127.0.0.1:6379> set test 1
OK
127.0.0.1:6379> 
```

![image-20210416171248979](\img\Docker基于Docker安装常用软件\image-20210416171248979.png)

注意：使用 exit 命令可退出 redis cli 。

## Docker 安装 MongoDB

MongoDB 是一个免费的开源跨平台面向文档的 NoSQL 数据库程序。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost myapp]# docker pull mongo
Using default tag: latest
latest: Pulling from library/mongo
6e0aa5e7af40: Downloading 
d47239a868b3: Download complete 
49cbb10cca85: Download complete 
9729d7ec22de: Download complete 
7b7fd72268d8: Download complete 
5e2934dacaf5: Download complete 
bf9da24d4b2c: Download complete 
d2f8c3715616: Download complete 
e9f96a4a45b0: Download complete 
bd66718f31e2: Download complete 
41ed4d1a1542: Download complete 
7336dfc228e2: Download complete 
latest: Pulling from library/mongo
6e0aa5e7af40: Pull complete 
d47239a868b3: Pull complete 
49cbb10cca85: Pull complete 
9729d7ec22de: Pull complete 
7b7fd72268d8: Pull complete 
5e2934dacaf5: Pull complete 
bf9da24d4b2c: Pull complete 
d2f8c3715616: Pull complete 
e9f96a4a45b0: Pull complete 
bd66718f31e2: Pull complete 
41ed4d1a1542: Pull complete 
7336dfc228e2: Pull complete 
Digest: sha256:b66f48968d757262e5c29979e6aa3af944d4ef166314146e1b3a788f0d191ac3
Status: Downloaded newer image for mongo:latest
docker.io/library/mongo:latest
[root@localhost myapp]# 
```

![image-20210416171646496](\img\Docker基于Docker安装常用软件\image-20210416171646496.png)

### 查看本地镜像

使用 docker images 查看 mongo 是否已安装

```
[root@localhost myapp]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
tomcat       latest    bd431ca8553c   5 days ago     667MB
redis        latest    de974760ddb2   5 days ago     105MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
mongo        latest    30b3be246e39   7 days ago     449MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
python       3.6.5     9a58cce9b09f   2 years ago    912MB
[root@localhost myapp]# 
```

![image-20210416171706539](\img\Docker基于Docker安装常用软件\image-20210416171706539.png)

在上图中可以看到我们已经安装了最新版本（latest）的 mongo 镜像。

### 运行容器

安装完成后，我们可以使用以下命令来运行 mongo 容器：

```
[root@localhost myapp]# docker run -itd --name mongo-test --hostname mongo-test -p 27017:27017 mongo --auth
9cea671091e5a8cb98d9a254fced910638351cb626f261713b1bef363d63d419
[root@localhost myapp]# 
```

![image-20210416171741750](\img\Docker基于Docker安装常用软件\image-20210416171741750.png)

参数说明：
- -p 27017:27017 ：映射容器服务的 27017 端口到宿主机的 27017 端口。外部可以直接通过 宿主机 ip:27017 访问到 mongo 的服务。
- --auth：需要密码才能访问容器服务。

可以通过 docker ps 命令查看容器的运行信息：

```
[root@localhost myapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                      NAMES
9cea671091e5   mongo     "docker-entrypoint.s…"   24 seconds ago   Up 23 seconds   0.0.0.0:27017->27017/tcp   mongo-test
[root@localhost myapp]# 
```

可以看到 mongo-test 容器处于正在运行的状态。

接着使用以下命令添加用户和设置密码，并且尝试连接。

```
[root@localhost myapp]# docker exec -it mongo-test mongo admin
MongoDB shell version v4.4.5
connecting to: mongodb://127.0.0.1:27017/admin?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("deb88e16-ca76-475e-9fff-df65562a3acf") }
MongoDB server version: 4.4.5
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
	https://community.mongodb.com
> db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});
Successfully added user: {
	"user" : "admin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
> Successfully added user: {
... "user" : "admin",
... "roles" : [
... {
... "role" : "userAdminAnyDatabase",
... "db" : "admin"
... }
... ]
... }
uncaught exception: SyntaxError: unexpected token: identifier :
@(shell):1:13
> db.auth('admin', '123456')
1
> 
```

![image-20210416171935026](\img\Docker基于Docker安装常用软件\image-20210416171935026.png)

可以看到mongo命令已正确执行。

## Docker 安装 Apache

Apache是世界使用排名第一的Web服务器软件。它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩充，将Perl/Python等解释器编译到服务器中。同时Apache音译为阿帕奇，是北美印第安人的一个部落，叫阿帕奇族，在美国的西南部。也是一个基金会的名称、一种武装直升机等等。

### 拉取镜像

这里我们拉取官方的最新版本的镜像：

```
[root@localhost myapp]# docker pull httpd
Using default tag: latest
latest: Pulling from library/httpd
f7ec5a41d630: Already exists 
d1589b6d8645: Pull complete 
83d3755a8d28: Pull complete 
f8459b08e404: Pull complete 
30fabbf5a067: Pull complete 
Digest: sha256:a6e472ad921c93d9fc2cbe2ff07560b9a526c145c4e10faff3aeb28c48cce585
Status: Downloaded newer image for httpd:latest
docker.io/library/httpd:latest
[root@localhost myapp]# 
```

![image-20210416172014106](\img\Docker基于Docker安装常用软件\image-20210416172014106.png)

### 查看本地镜像

使用 docker images 查看 apache 是否已安装

```
[root@localhost myapp]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    62d49f9bab67   2 days ago     133MB
tomcat       latest    bd431ca8553c   5 days ago     667MB
redis        latest    de974760ddb2   5 days ago     105MB
httpd        latest    0b932df43057   5 days ago     138MB
node         latest    d2850632b602   6 days ago     936MB
mysql        latest    cbe8815cbea8   6 days ago     546MB
mongo        latest    30b3be246e39   7 days ago     449MB
ubuntu       latest    26b77e58432b   13 days ago    72.9MB
centos       centos7   8652b9f0cb4c   5 months ago   204MB
php          5.6-fpm   3458979c7744   2 years ago    344MB
python       3.6.5     9a58cce9b09f   2 years ago    912MB
[root@localhost myapp]# 
```

![image-20210416172039214](\img\Docker基于Docker安装常用软件\image-20210416172039214.png)

在上图中可以看到我们已经安装了最新版本（latest）的 apache 镜像。

### 运行容器

```
[root@localhost myapp]# docker run -p 80:80 -d httpd
12c5903d0c65235affbd4441ab273ea919311fcce206d299be3b6059935bcfaa
[root@localhost myapp]# 
```
![image-20210416172128196](\img\Docker基于Docker安装常用软件\image-20210416172128196.png)

命令说明：
- -p 80:80: 将容器的 80 端口映射到主机的 80 端口。

通过 docker ps 查看容器运行情况，

```
[root@localhost myapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS                NAMES
12c5903d0c65   httpd     "httpd-foreground"   28 seconds ago   Up 26 seconds   0.0.0.0:80->80/tcp   modest_hypatia
[root@localhost myapp]# 
```

![image-20210416172152854](\img\Docker基于Docker安装常用软件\image-20210416172152854.png)

可见，httpd 容器已顺利启动。

通过浏览器进一步验证，地址栏输入：IP，

![image-20210416172211609](\img\Docker基于Docker安装常用软件\image-20210416172211609.png)

可以看到 apache 配置正确输出的 It works! 提示。

这表明， apache 已正确安装。