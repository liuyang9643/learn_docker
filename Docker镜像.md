## 准备实验环境
打开终端，执行以下命令，拉取所需的镜像。

```
docker pull ubuntu
docker pull ubuntu:16.04
docker pull ubuntu:15.10
docker pull hello-world
docker pull training/webapp
```

---

## 列出镜像列表
可以使用 docker images 来列出本地主机上的镜像。

```
[root@bogon ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED       SIZE
ubuntu            latest    26b77e58432b   10 days ago   72.9MB
ubuntu            16.04     f6f49faac5cf   2 weeks ago   132MB
hello-world       latest    d1165f221234   5 weeks ago   13.3kB
ubuntu            15.10     9b9cb95443b5   4 years ago   137MB
training/webapp   latest    6fae60ef3446   5 years ago   349MB
[root@bogon ~]# 

```

![image-20210413154411789](\img\Docker镜像\image-20210413154411789.png)

各个选项说明:
- REPOSITORY：镜像的仓库源
- TAG：镜像的标签
- IMAGE ID：镜像ID
- CREATED：镜像创建时间
- SIZE：镜像大小

同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，如 ubuntu 仓库源里，有 latest、16.04、15.10 等多个不同的版本，通过使用 REPOSITORY:TAG 来定义不同的镜像。

所以，如果要使用版本为16.04的ubuntu系统镜像来运行容器时，命令如下：

```bash
[root@bogon ~]# docker run -i -t ubuntu:16.04 /bin/bash
root@72b83bda899c:/# exit
exit
[root@bogon ~]# 
```

![image-20210413154540492](\img\Docker镜像\image-20210413154540492.png)

参数说明：
- -i: 交互式操作。
- -t: 终端。
- ubuntu:16.04: 这是指用 ubuntu 16.04 版本镜像为基础来启动容器。
- /bin/bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。
如果要使用版本为 15.10 的 ubuntu 系统镜像来运行容器时，命令如下：

```
[root@bogon ~]# docker run -i -t ubuntu:15.10 /bin/bash
root@f01a76d9b2db:/# exit
exit
[root@bogon ~]# 
```

![image-20210413154613985](\img\Docker镜像\image-20210413154613985.png)

如果不指定镜像的版本标签，例如只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像。

---

## 获取一个新的镜像
当在本地主机上使用一个不存在的镜像时, Docker会自动下载这个镜像。

使用 docker pull 命令可预先下载这个镜像。

```
[root@bogon ~]# docker pull ubuntu:14.04
14.04: Pulling from library/ubuntu
2e6e20c8e2e6: Pull complete 
0551a797c01d: Pull complete 
512123a864da: Pull complete 
Digest: sha256:4a8a6fa8810a3e01352981b35165b0b28403fe2a4e2535e315b23b4a69cd130a
Status: Downloaded newer image for ubuntu:14.04
docker.io/library/ubuntu:14.04
[root@bogon ~]# docker run -i -t ubuntu:14.04 /bin/bash
root@f841ddedc1f5:/# exit
exit
[root@bogon ~]# 
```

![image-20210413154905855](\img\Docker镜像\image-20210413154905855.png)

下载完成后，可直接使用该镜像运行容器。

---

## 查找镜像

如果需要某个镜像时，可以从 Docker Hub 网站搜索镜像。

Docker Hub 网址为： https://hub.docker.com/

也可以使用 docker search 命令搜索镜像。比如需要一个 httpd 镜像来用于构建 web 服务。可通过 docker search 搜索 httpd ，从而找到适合的镜像。

