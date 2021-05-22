# Docker 容器互联

在《 Docker 容器》实验中，通过网络端口映射实现了 docker 容器内服务的访问。

首先在容器中运行 Web 应用，然后通过 -P 或 -p 参数来指定端口映射，这样在外部也可以访问这些应用。

下面先复习一下通过端口访问 docker 容器内服务的方法。

**实验说明**

建议在进行本实验之前，首先完成实验以下实验：
- 《01 Docker 入门》
- 《02 Docker 容器》
- 《03 Docker 镜像》

---

## 网络端口映射

首先，创建了一个 Python Web 应用的容器。

```
[root@bogon ~]# cd /mnt/docker/webapp/webapp/
[root@bogon webapp]# docker run -d -P training/webapp python app.py
5fe23d6b8eab4c9506f1c2a44e662fa0eed2ff2a51e5089227a3534112afada6
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE             COMMAND           CREATED         STATUS         PORTS                     NAMES
5fe23d6b8eab   training/webapp   "python app.py"   7 seconds ago   Up 5 seconds   0.0.0.0:49154->5000/tcp   dazzling_chaplygin
[root@bogon webapp]# 
```

![image-20210417093535483](\img\Docker容器互联\image-20210417093535483.png)

通过 -P 参数创建一个容器，使用 docker ps 可以看到容器端口 5000 绑定主机端口 49154。

使用地址：IP:49154 访问容器内的Web服务。

![image-20210417093650823](\img\Docker容器互联\image-20210417093650823.png)

也可以使用 -p 指定容器端口所绑定的主机端口。

两种方式的区别是:

- -P ：容器内部端口随机映射到主机端口。
- -p ：容器内部端口绑定到指定的主机端口。

```
[root@bogon webapp]# docker run -d -p 80:5000 training/webapp python app.py
155ed7902b5a2afbcab0350c7249475887f012094344e3715f5927827079ff9e
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE             COMMAND           CREATED         STATUS         PORTS                  NAMES
155ed7902b5a   training/webapp   "python app.py"   4 seconds ago   Up 3 seconds   0.0.0.0:80->5000/tcp   heuristic_chandrasekhar
[root@bogon webapp]# 
```

![image-20210417093726813](\img\Docker容器互联\image-20210417093726813.png)

通过chrome浏览器访问容器服务，由于80是Web服务的默认端口，因此可以直接输入IP地址访问服务。

![image-20210417093757588](\img\Docker容器互联\image-20210417093757588.png)

另外，可以指定容器绑定的网络地址，比如绑定 127.0.0.1。

```
[root@bogon webapp]# docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
d85aed6dc4a73b4306f563df3543fb33cd814178ec8a0e7fbd26269fefc29d66
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE             COMMAND           CREATED         STATUS         PORTS                      NAMES
d85aed6dc4a7   training/webapp   "python app.py"   4 seconds ago   Up 3 seconds   127.0.0.1:5001->5000/tcp   quizzical_bardeen
[root@bogon webapp]# 
```

![image-20210417093903195](\img\Docker容器互联\image-20210417093903195.png)


这样就可以通过访问 127.0.0.1:5001 来访问容器的 5001 端口。

![image-20210417094100536](\img\Docker容器互联\image-20210417094100536.png)

上面的例子中，默认都是绑定 tcp 端口，如果要绑定 UDP 端口，可以在端口后面加上 /udp。

例如：docker run -d -p IP:端口:Docker端口/udp example/example_app python example.py

-P 和 -p 选项使得在容器外部通过宿主机可以访问容器内部的服务，例如对于主机 H 和容器 A，通过 -P 或 -p 进行端口映射后， 可以通过主机 H 的 IP 地址和所映射的端口号来访问服务。

对于容器B，如果容器 B 到主机 H 的网络是互通的，也可以通过 H 的IP地址和端口号来访问服务。

下面演示上述访问过程，验证操作包括以下几部分：  
- 查看宿主机的IP地址
- 使用Ubuntu镜像启动容器作为容器 B  
- 为容器A安装wget工具，需要分别执行：apt-get update和apt-get install wget两条命令
- 使用命令 `wget IP:32771` 获取容器 A 内的Web服务的页面 index.html 。
- 使用命令 `cat index.html` 查看页面内容 。


上述步骤的操作过程如下：

