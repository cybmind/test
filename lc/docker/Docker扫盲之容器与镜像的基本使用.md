## 介绍
先提三个问，docker是什么？docker能帮我们解决什么问题？为什么要使用docker？本文将快速带领你走进容器的世界。
1. docker是什么？
官方解释：Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口

	个人大白话：或许你现在还没有用过docker，但是你可能用过虚拟机吧？docker和虚拟机的差别在哪儿呢？首先第一个，虚拟机给人的第一映像就是大和重，想要部署一个环境到虚拟机上面，就必须安装一个操作系统才能做相关的操作，其次就是你不管安装什么你都得安装一个操作系统才能运行。但是现在docker的出现就解决了这个问题，为什么docker就解决了这个问题呢？其实docker就是将这个软件所需要的操作系统或者相关环境给精简了，去除了所有你用不到的东西，只留下软件最基本的依赖，这些依赖会一层依赖一层，这就会让一个docker的镜像非常小，比起整个操作系统轻便了非常多，而且最主要的是可以将你的环境打包成一个镜像发布到dockerhub等地方，然后你就在各个流行的linux上面直接将镜像下载下来就可以直接使用了，可以说就是一次部署，到处使用。

2. docker能帮我们解决什么问题？
众所周知，每次我们的项目代码编写完成过后，会将代码发布到服务器上，但是服务器必须要安装一系列我们需要的工具或者依赖包，亦或者是一些依赖的软件，这个时候发布项目的时间成本就太高了，如果碰到不熟悉的工作人员，这个时间可能会拉的更长，但是有了docker就不一样了，我们只需要简单的会docker，会一些基础的image和container相关的命令，就能完成这项工作，而且在很多太服务器的时候，也只需要一个docker就能帮我们完成部署。
3. 为什么要使用docker？
可能你在想我都有一个Linux系统了还要docker来干嘛？但是你有没有想过当你系统的环境需要迁移的时候，比如说你们需要集群，或者需要搭建测试服务器这些等等等等，你就需要花大量的时间来搭建环境，然后下载一些相应的包，但是这中间很有可能出现各种各样的坑，这些都是无法避免的，但是docker就是一个容器，它不管在那个环境上面运行，它都依赖于自己的容器，而不会去依赖装载容器的系统，这也就避免了因为系统的原因导致安装的步骤都不一样。

## docker hello world
还是以程序员的灵魂hello world来作为例子，然后快速进入docker的世界，可以使用`docker run 镜像名`来启动一个镜像，当这个镜像在本地不存在时，那么docker会到docker仓库去获取这个镜像。
执行命令
```
docker run hello-world
```
响应结果
```
[root@localhost ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
Trying to pull repository docker.io/library/hello-world ... 
latest: Pulling from docker.io/library/hello-world
1b930d010525: Pull complete 
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for docker.io/hello-world:latest
WARNING: IPv4 forwarding is disabled. Networking will not work.

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

[root@localhost ~]#
```

## docker的基本使用
### 查看镜像
上面我们已经运行了一个hello-word，那么我们来看一下执行了hello-world我们会生成那些东西？首先第一个是镜像，也是最终要的一个。执行以下命令可以查看容器中的所有镜像
执行命令
```
docker images
```
响应结果
```
[root@localhost ~]# docker images
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
docker.io/hello-world               latest              fce289e99eb9        2 months ago        1.84 kB
[root@localhost ~]# 
```
从上面的响应信息可以看到我们已经有了一个镜像 `docker.io/hello-world`，分析一下这个镜像的字段吧
1. REPOSITORY：这个是镜像所在的仓库，可以理解一种类型的数据，比如牙膏。
2. TAG：仓库的标签，这里的latest是对仓库里面的类型进行标记，可以理解成是牙膏里面的中华牙膏这个标记（其实就是版本），当没有指定标签时，那么这个latest就是默认的标签，一个仓库可以打多个标签，每个标签和仓库组成一组你想要的数据，当你需要获取指定标签的时候只需要这样写`docker run hello-world:latest`
3. IMAGE ID：这个镜像的id，也是唯一的id，比如你每次打个标签它的值都会变，我们启动的时候也可以使用`docker run fce289e99eb9` 来启动。
4. CREATED：这里是代表这个镜像的创建时间距离现在已经过去了多久。
5. SIZE：顾名思义，这就是这个镜像的大小。

