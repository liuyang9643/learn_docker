

# Docker 仓库管理

使用仓库（Repository）可以集中存放和管理镜像。

本实验包含三部分内容：

- 如何使用公共的 Docker 仓库。
- 如何使用 Registry 搭建 Docker 仓库。
- 如何使用 VMWare Harbor 搭建自己的Docker仓库。

## Docker Hub
目前 Docker 官方维护了一个公共仓库 Docker Hub，访问地址为：https://hub.docker.com

大部分需求都可以通过在 Docker Hub 中直接下载镜像实现。从 Docker Hub 上下载镜像时，不需要注册账号。

以 hello-world 为关键词进行搜索：

```
NAME                                       DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
hello-world                                Hello World! (an example of minimal Dockeriz…   1415      [OK]       
kitematic/hello-world-nginx                A light-weight nginx container that demonstr…   148                  
tutum/hello-world                          Image to test docker deployments. Has Apache…   80                   [OK]
dockercloud/hello-world                    Hello World!                                    19                   [OK]
crccheck/hello-world                       Hello World web server in under 2.5 MB          14                   [OK]
vad1mo/hello-world-rest                    A simple REST Service that echoes back all t…   5                    [OK]
ppc64le/hello-world                        Hello World! (an example of minimal Dockeriz…   2                    
datawire/hello-world                       Hello World! Simple Hello World implementati…   1                    [OK]
markmnei/hello-world-java-docker           Hello-World-Java-docker                         1                    [OK]
thomaspoignant/hello-world-rest-json       This project is a REST hello-world API to bu…   1                    
ansibleplaybookbundle/hello-world-db-apb   An APB which deploys a sample Hello World! a…   1                    [OK]
souravpatnaik/hello-world-go               hello-world in Golang                           1                    
ansibleplaybookbundle/hello-world-apb      An APB which deploys a sample Hello World! a…   1                    [OK]
rancher/hello-world                                                                        1                    
strimzi/hello-world-consumer                                                               0                    
strimzi/hello-world-streams                                                                0                    
koudaiii/hello-world                                                                       0                    
freddiedevops/hello-world-spring-boot                                                      0                    
burdz/hello-world-k8s                      To provide a simple webserver that can have …   0                    [OK]
garystafford/hello-world                   Simple hello-world Spring Boot service for t…   0                    [OK]
strimzi/hello-world-producer                                                               0                    
infrastructureascode/hello-world           A tiny "Hello World" web server with a healt…   0                    [OK]
businessgeeks00/hello-world-nodejs                                                         0                    
airwavetechio/hello-world                                                                  0                    
nirmata/hello-world                                                                        0                    [OK]
[root@bogon ~]# 
```

![image-20210419144618218](\img\Docker仓库管理\image-20210419144618218.png)

可以看到Docker Hub给出的 hello-world 镜像列表，这里选择第一个镜像。

```
[root@bogon ~]# docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
b8dfde127a29: Downloading 
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:f2266cbfc127c960fd30e76b7c792dc23b588c0db76233517e1891a4e357d519
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
[root@bogon ~]# 
```

!![image-20210419144734069](\img\Docker仓库管理\image-20210419144734069.png)

但是，如果用 Docker Hub 存放（push）自己的镜像时，需要首先在Docker Hub上注册账号。

### 注册

在 https://hub.docker.com 免费注册一个 Docker 账号。打开官网后，点击 “Sign Up Docker Hub”注册账号，注册完毕后，点击“Sign In”登录网站。

![image-20210419144916054](\img\Docker仓库管理\image-20210419144916054.png)

### 登录

登录需要输入用户名和密码，登录成功后，我们就可以往自己账号下的推送镜像了。

使用 docker login 登录 Docker Hub。

```
[root@bogon ~]# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: liuyang9643
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[root@bogon ~]# 
```

![image-20210419145010259](\img\Docker仓库管理\image-20210419145010259.png)

输入在Docker Hub网站上注册的账号和密码后，会提示登录成功（Login Succeeded）。

### 退出

退出 docker hub 可以使用 docker logout 命令：

```
[root@bogon ~]# docker logout
Removing login credentials for https://index.docker.io/v1/
[root@bogon ~]# 
```

![image-20210419145113644](\img\Docker仓库管理\image-20210419145113644.png)