```
[root@bogon webapp]# ip addr | grep ens33 #查看宿主机的IP地址:192.168.20.130
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.20.130/24 brd 192.168.20.255 scope global noprefixroute dynamic ens33
[root@bogon webapp]# docker run -itd ubuntu      #启动容器B
c08d95570fc6c3e6405940660e3224169ad621a356859680888b2df745b348ac
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
c08d95570fc6   ubuntu    "/bin/bash"   34 seconds ago   Up 33 seconds             nostalgic_fermi
[root@bogon webapp]# docker exec -it c08 /bin/bash
root@c08d95570fc6:/# apt-get update    #更新安装源
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]
Get:2 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
Get:3 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 Packages [21.6 kB]
Get:4 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [683 kB]
Get:5 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:6 http://archive.ubuntu.com/ubuntu focal-backports InRelease [101 kB]
...
root@c08d95570fc6:/# apt-get install wget    #安装wget
Reading package lists... Done
Building dependency tree       
Reading state information... Done
...
root@c08d95570fc6:/# apt install iputils-ping    #安装ping
Reading package lists... Done
Building dependency tree       
Reading state information... Done
...
root@c08d95570fc6:/# ping 192.168.20.130    #判断到宿主机是否互通，按 CTRL+C 退出 ping 命令
PING 192.168.20.130 (192.168.20.130) 56(84) bytes of data.
64 bytes from 192.168.20.130: icmp_seq=1 ttl=64 time=0.053 ms
64 bytes from 192.168.20.130: icmp_seq=2 ttl=64 time=0.065 ms
64 bytes from 192.168.20.130: icmp_seq=3 ttl=64 time=0.058 ms
...
root@c08d95570fc6:/# wget 192.168.20.130    #访问容器A内的Web服务
--2021-04-17 01:54:34--  http://192.168.20.130/
Connecting to 192.168.20.130:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12 [text/html]
Saving to: 'index.html'

index.html                               100%[================================================================================>]      12  --.-KB/s    in 0s      

2021-04-17 01:54:34 (2.91 MB/s) - 'index.html' saved [12/12]

root@c08d95570fc6:/# cat index.html     #查看获取的页面
Hello world!root@c08d95570fc6:/# 
```

注意，在上述过程中，由于输出信息过多，将部分输出信息使用...替代。

可以看到，

- 容器B可以正常访问容器A中的服务，即文件index.html可以正常下载。
- cat index.html 也正确地输出了容器A中的Web服务的首页内容。

然而，上述操作存在的问题是，容器A在访问容器B最好的方式是直接访问，而不是先绕到宿主机上，然后再绕到容器A上。同时，假如容器A中的服务只被容器B使用，将容器A的服务暴露在宿主机上也不是一种安全的做法。

因此，需要一种，在容器B内直接访问容器A中的服务的方法，而且最好可以使用容器名称，而不是IP地址。

事实上，Docker提供了一种简便的容器互联的方式：--link

## Docker 容器互联：--link

容器的互联是一种让多个容器中的应用进行快速交互的方式。它会在源和接收容器之间创建连接关系，接收容器可以通过容器名快速访问到源容器，而不用指定具体的IP地址。

连接系统依据容器的名称来执行。因此，首先需要自定义一个好记的容器命名。虽然当创建容器的时候，系统默认会分配一个名字，但自定义命名容器有两个好处：

- 自定义的命名，比较好记，一目了然，使用--name标记可以为容器自定义命令。

- 当要连接其他容器时候（即便重启），也可以使用容器名而不用改变

注： 容器的名称是唯一的。如果已经命名了一个叫web的容器，当再次使用web这个名字的时候，需要先docker rm命令删除之前创建的同名容器。

使用--link 参数可以让容器之间安全的进行交互。--link参数的格式为：

--link name:alias

其中，name是要链接的容器的名称，alias是别名。

接下来，使用--link的方式实现容器B到容器A的访问，

```
[root@bogon webapp]# docker run -d --name web training/webapp python app.py
b16cf81a9f03c33a3deb4b1ae35a980650dfb2c4f14d7142a64ef8b744a5199e
[root@bogon webapp]# docker run -itd --link web:pyweb ubuntu
14e82f940f6eef0ffc5ae5528e548340d63c423ff777ac60571da0d4dc945847
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
14e82f940f6e   ubuntu    "/bin/bash"   11 seconds ago   Up 10 seconds             nostalgic_chatelet
[root@bogon webapp]# docker exec -it 14e /bin/bash
root@14e82f940f6e:/# apt-get update 
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]
Get:2 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
Get:3 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [773 kB]
...
root@14e82f940f6e:/# apt-get install wget 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
...
root@14e82f940f6e:/# wget pyweb:5000
--2021-04-17 02:27:42--  http://pyweb:5000/
Resolving pyweb (pyweb)... 172.17.0.6
Connecting to pyweb (pyweb)|172.17.0.6|:5000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12 [text/html]
Saving to: 'index.html'

index.html                               100%[================================================================================>]      12  --.-KB/s    in 0s      

2021-04-17 02:27:42 (3.47 MB/s) - 'index.html' saved [12/12]

root@14e82f940f6e:/# cat index.html 
Hello world!root@14e82f940f6e:/# 
```