```
[root@bogon ~]# docker search httpd
NAME                                    DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
httpd                                   The Apache HTTP Server Project                  3454      [OK]       
centos/httpd-24-centos7                 Platform for running Apache httpd 2.4 or bui¡­   39                   
centos/httpd                                                                            33                   [OK]
polinux/httpd-php                       Apache with PHP in Docker (Supervisor, CentO¡­   4                    [OK]
salim1983hoop/httpd24                   Dockerfile running apache config                2                    [OK]
publici/httpd                           httpd:latest                                    1                    [OK]
manageiq/httpd                          Container with httpd, built on CentOS for Ma¡­   1                    [OK]
solsson/httpd-openidc                   mod_auth_openidc on official httpd image, ve¡­   1                    [OK]
inanimate/httpd-ssl                     A play container with httpd, ssl enabled, an¡­   1                    [OK]
hypoport/httpd-cgi                      httpd-cgi                                       1                    [OK]
dockerpinata/httpd                                                                      1                    
dariko/httpd-rproxy-ldap                Apache httpd reverse proxy with LDAP authent¡­   1                    [OK]
clearlinux/httpd                        httpd HyperText Transfer Protocol (HTTP) ser¡­   1                    
lead4good/httpd-fpm                     httpd server which connects via fcgi proxy h¡­   1                    [OK]
jonathanheilmann/httpd-alpine-rewrite   httpd:alpine with enabled mod_rewrite           1                    [OK]
appertly/httpd                          Customized Apache HTTPD that uses a PHP-FPM ¡­   0                    [OK]
amd64/httpd                             The Apache HTTP Server Project                  0                    
centos/httpd-24-centos8                                                                 0                    
interlutions/httpd                      httpd docker image with debian-based config ¡­   0                    [OK]
manasip/httpd                                                                           0                    
manageiq/httpd_configmap_generator      Httpd Configmap Generator                       0                    [OK]
trollin/httpd                                                                           0                    
ysli/httpd                              Httpd for DeepWeb                               0                    [OK]
itsziget/httpd24                        Extended HTTPD Docker image based on the off¡­   0                    [OK]
e2eteam/httpd                                                                           0                    
[root@bogon ~]# 
```

![image-20210413154958710](\img\Docker镜像\image-20210413154958710.png)

其中，各字段含义如下：

- NAME: 镜像仓库源的名称
- DESCRIPTION: 镜像的描述
- OFFICIAL: 是否 docker 官方发布
- stars: 类似 Github 里面的 star，表示点赞、喜欢的意思。
- AUTOMATED: 自动构建。

---

## 拖取镜像

使用上图中的 httpd 官方版本的镜像，使用命令 docker pull 来下载该镜像。

```
[root@bogon ~]# docker pull httpd
Using default tag: latest
latest: Pulling from library/httpd
f7ec5a41d630: Pull complete 
d1589b6d8645: Pull complete 
83d3755a8d28: Pull complete 
f8459b08e404: Pull complete 
30fabbf5a067: Pull complete 
Digest: sha256:a6e472ad921c93d9fc2cbe2ff07560b9a526c145c4e10faff3aeb28c48cce585
Status: Downloaded newer image for httpd:latest
docker.io/library/httpd:latest
[root@bogon ~]# 
```

![image-20210413155102890](\img\Docker镜像\image-20210413155102890.png)

下载完成后，就可以使用该镜像了。

```
[root@bogon ~]# docker run -itd -P --name httpd_test httpd
eb07c2bcd56fa89634c1138a2c88b45d913f9dee66c871225a2f4222ed3cd8e5
[root@bogon ~]# docker ps -l
CONTAINER ID   IMAGE     COMMAND              CREATED         STATUS         PORTS                   NAMES
eb07c2bcd56f   httpd     "httpd-foreground"   6 seconds ago   Up 5 seconds   0.0.0.0:49153->80/tcp   httpd_test
[root@bogon ~]# 
```

![image-20210413155129147](\img\Docker镜像\image-20210413155129147.png)

使用 docker run 启动 httpd 容器，通过 docker ps 可查看到 httpd 容器的 80 端口已映射给宿主机的 49153 端口。

双击浏览器，在地址栏中输入：192.168.20.130(本机IP):32769，显示页面如下：

![image-20210413155238382](\img\Docker镜像\image-20210413155238382.png)

因此，httpd服务已顺利启动。

---

## 删除镜像

镜像删除使用 docker rmi 命令，比如我们删除 hello-world 镜像：

```
[root@bogon ~]# docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726
Deleted: sha256:f22b99068db93900abe17f7f5e09ec775c2826ecfe9db961fea68293744144bd
[root@bogon ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED       SIZE
httpd             latest    0b932df43057   2 days ago    138MB
ubuntu            latest    26b77e58432b   10 days ago   72.9MB
ubuntu            16.04     f6f49faac5cf   2 weeks ago   132MB
ubuntu            14.04     13b66b487594   2 weeks ago   197MB
ubuntu            15.10     9b9cb95443b5   4 years ago   137MB
training/webapp   latest    6fae60ef3446   5 years ago   349MB
[root@bogon ~]# 
```