### 查看容器
这里我们来看下由hello-world生成的容器是什么样的
执行命令
```
docker ps  -a
```
响应结果
```
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                      PORTS               NAMES
e91d9159c248        hello-world                     "/hello"                 2 hours ago         Exited (0) 2 hours ago                          adoring_mestorf
[root@localhost ~]# 
```
分析字段
1. CONTAINER ID：容器id
2. IMAGE：容器所属的镜像
3. COMMAND：启动时的默认执行命令，这里的命令时hello的原因时因为镜像的基础镜像在生成的时候指定的，因为我们时从服务器上面拿下来的。
4. CREATED：容器创建的时间距离下限的时间
5. STATUS：这个容器现在的状态，可以看到容器现在已经时退出了
6. PORTS：这个容器启动的时候映射在本机上面的端口，由于我们没有设置就没有值，可以通过 `-p 宿主机端口:容器端口` 来设置
7. NAMES：容器的名字，可以通过`--name 容器名` 来设置

### 启动容器
我们可以通过`docker start 容器id` 来启动一个容器
执行命令：
```
docker start 你的容器id
```
但是可以看到我们的容器时什么都没有输出，这是为什么呢？刚开始不是输出来hello world吗？因为在docker里面容器启动的时候只会输出启动的id，想要看详细信息只有进入容器查看，可以使用`docker ps`命令来查看运行的容器, 但是我们执行docker ps为什么也没有任何信息？这是因为我们没有后台运行，必须要在运行(执行run的时候)容器的时候加上`-d` 参数。
```
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@localhost ~]# 
```
### 搜索镜像
由于我们的hello-world的例子并不支持后台，我们可以重新获取一个镜像来执行,我们可以先执行`docker search 镜像名`来查找你想要的镜像, 我们这里来下载一个Centos的镜像
执行命令
```
docker search centos
```
响应结果
```
[root@localhost ~]# docker search centos
INDEX       NAME                                         DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
docker.io   docker.io/centos                             The official build of CentOS.                   5261      [OK]       
docker.io   docker.io/ansible/centos7-ansible            Ansible on Centos7                              121                  [OK]
docker.io   docker.io/jdeathe/centos-ssh                 CentOS-6 6.10 x86_64 / CentOS-7 7.5.1804 x...   109                  [OK]
docker.io   docker.io/consol/centos-xfce-vnc             Centos container with "headless" VNC sessi...   83                   [OK]
docker.io   docker.io/imagine10255/centos6-lnmp-php56    centos6-lnmp-php56                              52                   [OK]
docker.io   docker.io/centos/mysql-57-centos7            MySQL 5.7 SQL database server                   49                   
docker.io   docker.io/tutum/centos                       Simple CentOS docker image with SSH access      44                   
docker.io   docker.io/gluster/gluster-centos             Official GlusterFS Image [ CentOS-7 +  Glu...   40                   [OK]
docker.io   docker.io/openshift/base-centos7             A Centos7 derived base image for Source-To...   39                   
docker.io   docker.io/centos/postgresql-96-centos7       PostgreSQL is an advanced Object-Relationa...   37                   
docker.io   docker.io/kinogmt/centos-ssh                 CentOS with SSH                                 26                   [OK]
docker.io   docker.io/centos/httpd-24-centos7            Platform for running Apache httpd 2.4 or b...   22                   
docker.io   docker.io/centos/php-56-centos7              Platform for building and running PHP 5.6 ...   20                   
docker.io   docker.io/openshift/jenkins-2-centos7        A Centos7 based Jenkins v2.x image for use...   20                   
docker.io   docker.io/pivotaldata/centos-gpdb-dev        CentOS image for GPDB development. Tag nam...   10                   
docker.io   docker.io/openshift/wildfly-101-centos7      A Centos7 based WildFly v10.1 image for us...   6                    
docker.io   docker.io/openshift/jenkins-1-centos7        DEPRECATED: A Centos7 based Jenkins v1.x i...   4                    
docker.io   docker.io/darksheer/centos                   Base Centos Image -- Updated hourly             3                    [OK]
docker.io   docker.io/pivotaldata/centos                 Base centos, freshened up a little with a ...   2                    
docker.io   docker.io/pivotaldata/centos-mingw           Using the mingw toolchain to cross-compile...   2                    
docker.io   docker.io/blacklabelops/centos               CentOS Base Image! Built and Updates Daily!     1                    [OK]
docker.io   docker.io/openshift/wildfly-81-centos7       A Centos7 based WildFly v8.1 image for use...   1                    
docker.io   docker.io/pivotaldata/centos-gcc-toolchain   CentOS with a toolchain, but unaffiliated ...   1                    
docker.io   docker.io/jameseckersall/sonarr-centos       Sonarr on CentOS 7                              0                    [OK]
docker.io   docker.io/smartentry/centos                  centos with smartentry                          0                    [OK]
[root@localhost ~]#
```
1. INDEX：这个镜像的索引
2. NAME：镜像名称
3. DESCRIPTION：镜像描述
4. STARS：这个镜像有多少个人在使用，可以更具这个数量来选取自己需要的镜像
5. OFFICIAL：是否是官方发布的镜像
6. AUTOMATED：是否支持自动化

