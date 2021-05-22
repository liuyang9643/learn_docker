# Docker 容器
---
## 1. Docker 客户端
docker 客户端非常简单 ,可以直接输入 docker 命令来查看到 Docker 客户端的所有命令选项。
```bash
[root@localhost ~]# docker

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Build with BuildKit (Docker Inc., v0.5.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.

To get more help with docker, check out our guides at https://docs.docker.com/go/guides/
[root@localhost ~]# 
```

![image-20210416102058635](\img\Docker容器\image-20210416102058635.png)

可通过命令`docker command --help`深入了解某个Docker命令使用方法。

例如要查看`docker run`的具体使用方法：

```bash
[root@localhost ~]# docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                  Add a custom host-to-IP mapping (host:ip)
  -a, --attach list                    Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16            Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device list       Block IO weight (relative device weight) (default [])
      --cap-add list                   Add Linux capabilities
      --cap-drop list                  Drop Linux capabilities
      --cgroup-parent string           Optional parent cgroup for the container
      --cgroupns string                Cgroup namespace to use (host|private)
                                       'host':    Run the container in the Docker host's cgroup namespace
                                       'private': Run the container in its own private cgroup namespace
                                       '':        Use the cgroup namespace as configured by the
                                                  default-cgroupns-mode option on the daemon (default)
      --cidfile string                 Write the container ID to the file
      --cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int              Limit CPU real-time period in microseconds
      --cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                 CPU shares (relative weight)
      --cpus decimal                   Number of CPUs
      --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                         Run container in background and print container ID
      --detach-keys string             Override the key sequence for detaching a container
      --device list                    Add a host device to the container
      --device-cgroup-rule list        Add a rule to the cgroup allowed devices list
      --device-read-bps list           Limit read rate (bytes per second) from a device (default [])
      --device-read-iops list          Limit read rate (IO per second) from a device (default [])
      --device-write-bps list          Limit write rate (bytes per second) to a device (default [])
      --device-write-iops list         Limit write rate (IO per second) to a device (default [])
      --disable-content-trust          Skip image verification (default true)
      --dns list                       Set custom DNS servers
      --dns-option list                Set DNS options
      --dns-search list                Set custom DNS search domains
      --domainname string              Container NIS domain name
      --entrypoint string              Overwrite the default ENTRYPOINT of the image
  -e, --env list                       Set environment variables
      --env-file list                  Read in a file of environment variables
      --expose list                    Expose a port or a range of ports
      --gpus gpu-request               GPU devices to add to the container ('all' to pass all GPUs)
      --group-add list                 Add additional groups to join
      --health-cmd string              Command to run to check health
      --health-interval duration       Time between running the check (ms|s|m|h) (default 0s)
      --health-retries int             Consecutive failures needed to report unhealthy
      --health-start-period duration   Start period for the container to initialize before starting health-retries countdown (ms|s|m|h) (default 0s)
      --health-timeout duration        Maximum time to allow one check to run (ms|s|m|h) (default 0s)
      --help                           Print usage
  -h, --hostname string                Container host name
      --init                           Run an init inside the container that forwards signals and reaps processes
  -i, --interactive                    Keep STDIN open even if not attached
      --ip string                      IPv4 address (e.g., 172.30.100.104)
      --ip6 string                     IPv6 address (e.g., 2001:db8::33)
      --ipc string                     IPC mode to use
      --isolation string               Container isolation technology
      --kernel-memory bytes            Kernel memory limit
  -l, --label list                     Set meta data on a container
      --label-file list                Read in a line delimited file of labels
      --link list                      Add link to another container
      --link-local-ip list             Container IPv4/IPv6 link-local addresses
      --log-driver string              Logging driver for the container
      --log-opt list                   Log driver options
      --mac-address string             Container MAC address (e.g., 92:d0:c6:0a:29:33)
  -m, --memory bytes                   Memory limit
      --memory-reservation bytes       Memory soft limit
      --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
      --mount mount                    Attach a filesystem mount to the container
      --name string                    Assign a name to the container
      --network network                Connect a container to a network
      --network-alias list             Add network-scoped alias for the container
      --no-healthcheck                 Disable any container-specified HEALTHCHECK
      --oom-kill-disable               Disable OOM Killer
      --oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)
      --pid string                     PID namespace to use
      --pids-limit int                 Tune container pids limit (set -1 for unlimited)
      --platform string                Set platform if server is multi-platform capable
      --privileged                     Give extended privileges to this container
  -p, --publish list                   Publish a container's port(s) to the host
  -P, --publish-all                    Publish all exposed ports to random ports
      --pull string                    Pull image before running ("always"|"missing"|"never") (default "missing")
      --read-only                      Mount the container's root filesystem as read only
      --restart string                 Restart policy to apply when a container exits (default "no")
      --rm                             Automatically remove the container when it exits
      --runtime string                 Runtime to use for this container
      --security-opt list              Security Options
      --shm-size bytes                 Size of /dev/shm
      --sig-proxy                      Proxy received signals to the process (default true)
      --stop-signal string             Signal to stop a container (default "SIGTERM")
      --stop-timeout int               Timeout (in seconds) to stop a container
      --storage-opt list               Storage driver options for the container
      --sysctl map                     Sysctl options (default map[])
      --tmpfs list                     Mount a tmpfs directory
  -t, --tty                            Allocate a pseudo-TTY
      --ulimit ulimit                  Ulimit options (default [])
  -u, --user string                    Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                  User namespace to use
      --uts string                     UTS namespace to use
  -v, --volume list                    Bind mount a volume
      --volume-driver string           Optional volume driver for the container
      --volumes-from list              Mount volumes from the specified container(s)
  -w, --workdir string                 Working directory inside the container
[root@localhost ~]# 
```
![image-20210416102217122](\img\Docker容器\image-20210416102217122.png)