![image-20210413155541567](\img\Docker镜像\image-20210413155541567.png)

从 docker images 命令的返回结果可看出该容器已被删除。

---

## 创建镜像
当 docker 镜像仓库中的镜像不能满足需求时，可通过以下两种方式对镜像进行更改。

- 从已经创建的容器中更新镜像，并且提交这个镜像。
- 使用 Dockerfile 指令来创建一个新的镜像。

### 更新镜像

假如现在需要在 ubuntu 16.04 镜像中使用 gcc 编译 C 源文件。

使用 ubuntu 16.04 创建一个新容器，在该容器内运行 gcc -v 可查看 gcc 是否安装。

```
[root@bogon ~]# docker run -i -t ubuntu:16.04 /bin/bash
root@4842474755a1:/# gcc -v
bash: gcc: command not found
root@4842474755a1:/# 
```

![image-20210413155658354](\img\Docker镜像\image-20210413155658354.png)

可见，当前 gcc 并未安装。

在容器4842474755a1内使用 `apt-get update` 命令进行更新，然后使用 `apt-get install gcc` 安装 gcc 。

```
root@4842474755a1:/# apt-get update
Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [109 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
Get:3 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [2002 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [109 kB]          
Get:5 http://archive.ubuntu.com/ubuntu xenial-backports InRelease [107 kB]        
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 Packages [1558 kB]       
Get:7 http://archive.ubuntu.com/ubuntu xenial/restricted amd64 Packages [14.1 kB]
Get:8 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages [9827 kB]
Get:9 http://archive.ubuntu.com/ubuntu xenial/multiverse amd64 Packages [176 kB]          
Get:10 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [2524 kB]      
Get:11 http://archive.ubuntu.com/ubuntu xenial-updates/restricted amd64 Packages [16.4 kB]  
Get:12 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [1540 kB]    
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/multiverse amd64 Packages [26.2 kB]                                                                       
Get:14 http://archive.ubuntu.com/ubuntu xenial-backports/main amd64 Packages [10.9 kB]                                                                           
Get:15 http://archive.ubuntu.com/ubuntu xenial-backports/universe amd64 Packages [12.6 kB]                                                                       
Get:16 http://security.ubuntu.com/ubuntu xenial-security/restricted amd64 Packages [15.9 kB]                                                                     
Get:17 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [984 kB]                                                                        
Get:18 http://security.ubuntu.com/ubuntu xenial-security/multiverse amd64 Packages [8820 B]                                                                      
Fetched 19.3 MB in 16s (1193 kB/s)                                                                                                                               
Reading package lists... Done
root@4842474755a1:/# apt-get install gcc 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  binutils cpp cpp-5 gcc-5 libasan2 libatomic1 libc-dev-bin libc6-dev libcc1-0 libcilkrts5 libgcc-5-dev libgmp10 libgomp1 libisl15 libitm1 liblsan0 libmpc3
  libmpfr4 libmpx0 libquadmath0 libtsan0 libubsan0 linux-libc-dev manpages manpages-dev
Suggested packages:
  binutils-doc cpp-doc gcc-5-locales gcc-multilib make autoconf automake libtool flex bison gdb gcc-doc gcc-5-multilib gcc-5-doc libgcc1-dbg libgomp1-dbg
  libitm1-dbg libatomic1-dbg libasan2-dbg liblsan0-dbg libtsan0-dbg libubsan0-dbg libcilkrts5-dbg libmpx0-dbg libquadmath0-dbg glibc-doc man-browser
The following NEW packages will be installed:
  binutils cpp cpp-5 gcc gcc-5 libasan2 libatomic1 libc-dev-bin libc6-dev libcc1-0 libcilkrts5 libgcc-5-dev libgmp10 libgomp1 libisl15 libitm1 liblsan0 libmpc3
  libmpfr4 libmpx0 libquadmath0 libtsan0 libubsan0 linux-libc-dev manpages manpages-dev
0 upgraded, 26 newly installed, 0 to remove and 0 not upgraded.
Need to get 29.1 MB of archives.
After this operation, 103 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu xenial/main amd64 libgmp10 amd64 2:6.1.0+dfsg-2 [240 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial/main amd64 libmpfr4 amd64 3.1.4-1 [191 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial/main amd64 libmpc3 amd64 1.0.3-1 [39.7 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial/main amd64 manpages all 4.04-2 [1087 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 binutils amd64 2.26.1-1ubuntu1~16.04.8 [2312 kB]                                                
Get:6 http://archive.ubuntu.com/ubuntu xenial/main amd64 libisl15 amd64 0.16.1-1 [524 kB]                                                                        
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 cpp-5 amd64 5.4.0-6ubuntu1~16.04.12 [7783 kB]                                                   
Get:8 http://archive.ubuntu.com/ubuntu xenial/main amd64 cpp amd64 4:5.3.1-1ubuntu1 [27.7 kB]                                                                    
Get:9 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libcc1-0 amd64 5.4.0-6ubuntu1~16.04.12 [38.8 kB]                                                
Get:10 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgomp1 amd64 5.4.0-6ubuntu1~16.04.12 [55.2 kB]                                               
Get:11 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libitm1 amd64 5.4.0-6ubuntu1~16.04.12 [27.4 kB]                                                
Get:12 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libatomic1 amd64 5.4.0-6ubuntu1~16.04.12 [8892 B]                                              
Get:13 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libasan2 amd64 5.4.0-6ubuntu1~16.04.12 [265 kB]                                                
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 liblsan0 amd64 5.4.0-6ubuntu1~16.04.12 [105 kB]                                                
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libtsan0 amd64 5.4.0-6ubuntu1~16.04.12 [244 kB]                                                
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libubsan0 amd64 5.4.0-6ubuntu1~16.04.12 [95.3 kB]                                              
Get:17 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libcilkrts5 amd64 5.4.0-6ubuntu1~16.04.12 [40.0 kB]                                            
Get:18 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libmpx0 amd64 5.4.0-6ubuntu1~16.04.12 [9762 B]                                                 
Get:19 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libquadmath0 amd64 5.4.0-6ubuntu1~16.04.12 [131 kB]                                            
Get:20 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libgcc-5-dev amd64 5.4.0-6ubuntu1~16.04.12 [2239 kB]                                           
Get:21 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 gcc-5 amd64 5.4.0-6ubuntu1~16.04.12 [8612 kB]                                                  
Get:22 http://archive.ubuntu.com/ubuntu xenial/main amd64 gcc amd64 4:5.3.1-1ubuntu1 [5244 B]                                                                    
Get:23 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libc-dev-bin amd64 2.23-0ubuntu11.2 [68.8 kB]                                                  
Get:24 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 linux-libc-dev amd64 4.4.0-208.240 [836 kB]                                                    
Get:25 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libc6-dev amd64 2.23-0ubuntu11.2 [2083 kB]                                                     
Get:26 http://archive.ubuntu.com/ubuntu xenial/main amd64 manpages-dev all 4.04-2 [2048 kB]                                                                      
Fetched 29.1 MB in 2min 50s (171 kB/s)                                                                                                                           
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libgmp10:amd64.
(Reading database ... 4780 files and directories currently installed.)
Preparing to unpack .../libgmp10_2%3a6.1.0+dfsg-2_amd64.deb ...
Unpacking libgmp10:amd64 (2:6.1.0+dfsg-2) ...
Selecting previously unselected package libmpfr4:amd64.
Preparing to unpack .../libmpfr4_3.1.4-1_amd64.deb ...
Unpacking libmpfr4:amd64 (3.1.4-1) ...
Selecting previously unselected package libmpc3:amd64.
Preparing to unpack .../libmpc3_1.0.3-1_amd64.deb ...
Unpacking libmpc3:amd64 (1.0.3-1) ...
Selecting previously unselected package manpages.
Preparing to unpack .../manpages_4.04-2_all.deb ...
Unpacking manpages (4.04-2) ...
Selecting previously unselected package binutils.
Preparing to unpack .../binutils_2.26.1-1ubuntu1~16.04.8_amd64.deb ...
Unpacking binutils (2.26.1-1ubuntu1~16.04.8) ...
Selecting previously unselected package libisl15:amd64.
Preparing to unpack .../libisl15_0.16.1-1_amd64.deb ...
Unpacking libisl15:amd64 (0.16.1-1) ...
Selecting previously unselected package cpp-5.
Preparing to unpack .../cpp-5_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking cpp-5 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package cpp.
Preparing to unpack .../cpp_4%3a5.3.1-1ubuntu1_amd64.deb ...
Unpacking cpp (4:5.3.1-1ubuntu1) ...
Selecting previously unselected package libcc1-0:amd64.
Preparing to unpack .../libcc1-0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libcc1-0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libgomp1:amd64.
Preparing to unpack .../libgomp1_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libgomp1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libitm1:amd64.
Preparing to unpack .../libitm1_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libitm1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libatomic1:amd64.
Preparing to unpack .../libatomic1_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libatomic1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libasan2:amd64.
Preparing to unpack .../libasan2_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libasan2:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package liblsan0:amd64.
Preparing to unpack .../liblsan0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking liblsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libtsan0:amd64.
Preparing to unpack .../libtsan0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libtsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libubsan0:amd64.
Preparing to unpack .../libubsan0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libubsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libcilkrts5:amd64.
Preparing to unpack .../libcilkrts5_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libcilkrts5:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libmpx0:amd64.
Preparing to unpack .../libmpx0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libmpx0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libquadmath0:amd64.
Preparing to unpack .../libquadmath0_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libquadmath0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package libgcc-5-dev:amd64.
Preparing to unpack .../libgcc-5-dev_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking libgcc-5-dev:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package gcc-5.
Preparing to unpack .../gcc-5_5.4.0-6ubuntu1~16.04.12_amd64.deb ...
Unpacking gcc-5 (5.4.0-6ubuntu1~16.04.12) ...
Selecting previously unselected package gcc.
Preparing to unpack .../gcc_4%3a5.3.1-1ubuntu1_amd64.deb ...
Unpacking gcc (4:5.3.1-1ubuntu1) ...
Selecting previously unselected package libc-dev-bin.
Preparing to unpack .../libc-dev-bin_2.23-0ubuntu11.2_amd64.deb ...
Unpacking libc-dev-bin (2.23-0ubuntu11.2) ...
Selecting previously unselected package linux-libc-dev:amd64.
Preparing to unpack .../linux-libc-dev_4.4.0-208.240_amd64.deb ...
Unpacking linux-libc-dev:amd64 (4.4.0-208.240) ...
Selecting previously unselected package libc6-dev:amd64.
Preparing to unpack .../libc6-dev_2.23-0ubuntu11.2_amd64.deb ...
Unpacking libc6-dev:amd64 (2.23-0ubuntu11.2) ...
Selecting previously unselected package manpages-dev.
Preparing to unpack .../manpages-dev_4.04-2_all.deb ...
Unpacking manpages-dev (4.04-2) ...
Processing triggers for libc-bin (2.23-0ubuntu11.2) ...
Setting up libgmp10:amd64 (2:6.1.0+dfsg-2) ...
Setting up libmpfr4:amd64 (3.1.4-1) ...
Setting up libmpc3:amd64 (1.0.3-1) ...
Setting up manpages (4.04-2) ...
Setting up binutils (2.26.1-1ubuntu1~16.04.8) ...
Setting up libisl15:amd64 (0.16.1-1) ...
Setting up cpp-5 (5.4.0-6ubuntu1~16.04.12) ...
Setting up cpp (4:5.3.1-1ubuntu1) ...
Setting up libcc1-0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libgomp1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libitm1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libatomic1:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libasan2:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up liblsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libtsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libubsan0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libcilkrts5:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libmpx0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libquadmath0:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up libgcc-5-dev:amd64 (5.4.0-6ubuntu1~16.04.12) ...
Setting up gcc-5 (5.4.0-6ubuntu1~16.04.12) ...
Setting up gcc (4:5.3.1-1ubuntu1) ...
Setting up libc-dev-bin (2.23-0ubuntu11.2) ...
Setting up linux-libc-dev:amd64 (4.4.0-208.240) ...
Setting up libc6-dev:amd64 (2.23-0ubuntu11.2) ...
Setting up manpages-dev (4.04-2) ...
Processing triggers for libc-bin (2.23-0ubuntu11.2) ...
root@4842474755a1:/# 
```