### 推送

如果当前处于登出状态，需要重新登录。

登录后，通过 docker push 命令将自己的镜像推送到 Docker Hub。

```
[root@bogon ~]# docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: liuyang9643
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[root@bogon ~]# docker tag hello-world:latest liuyang9643/hello-world:latest
[root@bogon ~]# docker push liuyang9643/hello-world:latest
The push refers to repository [docker.io/liuyang9643/hello-world]
f22b99068db9: Pushed 
latest: digest: sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792 size: 525
[root@bogon ~]# 
```

![image-20210419145855921](\img\Docker仓库管理\image-20210419145855921.png)

在上述命令中的 username 请替换为你自己的 Docker 账号用户名。

推送结束后，可以在官网上点击 Repositories 查看自己账号下的镜像。

![image-20210419145946153](\img\Docker仓库管理\image-20210419145946153.png)

## 基于 Registry 搭建仓库

在 Docker 中，当执行 docker pull image_name 时 ，实际上是从 registry.hub.docker.com 这个地址去查找，这就是 Docker 公司提供的公共仓库。

在工作中，不可能把企业项目push到公有仓库进行管理。为了更好的管理镜像，Docker不仅提供了一个中央仓库，同时也允许用户搭建本地私有仓库。本节介绍如何使用registry搭建私有仓库，下节介绍如何使用VMWare Harbor搭建仓库。

registry搭建仓库的实验分为两部分：单机版仓库和分布式仓库。

### registry 本地仓库 

**搭建**  

Docker 官方提供了一个搭建私有仓库的镜像 registry ，只需把镜像下载下来，运行容器并暴露 5000 端口，就可以使用了。

Registry服务默认会将上传的镜像保存在容器的/var/lib/registry，我们将主机的/opt/registry目录挂载到该目录，即可实现将镜像保存到主机的/opt/registry目录了。

```
[root@bogon ~]# docker pull registry:2
2: Pulling from library/registry
ddad3d7c1e96: Pull complete 
6eda6749503f: Pull complete 
363ab70c2143: Pull complete 
5b94580856e6: Pull complete 
12008541203a: Pull complete 
Digest: sha256:bac2d7050dc4826516650267fe7dc6627e9e11ad653daca0641437abdf18df27
Status: Downloaded newer image for registry:2
docker.io/library/registry:2
[root@bogon ~]#  docker run -d -v /opt/registry:/var/lib/registry -p 5000:5000 --name myregistry registry:2
ac9125535dd4d96e4361aa86ddaf78907867aac21e17c593a4ca1901d1e47978
[root@bogon ~]# docker ps -l
CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS                    NAMES
ac9125535dd4   registry:2   "/entrypoint.sh /etc…"   5 seconds ago   Up 5 seconds   0.0.0.0:5000->5000/tcp   myregistry
[root@bogon ~]# 
```

![image-20210419150147234](\img\Docker仓库管理\image-20210419150147234.png)

浏览器访问http://192.168.20.130:5000/v2/_catalog，出现下面情况说明registry运行正常。

![image-20210419150518174](\img\Docker仓库管理\image-20210419150518174.png)

可以看到，当前还没有上传任何镜像。

### 验证

现在通过 push hello-world 镜像验证 registry 是否正常。

注意在推送前，需要将镜像做一个新的标记（tag），标记的格式为：

仓库地址/镜像名:版本号

使用 push 命令时，push的语法为：

docker push 仓库地址/镜像名:版本号

```
[root@bogon ~]# docker tag hello-world:latest localhost:5000/hello-world:latest
[root@bogon ~]# docker push localhost:5000/hello-world:latest
The push refers to repository [localhost:5000/hello-world]
f22b99068db9: Pushed 
latest: digest: sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792 size: 525
[root@bogon ~]# 
```

![image-20210419150715046](\img\Docker仓库管理\image-20210419150715046.png)

可以看出，hello-world 镜像推送成功。

通过浏览器验证 hello-world 镜像推送成功。

![image-20210419150734073](\img\Docker仓库管理\image-20210419150734073.png)

可以看到仓库中已显示 hello-world 镜像。

接着，使用私有仓库下载镜像，首先移除本地的 hello-world 镜像：

然后，通过pull下载镜像，pull命令的格式为：