在上述过程中，首先使用training/webapp创建了容器web（作为容器A），然后使用ubuntu镜像创建了容器B，在容器B内安装wget。

安装完毕后，访问容器A的web服务，可以看到正确地获取到了index.html文件，且文件中的内容正确。

Docker相当于在两个互联的容器之间创建了一个虚拟通道，而且不用映射他们的端口到宿主主机上。在启动db容器的时候并没有使用-p和-P标记，从而避免了暴露数据库服务端口到外部网络上。

Docker通过两种方式为容器公开连接信息：

- 更新环境变量；

- 更新/etc/hosts文件。

使用env命令来查看 web 容器的环境变量：

```
[root@bogon webapp]# docker run --rm --name test2 --link web:pyweb ubuntu env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=880205e6b0fc
PYWEB_PORT=tcp://172.17.0.6:5000
PYWEB_PORT_5000_TCP=tcp://172.17.0.6:5000
PYWEB_PORT_5000_TCP_ADDR=172.17.0.6
PYWEB_PORT_5000_TCP_PORT=5000
PYWEB_PORT_5000_TCP_PROTO=tcp
PYWEB_NAME=/test2/pyweb
HOME=/root
[root@bogon webapp]# 
```

![image-20210417102915668](\img\Docker容器互联\image-20210417102915668.png)

其中DB_开头的环境变量是提供给 test2 容器连接 web 容器使用的环境变量。前缀采用大写的连接别名。

上述命令中的--rm表示容器运行完毕后，删除容器。

除了环境变量，Docker还添加host信息到 test2 容器的/etc/hosts的文件中。


```
[root@bogon webapp]# docker run --rm --name test2 --link web:pyweb ubuntu cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.6	pyweb b16cf81a9f03 web
172.17.0.8	a520b39cd25a
[root@bogon webapp]# 
```

![image-20210417103038932](\img\Docker容器互联\image-20210417103038932.png)

这里有两个host信息，一个是 test2 容器，test2 容器用自己的id a520b39cd25a作为默认主机名，另一个是 web 容器的IP和主机名(b16cf81a9f03)。

可以连接多个容器到一个容器，例如有容器A1、A2、A3，可以通过--link A1:A1 --link A2:A2 --link A3:A3将A1~A3都连接到容器B。

## Docker 网络

接下来介绍如何使用Docker网络实现多个容器之间的互联互通。

将学习以下内容：
- 如何创建Docker网络。
- 如何将容器连接网络。
- 如何为容器配置DNS。

### 新建网络

下面先创建一个新的 Docker 网络。

```
[root@bogon webapp]# docker network create -d bridge mynet
569a492e8b4dad41569e45b390e26a46223ec82cafc3410b74fe9f04d2c5a89e
[root@bogon webapp]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
bb15a7365662   bridge    bridge    local
7ad633f20839   host      host      local
569a492e8b4d   mynet     bridge    local
566aaab62714   none      null      local
[root@bogon webapp]# 
```

![image-20210417103237819](\img\Docker容器互联\image-20210417103237819.png)

参数说明：

- -d：参数指定 Docker 网络类型，有 bridge、overlay。

其中 overlay 网络类型用于 Swarm mode，本实验中可以先不考虑 overlay 模式。

使用 docker network ls 可查看当前的Docker网络列表。可以看到mynet已成功创建，mynet的网络ID为：569a492e8b4d。

### 连接容器

运行两个ubuntu容器并连接到新建的 mynet 网络:

```
[root@bogon webapp]# docker run -itd --name test1 --network mynet ubuntu 
79c3bc5c7332939b8b6c2632fb8472d9e780874217603a479de31c0b98dbdbf5
[root@bogon webapp]# docker run -itd --name test2 --network mynet ubuntu 
66e8ecbe2fb98c3241cebb9c743141326f8c616a1a8c1772f0c682eb183c2d8b
[root@bogon webapp]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
66e8ecbe2fb9   ubuntu    "/bin/bash"   4 seconds ago   Up 3 seconds             test2
79c3bc5c7332   ubuntu    "/bin/bash"   9 seconds ago   Up 8 seconds             test1
[root@bogon webapp]# 
```