## 2. 容器使用
### 获取镜像
如果本地没有ubuntu镜像，可使用`docker pull`下载：

```
[root@localhost ~]# docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
a70d879fa598: Downloading [>                                                  ]  292.8kB/28.57MB
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
![image-20210416102604705](\img\Docker容器\image-20210416102604705.png)

### 启动容器
以下命令使用 ubuntu 镜像启动一个容器，参数表示以命令行模式进入该容器：

```
[root@localhost ~]# docker run -it ubuntu /bin/bash
root@b08592a626ee:/# 
```
参数说明：
- -i: 交互式操作。
- -t: 终端。
- ubuntu: ubuntu 镜像。
- /bin/bash：放在镜像名后的是命令，这里需要使用交互式方式Shell访问容器，因此用的是 /bin/bash。

![image-20210416102724203](\img\Docker容器\image-20210416102724203.png)

要退出终端，直接输入 `exit`:

```
root@b08592a626ee:/# exit
exit
[root@localhost ~]# 
```

![image-20210416102938080](\img\Docker容器\image-20210416102938080.png)

### 启动已停止运行的容器
查看所有的容器命令如下：

```
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED         STATUS                       PORTS     NAMES
b08592a626ee   ubuntu                    "/bin/bash"              3 minutes ago   Exited (0) 2 minutes ago               quirky_agnesi
48753028240e   liuyang/ubuntu_gcc:v1.0   "/bin/bash"              21 hours ago    Exited (255) 9 minutes ago   80/tcp    optimistic_gates
1a2439910ffc   b055864fc502              "/bin/bash"              21 hours ago    Created                      80/tcp    intelligent_bohr
80a4870e7b94   liuyang/my_ubuntu:v2.0    "/bin/bash"              47 hours ago    Exited (255) 21 hours ago              ubuntu_gcc
4842474755a1   ubuntu:16.04              "/bin/bash"              2 days ago      Exited (0) 2 days ago                  cool_bouman
eb07c2bcd56f   httpd                     "httpd-foreground"       2 days ago      Exited (0) 2 days ago                  httpd_test
f841ddedc1f5   ubuntu:14.04              "/bin/bash"              2 days ago      Exited (0) 2 days ago                  epic_wescoff
f01a76d9b2db   ubuntu:15.10              "/bin/bash"              2 days ago      Exited (0) 2 days ago                  cool_chatterjee
72b83bda899c   ubuntu:16.04              "/bin/bash"              2 days ago      Exited (0) 2 days ago                  boring_shtern
078ebb0bd591   ubuntu:16.04              "/bin/bash -c 'while…"   3 days ago      Exited (137) 3 days ago                clever_goldwasser
fc7558995c87   ubuntu:16.04              "/bin/bash"              3 days ago      Exited (0) 3 days ago                  fervent_engelbart
37319818161a   ubuntu:16.04              "/bin/echo 'hello wo…"   3 days ago      Exited (0) 3 days ago                  romantic_nightingale
[root@localhost ~]# 
```

![image-20210416103024361](\img\Docker容器\image-20210416103024361.png)

**注意**：`docker ps`命令只显示当前正在运行的容器，-a选项表示显示所有容器，包括停止运行的容器。

从 Status 一列中可以看出容器 b08592a626ee已处于停止运行状态。

使用 docker start 启动一个已停止的容器：

```
[root@localhost ~]# docker start b08592a626ee
b08592a626ee
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
b08592a626ee   ubuntu    "/bin/bash"   4 minutes ago   Up 3 seconds             quirky_agnesi
[root@localhost ~]# 
```

![image-20210416103125364](\img\Docker容器\image-20210416103125364.png)

可以看出，容器 b08592a626ee已顺利启动。

### 后台运行
在大部分的场景下，docker 的服务是在后台运行的，可以通过 -d 指定容器的运行模式。

```
[root@localhost ~]# docker run -itd --name ubuntu_test ubuntu /bin/bash
9bc310b69280c30b4338e6d9d6429ec03cda9b8d769c28a8f6da3ba56b5fd4bc
[root@localhost ~]# 
```

![image-20210416103240097](\img\Docker容器\image-20210416103240097.png)

**注意**：加了 -d 参数默认不会进入容器，可通过 `docker exec`命令进入容器（后续详细介绍）。

### 停止一个容器

停止容器的命令为：docker stop <容器 ID>

示例如下：

```
[root@localhost ~]# docker stop b08
b08
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
9bc310b69280   ubuntu    "/bin/bash"   About a minute ago   Up About a minute             ubuntu_test
[root@localhost ~]# docker restart b08
b08
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
9bc310b69280   ubuntu    "/bin/bash"   2 minutes ago   Up 2 minutes             ubuntu_test
b08592a626ee   ubuntu    "/bin/bash"   8 minutes ago   Up 1 second              quirky_agnesi
[root@localhost ~]# 
```

![image-20210416103455548](\img\Docker容器\image-20210416103455548.png)

除了`docker start`外，停止的容器也可以通过命令`docker restart <容器 ID>`重启：

### 进入容器
在使用 -d 参数时，容器启动后会在后台运行。此时想要进入容器，可通过以下命令进入：
- docker attach
- docker exec

推荐使用 `docker exec` 命令，因为使用该命令退出容器终端后，不会导致容器停止运行。
#### attach 命令
下面演示了 `docker attach` 命令的使用。

```
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS         PORTS     NAMES
9bc310b69280   ubuntu    "/bin/bash"   4 minutes ago    Up 4 minutes             ubuntu_test
b08592a626ee   ubuntu    "/bin/bash"   10 minutes ago   Up 2 minutes             quirky_agnesi
[root@localhost ~]# docker attach 9bc
root@9bc310b69280:/# exit
exit
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS         PORTS     NAMES
b08592a626ee   ubuntu    "/bin/bash"   11 minutes ago   Up 2 minutes             quirky_agnesi
[root@localhost ~]# 
```

![image-20210416103813029](\img\Docker容器\image-20210416103813029.png)

可以看出，使用 docker attach 命令进入容器后，如果从该容器退出，会导致容器停止运行。
#### exec 命令
下面演示了 `docker exec` 命令的使用。

```
[root@localhost ~]# docker start  9bc
9bc
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
9bc310b69280   ubuntu    "/bin/bash"   14 minutes ago   Up 5 seconds              ubuntu_test
b08592a626ee   ubuntu    "/bin/bash"   20 minutes ago   Up 11 minutes             quirky_agnesi
[root@localhost ~]# docker exec -it 9bc /bin/bash
root@9bc310b69280:/# exit
exit
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
9bc310b69280   ubuntu    "/bin/bash"   14 minutes ago   Up 25 seconds             ubuntu_test
b08592a626ee   ubuntu    "/bin/bash"   20 minutes ago   Up 12 minutes             quirky_agnesi
[root@localhost ~]# 
```

![image-20210416104702765](\img\Docker容器\image-20210416104702765.png)

可以看出，使用 docker exec命令进入容器后，从容器退出不会导致容器停止运行。因此，推荐使用 docker exec 命令。

请使用 docker exec --help 命令查看 docker exec 命令的更多参数说明。

### 导出和导入容器

#### 导出容器

如果要导出本地某个容器，可以使用 docker export 命令。

```
[root@localhost ~]# docker export 9bc -o ubuntu_test.tar
[root@localhost ~]# ls -l ubuntu_test.tar
-rw-------. 1 root root 75287552 4月  16 10:49 ubuntu_test.tar
[root@localhost ~]# 
```

!![image-20210416104941579](\img\Docker容器\image-20210416104941579.png)

导出容器 9bc310b69280快照到本地文件 ubuntu_test.tar ，从上图中可以看出桌面上生成了文件 ubuntu_test.tar 。

#### 导入容器快照

可以使用 docker import 从容器快照文件中再导入为镜像，以下命令将快照文件 ubuntu_test.tar 导入为镜像 cg/my_ubuntu:v1:

```
[root@localhost ~]# docker import ubuntu_test.tar cg/my_ubuntu:v1.0
sha256:5e4598dfb30e5f478267625756bead6a0af41f87d24b63d288beaf0a952a6d12
[root@localhost ~]# docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
cg/my_ubuntu         v1.0      5e4598dfb30e   4 seconds ago   72.9MB
liuyang/ubuntu_gcc   dev       e6425f492f18   21 hours ago    262MB
liuyang/ubuntu_gcc   v1.0      e6425f492f18   21 hours ago    262MB
liuyang/my_ubuntu    v2.0      6aeab10bf9b6   2 days ago      261MB
httpd                latest    0b932df43057   5 days ago      138MB
ubuntu               latest    26b77e58432b   13 days ago     72.9MB
ubuntu               16.04     f6f49faac5cf   3 weeks ago     132MB
ubuntu               14.04     13b66b487594   3 weeks ago     197MB
ubuntu               15.10     9b9cb95443b5   4 years ago     137MB
training/webapp      latest    6fae60ef3446   5 years ago     349MB
[root@localhost ~]# 
```

![image-20210416105117352](\img\Docker容器\image-20210416105117352.png)

### 删除容器
删除容器使用 docker rm 命令：

```
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS                        PORTS     NAMES
9bc310b69280   ubuntu                    "/bin/bash"              20 minutes ago   Up 6 minutes                            ubuntu_test
b08592a626ee   ubuntu                    "/bin/bash"              26 minutes ago   Up 17 minutes                           quirky_agnesi
48753028240e   liuyang/ubuntu_gcc:v1.0   "/bin/bash"              21 hours ago     Exited (255) 32 minutes ago   80/tcp    optimistic_gates
1a2439910ffc   b055864fc502              "/bin/bash"              21 hours ago     Created                       80/tcp    intelligent_bohr
80a4870e7b94   liuyang/my_ubuntu:v2.0    "/bin/bash"              47 hours ago     Exited (255) 21 hours ago               ubuntu_gcc
4842474755a1   ubuntu:16.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   cool_bouman
eb07c2bcd56f   httpd                     "httpd-foreground"       2 days ago       Exited (0) 2 days ago                   httpd_test
f841ddedc1f5   ubuntu:14.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   epic_wescoff
f01a76d9b2db   ubuntu:15.10              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   cool_chatterjee
72b83bda899c   ubuntu:16.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   boring_shtern
078ebb0bd591   ubuntu:16.04              "/bin/bash -c 'while…"   3 days ago       Exited (137) 3 days ago                 clever_goldwasser
fc7558995c87   ubuntu:16.04              "/bin/bash"              3 days ago       Exited (0) 3 days ago                   fervent_engelbart
37319818161a   ubuntu:16.04              "/bin/echo 'hello wo…"   3 days ago       Exited (0) 3 days ago                   romantic_nightingale
[root@localhost ~]# docker stop 9bc
9bc
[root@localhost ~]# docker rm 9bc
9bc
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS                        PORTS     NAMES
b08592a626ee   ubuntu                    "/bin/bash"              26 minutes ago   Up 18 minutes                           quirky_agnesi
48753028240e   liuyang/ubuntu_gcc:v1.0   "/bin/bash"              21 hours ago     Exited (255) 32 minutes ago   80/tcp    optimistic_gates
1a2439910ffc   b055864fc502              "/bin/bash"              21 hours ago     Created                       80/tcp    intelligent_bohr
80a4870e7b94   liuyang/my_ubuntu:v2.0    "/bin/bash"              47 hours ago     Exited (255) 21 hours ago               ubuntu_gcc
4842474755a1   ubuntu:16.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   cool_bouman
eb07c2bcd56f   httpd                     "httpd-foreground"       2 days ago       Exited (0) 2 days ago                   httpd_test
f841ddedc1f5   ubuntu:14.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   epic_wescoff
f01a76d9b2db   ubuntu:15.10              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   cool_chatterjee
72b83bda899c   ubuntu:16.04              "/bin/bash"              2 days ago       Exited (0) 2 days ago                   boring_shtern
078ebb0bd591   ubuntu:16.04              "/bin/bash -c 'while…"   3 days ago       Exited (137) 3 days ago                 clever_goldwasser
fc7558995c87   ubuntu:16.04              "/bin/bash"              3 days ago       Exited (0) 3 days ago                   fervent_engelbart
37319818161a   ubuntu:16.04              "/bin/echo 'hello wo…"   3 days ago       Exited (0) 3 days ago                   romantic_nightingale
[root@localhost ~]# 
```

![image-20210416105318568](\img\Docker容器\image-20210416105318568.png)

可见通过 docker rm 删除容器9bc310b69280后，在容器列表中已经移除了该容器。

通过 `docker container prune` 命令可将处于终止状态的容器全部删除。

以下操作演示了该命令的使用，首先使用 ubuntu 镜像创建三个容器，并将其终止，


```
[root@localhost ~]# docker run -itd --name ubuntu_test1 ubuntu 
ab1da71cec8e01fc46d194ea1d5e518eac68fbe01be953f097c7517638a06096
[root@localhost ~]# docker run -itd --name ubuntu_test2 ubuntu 
e2e2e722d937c7f0c1f73d6a84f5c7b8036314226644e81744d8f738da00f70b
[root@localhost ~]# docker run -itd --name ubuntu_test3 ubuntu 
a53dcd1a135eb36ec573639fd9052101ec48456e10c40a10199df5d965311cb8
[root@localhost ~]# docker stop ubuntu_test1
ubuntu_test1
[root@localhost ~]# docker stop ubuntu_test2
ubuntu_test2
[root@localhost ~]# docker stop ubuntu_test3
ubuntu_test3
[root@localhost ~]# 
```

![image-20210416105449554](\img\Docker容器\image-20210416105449554.png)

然后使用`docker container prune`删除停止的容器，操作如下：

```
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED              STATUS                        PORTS     NAMES
a53dcd1a135e   ubuntu                    "/bin/bash"              56 seconds ago       Exited (0) 40 seconds ago               ubuntu_test3
e2e2e722d937   ubuntu                    "/bin/bash"              About a minute ago   Exited (0) 42 seconds ago               ubuntu_test2
ab1da71cec8e   ubuntu                    "/bin/bash"              About a minute ago   Exited (0) 43 seconds ago               ubuntu_test1
b08592a626ee   ubuntu                    "/bin/bash"              28 minutes ago       Up 20 minutes                           quirky_agnesi
48753028240e   liuyang/ubuntu_gcc:v1.0   "/bin/bash"              21 hours ago         Exited (255) 35 minutes ago   80/tcp    optimistic_gates
1a2439910ffc   b055864fc502              "/bin/bash"              21 hours ago         Created                       80/tcp    intelligent_bohr
80a4870e7b94   liuyang/my_ubuntu:v2.0    "/bin/bash"              47 hours ago         Exited (255) 21 hours ago               ubuntu_gcc
4842474755a1   ubuntu:16.04              "/bin/bash"              2 days ago           Exited (0) 2 days ago                   cool_bouman
eb07c2bcd56f   httpd                     "httpd-foreground"       2 days ago           Exited (0) 2 days ago                   httpd_test
f841ddedc1f5   ubuntu:14.04              "/bin/bash"              2 days ago           Exited (0) 2 days ago                   epic_wescoff
f01a76d9b2db   ubuntu:15.10              "/bin/bash"              2 days ago           Exited (0) 2 days ago                   cool_chatterjee
72b83bda899c   ubuntu:16.04              "/bin/bash"              2 days ago           Exited (0) 2 days ago                   boring_shtern
078ebb0bd591   ubuntu:16.04              "/bin/bash -c 'while…"   3 days ago           Exited (137) 3 days ago                 clever_goldwasser
fc7558995c87   ubuntu:16.04              "/bin/bash"              3 days ago           Exited (0) 3 days ago                   fervent_engelbart
37319818161a   ubuntu:16.04              "/bin/echo 'hello wo…"   3 days ago           Exited (0) 3 days ago                   romantic_nightingale
[root@localhost ~]# docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
a53dcd1a135eb36ec573639fd9052101ec48456e10c40a10199df5d965311cb8
e2e2e722d937c7f0c1f73d6a84f5c7b8036314226644e81744d8f738da00f70b
ab1da71cec8e01fc46d194ea1d5e518eac68fbe01be953f097c7517638a06096
48753028240e65905b30e7f6f96fa6e35545d05d75ea016a8b313fc375c59ac0
1a2439910ffc1134a32390a98164cd632c0ee227320c550dd6fc3aca5b4894dd
80a4870e7b943852dc565814ee10b9f89ee74489db9cf923905fadb9e9c730bd
4842474755a112c5cb5fd7a523b53bbbb915ed81a12460957585358cc2ff2de0
eb07c2bcd56fa89634c1138a2c88b45d913f9dee66c871225a2f4222ed3cd8e5
f841ddedc1f57421d74856ec5da9b6e4eca1a0d9cd879e35a8028d80aa158e03
f01a76d9b2dbdb3af5f917ac3329f22a5375d22f3ce90a403a1a13b6fbcf903b
72b83bda899c23affea6b5582ec7beb0ac14f7edfed16f84870ad8ee97d79f06
078ebb0bd591faa830eb68ed2e39347f315057703ae6624c8305e7ff97632b53
fc7558995c878c0f1c82918ec4922a7c033a4a3192648ea34d9a1e1d1c9d769f
37319818161aaa7dcd31ef4d80ff4424842ad91c0e65690e501c1b0371ec83bd

