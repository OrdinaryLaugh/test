@[TOC]
# 为什么要学习容器化技术
## 人们遇到了什么问题
在很久很久以前，人们部署服务的时候，一台物理机上只能同时运行一个操作系统，大家部署服务的时候会遇到以下几个问题
- 环境搭建复杂：每个物理机上都需要手动去搭建一套环境
- 迁移性差，做系统扩展的时候，有硬件限制，环境也不好迁移 
- 稳定性差，一个机器部署多个服务的话，会出现资源抢占，并且一旦出现资源问题，就会引发雪崩效应
![在这里插入图片描述](https://img-blog.csdnimg.cn/78c26f8947ec4adda04cfac44062b63b.png)

基于上述原因，为了隔离各个服务，开始只能一个物理机部署一个服务，但是人们很快发现这样有些服务不能很好的利用机器的资源，会造成资源浪费，为了解决这种问题，就渐渐的出现了虚拟化技术
## 虚拟化技术
虚拟化技术（目前的云服务器用的就是这种理念）简单来讲就是在宿主机上再虚拟出来一个完整的操作系统，这样做有如下优点：
- 可以实现系统隔离
- 可以充分利用资源
- 迁移方便

常见的虚拟化技术软件：
- vmware esxi：直接运行在硬件之上本身就是一个操作系统，对于硬件资源进行了虚拟化操作，一般是企业进行采购使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/38cad1fb60744e4dae0c71e7d19783c1.png)
<center>I型虚拟化</center>

- vmware workstation：windows平台上创建虚拟机，属于个人学习使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/16e83dcc8c8d409ca27ddc3df059fb28.png)<center>II型虚拟化<center>

- kvm：linux平台的虚拟化工具，类似于vmware workstation 属于个人学习使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/dbc8803e125a4a928ab6b45109ea8e66.png)
<center>kvm虚拟化原理<center>

但是实际上用户可能不是为了得到一个完整的系统，而仅仅是想使用多个应用程序，并且他们之间互不影响，但是如果使用虚拟机，就会大大占用资源，因为有时候一个程序可能只占用了20%的机器资源，但是这个完整的操作系统就占用了10%的资源，这对资源来讲是极大的浪费
**虚拟化技术和容器化技术不是谁比谁更好，也不是对立的，只是应用场景不同罢了，现如今虚拟化技术和容器技术既可以单独使用也可以配合使用**
# Docker容器化技术应用
## 概述
简单来说：有效的将单个操作系统的资源划分到孤立的组中，以便更好的在孤立的组之间平衡有冲突的资源使用需求，这种技术就是容器技术。
而目前比较有名且使用范围较广的就是docker容器化技术