![image-20210417104130497](\img\Docker容器互联\image-20210417104130497.png)

下面通过 ping 来证明 test1 容器和 test2 容器建立了互联关系。

由于test1和test2容器中没有ping命令，可以先制作一个安装了ping和wget这两个工具的Ubuntu镜像，操作过程如下：

```
[root@bogon webapp]# docker run -itd --name test --network mynet ubuntu 
ba89adcfd1d73394846e8ec78c59154cc8a355cc594e474fee6d3b6c5014f16c
[root@bogon webapp]# docker exec -it test /bin/bash
root@ba89adcfd1d7:/# apt-get update
Get:1 http://archive.ubuntu.com/ubuntu focal InRelease [265 kB]
Get:2 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]
...
root@ba89adcfd1d7:/# apt-get install -y iputils-ping wget
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  ca-certificates libcap2 libcap2-bin libpam-cap libpsl5 libssl1.1 openssl publicsuffix
The following NEW packages will be installed:
  ca-certificates iputils-ping libcap2 libcap2-bin libpam-cap libpsl5 libssl1.1 openssl publicsuffix wget
0 upgraded, 10 newly installed, 0 to remove and 9 not upgraded.
...
root@ba89adcfd1d7:/# exit
exit
[root@bogon webapp]# docker ps -l
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
ba89adcfd1d7   ubuntu    "/bin/bash"   2 minutes ago   Up 2 minutes             test
[root@bogon webapp]# docker commit ba8 liuyang/ubuntu_net:v1.0 
sha256:d870b307338875e07767054d77444f2a6f2b26cbf63714f2540dfa53e74b2008
[root@bogon webapp]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
liuyang/ubuntu_net   v1.0      d870b3073388   8 seconds ago   109MB
ubuntu               latest    26b77e58432b   2 weeks ago     72.9MB
training/webapp      latest    6fae60ef3446   5 years ago     349MB
[root@bogon webapp]# 
```

![image-20210417105344702](\img\Docker容器互联\image-20210417105344702.png)

liuyang/ubuntu_net就是集成了ping和wget的新镜像。

接着使用liuyang/ubuntu_net创建两个容器test1和test2，并连接到mynet，然后测试是否互通。

操作命令如下：

```
[root@bogon webapp]# docker run -itd --name test1 --network mynet liuyang/ubuntu_net:v1.0 
acb0415c33935da4b817a3b2a6ffbafb400575ddcaaf04216495379b1338d825
[root@bogon webapp]# docker run -itd --name test2 --network mynet liuyang/ubuntu_net:v1.0
663c8569c921f2d45afefbd8f1e5935696339796d81ed17a990eb839cd3c1881
[root@bogon webapp]# docker exec -it test1 /bin/bash
root@acb0415c3393:/# ping test2
PING test2 (172.18.0.3) 56(84) bytes of data.
64 bytes from test2.mynet (172.18.0.3): icmp_seq=1 ttl=64 time=0.090 ms
64 bytes from test2.mynet (172.18.0.3): icmp_seq=2 ttl=64 time=0.062 ms
64 bytes from test2.mynet (172.18.0.3): icmp_seq=3 ttl=64 time=0.066 ms
^C
--- test2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.062/0.072/0.090/0.012 ms
root@acb0415c3393:/# exit
exit
[root@bogon webapp]# docker exec -it test2 /bin/bash
root@663c8569c921:/# ping test1
PING test1 (172.18.0.2) 56(84) bytes of data.
64 bytes from test1.mynet (172.18.0.2): icmp_seq=1 ttl=64 time=0.043 ms
64 bytes from test1.mynet (172.18.0.2): icmp_seq=2 ttl=64 time=0.061 ms
64 bytes from test1.mynet (172.18.0.2): icmp_seq=3 ttl=64 time=0.081 ms
^C
--- test1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.043/0.061/0.081/0.015 ms
root@663c8569c921:/# 
```



![image-20210417105613346](\img\Docker容器互联\image-20210417105613346.png)

由此可见，连接到mynet的test1和test2之间的网络是互通的。因此，test1 容器和 test2 容器建立了互联关系。

注意：在ping命令执行过程中，按CTRL+C可退出执行。

如果有多个容器之间需要互相连接，推荐使用 Docker Compose，后续实验会详细介绍。

### 设定网段和IP

创建网络时，可以指定网段和子网掩码。