Total reclaimed space: 129.6MB
[root@localhost ~]# 
```
![image-20210416105544467](\img\Docker容器\image-20210416105544467.png)

首先使用 `docker ps -a` 查看所有容器状态，可以看到ubuntu_test1、ubuntu_test2、ubuntu_test3这三个容器都已经处于终止状态。

`docker container prune` 命令执行后，再使用 `docker ps -a` 查看，可以发现ubuntu_test1、ubuntu_test2、ubuntu_test3这三个容器都已删除。

**注意**：实验环境中还存在其他已经停止运行的容器，这些容器也被 `docker container prune` 命令删除。这个容器是运行其他实验时遗留的容器，如果你的环境中不存在该容器也不用担心。

---

## 3. 实战：运行Web应用

前面运行的容器并没有特别的用处。

接下来尝试使用 docker 构建一个 web 应用程序。

### 实战环境准备

本次实战中，将在 docker 容器中基于 Python Flask 运行一个简单的web应用。

本实验项目的目录为：/mnt/course/docker/webapp，所需运行的程序的目录为/mnt/course/docker/webapp/webapp。因此，首先将工作目录切换为该目录，

```
[root@localhost ~]# cd /mnt/docker/webapp/webapp/
[root@localhost webapp]# pwd
/mnt/docker/webapp/webapp
[root@localhost webapp]# ls
0.0.0.0_5000  app.py  Procfile  requirements.txt  tests.py
[root@localhost webapp]# 
```

![image-20210416140222838](\img\Docker容器\image-20210416140222838.png)

使用 `docker pull` 命令下载webapp镜像，webapp镜像中封装了Python Flask环境，

```
[root@localhost webapp]# docker pull training/webapp
Using default tag: latest
latest: Pulling from training/webapp
Image docker.io/training/webapp:latest uses outdated schema1 manifest format. Please upgrade to a schema2 image for better future compatibility. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/
e190868d63f8: Already exists 
909cd34c6fd7: Already exists 
0b9bfabab7c1: Already exists 
a3ed95caeb02: Already exists 
10bbbc0fc0ff: Already exists 
fca59b508e9f: Already exists 
e7ae2541b15b: Already exists 
9dd97ef58ce9: Already exists 
a4c1b0cb7af7: Already exists 
Digest: sha256:06e9c1983bd6d5db5fba376ccd63bfa529e8d02f23d5079b8f74a616308fb11d
Status: Image is up to date for training/webapp:latest
docker.io/training/webapp:latest
[root@localhost webapp]# 
```

![image-20210416140315891](\img\Docker容器\image-20210416140315891.png)

本应用的逻辑实现位于app.py文件中，本应用的目的是在浏览器的页面上输出：Hello World，app.py的代码内容如下：

```
[root@localhost webapp]# cat app.py 
import os

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    provider = str(os.environ.get('PROVIDER', 'world'))
    return 'Hello '+provider+'!'