docker pull 仓库地址/镜像名:版本号

```
[root@bogon ~]# docker images
REPOSITORY                   TAG       IMAGE ID       CREATED       SIZE
registry                     2         1fd8e1b0bb7e   4 days ago    26.2MB
hello-world                  latest    d1165f221234   6 weeks ago   13.3kB
liuyang9643/hello-world      latest    d1165f221234   6 weeks ago   13.3kB
localhost:5000/hello-world   latest    d1165f221234   6 weeks ago   13.3kB
[root@bogon ~]# docker rmi hello-world:latest liuyang9643/hello-world:latest  localhost:5000/hello-world:latest 
Untagged: hello-world:latest
Untagged: hello-world@sha256:f2266cbfc127c960fd30e76b7c792dc23b588c0db76233517e1891a4e357d519
Untagged: liuyang9643/hello-world:latest
Untagged: liuyang9643/hello-world@sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792
Untagged: localhost:5000/hello-world:latest
Untagged: localhost:5000/hello-world@sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726
Deleted: sha256:f22b99068db93900abe17f7f5e09ec775c2826ecfe9db961fea68293744144bd
[root@bogon ~]# docker images 
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
registry     2         1fd8e1b0bb7e   4 days ago   26.2MB
[root@bogon ~]# docker pull localhost:5000/hello-world:latest
latest: Pulling from hello-world
b8dfde127a29: Pull complete 
Digest: sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792
Status: Downloaded newer image for localhost:5000/hello-world:latest
localhost:5000/hello-world:latest
[root@bogon ~]# docker image
image   images  
[root@bogon ~]# docker images
REPOSITORY                   TAG       IMAGE ID       CREATED       SIZE
registry                     2         1fd8e1b0bb7e   4 days ago    26.2MB
localhost:5000/hello-world   latest    d1165f221234   6 weeks ago   13.3kB
[root@bogon ~]# 
```

![image-20210419150912773](\img\Docker仓库管理\image-20210419150912773.png)

可以看到 hello-world 镜像已被成功下载。

对镜像做一个 hello-world:latest 的标记。

```
[root@bogon ~]# docker tag localhost:5000/hello-world:latest hello-world:latest
[root@bogon ~]# docker images
REPOSITORY                   TAG       IMAGE ID       CREATED       SIZE
registry                     2         1fd8e1b0bb7e   4 days ago    26.2MB
hello-world                  latest    d1165f221234   6 weeks ago   13.3kB
localhost:5000/hello-world   latest    d1165f221234   6 weeks ago   13.3kB
[root@bogon ~]# 
```

![image-20210419150949531](\img\Docker仓库管理\image-20210419150949531.png)

使用本地镜像的另一个好处是，一般局域网的速度要远快于互联网，因此，push和pull的速度都较快。

### registry 远程仓库

到此为止，已经在本地系统上完成了registry仓库的搭建。然而，一般镜像仓库是需要被远程访问，即仓库服务器搭建好了之后，在其他docker客户端上可以将镜像推送到这个镜像仓库，或者从这个镜像仓库下载镜像。

由于我们的实验环境只有当前桌面环境这一台服务器，为了模拟多台服务器，这里，借助于 dind 镜像。

dind 镜像的功能是在 Docker 容器中运行 Docker 容器，可以用dind创建一个 registry_server 容器，作为镜像仓库；再创建一个 registry_client 容器，作为普通的docker服务器，然后从这个 docker服务器上向registry_server推送镜像或者从registry_server上下载镜像。这样就实现了，镜像仓库被远程访问的功能。

首先下载dind镜像，然后创建registry_server 和 registry_client，并为这两台服务器分配IP地址。

命令如下：