### 获取镜像
上面我们已经使用`docker search centos`来搜索到了很多的惊醒，现在我们来使用`docker pull 镜像名`来获取镜像吧
执行命令
```
docker pull docker.io/centos
```
响应结果
```
[root@localhost ~]# docker pull docker.io/centos
Using default tag: latest
Trying to pull repository docker.io/library/centos ... 
latest: Pulling from docker.io/library/centos
8ba884070f61: Pull complete 
Digest: sha256:8d487d68857f5bc9595793279b33d082b03713341ddec91054382641d14db861
Status: Downloaded newer image for docker.io/centos:latest
[root@localhost ~]#
```
可以从响应结果来看到我们的惊喜那个已经成功的获取下来了，执行`docker images`来查看我们的镜像
执行命令
```
docker images
```
响应结果
```
[root@localhost ~]# docker images
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos                    latest              9f38484d220f        6 days ago          202 MB
docker.io/hello-world               latest              fce289e99eb9        2 months ago        1.84 kB
[root@localhost ~]#
```
从上面的响应结果我们可以看到我们多了一个centos的镜像，现在来启动这个镜像吧，使用`docker run 镜像名`来启动镜像
执行命令
```
docker run -d -ti --name docker.io/centos /bin/bash
```
参数描述
1. -d：让启动的这个容器以后台方式执行
2. -ti：可以让容器接受和控制台交互还有可以在控制台输出信息
3. /bin/bash：让容器启动时执行该命令

带上参数的原因是Docker中系统镜像的缺省命令是 bash，如果不加 -ti bash 命令执行了自动会退出。这是因为如果没有衔接输入流，本身就会马上结束。加-ti 后docker命令会为容器分配一个伪终端，并接管其stdin/stdout支持交互操作，这时候bash命令不会自动退出。