if __name__ == '__main__':
    # Bind to PORT if defined, otherwise default to 5000.
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port)
[root@localhost webapp]# 
```

![image-20210416140408172](\img\Docker容器\image-20210416140408172.png)

使用webapp容器运行应用，

```
[root@localhost webapp]# docker run -d -P training/webapp python app.py
5ffee41b99a6ef06374bf31ee02f0742c24115a76e5c8f33aedc8f4631f412a3
[root@localhost webapp]# 
```

![image-20210416140524141](\img\Docker容器\image-20210416140524141.png)

参数说明:

- -d：让容器在后台运行。

- -P：将容器内部使用的网络端口映射到宿主机上。

### 查看 WEB 应用容器

使用 docker ps 来查看正在运行的容器：

```
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
5ffee41b99a6   training/webapp   "python app.py"   30 seconds ago   Up 28 seconds   0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# 
```

![image-20210416140611027](\img\Docker容器\image-20210416140611027.png)

可以看出，这里多了端口信息，

```
PORTS
0.0.0.0:49153->5000/tcp
```

Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 49153上。

这时可以通过浏览器访问WEB应用，打开浏览器。在地址栏输入：IP:32768，可以看到如下图所示的页面，

![image-20210416141005351](\img\Docker容器\image-20210416141005351.png)

在页面上显示了 Hello world!

也可以通过 -p 参数指定所要映射的端口，

```
[root@localhost webapp]# docker run -d -p 5000:5000 training/webapp python app.py
1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df
[root@localhost webapp]# 
```

![image-20210416141057699](\img\Docker容器\image-20210416141057699.png)

docker ps查看正在运行的容器

```
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
1e5a87f018cf   training/webapp   "python app.py"   40 seconds ago   Up 39 seconds   0.0.0.0:5000->5000/tcp    heuristic_kapitsa
5ffee41b99a6   training/webapp   "python app.py"   6 minutes ago    Up 6 minutes    0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# 
```

![image-20210416141130831](\img\Docker容器\image-20210416141130831.png)


可以看到，容器内部的 5000 端口映射到本地主机的 5000 端口上。

在浏览器中，使用5000端口访问服务：

![image-20210416141145884](\img\Docker容器\image-20210416141145884.png)

### 网络端口的快捷方式

通过 docker ps 命令可以查看到容器的端口映射，docker 还提供了另一个快捷方式 docker port，使用 docker port 可以查看指定 （ID 或者名字）容器的某个确定端口映射到宿主机的端口号。

上面创建的 web 应用容器 ID 为 1e5a87f018cf名字为 heuristic_kapitsa。

可以使用 docker port 1e5a87f018cf或 docker port heuristic_kapitsa来查看容器端口的映射情况。

```
[root@localhost webapp]# docker port 1e5
5000/tcp -> 0.0.0.0:5000
[root@localhost webapp]# docker port heuristic_kapitsa 
5000/tcp -> 0.0.0.0:5000
[root@localhost webapp]# 