![image-20210413160247431](\img\Docker镜像\image-20210413160247431.png)

待安装命令执行完毕后，使用命令 `gcc -v` 查看 gcc 的版本号。

```
root@4842474755a1:/# gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-linux-gnu/5/lto-wrapper
Target: x86_64-linux-gnu
Configured with: ../src/configure -v --with-pkgversion='Ubuntu 5.4.0-6ubuntu1~16.04.12' --with-bugurl=file:///usr/share/doc/gcc-5/README.Bugs --enable-languages=c,ada,c++,java,go,d,fortran,objc,obj-c++ --prefix=/usr --program-suffix=-5 --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-vtable-verify --enable-libmpx --enable-plugin --with-system-zlib --disable-browser-plugin --enable-java-awt=gtk --enable-gtk-cairo --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-5-amd64/jre --enable-java-home --with-jvm-root-dir=/usr/lib/jvm/java-1.5.0-gcj-5-amd64 --with-jvm-jar-dir=/usr/lib/jvm-exports/java-1.5.0-gcj-5-amd64 --with-arch-directory=amd64 --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --enable-objc-gc --enable-multiarch --disable-werror --with-arch-32=i686 --with-abi=m64 --with-multilib-list=m32,m64,mx32 --enable-multilib --with-tune=generic --enable-checking=release --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu
Thread model: posix
gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.12) 
root@4842474755a1:/# 
```