```
[root@bogon ~]# docker pull jpetazzo/dind
Using default tag: latest
latest: Pulling from jpetazzo/dind
28bfaceaff9b: Pull complete 
ac540055f2f8: Pull complete 
2965585ef8b8: Pull complete 
2416bb9f3ad2: Pull complete 
93b55a6a6807: Pull complete 
1869af86af90: Pull complete 
fa90c21e948c: Pull complete 
4cc8c36bb9c7: Pull complete 
Digest: sha256:f48a1bbf379afdb7a7685abd0130ccd2f214662b086eb7320c296ee83fc6448e
Status: Downloaded newer image for jpetazzo/dind:latest
docker.io/jpetazzo/dind:latest
[root@bogon ~]# docker network create registry_net --subnet=172.20.0.0/16
58ef67b51193c7ff12a543a228df9e52c908692cafca2a9a4236ce7f72c0c735
[root@bogon ~]# docker run -itd --privileged --name registry_server --hostname server --network registry_net --ip 172.20.1.1 jpetazzo/dind
989b578aa492b0dc0d545a7cd09b0a87940706cea1feb8d7c2b6c4dcecbb436b
[root@bogon ~]# docker run -itd --privileged --name registry_client --hostname client --network registry_net --ip 172.20.1.2 jpetazzo/dind
32bcc29ce0507a224b992abd89f3987a9dd6bfe96a3f05aa20440afdc539cab0
[root@bogon ~]#
```

![image-20210419151722077](\img\Docker仓库管理\image-20210419151722077.png)

上述命令分别创建了 registry_server（172.20.1.1） 和 registry_client（172.20.1.2） ，registry_server 为仓库服务器， registry_client 为用于访问仓库服务器的远程机。

在registry_server上部署registry:2服务：

```
root@cg:~/Desktop# docker exec -it registry_server /bin/bash
root@server:/# docker pull registry:2
2: Pulling from library/registry
486039affc0a: Pull complete 
ba51a3b098e6: Pull complete 
8bb4c43d6c8e: Pull complete 
6f5f453e5f2d: Pull complete 
42bc10b72f42: Pull complete 
Digest: sha256:7d081088e4bfd632a88e3f3bcd9e007ef44a796fddfe3261407a3f9f04abe1e7
Status: Downloaded newer image for registry:2
root@server:/# docker run -d -v /opt/registry:/var/lib/registry -p 5000:5000 --name myregistry registry:2
4eb138b9e11935acf1b077b07e58f82d36b00062bf10cc9f8ddb1551d96c97b8
root@server:/# 
```