```

![image-20210416142750498](\img\Docker容器\image-20210416142750498.png)

### 查看 WEB 应用程序日志

docker logs [ID或者名字] 可以查看容器内部的标准输出。

```
[root@localhost webapp]# docker logs 1e5
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET / HTTP/1.1" 200 -
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET /favicon.ico HTTP/1.1" 404 -
[root@localhost webapp]# docker logs heuristic_kapitsa 
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET / HTTP/1.1" 200 -
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET /favicon.ico HTTP/1.1" 404 -
[root@localhost webapp]# 
```

![image-20210416142835848](\img\Docker容器\image-20210416142835848.png)

可以使用 -f 选项，这样 docker logs 会像 tail -f 一样来输出容器内部的标准输出。


```
[root@localhost webapp]# docker logs -f 1e5 
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET / HTTP/1.1" 200 -
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET /favicon.ico HTTP/1.1" 404 -
^C
[root@localhost webapp]# docker logs -f heuristic_kapitsa 
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET / HTTP/1.1" 200 -
192.168.20.1 - - [16/Apr/2021 06:11:39] "GET /favicon.ico HTTP/1.1" 404 -
^C
[root@localhost webapp]# 
```

![image-20210416142923934](\img\Docker容器\image-20210416142923934.png)

按CTRL+C可退出该命令。

从上面，可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。

### 查看WEB应用程序容器的进程

使用 `docker top` 查看容器内部运行的进程

```
[root@localhost webapp]# docker top 1e5
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                6906                6886                0                   14:10               ?                   00:00:00            python app.py
[root@localhost webapp]# 
```

![image-20210416143000544](\img\Docker容器\image-20210416143000544.png)

### 检查 WEB 应用程序

使用 docker inspect 查看 Docker 的底层信息，该命令会返回一个记录了 Docker 容器配置和状态信息的JSON 文件。

```
[root@localhost webapp]# docker inspect 1e5
[
    {
        "Id": "1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df",
        "Created": "2021-04-16T06:10:24.627169492Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 6906,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-04-16T06:10:25.356702954Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:6fae60ef344644649a39240b94d73b8ba9c67f898ede85cf8e947a887b3e6557",
        "ResolvConfPath": "/var/lib/docker/containers/1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df/hostname",
        "HostsPath": "/var/lib/docker/containers/1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df/hosts",
        "LogPath": "/var/lib/docker/containers/1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df/1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df-json.log",
        "Name": "/heuristic_kapitsa",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "5000/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "5000"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/2ae1ead9312f7cb1ef1cae93170b71b29a417cd72c023c0fa0ddc33b6f363f6b-init/diff:/var/lib/docker/overlay2/7df0ef54b414de4990df9bd66b47a1648c5691b4b6efaec70ab79ed27ab2aa21/diff:/var/lib/docker/overlay2/db62e4430836aedb8db539ad7096c34250aef3a3d782052325843f84759a289d/diff:/var/lib/docker/overlay2/53cc8675979c9ca807224b3fe7d70ca01515b6fbce12f5945285c988002afc1d/diff:/var/lib/docker/overlay2/c7bb8a29564302e13c88688ecbe1e48055d7532c323bad27067c30a274c175bc/diff:/var/lib/docker/overlay2/4bc22811c59503e408c7bb7b05f1921712c623f8beb0529e97d1d9e67b0c8da9/diff:/var/lib/docker/overlay2/ef8cc2fb7f2fea6920b70ea25462322ed8959250009cf131dfa800f089b02742/diff:/var/lib/docker/overlay2/ac5fe4c26c17491edf68e759f26341be4aacc4fca51a7601c268a1a48b238396/diff:/var/lib/docker/overlay2/baf6c9b088eccb847a7a2472d9d2cb07517f48f8b22181cce49ec2abad602460/diff:/var/lib/docker/overlay2/a6693875922e6f97e023d7af4c564e9fa7ada4ce7815e489005db6a046214fbf/diff:/var/lib/docker/overlay2/070f9f1bb8b69cdd1537c37cd5489f8d89ee8c1dc879869eee09578e327c8c9b/diff:/var/lib/docker/overlay2/cd9e85f58ac80e8ab1dc79505e370d9809780195e748f4c8115cee7b5229645f/diff:/var/lib/docker/overlay2/0394b5afbddd806b93814f6d022a2ef40d9e7b4153c046501a402fd53a6e347d/diff:/var/lib/docker/overlay2/995f799defd056a4d7abf3182682928c906d720f171a44a45ea1945ad43aff5a/diff",
                "MergedDir": "/var/lib/docker/overlay2/2ae1ead9312f7cb1ef1cae93170b71b29a417cd72c023c0fa0ddc33b6f363f6b/merged",
                "UpperDir": "/var/lib/docker/overlay2/2ae1ead9312f7cb1ef1cae93170b71b29a417cd72c023c0fa0ddc33b6f363f6b/diff",
                "WorkDir": "/var/lib/docker/overlay2/2ae1ead9312f7cb1ef1cae93170b71b29a417cd72c023c0fa0ddc33b6f363f6b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "1e5a87f018cf",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "5000/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "python",
                "app.py"
            ],
            "Image": "training/webapp",
            "Volumes": null,
            "WorkingDir": "/opt/webapp",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "788f4f62c32cac6b8101b54fa186f357d5e8785a2bef2fdc13b2d609c3bceb0e",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "5000/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "5000"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/788f4f62c32c",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "e29f9a2fbdd78aceb420f49c764573e2aaefac8441a1ddd58e0c89da88d1eb93",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.4",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:04",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "bb15a7365662dbd070aa2d494ca450dea09197243045e5d08307d3af3cb195bf",
                    "EndpointID": "e29f9a2fbdd78aceb420f49c764573e2aaefac8441a1ddd58e0c89da88d1eb93",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.4",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:04",
                    "DriverOpts": null
                }
            }
        }
    }
]
[root@localhost webapp]# 
```

![image-20210416143052461](\img\Docker容器\image-20210416143052461.png)

### 停止 WEB 应用容器

使用docker stop停止应用，

```
[root@localhost webapp]# docker stop 1e5
1e5
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
5ffee41b99a6   training/webapp   "python app.py"   26 minutes ago   Up 26 minutes   0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# 
```

可以看到该容器已停止。

再次使用浏览器访问，

![image-20210416143208619](\img\Docker容器\image-20210416143208619.png)

可以发现服务已经停止运行。

### 重启WEB应用容器

可通过docker start <容器ID> 或者 docker restart <容器ID>启动停止运行的容器，

已经停止的容器，可以使用命令 docker start 来启动。

```
[root@localhost webapp]# docker start 1e5
1e5
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
1e5a87f018cf   training/webapp   "python app.py"   23 minutes ago   Up 1 second     0.0.0.0:5000->5000/tcp    heuristic_kapitsa
5ffee41b99a6   training/webapp   "python app.py"   28 minutes ago   Up 28 minutes   0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# docker stop 1e5
1e5
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
5ffee41b99a6   training/webapp   "python app.py"   29 minutes ago   Up 29 minutes   0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# docker restart 1e5
1e5
[root@localhost webapp]# docker ps
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                     NAMES
1e5a87f018cf   training/webapp   "python app.py"   23 minutes ago   Up 2 seconds    0.0.0.0:5000->5000/tcp    heuristic_kapitsa
5ffee41b99a6   training/webapp   "python app.py"   29 minutes ago   Up 29 minutes   0.0.0.0:49153->5000/tcp   flamboyant_yalow
b08592a626ee   ubuntu            "/bin/bash"       4 hours ago      Up 4 hours                                quirky_agnesi
[root@localhost webapp]# 
```

![image-20210416143440285](\img\Docker容器\image-20210416143440285.png)

容器启动后，再次使用chrome浏览器访问Web服务：

![image-20210416143454778](\img\Docker容器\image-20210416143454778.png)

可见，服务已经可以正常访问。

可以使用命令 `docker ps -l` 查询最后一次创建的容器：

```
[root@localhost webapp]# docker ps -l
CONTAINER ID   IMAGE             COMMAND           CREATED          STATUS          PORTS                    NAMES
1e5a87f018cf   training/webapp   "python app.py"   24 minutes ago   Up 50 seconds   0.0.0.0:5000->5000/tcp   heuristic_kapitsa
[root@localhost webapp]# 
```

![image-20210416143530237](\img\Docker容器\image-20210416143530237.png)

### 移除WEB应用容器

可使用 docker rm 命令删除容器，例如对于容器ca1fb988c6dc，

```
[root@localhost webapp]# docker rm 1e5
Error response from daemon: You cannot remove a running container 1e5a87f018cfd5549f690deddf94a29f6f3d3333a2292c6266da29163be659df. Stop the container before attempting removal or force remove
[root@localhost webapp]# docker stop 1e5
1e5
[root@localhost webapp]# docker rm 1e5
1e5
[root@localhost webapp]# 
```

![image-20210416143647579](\img\Docker容器\image-20210416143647579.png)

删除容器时，容器必须是停止状态，否则会报错。