![image-20210413160330778](\img\Docker镜像\image-20210413160330778.png)

完成这些操作之后，输入 exit 命令退出容器 4842474755a1。

此时 容器 4842474755a1即为按我们的需求更改的容器。

在容器所运行的宿主机上通过命令 docker commit 基于更新后的容器生成新镜像。

```
[root@bogon ~]# docker commit -m "gcc installed" -a "cg" 4842474755a1 liuyang/my_ubuntu:v2.0
sha256:6aeab10bf9b62d5357795d9aa547a23ace58692aea825f920eead99fe9946e9a
[root@bogon ~]# docker images
REPOSITORY          TAG       IMAGE ID       CREATED          SIZE
liuyang/my_ubuntu   v2.0      6aeab10bf9b6   12 seconds ago   261MB
httpd               latest    0b932df43057   2 days ago       138MB
ubuntu              latest    26b77e58432b   10 days ago      72.9MB
ubuntu              16.04     f6f49faac5cf   2 weeks ago      132MB
ubuntu              14.04     13b66b487594   2 weeks ago      197MB
ubuntu              15.10     9b9cb95443b5   4 years ago      137MB
training/webapp     latest    6fae60ef3446   5 years ago      349MB
[root@bogon ~]# 
```

各个参数说明：
- -m: 提交的描述信息
- -a: 指定镜像作者
- 4842474755a1：容器 ID
- liuyang/my_ubuntu:v2.0: 指定要创建的目标镜像名

