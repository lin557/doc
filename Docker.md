# docker说明



Docker 是一个开源的应用容器引擎，基于 [Go 语言](https://www.runoob.com/go/go-tutorial.html) 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。



想要玩docker，window至少是window 10 21th以上，因为docker是跑在linux上的，只有win10上有wsl才能跑docker。或使用linux。当然也可以在win7上安装虚拟机，将docker安装在虚拟机上的linux系统中。



## 1. 基本概念



### 1.1 镜像（Image）

Docker 镜像可以看作是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。可以理解为一个光盘，或ISO镜像，是一个**只读**模板。



### 1.2 容器（Container）

Docker 容器是镜像的实例化。一个Docker容器包含了所有的某个应用运行所需要的环境。每一个 Docker 容器都是从 Docker 镜像创建的。Docker 容器可以运行、开始、停止、移动和删除。每一个 Docker 容器都是独立和安全的应用平台，Docker 容器是 Docker 的运行部分。



### 1.3 仓库（Repository）

Docker 仓库用来保存镜像。Docker 仓库也有公有和私有的概念。公有的 Docker 仓库名字是 Docker Hub。Docker Hub 提供了庞大的镜像集合供使用。这些镜像可以是自己创建，或者在别人的镜像基础上创建。Docker 仓库是 Docker 的分发部分。



## 2. 命令大全



### 2.1 镜像操作



#### 2.1.1 pull 拉取镜像

从镜像仓库中拉取或者更新指定镜像

```sh
# OPTIONS说明：
# -a :拉取所有 tagged 镜像
# --disable-content-trust :忽略镜像的校验,默认开启
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

示例
```sh
docker pull transcodegroup/gateway-rtpserver:21.11.2
```



#### 2.1.2 push 上传镜像

将本地的镜像上传到镜像仓库,要先登陆到镜像仓库

```sh
# OPTIONS说明：
# --disable-content-trust :忽略镜像的校验,默认开启
docker push [OPTIONS] NAME[:TAG]
```

示例
```sh
docker push transcodegroup/gateway-rtpserver:21.11.2
```



#### 2.1.3 search 查找镜像

从Docker Hub查找镜像

```sh
# OPTIONS说明：
# --automated :只列出 automated build类型的镜像；
# --no-trunc :显示完整的镜像描述；
# -f <过滤条件>:列出收藏数不小于指定值的镜像。
docker search [OPTIONS] TERM
```

示例

```sh
docker search -f stars=10 java
```

```sh
# NAME: 镜像仓库源的名称
# DESCRIPTION: 镜像的描述
# OFFICIAL: 是否 docker 官方发布
# stars: 类似 Github 里面的 star，表示点赞、喜欢的意思。
# AUTOMATED: 自动构建。

NAME                  DESCRIPTION                           STARS   OFFICIAL   AUTOMATED
java                  Java is a concurrent, class-based...   1037    [OK]       
anapsix/alpine-java   Oracle Java 8 (and 7) with GLIBC ...   115                [OK]
develar/java                                                 46                 [OK]
isuper/java-oracle    This repository contains all java...   38                 [OK]
lwieske/java-8        Oracle Java 8 Container - Full + ...   27                 [OK]
nimmis/java-centos    This is docker images of CentOS 7...   13                 [OK]
```



#### 2.1.4 images 查看本地镜像

列出本地镜像。

```sh
# OPTIONS说明：
# -a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
# --digests :显示镜像的摘要信息；
# -f :显示满足条件的镜像；
# --format :指定返回值的模板文件；
# --no-trunc :显示完整的镜像信息；
# -q :只显示镜像ID。
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

示例

```sh
# 查看本地镜像列表。
docker images
```

```sh
# 列出本地镜像中REPOSITORY为ubuntu的镜像列表。
docker images ubuntu

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              14.04               90d5884b1ee0        9 weeks ago         188 MB
ubuntu              15.10               4e3b13c8a266        3 months ago        136.3 MB
```



#### 2.1.5 rmi 删除本地镜像

删除本地一个或多个镜像。

```sh
# OPTIONS说明：
# -f :强制删除；
# --no-prune :不移除该镜像的过程镜像，默认移除；
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

示例

```sh
# 强制删除本地镜像 runoob/ubuntu:v4。
docker rmi -f runoob/ubuntu:v4
```

```sh
# 用镜像id进行删除
docker rmi 90d5884b1ee0
```



#### 2.1.6 prune

删除不再使用的 docker 对象

```sh
# 删除所有未被 tag 标记和未被容器使用的镜像
docker image prune
```

```sh
# 删除所有未被容器使用的镜像
docker image prune -a
```

```sh
# 删除所有停止运行的容器
docker container prune
```

```sh
# 删除所有未被挂载的卷
docker volume prune
```

```sh
# 删除所有网络
docker network prune
```

```sh
# 删除 docker 所有资源
docker system prune
```





### 2.2 容器操作







## 3. 打包方式

这里只从java springboot的打包进行说明





## 4. 基础镜像



### 4.1 openjdk

常用的是 openjdk:8-jdk-alpine

官方说明: https://github.com/docker-library/docs/blob/d4f015a4a99883c6b8691ec6aaf24a74cd02916a/openjdk/README.md



#### 4.1.1 stretch关键字

以8-jre-stretch这个tag为例，其中的stretch表明这个镜像的操作系统是debian9，这是debian的一个稳定版本



#### 4.1.2 alpine关键字

以13-ea-19-jdk-alpine3.9这个tag为例，其中的alpine表明镜像的操作系统是alpine linux，alpine linux本身很小，alpine镜像的大小是5M左右



#### 4.1.3 oraclelinux7关键字

以13-ea-oraclelinux7这个tag为例，其中的oraclelinux7表明镜像的操作系统是Oracle Linux 7，从jdk12开始，openjdk官方开始提供基于Oracle Linux 7的jdk镜像



#### 4.1.4 slim关键字

以8-jre-slim这个tag为例，其中的slim表明当前的jre并非标准jre版本，而是headless版本，该版本的特点是去掉了UI、键盘、鼠标相关的库，因此更加精简，适合服务端应用使用，官方的建议是除非有明确的体积限制时再考虑使用该版本；



#### 4.1.5 ea关键字

以13-ea-19-jdk-alpine3.9这个tag为例，其中的ea的意思是"Early Access"，这里代表jdk13正是发布之前的预览版本，该版本带有新特性并且修复了若干bug，但毕竟是预览版，质量还未达到release要求，不推荐生产环境使用；



### 4.2 srs

srs 是一个简单高效的实时视频服务器，支持RTMP/WebRTC/HLS/HTTP-FLV/SRT。



#### 4.2.1 拉取镜像

````sh
# 拉取官方的最新版本的镜像 目前稳定版是3
# SRS 4.0还没有发布
docker pull ossrs/srs:3
````



#### 4.2.2 运行容器

```
docker run --rm -p 1935:1935 -p 1985:1985 -p 8080:8080 ossrs/srs:3
```



#### 4.2.3 配置说明



### 4.3 nginx

Nginx 是一个高性能的 HTTP 和反向代理 web 服务器，同时也提供了 IMAP/POP3/SMTP 服务 。



#### 4.3.1 拉取镜像

```sh
# 拉取官方的最新版本的镜像
docker pull nginx:latest
```



#### 4.3.2 运行容器

```
docker run --name my-nginx -p 8080:80 -d nginx
```



#### 4.3.3 配置说明





### 4.4 redis

Redis 是一个开源的使用 ANSI C 语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value 的 NoSQL 数据库，并提供多种语言的 API。



#### 4.4.1 拉取镜像

```sh
# 拉取官方的最新版本的镜像
docker pull redis:latest
```



#### 4.4.2 运行容器

```
docker run -itd --name my-redis -p 6379:6379 redis
```



####　4.４.3 配置说明



### 4.5 mysql

MySQL 是世界上最受欢迎的开源数据库。凭借其可靠性、易用性和性能，MySQL 已成为 Web 应用程序的数据库优先选择。



#### 4.5.1 拉取镜像

```sh
# 拉取官方的最新版本的镜像
docker pull mysql:latest
```



#### 4.5.2 运行容器

```
docker run -itd --name my-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```



#### 4.5.3 配置说明



## 5 Docker Compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