响应结果
```
[root@localhost ~]# docker run -d -ti docker.io/centos /bin/bash
9687356aaf8e8b7ce24be22af28c6d6b1d9bc130c34e0211f6da05fc7a244d2b
```
现在容器已经启动起来了，我们可以上面学习的`docker ps -a`命令来查看所有的容器
执行命令
```
docker ps -a
```
响应结果
```
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                      PORTS               NAMES
9687356aaf8e        docker.io/centos                "/bin/bash"              2 minutes ago       Up 2 minutes                                    youthful_mccarthy
e91d9159c248        hello-world                     "/hello"                 2 hours ago         Exited (0) 36 minutes ago                       adoring_mestorf
[root@localhost ~]#
```
从上面可以看到已经有两个运行中的容器了,由于我们刚才启动容器的时候使用来一些额外的参数，现在我们在来看是否有一个运行中的容器。
执行命令
```
 docker ps
```
响应结果
```
[root@localhost ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
9687356aaf8e        docker.io/centos    "/bin/bash"         5 minutes ago       Up 5 minutes                            youthful_mccarthy
[root@localhost ~]# 
```
可以从响应结果里面看到我们的容器已经在运行了
### 进入容器
容器是启动了，但是我们没法操作呀，这里介绍一下怎么进入容器，可以使用该命令进入容器`docker exec -ti 容器id bash`
执行命令
```
docker exec -ti 9687356aaf8e bash
```
响应结果
```
[root@localhost ~]# docker exec -ti 9687356aaf8e bash
[root@9687356aaf8e /]# 
```
可以从响应结果来看我们登陆的服务器id变成来容器id，也就是所我们进入了容器了，现在你就可以在容器里面做你想要做的事了
### 退出容器
我们现在已经在容器里面了，但是想要退出出来怎么办呢？可以执行`exit`命令来退出，还可以按键盘上的`ctrl + p + q`来退出，这两种退出方式都不会让容器停止
### 停止容器
我们现在已经成功的启动了一个容器了，现在介绍一下如何停止一个容器，可以使用命令`docker stop 容器id`来停止容器
执行命令
```
docker stop 9687356aaf8e
```
响应结果
```
[root@localhost ~]# docker stop 9687356aaf8e
9687356aaf8e
[root@localhost ~]# 
```
可以看到停止容器时会返回一个容器的id
### 删除容器
当容器过多或者容器无用的时候我们就需要将容器删除，使用以下命令可以删除一个容器`docker rm 你的容器id`
执行命令
```
docker rm 9687356aaf8e
```
响应结果
```
[root@localhost ~]# docker rm 9687356aaf8e
9687356aaf8e
[root@localhost ~]#
```
可以看到还是返回了一个被删除的容器id，我们执行以下docker ps -a 来看下容器到底有没有被删除掉(想要删除容器必须让容器先停止)
执行命令
```
[root@localhost ~]# docker ps -a
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS                      PORTS               NAMES
e91d9159c248        hello-world                     "/hello"                 3 hours ago         Exited (0) 52 minutes ago                       adoring_mestorf
[root@localhost ~]# 
```
### 删除镜像
当我们生成来无用镜像的时候就需要将这个镜像删除掉，但是需要注意的是，删除镜像的时候这个镜像不能有容器，也就是说必须要先将容器删除掉才行，可以执行`docker rmi 镜像id`来删除一个镜像
执行命令
```
docker rmi 9f38484d220f
```
响应结果
```
[root@localhost ~]# docker rmi 9f38484d220f
Untagged: docker.io/centos:latest
Untagged: docker.io/centos@sha256:8d487d68857f5bc9595793279b33d082b03713341ddec91054382641d14db861
Deleted: sha256:9f38484d220fa527b1fb19747638497179500a1bed8bf0498eb788229229e6e1
Deleted: sha256:d69483a6face4499acb974449d1303591fcbb5cdce5420f36f8a6607bda11854
[root@localhost ~]#
```
可以看到这里是返回了多个信息，因为开头我们就介绍了镜像是一层一层的相互继承的，想要删除这个镜像必须一层一层的删除，所以这里会有多个删除记录。