使用 docker images 命令来查看新镜像 liuyang/my_ubuntu:v2.0。

![image-20210413160613528](\img\Docker镜像\image-20210413160613528.png)

接下来使用新镜像创建容器，使用 gcc 编译该 hello_world.c 文件，并运行编译后的二进制程序。

hello_world.c 文件位于 ~/course/docker/c_hello_world，首先使用 cd 命令进入该目录，在创建容器后，将 hello_world.c 文件通过 docker cp 命令拷贝到容器中。

具体操作过程如下：

```
[root@bogon c_hello_world]# docker run -itd --name ubuntu_gcc liuyang/my_ubuntu:v2.0 
80a4870e7b943852dc565814ee10b9f89ee74489db9cf923905fadb9e9c730bd
[root@bogon c_hello_world]# docker ps -l
CONTAINER ID   IMAGE                    COMMAND       CREATED         STATUS         PORTS     NAMES
80a4870e7b94   liuyang/my_ubuntu:v2.0   "/bin/bash"   7 seconds ago   Up 5 seconds             ubuntu_gcc
[root@bogon c_hello_world]# docker cp hellow_world.c 80a:/home
[root@bogon c_hello_world]# docker exec -it 80a /bin/bash
root@80a4870e7b94:/# cd home/
root@80a4870e7b94:/home# ls
hellow_world.c
root@80a4870e7b94:/home# gc
gcc           gcc-ar        gcc-nm        gcc-ranlib    gcov          gcov-tool     
gcc-5         gcc-ar-5      gcc-nm-5      gcc-ranlib-5  gcov-5        gcov-tool-5   
root@80a4870e7b94:/home# gc
gcc           gcc-ar        gcc-nm        gcc-ranlib    gcov          gcov-tool     
gcc-5         gcc-ar-5      gcc-nm-5      gcc-ranlib-5  gcov-5        gcov-tool-5   
root@80a4870e7b94:/home# gcc hellow_world.c -o hello_world
root@80a4870e7b94:/home# ./hello_world 
Hello World !root@80a4870e7b94:/home# exit
exit
[root@bogon c_hello_world]# 

```