![remote_registry_deploy](https://test.educg.net/userfiles/markdown/exp/2020_2/703ll1581491137.png)

打开浏览器，输入地址：172.20.1.1:5000/v2/_catalog，查看服务是否启动。

![image-20210419151948337](\img\Docker仓库管理\image-20210419151948337.png)

可见，服务已正常启动。

在终端中，打开一个标签页。

在新打开的标签页里，进入 registry_client 。

![image-20210419152121494](\img\Docker仓库管理\image-20210419152121494.png)

首先修改/etc/hosts文件，添加内容到该文件尾部：

```
172.20.1.1      server
```

![image-20210419153022643](\img\Docker仓库管理\image-20210419153022643.png)

然后，下载 hello-world 镜像，并推送给 registry_server 

```
root@client:/# docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:f2266cbfc127c960fd30e76b7c792dc23b588c0db76233517e1891a4e357d519
Status: Downloaded newer image for hello-world:latest
root@client:/# docker tag hello-world server:5000/hello-world:latest
root@client:/# docker push server:5000/hello-world:latest
The push refers to repository [server:5000/hello-world]
Get https://server:5000/v2/: http: server gave HTTP response to HTTPS client
root@client:/# 
```

可以看到推送失败了。这是因为从 Docker 1.13.2 版本开始，使用 registry 时，必须使用TLS保证其安全。

有两种方法，可以让镜像顺利推送到仓库服务器上。一种是修改仓库服务器的Docker配置文件 daemon.json ，将仓库服务器配置为非安全服务器。另一种方式是配置TLS认证。

这里，分别用这两种方式进行配置。

#### 修改仓库服务器的 Docker 配置

编辑 /etc/docker/daemon.json 文件，添加如下信息：

```
{
	"insecure-registries":["server:5000","172.20.1.1:5000"]
}
```

![image-20210419153250403](\img\Docker仓库管理\image-20210419153250403.png)

修改完毕后，重启Docker。在 dind 中，第一次重启 Docker 的方式为：


```
root@client:/# ps -ef |grep dockerd
root         60      1  0 07:17 pts/0    00:00:02 dockerd
root        312    214  0 07:33 pts/2    00:00:00 grep --color=auto dockerd
root@client:/# 
root@client:/# kill -9 60
root@client:/# service docker start
mount: cgroup already mounted or cpu busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/cpu,cpuacct
rmdir: failed to remove 'cpu': Not a directory
mount: cgroup already mounted or cpuacct busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/cpu,cpuacct
rmdir: failed to remove 'cpuacct': Not a directory
mount: cgroup already mounted or net_cls busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/net_cls,net_prio
rmdir: failed to remove 'net_cls': Not a directory
mount: cgroup already mounted or net_prio busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/net_cls,net_prio
rmdir: failed to remove 'net_prio': Not a directory
 * Starting Docker: docker                                                                                                                                 [ OK ] 
root@client:/# 
```

![image-20210419153416894](\img\Docker仓库管理\image-20210419153416894.png)

在后续的实验中，可采用如下方式在 dind 中重启 Docker

```
# service docker stop
# service docker start
```

通过 docker info 查看 insecure-registies 是否正确设置：

```
root@client:/# docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 1
Server Version: 18.05.0-ce
Storage Driver: overlay2
 Backing Filesystem: xfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 773c489c9c1b21a6d78b5c538cd395416ec50f88
runc version: 4fc53a81fb7c994640722ac585fa9ca548971871
init version: 949e6fa
Kernel Version: 3.10.0-1160.el7.x86_64
Operating System: Ubuntu 14.04.5 LTS (containerized)
OSType: linux
Architecture: x86_64
CPUs: 4
Total Memory: 1.777GiB
Name: client
ID: JLFF:73CW:34SR:ILQK:3JIA:XKMO:5FLJ:43D6:HUFG:LXVY:4CSK:7XVT
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 server:5000
 172.20.1.1:5000
 127.0.0.0/8
Live Restore Enabled: false

WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
root@client:/# 
```

![image-20210419153458149](\img\Docker仓库管理\image-20210419153458149.png)

可以看到，insecure-registies 已被正确设置。

接下来，重新推送 hello-world 镜像：


```
root@client:/# docker push server:5000/hello-world
The push refers to repository [server:5000/hello-world]
f22b99068db9: Pushed 
latest: digest: sha256:1b26826f602946860c279fce658f31050cff2c596583af237d971f4629b57792 size: 525
root@client:/# 
```

![image-20210419153539206](\img\Docker仓库管理\image-20210419153539206.png)

通过 Chrome 验证 hello-world 镜像推送成功。

![image-20210419153617290](\img\Docker仓库管理\image-20210419153617290.png)

可见，hello-world 镜像已成功推送。

#### 配置TLS

接下来使用配置TLS（安全传输层协议）认证的方式配置仓库服务器和 Docker 客户端。

首先，将 Docker 客户端的 daemon.json 配置文件移除，并重启Docker。

```
root@client:/# service docker stop
 * Stopping Docker: docker                                                                                                                                 [ OK ] 
root@client:/# service docker start
mount: cgroup already mounted or cpu busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/cpu,cpuacct
rmdir: failed to remove 'cpu': Not a directory
mount: cgroup already mounted or cpuacct busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/cpu,cpuacct
rmdir: failed to remove 'cpuacct': Not a directory
mount: cgroup already mounted or net_cls busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/net_cls,net_prio
rmdir: failed to remove 'net_cls': Not a directory
mount: cgroup already mounted or net_prio busy
mount: according to mtab, cgroup is already mounted on /sys/fs/cgroup/net_cls,net_prio
rmdir: failed to remove 'net_prio': Not a directory
 * Starting Docker: docker                                                                                                                                 [ OK ] 
root@client:/# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
root@client:/# 
```

![image-20210419153738580](\img\Docker仓库管理\image-20210419153738580.png)

下载 nginx 镜像， 并尝试推送镜像仓库，确认 Docker 镜像仓库无法推送。


```
root@client:/# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
f7ec5a41d630: Pull complete 
aa1efa14b3bf: Pull complete 
b78b95af9b17: Pull complete 
c7d6bca2b8dc: Pull complete 
cf16cd8e71e0: Pull complete 
0241c68333ef: Pull complete 
Digest: sha256:75a55d33ecc73c2a242450a9f1cc858499d468f077ea942867e662c247b5e412
Status: Downloaded newer image for nginx:latest
root@client:/# docker tag nginx server:5000/nginx
root@client:/# docker push server:5000/nginx 
The push refers to repository [server:5000/nginx]
Get https://server:5000/v2/: http: server gave HTTP response to HTTPS client
root@client:/# 
```

![image-20210419154034697](\img\Docker仓库管理\image-20210419154034697.png)

首先在仓库服务器上，生成带有自签名的证书：

```
root@server:/# mkdir -p /opt/docker/registry/certs
root@server:/# openssl req -newkey rsa:4096 -nodes -sha256 -keyout /opt/docker/registry/certs/domain.key -x509 -days 365 -out /opt/docker/registry/certs/domain.crt
Generating a 4096 bit RSA private key
................................................................................................++
................++
writing new private key to '/opt/docker/registry/certs/domain.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:BJ
Locality Name (eg, city) []:BJ
Organization Name (eg, company) [Internet Widgits Pty Ltd]:liuyang
Organizational Unit Name (eg, section) []:liuyang
Common Name (e.g. server FQDN or YOUR name) []:server
Email Address []:1019798260@qq.com
root@server:/# 
```

![image-20210419154518728](\img\Docker仓库管理\image-20210419154518728.png)

根据提示信息输入自己的信息，在路径 /opt/docker/registry/certs 下生成签名证书。

删除之前创建的 myregistry 容器，重新创建带有 TLS 认证的 registry 容器。

```
root@server:/# docker stop myregistry
myregistry
root@server:/# docker rm myregistry
myregistry
root@server:/# docker run -d --name myregistry -p 5000:5000 -v /opt/docker/registry/certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key registry:2
6caf704f4b935cb5e44c2931260011e603d40eb233e4b738201ee00c979ad302
root@server:/# 
```

![image-20210419154647442](\img\Docker仓库管理\image-20210419154647442.png)

然后回到 Client 端，首先需要创建/etc/docker/certs.d/目录。

然后再该目录下创建与这个registry服务器域名一致的目录：server:5000

```
root@client:/# mkdir /etc/docker/certs.d
root@client:/# cd /etc/docker/certs.d/
root@client:/etc/docker/certs.d# mkdir server:5000
root@client:/etc/docker/certs.d# 
```

![image-20210419154746408](\img\Docker仓库管理\image-20210419154746408.png)

然后将证书 domain.crt 复制到每一个 Client 的 /etc/docker/certs.d/server:5000/ 目录下。

这里，采用scp复制，复制前需要在 server 端安装 ssh 服务器，在 client 端安装 ssh 客户端。

首先在 server 端配置 ssh 服务器：


```
root@server:/# apt-get update
apt-get install -y openssh-server
```

ssh 安装完毕后，需要允许root登录，修改 /etc/ssh/sshd_config 文件，将 PermitRootLogin 这一行改为：

```
PermitRootLogin yes
```

如下图所示

![image-20210419155109793](\img\Docker仓库管理\image-20210419155109793.png)

修改完毕后，重启 ssh 服务器，修改过程如下所示：

```
root@server:/# vi /etc/ssh/sshd_config 
root@server:/# cat /etc/ssh/sshd_config | grep PermitRoot
PermitRootLogin yes 
# the setting of "PermitRootLogin without-password".
root@server:/# service ssh restart
 * Restarting OpenBSD Secure Shell server sshd                                                                                                             [ OK ] 
root@server:/# 
```

![image-20210419155200709](\img\Docker仓库管理\image-20210419155200709.png)

为仓库服务器设置root密码，这里设置为（可根据自己喜好设置）：

liu123

```
root@server:/# passwd root
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
root@server:/# 
```

![image-20210419155309980](\img\Docker仓库管理\image-20210419155309980.png)

接着回到 Client 端安装 ssh 客户端。

```
root@client:/etc/docker/certs.d# apt-get update
root@client:/etc/docker/certs.d# apt-get install -y openssh-client
```

安装完毕后，将认证文件从仓库服务器拷贝到本地。

```
root@client:/etc/docker/certs.d# scp -p root@172.20.1.1:/opt/docker/registry/certs/domain.crt /etc/docker/certs.d/server\:5000/ca.crt
The authenticity of host '172.20.1.1 (172.20.1.1)' can't be established.
ECDSA key fingerprint is 39:28:25:3e:5f:c1:ef:ee:17:4f:3a:cc:2d:df:af:b8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.20.1.1' (ECDSA) to the list of known hosts.
root@172.20.1.1's password: 
domain.crt                                                                                                                      100% 2074     2.0KB/s   00:00    
root@client:/etc/docker/certs.d# 
```

![image-20210419155602016](\img\Docker仓库管理\image-20210419155602016.png)

现在重新推送 nginx 镜像，输出信息如下：


```
root@client:/etc/docker/certs.d# docker push server:5000/nginx
The push refers to repository [server:5000/nginx]
64ee8c6d0de0: Pushed 
974e9faf62f1: Pushed 
15aac1be5f02: Pushed 
23c959acc3d0: Pushed 
4dc529e519c4: Pushed 
7e718b9c0c8c: Pushed 
latest: digest: sha256:42bba58a1c5a6e2039af02302ba06ee66c446e9547cbfb0da33f4267638cdb53 size: 1570
root@client:/etc/docker/certs.d# 
```

![image-20210419155644037](\img\Docker仓库管理\image-20210419155644037.png)

使用 Chrome 查看镜像列表（在提示页面点击：高级->继续）：

![remote_registry_TLS_chrome_nginx](https://test.educg.net/userfiles/markdown/exp/2020_2/703ll1581491701.png)

可以看到 nginx 已被成功推送于仓库服务器上。

从仓库服务器下载 ngnix 镜像进一步验证仓库服务器已正确配置。

```
root@client:/etc/docker/certs.d# docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
nginx                     latest              62d49f9bab67        5 days ago          133MB
server:5000/nginx         latest              62d49f9bab67        5 days ago          133MB
server:5000/hello-world   latest              d1165f221234        6 weeks ago         13.3kB
hello-world               latest              d1165f221234        6 weeks ago         13.3kB
root@client:/etc/docker/certs.d# docker rmi nginx:latest
Untagged: nginx:latest
Untagged: nginx@sha256:75a55d33ecc73c2a242450a9f1cc858499d468f077ea942867e662c247b5e412
root@client:/etc/docker/certs.d# docker rmi server:5000/nginx
Untagged: server:5000/nginx:latest
Untagged: server:5000/nginx@sha256:42bba58a1c5a6e2039af02302ba06ee66c446e9547cbfb0da33f4267638cdb53
Deleted: sha256:62d49f9bab67f7c70ac3395855bf01389eb3175b374e621f6f191bf31b54cd5b
Deleted: sha256:3444fb58dc9e8338f6da71c1040e8ff532f25fab497312f95dcee0f756788a84
Deleted: sha256:f85cfdc7ca97d8856cd4fa916053084e2e31c7e53ed169577cef5cb1b8169ccb
Deleted: sha256:704bf100d7f16255a2bc92e925f7007eef0bd3947af4b860a38aaffc3f992eae
Deleted: sha256:d5955c2e658d1432abb023d7d6d1128b0aa12481b976de7cbde4c7a31310f29b
Deleted: sha256:11126fda59f7f4bf9bf08b9d24c9ea45a1194f3d61ae2a96af744c97eae71cbf
Deleted: sha256:7e718b9c0c8c2e6420fe9c4d1d551088e314fe923dce4b2caf75891d82fb227d
root@client:/etc/docker/certs.d# docker pull server:5000/nginx
Using default tag: latest
latest: Pulling from nginx
f7ec5a41d630: Pull complete 
aa1efa14b3bf: Pull complete 
b78b95af9b17: Pull complete 
c7d6bca2b8dc: Pull complete 
cf16cd8e71e0: Pull complete 
0241c68333ef: Pull complete 
Digest: sha256:42bba58a1c5a6e2039af02302ba06ee66c446e9547cbfb0da33f4267638cdb53
Status: Downloaded newer image for server:5000/nginx:latest
root@client:/etc/docker/certs.d# docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
server:5000/nginx         latest              62d49f9bab67        5 days ago          133MB
hello-world               latest              d1165f221234        6 weeks ago         13.3kB
server:5000/hello-world   latest              d1165f221234        6 weeks ago         13.3kB
root@client:/etc/docker/certs.d# 
```

![image-20210419161208783](\img\Docker仓库管理\image-20210419161208783.png)

可见，可以顺利地从仓库服务器下载到 nginx 镜像。