docker主要作用就是提供一个容器环境，使我们的程序独立地运行在容器中。
docker最初是dotCloud公司基于google公司退出的Golong语言开发而来，基于linux内核的[Cgroups](https://baike.baidu.com/item/Cgroup/4988200)、NameSpace，以及[Union FS](https://cloud.tencent.com/developer/article/2134040)等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。

docker这个听起来很玄的技术，实际上就是在创建一个容器进程时，指定了这个进程所需要启用的一组Namespace参数。这样容器就只能看到当前namespace限定的资源、文件、设备、网络等等资源。而对于宿主机以及其他不相干的程序完全看不到。

总之，容器只是一个特殊一点的进程而已。

## Docker的C/S模式
在Docker Client中来运行Docker的各种命令，而这些命令会传送给在Docker的宿主机上运行的Docker的守护进程。Docker的守护进程是负责来实现Docker的各种功能，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/05eba6bc4e3b45b8baec9ad6355e0ee2.png)
Docker的守护进程允许一下两种方式的调用：
- API接口：用户可以通过docker官方提供的API接口给Server Docker Daemon发送指令，例如，用户可以编写一个可视化界面进行docker容器的管理，点击各种按钮调用api接口进而完成相应的功能
![在这里插入图片描述](https://img-blog.csdnimg.cn/93fbf8d5548b489e9964759577f333b5.png#pic_center)
- 客户端命令行：用户直接在宿主机上发送指令来完成一些功能调用
![在这里插入图片描述](https://img-blog.csdnimg.cn/950340482a984c15b79398ae82f53f4c.png#pic_center)
## 简单使用
这里简单说一下，我们一般情况下是如何使用docker的：
 1. 首先我们启动下载好的docker server
 2. 获取镜像：获取镜像通常有以下两种方式
    - 通过dockerfile构建一个镜像
    - 从网络上下载别人制作好的镜像，或者拷贝别人制作好的镜像
 3. 启动docker容器：选择某一个镜像，加载它，执行完成之后，你将获得一个对应的容器
 4. 通过docker容器制作镜像：如果你启动了一个centos镜像，并且在该容器内，做了一些修改，例如在该容器内部搭建了java环境，并且写了服务启动脚本，你希望后续组内其他成员可以直接拷贝你所搭建的这套环境的时候，只需要执行命令，将你的容器制作成镜像，就可以直接分享给你的小伙伴了，他们可以通过启动指定镜像就获得一个跟你一样的搭建了java环境和脚本的容器
 5. 镜像上传至镜像仓库（DockerHub）：如果你希望这个镜像被更多的人使用到的话，你可以搭建一个本地或者网络上的镜像仓库，后续其他人就可以通过镜像仓库拉取他想要的镜像文件了
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ca26201ded048a5a235e84bdcfafeb9.png)
## docker核心组件：
### Images(镜像)
镜像是一个只读模板，用于创建容器，也可以通过dockerfile文本描述镜像的内容。
镜像的概念类似于编程开发里面向对象的类，从一个基类开始（基础镜像Base Image）
构建容器的过程，就是运行镜像，生成容器实例。
Docker镜像的描述文件是[Dockerfile](https://docs.docker.com/engine/reference/builder/)，包含了如下指令
- FROM定义基础镜像
- MAINTAINER 作者
- RUN 运行LINUX命令
- ADD 添加文件/目录
- ENV 环境变量
- CMD 运行进程

dockerfile案例：
```yml
# 指明构建的新镜像是来自于 centos:7 基础镜像
FROM centos:7
# 通过镜像标签声明了作者信息
LABEL maintainer="mrhelloworld.com"
# 设置工作目录
WORKDIR /usr/local
# 新镜像构建成功以后创建指定目录
RUN mkdir -p /usr/local/java && mkdir -p /usr/local/tomcat
# 拷贝文件到镜像中并解压
ADD jdk-11.0.6_linux-x64_bin.tar.gz /usr/local/java
ADD apache-tomcat-9.0.37.tar.gz /usr/local/tomcat
# 暴露容器运行时的 8080 监听端口给外部
EXPOSE 8080
# 设置容器内 JAVA_HOME 环境变量
ENV JAVA_HOME /usr/local/java/jdk-11.0.6/
ENV PATH $PATH:$JAVA_HOME/bin
# 启动容器时启动 tomcat 并查看 tomcat 日志信息
ENTRYPOINT /usr/local/tomcat/apache-tomcat-9.0.37/bin/startup.sh && tail -f /usr/local/tomcat/apache-tomcat-9.0.37/logs/catalina.out
```
### Containers(容器)
容器是一个镜像运行的实例
创建容器的过程：
- 获取镜像，如docker pull centos，从镜像仓库拉取
- 使用镜像创建容器
- 分配文件系统，挂在一个读写层，在读写层加载镜像
- 分配网络/网桥接口，创建一个网络接口，让容器和宿主机通信
- 容器获取IP地址
- 执行容器命令，如/bin/bash
- 发反馈容器启动结果
### [Registry](https://blog.csdn.net/JohnnyG2000/article/details/126292035)
Docker镜像需要进行管理，docker提供了Registry，其实他也是一个容器。可以基于该容器运行私有仓库，也可以使用DockerHub互联网共有镜像仓库。
# 容器网络（docker network）
## 概述
### 1、docker网络实现的原理
Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥(docker0),Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，成为Container-IP，同时Docker’网桥是 每个容器的默认网关。
因为同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接通信。

Docker网桥是宿主机虚拟出来的，并不是真实存在网络设备，外部网络是无法寻址到的，这也意味着外部网络无法直接通过Container-IP访问到容器。

如果容器希望外部访问能访问到，可以通过映射容器端口到宿主主机（端口映射），即 docker run创建容器时通过-p或-P参数来启用，访问容器的时候就通过[宿主机IP]：[容器端口]访问容器 。

[容器访问外部原理](https://blog.csdn.net/qq_34556414/article/details/108718522)

### 2、容器的端口映射
什么叫端口映射
 - 把容器的端口映射为宿主机的一个随机或者特定端口，使用外部用户可以访问容器服务
- 端口映射本是在容器底层做了 iptables 地址转发，出去的流量做 SNAT 源地址转发，进来的流量做 DNAT 目标地址转发

常见的四种端口映射：
- 随机端口映射：就是把容器的端口随机映射为宿主机的一个端口。
- 指定端口映射：就是把容器的端口映射为宿主机的指定端口。
- 指定网卡随机端口映射：就是把容器的端口映射为宿主机的指定网卡的随机端口。
- 指定网卡端口映射：就是把容器的端口映射为宿主机的指定网卡的指定端口。
## docker的网络模式
下面是常用的五种网络模式，除了下面的五种常用的网络模式之外还有overlay网络模式（默认为ingress）
| 选项 | 描述 |
|--|--|
| Bridge|默认为该模式，此模式会为每一个容器分配，设置IP等，并将容器连接到一个docker0的虚拟网桥，通过docker0网桥以及iptables nat表配置与宿主机通信。|
| Host|基础镜像(容器不会虚拟出自己的网卡，配置主机的IP等，而是使用宿主机的IP和端口;|
| Container|创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP和端口范围；|
| None|该模式关闭了网络功能|
| maclan|通过为容器提供 MAC 和 IP 地址，直接通过主机接口（或子接口）访问容器接口|
> Docker 使用了Linux的Namespace技术来进行资源隔离，如PID Namespace隔离进程，Mount Namespace隔离文件系统，Network Namespace隔离网络等。一个Network Namespace提供了一份独立的网络环境，包括网卡，路由，iptables 规则等都与其他Network Namespace隔离。一个Docker容器一般会分配一个独立的Network Namespace。
### Bridge模（默认模式）
**模式特点**：
① 当Docker进程启动时，会在主机上创建一个名为docker0的虚拟网桥，此主机上启动的Docker容器会连接到这个虚拟网桥上。虚拟网桥的工作方式和物理交换机类似，这样主机上的所有容器就通过交换机连在了一个二层网络中。
② 从docker0子网中分配一个IP给容器使用，并设置docker0的IP地址为容器的默认网关。在主机上创建一对虚拟网卡veth pair设备。veth设备总是成对出现的，它们组成了一个数据的通道，数据从一个设备进入，就会从另一个设备出来。因此，veth设备常用来连接两个网络设备。
③ Docker将veth pair 设备的一端放在新创建的容器中，并命名为eth0（容器的网卡），另一端放在主机中，以veth*这样类似的名字命名，并将这个网络设备加入到docker0网桥中。可以通过 brctl show 命令查看。
④ 容器之间通过veth pair进行访问
⑤ 使用 docker run -p 时，docker实际是在iptables做了DNAT（目的地址转换）规则，实现端口转发功能。可以使用iptables -t nat -vnL 查看。
![在这里插入图片描述](https://img-blog.csdnimg.cn/0267e7fab48d41899bc9b6d154424026.png)
**应用场景**：
隔离性好，会占用宿主机端口，只占用一个真实IP，适用于大多数场景。
需要对宿主机端口资源进行规划。
### Host模式
**模式特点**：
① host模式：使用–net=host指定
② 相当于VMware中的桥接模式，与宿主机在同一个网络中，但是没有独立IP地址
③ 一个Docker 容器一般会分配一个独立的Network Namespace
但是如果启动容器的时候使用host 模式，那么这个容器将不会获得一个独立的Network Namespace ，而是和宿主机共用一个Network Namespace 。容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口.此时容器不再拥有隔离的、独立的网络栈，不拥有所有端口资源。
![在这里插入图片描述](https://img-blog.csdnimg.cn/93fead77a8c84b67bae72ac114640060.png)

**应用场景**：
隔离性最差，只占用一个真实IP，会占用宿主机端口，会出现端口冲突，性能最好。
能确认所有容器端口不冲突且默认都需要对外暴露时使用。
### Container模式
**模式特点**：
① container模式： 使用–net=contatiner:NAME_or_ID 指定
② 这个模式指定新创建的容器和已经存在的一个容器共享一个Network Namespace，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP，端口范围等。可以在一定程度上节省网络资源，容器内部依然不会拥有所有端口。
③ 同样，两个容器除了网络方面，其他的如文件系统，进程列表等还是隔离的。
④ 两个容器的进程可以通过lo网卡设备通信
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd7d6051f0f94da6a916a6cc549b1381.png)
**应用场景**：
隔离性好，只占用一个真实IP，会占用容器端口，性能差，开发网关应用时可以考虑。
### None模式
**模式特点**：
① none模式:使用 --net=none指定
② 使用none 模式，docker 容器有自己的network Namespace ，但是并不为Docker 容器进行任何网络配置。也就是说，这个Docker 容器没有网卡，ip， 路由等信息。
③ 这种网络模式下，容器只有lo 回环网络，没有其他网卡。
④ 这种类型没有办法联网，但是封闭的网络能很好的保证容器的安全性
⑤ 该容器将完全独立于网络，用户可以根据需要为容器添加网卡。此模式拥有所有端口。（none网络模式配置网络）
⑥ 特殊情况下才会用到，一般不用
**应用场景**：
一切全靠自定义，各项特性靠自己决定。
### macvlan模式
Docker 内置的 Macvlan 驱动（Windows上是 Transparent）就是为此场景而生。通过为容器提供 MAC 和 IP 地址，让容器在物理网络上成为‘一等公民’。因此无需端口映射或者额外桥接，可以直接通过主机接口（或子接口）访问容器接口。但是，Macvlan 的缺点是需要将主机网卡（NIC）设置为混杂模式（Promiscuous Mode）
**模式特点**：
①独立IP，维护方便，不占用主机端口号，使用容器默认端口
②拥有稳定的IP后，容器间跨主机通信成为可能，因为无论是物理机、虚拟机、容器都是物理网络的‘一等公民’，容器间通信时直接指定IP即可
③网络环境需要支持混杂模式，公有云不支持（不过都上公有云了，为什么不直接用云供应商提供的容器服务呢？）
④部分大规模应用集群，内网ip地址是稀缺资源，例如阿里云、腾讯云，不适合使用 Macvlan
⑤默认容器端口均对外暴露，需要在内网构建应用防火墙/网关决定哪些IP的哪些端口允许外界访问，哪些不允许

![在这里插入图片描述](https://img-blog.csdnimg.cn/4efb840f89c44f03b20a640516b08da5.png)
**应用场景**：
Macvlan 对于公司内部的数据中心网络来说很棒（假设公司网络组能接受 NIC 设置为混杂模式），但是 Macvlan 在公有云上并不可行。
# docker镜像加载和分层原理
## Docker镜像加载原理
### 1、UnionFs联合文件系统
docker的镜像实际上是由一层一层的文件系统组成的，这种层级的文件系统被称为UnionFs
- Union文件系统是一种分层，轻量级并且高性能的文件系统，它对文件系统的修改作为一次提交来一层一层的叠加，同时可以将不同目录挂在到同一个虚拟文件系统下，而且目录的物理位置是分开的。
- Union文件系统可以把只读和可读写文件系统合并在一起，具有Copy-on-Write功能，允许只读文件系统的修改可以保存到可写文件系统当中。
- Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像(没有父镜像)，可以制作各种具体的应用镜像。
> 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统回包含所有底层的文件和目录。
### 2、Docker镜像加载原理
##### 1）base镜像
base 镜像简单来说就是不依赖其他任何镜像，完全从0开始建起，其他镜像都是建立在他的之上，可以比喻为大楼的地基，docker镜像的鼻祖。

base 镜像的特性：
（1）不依赖其他镜像，从 scratch 构建。
（2）其他镜像可以之为基础进行扩展。

所以，能称作 base 镜像的通常都是各种 Linux 发行版的 Docker 镜像，比如 Ubuntu, Debian, CentOS 等。

我们以 CentOS 为例查看 base 镜像包含哪些内容。
```shell
root@ubuntu:~# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
d9aaf4d82f24: Pull complete 
Digest: sha256:4565fe2dd7f4770e825d4bd9c761a81b26e49cc9e3c9631c58cfc3188be9505a
Status: Downloaded newer image for centos:latest

root@ubuntu:~# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              d123f4e55e12        3 weeks ago         197MB
ubuntu              12.04               5b117edd0b76        7 months ago        104MB
```
> 提问：docker cnetos的镜像大小200多M，和平时的所用的发行版的大小（几G）相差很大，为什么？

经典的Linux 操作系统由内核空间和用户空间组成，如下图。
![在这里插入图片描述](https://img-blog.csdnimg.cn/f526cc475d18449a8c4223777230a68f.png)

**对于 base 镜像来说，底层直接用 Host 的 kernel，自己只需要提供 rootfs 就行了。**
而对于一个精简的 OS，rootfs 可以很小，只需要包括最基本的命令、工具和程序库就可以了。相比其他 Linux 发行版，CentOS 的 rootfs 已经算臃肿的了，alpine 还不到 10MB。

由此可见对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

base 镜像提供的是最小安装的 Linux 发行版。
##### 2）bootfs
bootfs(boot file system)：主要包含 bootloader 和 kernel。
- bootloader 主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs，这一层与我们典型的Linux/Unix系统是一样的，包含bootfs加载器和内核。当bootfs加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。
##### 3）rootfs
rootfs(root file system)：在bootfs之上，包含类似于典型Linux系统中的/dev，/proc，/bin，/etc等标准目录文件。
- rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等。
## 分层原理

Docker 支持通过扩展现有镜像，创建新的镜像。

实际上，Docker Hub 中 99% 的镜像都是通过在 base 镜像中安装和配置需要的软件构建出来的。比如我们现在构建一个新的镜像，Dockerfile 如下：
```yml
# Version: 0.0.1
FROM debian                1.新镜像不再是从 scratch 开始，而是直接在 Debian base 镜像上构建。
MAINTAINER wzlinux
RUN apt-get update && apt-get install -y emacs        2.安装 emacs 编辑器。
RUN apt-get install -y apache2             3.安装 apache2。
CMD ["/bin/bash"]              4.容器启动时运行 bash。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d075a9f133e465d99959d5046c0c4a3.png)
可以看到，新镜像是从 base 镜像一层一层叠加生成的。每安装一个软件，就在现有镜像的基础上增加一层。

### 什么 Docker 镜像要采用这种分层结构呢？
 1. 分层最大的优点是共享资源。
 2. 多个镜像都可以基于相同的 Base 镜像构建而来，那么宿主机只需在磁盘上保存一份base 镜像即可。
 3. 同时内存中也只需要加载一份 Base 镜像，就可以为所有容器服务，而且镜像的每一层都可以被共享。
### Copy-on-Write(COW) 特性
如果多个容器共享一份基础镜像，当某个容器修改了基础镜像的内容，比如 /etc 下的文件，这时其他容器的 /etc 是否也会被修改？
答案是不会！
修改会被限制在单个容器内。
这就是我们接下来要说的容器 Copy-on-Write(COW) 特性。
- 新数据会直接存放在最上面的容器层。
- 修改现有数据会先从镜像层将数据复制到容器层，修改后的数据直接保存在容器层中，镜像层保持不变。
- 如果多个层中有命名相同的文件，用户只能看到最上面那层中的文件。
### 可写的容器层
当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

典型的Linux在启动后，首先将 rootfs 置为 readonly, 进行一系列检查, 然后将其切换为 “readwrite” 供用户使用。在docker中，起初也是将 rootfs 以readonly方式加载并检查，然而接下来利用 union mount 的将一个 readwrite 文件系统挂载在 readonly 的rootfs之上，并且允许再次将下层的 file system设定为readonly 并且向上叠加, 这样一组readonly和一个writeable的结构构成一个container的运行目录, 每一个被称作一个Layer。如下图所示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/7fac7469d55a417392ae1b409401379d.png)
所有对容器的改动，无论添加、删除、还是修改文件都只会发生在容器层中。只有容器层是可写的，容器层下面的所有镜像层都是只读的。

下面我们深入讨论容器层的细节。

镜像层数量可能会很多，所有镜像层会联合在一起组成一个统一的文件系统。如果不同层中有一个相同路径的文件，比如 /a，上层的 /a 会覆盖下层的 /a，也就是说用户只能访问到上层中的文件 /a。在容器层中，用户看到的是一个叠加之后的文件系统。

**添加文件**：在容器中创建文件时，新文件被添加到容器层中。
**读取文件**：在容器中读取某个文件时，Docker 会从上往下依次在各镜像层中查找此文件。一旦找到，立即将其复制到容器层，然后打开并读入内存。
**修改文件**：在容器中修改已存在的文件时，Docker 会从上往下依次在各镜像层中查找此文件。一旦找到，立即将其复制到容器层，然后修改之。
**删除文件**：在容器中删除文件时，Docker 也是从上往下依次在镜像层中查找此文件。找到后，会在容器层中记录下此删除操作。
只有当需要修改时才复制一份数据，这种特性被称作 Copy-on-Write。可见，容器层保存的是镜像变化的部分，不会对镜像本身进行任何修改。

这样就解释了我们前面提出的问题：容器层记录对镜像的修改，所有镜像层都是只读的，不会被容器修改，所以镜像可以被多个容器共享。

# 容器卷（Volumes）
## 管理数据
### 为什么要选择挂载
默认情况下，在容器内创建的所有文件都存储在可写容器层上。这意味着：
- 当该容器被删除时，数据也会被同样删除，如果有另一个进程需要数据，就很难从容器中取出数据。
- 容器的可写层与运行容器的宿主机紧密耦合，你无法将数据移动到其他地方。
- 写入容器的可写层需要存储驱动程序来管理文件系统。存储驱动程序提供了一个联合文件系统，使用 Linux 内核。与使用直接写入主机文件系统的数据卷相比，这种额外的抽象会降低性能 。
> Docker有两种方法：volumes 卷 和 bind mounts 绑定挂载 可以让容器在主机上存储文件，以便即使容器停止或删除后文件也能持久化保存。
Docker 还支持将文件存储在主机内存中的容器。此类文件不会持久保存。如果您在 Linux 上运行 Docker，则使用tmpfs 挂载将文件存储在主机的系统内存中。如果您在 Windows 上运行 Docker，命名管道用于将文件存储在主机的系统内存中。

### 选择正确的挂载方式
无论你选择那种挂载的方式，容器内的数据看起来都是一样的。数据以目录或文件系统中的单个文件的形式公开。

卷(volumes)、绑定挂载(bind mounts)和 tmpfs挂载(tmpfs mounts) 之间的差异就是数据在Docker主机上的位置不同
![](https://img-blog.csdnimg.cn/dca996cbd06848dfaadd8e28e9ad9e90.png#pic_center)
- 卷(volumes)：存储在由 Docker（/var/lib/docker/volumes/在 Linux 上）管理的主机文件系统的一部分中。非 Docker 进程不应修改文件系统的这一部分。卷是在 Docker 中持久化数据的最佳方式。
- 绑定挂载(bind mounts)：可以存储在主机系统的任何位置。它们甚至可能是重要的系统文件或目录。Docker 主机或 Docker 容器上的非 Docker 进程可以随时修改它们。
- tmpfs挂载(tmpfs mounts)：仅存储在主机系统的内存中，永远不会写入主机系统的文件系统。
## 卷（Volume）
### 概述
卷(volume)提供了将容器的特定文件系统路径连接回主机的能力，简单来说就是将容器的目录映射到主机上。如果容器中的目录已挂载，则该目录中的更改也会在主机上看到。如果我们在容器重启时挂载相同的目录，我们会看到相同的文件，这就是容器的持久化和同步操作。
> - Docker Volume 命令能让容器从宿主机中读取文件，或从容器中持久化数据到宿主机中，让容器与容器产生的数据分离开来，一个容器可以挂载多个不同的目录。
> - Volume的生命周期是独立于容器的生命周期之外的，即使容器删除了，Volume也会被保留下来，Docker不会因为这个Volume没有被容器使用而自动回收。
> - 在容器中，添加或修改这个文件夹里的文件也不会影响到容器的联合文件系统。
### 卷的优点
卷是Docker容器生成和使用数据的首选保存机制。绑定挂载依赖于主机的目录结构和操作系统，但卷是完全由Docker管理。

- 卷比绑定挂载更容易备份或迁移数据。
- 卷可以直接使用Docker CLI 命令或 Docker API 管理。
- 卷可以在 Linux 和 Windows 容器上运行。
- 卷可以在多个容器之间更安全的共享。
- 卷允许将卷挂载到远程主机或云服务器上，以加密卷的内容或添加其他功能。
- 新卷的内容可以由容器预先填充。
- Docker Desktop 上的卷比 来自Mac 和 Windows 主机的绑定挂载具有更高的性能。

此外，与将数据持久化到容器的可写层相比，卷通常是更好的选择，因为卷不会增加使用它的容器的大小，而且卷的内容存在于给定容器的生命周期之外
![在这里插入图片描述](https://img-blog.csdnimg.cn/0764c64e581c40b1bbe4da9ec1c77ceb.png#pic_center)
> 注意：如果你的容器需要生成非持久化状态数据，优先选择tmpfs 挂载以避免将数据永久存储在任何地方，并且避免写入容器的可写层来提高容器性能。
### 卷的操作
我们可以通过下面的命令创建一个容器，使用volume
```shell
docker run -d \
  --name devtest \
  -v myvol2:/app \
  nginx:latest
  ```
其中：-v选项，后面的第一个参数myvol2就是volume的名字，如果在容器使用volume时，volume不存在，那么会自动创建这个volume
```shell
[root@centos7 ~]# docker volume ls
DRIVER    VOLUME NAME
local     myvol2
[root@centos7 ~]#
```
可以查看该volume在docker 宿主机上的具体的路径
```shell
[root@centos7 ~]# docker volume inspect myvol2
[
    {
        "CreatedAt": "2022-08-23T22:04:20-04:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/myvol2/_data",
        "Name": "myvol2",
        "Options": null,
        "Scope": "local"
    }
]
[root@centos7 ~]#
```
Mountpoint就指明了这个volume在文件系统上的具体位置。

容器通过：-v myvol2:/app 将volume挂载到容器的/app目录中
```shell
[root@centos7 ~]# docker exec -it devtest df -h
Filesystem               Size  Used Avail Use% Mounted on
overlay                   50G  3.0G   48G   6% /
tmpfs                     64M     0   64M   0% /dev
tmpfs                    1.4G     0  1.4G   0% /sys/fs/cgroup
shm                       64M     0   64M   0% /dev/shm
/dev/mapper/centos-root   50G  3.0G   48G   6% /app
tmpfs                    1.4G     0  1.4G   0% /proc/asound
tmpfs                    1.4G     0  1.4G   0% /proc/acpi
tmpfs                    1.4G     0  1.4G   0% /proc/scsi
tmpfs                    1.4G     0  1.4G   0% /sys/firmware
[root@centos7 ~]#
```
向/app目录写文件：
```shell
docker exec -it devtest bash -c "echo test > /app/myfile.test"
```
查看卷中的文件是否已经被创建
```shell
[root@centos7 ~]# cd /var/lib/docker/volumes/myvol2/_data
[root@centos7 _data]# ls
myfile.test
[root@centos7 _data]# cat myfile.test 
test
[root@centos7 _data]#
```
没错，确实这个文件就是写到了这个目录中了。

注意：这个volume的内容写的过程是由docker来进行管理的。

查看容器中mount信息
```shell
docker inspect devtest
        "Mounts": [
            {
                "Type": "volume",
                "Name": "myvol2",
                "Source": "/var/lib/docker/volumes/myvol2/_data",
                "Destination": "/app",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ]
```
## 绑定挂载(bind mount)
使用bind mount时，将宿主机上的文件或者目录挂载到容器上。文件或者目录在主机上，是以绝对路径的方式来使用。

相比之下，当使用volume时，在主机上的Docker存储目录中创建一个新目录，由Docker管理该目录的内容。

要挂载的文件或目录不需要在Docker主机上已经存在。如果它还不存在，则按需创建它。bind mount的性能非常好，但是它们依赖于主机的文件系统具有特定的目录结构。

建议：如果你正在开发新的Docker应用程序，请考虑使用命名的volume。因为，对于bind mount来说，你不能使用Docker CLI命令来直接管理。
### bind mount使用
使用以下得命令，将一个容器通过bind 挂载的方式，将宿主机的目录，挂载到容器中
```shell
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  nginx:latest
```
查看容器的bind挂载情况
```shell
docker inspect devtest
"Mounts": [
            {
                "Type": "bind",
                "Source": "/tmp/source/target/target",
                "Destination": "/app",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ]
```
可以看到这里的type是bind类型
# 容器安全
## 理解Docker安全
Docker容器的安全性，很大程度上依赖于Linux系统自身，评估Docker的安全性时，主要考虑以下几个方面：
- Linux内核的命名空间机制提供的容器隔离安全
- Linux控制组机制对容器资源的控制能力安全。
- Linux内核的能力机制所带来的操作权限安全
- Docker程序（特别是服务端）本身的抗攻击性。
- 其他安全增强机制对容器安全性的影响。

可以看到docker提供了6种命名空间：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a18f2c1d70e6418994cf493ca9635552.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f41ac469edb4b528b47082e39fa1614.png)

> 命名空间隔离的安全
> - 当docker run启动一个容器时，Docker将在后台为容器创建一个独立的命名空间。
> - 在 Linux 内核中，有很多资源和对象是不能被 Namespace 化的，比如：时间。
> - 容器只是运行在宿主机上的一种特殊的进程，那么多个容器之间使用的就> - 还是同一个宿主机的操作系统内核。
> - 与虚拟机方式相比，通过Linux namespace来实现的隔离不是那么彻底。
命名空间提供了最基础也最直接的隔离。
控制组资源控制的安全

> 控制组资源控制的安全:
> - 当docker run启动一个容器时，Docker将在后台为容器创建一个独立的控制组策略集合。
> - Linux Cgroups提供了很多有用的特性，确保各容器可以公平地分享主机的内存、CPU、磁盘IO等资源。
> - 确保当发生在容器内的资源压力不会影响到本地主机系统和其他容器，它在防止拒绝服务攻击（DDoS）方面必不可少。

> 内核能力机制
> - 能力机制（Capability）是Linux内核一个强大的特性，可以提供细粒度的权限访问控制。
> - 大部分情况下，容器并不需要“真正的”root权限，容器只需要少数的能力即可。
> - 默认情况下，Docker采用“白名单”机制，禁用“必需功能”之外的其他权限。

> Docker服务端防护:
> - 使用Docker容器的核心是Docker服务端，确保只有可信的用户才能访问到Docker服务。
> - 将容器的root用户映射到本地主机上的非root用户，减轻容器和主机之间因权限提升而引起的安全问题。
> - 允许Docker 服务端在非root权限下运行，利用安全可靠的子进程来代理执行需要特权权限的操作。这些子进程只允许在特定范围内进行操作。

> 其他安全特性:
> - 在内核中启用GRSEC和PAX，这将增加更多的编译和运行时的安全检查；并且通过地址随机化机制来避免恶意探测等。启用该特性不需要Docker进行任何配置。
> - 使用一些有增强安全特性的容器模板。
> - 用户可以自定义更加严格的访问控制机制来定制安全策略。
> - 在文件系统挂载到容器内部时，可以通过配置只读模式来避免容器内的应用通过文件系统破坏外部环境，特别是一些系统运行状态相关的目录。
## 容器资源控制
> Linux Cgroups 的全称是 Linux Control Group。
> - 是限制一个进程组能够使用的资源上限，包括 CPU、内存、磁盘、网络带宽等等。
> - 对进程进行优先级设置、审计，以及将进程挂起和恢复等操作。

> Linux Cgroups 给用户暴露出来的操作接口是文件系统。
> - 它以文件和目录的方式组织在操作系统的 /sys/fs/cgroup 路径下。
> - 执行此命令查看：mount -t cgroup

所有容器资源限制的相关信息都在 /sys/fs/cgroup/memory/docker 此目录下，如果我们建立容器时不设置，那么docker内容器的目录内的文件都继承于/sys/fs/cgroup/memory/docker 内的其他文件 

资源控制包括以下几部分：
- cpu资源限制
- cpu优先级
- 内存资源限制
- 磁盘io限制
## 容器特权（危险操作）
- 设置特权级运行的容器：–privileged=true
- 有的时候我们需要容器具备更多的权限，比如操作内核模块，控制swap交换分区，挂载USB磁盘，修改MAC地址等。
- #docker run -it --name vm1 ubuntu bash
![在这里插入图片描述](https://img-blog.csdnimg.cn/073e3cb966104bd8bec4e2b46b3e3de7.png#pic_center)
开启容器特权
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e550c98a1244b07b02440dda60ff2a0.png#pic_center)
可以看到没有报错且可以查看磁盘情况。此权限的意思是全开，即root用户可以做几乎任何事情，近乎超户。但是此种方式权力又给的太大了不安全，我们可以给白名单。

设置容器白名单：
–cap-add --privileged=true 的权限非常大，接近于宿主机的权限，为了防止用户的滥用，需要增加限制，只提供给容器必须的权限。此时Docker 提供了权限白名单的机制，使用–cap-add添加必要的权限。
capabilities手册地址： http://man7.org/linux/man-pages/man7/capabilities.7.html
![在这里插入图片描述](https://img-blog.csdnimg.cn/d8994473333c42b3831f03184c7c81d1.png#pic_center)

# Docker容器编排（DockerCompose）
## 简介
Compose（容器编排） 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成一个应用。你需要定义一个 YAML 格式的配置文件docker-compose.yml，写好多个容器之间的调用关系。然后，只要一个命令，就能同时启动/关闭这些容器。主要实现对Docker容器集群的快速编排。
## 为什么需要 Compose?
docker建议我们每一个容器中只运行一个服务,因为docker容器本身占用资源极少,所以最好是将每个服务单独的分割开来但是这样我们又面临了一个问题？

如果我需要同时部署好多个服务,难道要每个服务单独写Dockerfile然后在构建镜像,构建容器,这样累都累死了,所以docker官方给我们提供了docker-compose多服务部署的工具

例如要实现一个Web微服务项目，除了Web服务容器本身，往往还需要再加上后端的数据库mysql服务容器，redis服务器，注册中心eureka，甚至还包括负载均衡容器等等。。。。。。

Compose允许用户通过一个**单独的docker-compose.yml模板文件**来定义一组相关联的应用容器为一个项目（project）。

可以很容易地用一个配置文件定义一个多容器的应用，然后使用一条指令安装这个应用的所有依赖，完成构建。Docker-Compose 解决了容器与容器之间如何管理编排的问题。
## DockerCompose使用
官网下载地址：https://docs.docker.com/compose/install/
下载完成就可以直接使用，无需安装
在docker-compose.yml文件所在的目录下通过指令构建服务
```
docker-compose up -d #-d表示以守护进程运行
```
docker-compose.yml使用示例：
```
version: "3" #指定版本号

#配置各个容器服务
services:
  #配置个人服务
  microService:
    #指定镜像
    image: zzyy_docker:1.6
    #指定容器名称
    container_name: ms01
    #指定端口映射
    ports:
      - "6001:6001"
    #指定容器卷
    volumes:
      - /app/microService:/data
    #指定网络模式
    networks: 
      - hypig_net 
    depends_on: #表示只有当Docker中运行着指定的容器时，该服务容器才运行
      - redis
      - mysql
  
  #配置redis容器
  redis:
    image: redis:6.0.8
    ports:
      - "6379:6379"
    volumes:
      - /app/redis/redis.conf:/etc/redis/redis.conf
      - /app/redis/data:/data
    networks: 
      - atguigu_net
    #指定容器运行的指令
    command: redis-server /etc/redis/redis.conf
 
  #配置mysql容器
  mysql:
    image: mysql:5.7
    #配置相关的环境
    environment:
      MYSQL_ROOT_PASSWORD: 'lzb'
      MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
      MYSQL_DATABASE: 'docker_test'
      MYSQL_USER: 'root'
      MYSQL_PASSWORD: 'lzb'
    ports:
       - "3306:3306"
    volumes:
       - /app/mysql/db:/var/lib/mysql
       - /app/mysql/conf/my.cnf:/etc/my.cnf
       - /app/mysql/init:/docker-entrypoint-initdb.d
    networks:
      - hypig_net
    command: --default-authentication-plugin=mysql_native_password #解决外部无法访问

#创建网络模式
networks: 
   hypig_net: 

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcb83d0cd3c24e66aea00d693e2fa68a.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3cb99301eb449bcb5905aa707163c99.png)
# 参考文档：
https://developer.aliyun.com/article/768343
https://blog.csdn.net/weixin_46618592/article/details/126555359
https://blog.51cto.com/wzlinux/2044797
https://blog.csdn.net/m0_52884709/article/details/127743854
https://blog.csdn.net/q908544703/article/details/126226073
https://www.jianshu.com/p/cc71aa34a67e
https://blog.csdn.net/weixin_71483812/article/details/127388903
https://blog.csdn.net/Gong_yz/article/details/129360646
https://blog.csdn.net/qq_34556414/article/details/120188653
https://www.jb51.net/article/260534.htm