![image-20210414113126008](\img\Docker镜像\image-20210414113126008.png)



### 构建镜像

接下来，使用命令 docker build ， 从零开始构建一个新的镜像。

首先需要一个 Dockerfile 文件，其中包含一系列告诉 Docker 如何构建镜像的指令。

该Dockerfile文件位于 ~/course/docker/Dockerfile/ubuntu_gcc，使用命令 cd 切换到该目录， 通过命令 cat 查看 Dockerfile 内容。

```
[root@bogon ~]# cd /mnt/docker/Dockerfile/ubuntu_gcc/
[root@bogon ubuntu_gcc]# ls -l
总用量 4
-rw-r--r--. 1 root root 249 4月  14 17:58 Dockerfile
[root@bogon ubuntu_gcc]# cat Dockerfile 
FROM    ubuntu:16.04
MAINTAINER      yourname "yourname@site.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd liuyang
RUN     /bin/echo 'liuyang:123456' |chpasswd
RUN     apt-get update && apt-get install -y gcc
EXPOSE  80
USER    liuyang
[root@bogon ubuntu_gcc]# 
```

![image-20210414175927181](\img\Docker镜像\image-20210414175927181.png)

每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

第一条FROM，指定使用哪个镜像源。

第二条MAINTAINER，说明当前镜像的维护人的信息。

RUN 指令用于说明在构建 docker 镜像时执行哪些命令，例如安装哪些软件。

EXPOSE 指令说明开放哪些端口。

然后，基于 Dockerfile 文件，通过 docker build 命令构建镜像。

```
[root@bogon ubuntu_gcc]# docker build -t liuyang/ubuntu_gcc:v1.0 .
Sending build context to Docker daemon  2.048kB
Step 1/8 : FROM    ubuntu:16.04
 ---> f6f49faac5cf
Step 2/8 : MAINTAINER      yourname "yourname@site.com"
 ---> Running in 505db667716d
Removing intermediate container 505db667716d
 ---> ae3dc4779e76
Step 3/8 : RUN     /bin/echo 'root:123456' |chpasswd
 ---> Running in a0e16e32eb74
Removing intermediate container a0e16e32eb74
 ---> 06652e9652dc
Step 4/8 : RUN     useradd liuyang
 ---> Running in 1d24e87a8368
Removing intermediate container 1d24e87a8368
 ---> d16313cca2d5
Step 5/8 : RUN     /bin/echo 'liuyang:123456' |chpasswd
 ---> Running in 97b5ae210843
Removing intermediate container 97b5ae210843
 ---> b1a2146b4c99
Step 6/8 : RUN     apt-get update && apt-get install -y gcc
 ---> Running in 228813a7a65e
.......
.......
Processing triggers for libc-bin (2.23-0ubuntu11.2) ...
Removing intermediate container 228813a7a65e
 ---> e0714c52f606
Step 7/8 : EXPOSE  80
 ---> Running in 75654b336753
Removing intermediate container 75654b336753
 ---> cd1b7368f769
Step 8/8 : USER    liuyang
 ---> Running in 5dc21fae1643
Removing intermediate container 5dc21fae1643
 ---> e6425f492f18
Successfully built e6425f492f18
Successfully tagged liuyang/ubuntu_gcc:v1.0
[root@bogon ubuntu_gcc]#  
```

![image-20210415135449144](\img\Docker镜像\image-20210415135449144.png)

参数说明：

- -t ：指定要创建的目标镜像名

- . ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

使用 docker images 查看创建的镜像

```
[root@bogon ubuntu_gcc]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
liuyang/ubuntu_gcc   v1.0      e6425f492f18   29 seconds ago   262MB
liuyang/my_ubuntu    v2.0      6aeab10bf9b6   46 hours ago     261MB
httpd                latest    0b932df43057   4 days ago       138MB
ubuntu               latest    26b77e58432b   12 days ago      72.9MB
ubuntu               16.04     f6f49faac5cf   2 weeks ago      132MB
ubuntu               14.04     13b66b487594   2 weeks ago      197MB
ubuntu               15.10     9b9cb95443b5   4 years ago      137MB
training/webapp      latest    6fae60ef3446   5 years ago      349MB
[root@bogon ubuntu_gcc]# 
```
![image-20210415135518879](\img\Docker镜像\image-20210415135518879.png)