首先把之前的网络和容器清除了。
```
[root@bogon webapp]# docker stop test1 
test1
[root@bogon webapp]# docker stop test2
test2
[root@bogon webapp]# docker rm test1
test1
[root@bogon webapp]# docker rm test2
test2
[root@bogon webapp]# docker network rm mynet
mynet
[root@bogon webapp]# 
```

![image-20210417110239771](\img\Docker容器互联\image-20210417110239771.png)

创建网络，指定网段和掩码。然后，创建容器，并手工指定IP地址：

```
[root@bogon webapp]# docker network create --subnet=172.19.0.0/16 mynet
35d619caeae70a7ef0c2a4f49113a997ddeed45271a3efd9f7e6d8b46fd798c0
[root@bogon webapp]# docker run -itd --name test1 --network mynet --ip 172.19.1.1 liuyang/ubuntu_net:v1.0 
6434fed6fbdbebdb91d1dee52a67de3106c1f79d624f2fb5e6b4779fd907b115
[root@bogon webapp]# docker run -itd --name test2 --network mynet --ip 172.19.1.2 liuyang/ubuntu_net:v1.0
3b98b6c6f48d91d51dd02c3eb0149a0f1e12d73e9284f7030d041bc9e250bbdf
[root@bogon webapp]# 
```

![image-20210417110517166](\img\Docker容器互联\image-20210417110517166.png)

设定mynet的网段为：172.19.0.0/16，为test1和test2分别分配的ip为172.19.1.1和172.19.1.2。

分别查看test1和test2的ip，并测试连通性。

```
[root@bogon webapp]# docker exec -it test1 /bin/bash 
root@6434fed6fbdb:/# hostname -I
172.19.1.1 
root@6434fed6fbdb:/# ping -c 3 test2 
PING test2 (172.19.1.2) 56(84) bytes of data.
64 bytes from test2.mynet (172.19.1.2): icmp_seq=1 ttl=64 time=0.059 ms
64 bytes from test2.mynet (172.19.1.2): icmp_seq=2 ttl=64 time=0.063 ms
64 bytes from test2.mynet (172.19.1.2): icmp_seq=3 ttl=64 time=0.068 ms

--- test2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2001ms
rtt min/avg/max/mdev = 0.059/0.063/0.068/0.003 ms
root@6434fed6fbdb:/# exit
exit
[root@bogon webapp]# docker exec -it test2 /bin/bash
root@3b98b6c6f48d:/# hostname -I
172.19.1.2 
root@3b98b6c6f48d:/# ping -c 3 test1
PING test1 (172.19.1.1) 56(84) bytes of data.
64 bytes from test1.mynet (172.19.1.1): icmp_seq=1 ttl=64 time=0.044 ms
64 bytes from test1.mynet (172.19.1.1): icmp_seq=2 ttl=64 time=0.065 ms
64 bytes from test1.mynet (172.19.1.1): icmp_seq=3 ttl=64 time=0.061 ms

--- test1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 0.044/0.056/0.065/0.009 ms
root@3b98b6c6f48d:/# exit 
exit
[root@bogon webapp]# 
```

![image-20210417110633667](\img\Docker容器互联\image-20210417110633667.png)

可以通过 `hostname -I` 查看ip，也可以通过ifconfig查看，使用ifconfig时会找不到命令的错误。可以通过命令 `apt-get install net-tools` 安装网络工具后解决。

可以看出，test1和test2的地址为设定的地址，而且可以互通。

### 配置 DNS

如果需要为指定的容器设置 DNS，则可以使用以下命令：

```
[root@bogon webapp]# docker run -it --rm --hostname=ubuntu_test liuyang/ubuntu_net:v1.0 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.2	ubuntu_test
[root@bogon webapp]# docker run -it --rm --dns=114.114.114.114 liuyang/ubuntu_net:v1.0 cat /etc/resolv.conf
search localdomain
nameserver 114.114.114.114
[root@bogon webapp]# docker run -it --rm --dns=114.114.114.114 --dns=8.8.8.8 --hostname=ubuntu_test liuyang/ubuntu_net:v1.0 cat /etc/resolv.conf
search localdomain
nameserver 114.114.114.114
nameserver 8.8.8.8
[root@bogon webapp]# 
```

![image-20210417111029673.png](\img\Docker容器互联\image-20210417111029673.png)

参数说明：

- --hostname=HOSTNAME：设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。也可以使用 -h HOSTNAME 。
- --dns=IP_ADDRESS：添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。

如果在容器启动时没有指定 --dns ，Docker 会默认用宿主主机上的 /etc/resolv.conf 来配置容器的 DNS。