可见，该镜像已成功创建，镜像名称为：liuyang/ubuntu_gcc，镜像ID为：e6425f492f18。

接下来，使用新的镜像创建容器

```
[root@bogon ubuntu_gcc]# docker run -itd liuyang/ubuntu_gcc:v1.0 
48753028240e65905b30e7f6f96fa6e35545d05d75ea016a8b313fc375c59ac0
[root@bogon ubuntu_gcc]# docker ps -l
CONTAINER ID   IMAGE                     COMMAND       CREATED          STATUS          PORTS     NAMES
48753028240e   liuyang/ubuntu_gcc:v1.0   "/bin/bash"   12 seconds ago   Up 11 seconds   80/tcp    optimistic_gates
[root@bogon ubuntu_gcc]# docker exec -it 487 /bin/bash
liuyang@48753028240e:/$ id liuyang
uid=1000(liuyang) gid=1000(liuyang) groups=1000(liuyang)
liuyang@48753028240e:/$ exit
exit
[root@bogon ubuntu_gcc]# 
```

![image-20210415135704565](\img\Docker镜像\image-20210415135704565.png)

从上面看到新镜像已经包含新创建的用户 liuyang。

在容器中，编译和运行 `hello_world.c` 文件。

```
[root@bogon ubuntu_gcc]# cd /mnt/docker/c_hello_world/
[root@bogon c_hello_world]# ls -l
总用量 4
-rw-r--r--. 1 root root 77 4月  14 11:11 hellow_world.c
[root@bogon c_hello_world]# docker cp hellow_world.c 487:/tmp
[root@bogon c_hello_world]# docker exec -it 487 /bin/bash
liuyang@48753028240e:/$ cd /tmp/
liuyang@48753028240e:/tmp$ ls -l
total 4
-rw-r--r--. 1 root root 77 Apr 14 03:11 hellow_world.c
liuyang@48753028240e:/tmp$ gcc hello_world.c -o hello_world
gcc: error: hello_world.c: No such file or directory
gcc: fatal error: no input files
compilation terminated.
liuyang@48753028240e:/tmp$ ls                              
hellow_world.c
liuyang@48753028240e:/tmp$ gcc hellow_world.c -o hellow_world   
liuyang@48753028240e:/tmp$ ./hellow_world 
Hello World !liuyang@48753028240e:/tmp$ 
```

![image-20210415140007093](\img\Docker镜像\image-20210415140007093.png)

### 设置镜像标签

可以使用 docker tag 命令，为镜像添加一个新的标签。

docker tag 命令的语法为：

docker tag <镜像ID> 新镜像名:新tag。

```
[root@bogon c_hello_world]# docker images 
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
liuyang/ubuntu_gcc   v1.0      e6425f492f18   6 minutes ago   262MB
liuyang/my_ubuntu    v2.0      6aeab10bf9b6   46 hours ago    261MB
httpd                latest    0b932df43057   4 days ago      138MB
ubuntu               latest    26b77e58432b   12 days ago     72.9MB
ubuntu               16.04     f6f49faac5cf   2 weeks ago     132MB
ubuntu               14.04     13b66b487594   2 weeks ago     197MB
ubuntu               15.10     9b9cb95443b5   4 years ago     137MB
training/webapp      latest    6fae60ef3446   5 years ago     349MB
[root@bogon c_hello_world]# docker tag e64  liuyang/ubuntu_gcc:dev
[root@bogon c_hello_world]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
liuyang/ubuntu_gcc   dev       e6425f492f18   6 minutes ago   262MB
liuyang/ubuntu_gcc   v1.0      e6425f492f18   6 minutes ago   262MB
liuyang/my_ubuntu    v2.0      6aeab10bf9b6   46 hours ago    261MB
httpd                latest    0b932df43057   4 days ago      138MB
ubuntu               latest    26b77e58432b   12 days ago     72.9MB
ubuntu               16.04     f6f49faac5cf   2 weeks ago     132MB
ubuntu               14.04     13b66b487594   2 weeks ago     197MB
ubuntu               15.10     9b9cb95443b5   4 years ago     137MB
training/webapp      latest    6fae60ef3446   5 years ago     349MB
[root@bogon c_hello_world]# 
```

使用 docker images 命令可以看到，ID为 e6425f492f18的镜像多一个标签dev。