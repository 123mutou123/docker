[TOC]



# **Docker**



## **1. Docker**简介



### **1.1 什么是容器**？



#### **1.1.1** 一种虚拟化的方案

但与传统的虚拟机不同，传统的虚拟机通过中间层将一台或多台独立的机器虚拟地运行于物理硬件之上

#### 1.1.2 **操作系统级别的虚拟化**

而容器则是直接运行在操作系统上的用户空间，故容器虚拟化也被称为操作系统虚拟化

#### 1.1.3 **只能运行于相同或相似内核的操作系统**

依赖于操作系统的特性，docker依赖于Linux内核特性：Namespace和Cgroups (Control Group)







### **1.2 Linux容器技术VS虚拟机**



![](C:\Users\chenbolan\Pictures\Camera Roll\1.PNG)

容器技术相比虚拟机磁盘占用空间更少、启动更快、系统性能好、支持容器量大







### **1.3 什么是Docker?**



#### **1.3.1 将应用程序自动部署到容器**

Go语言开源引擎     Github地址：https://github.com/docker/docker

2013年初 dotCloud公司

基于Apache2.0开源授权协议发行







### **1.4 Docker的目标**（特点）



#### 1.4.1 **提供简单轻量的建模方式**

易于上手、秒运行、充分利用系统资源

#### 1.4.2 **职责的逻辑分离**

开发人员关心容器中的运行程序

运维人员关心如何管理容器

#### 1.4.3 **快速高效的开发生命周期**

缩短代码从开发、测试到部署、上线运行的周期，开发、测试和生产都使用相同的环境

#### 1.4.4 **鼓励使用面向服务的构架**

高类聚、低耦合、单一任务、消除服务间的相互影响







### **1.5 Docker的使用场景**



#### 1.5.1 使用**Docker**容器开发、测试、部署任务

#### 1.5.2 **创建隔离的运行环境**

如：不同版本、不同用户等

#### 1.5.3 **搭建测试环境**

程序在不同系统下的兼容性甚至是搭建集群部署的测试

#### 1.5.4 **构建多用户平台即服务**（PaaS）基础设施

#### 1.5.5 **提供软件即服务**（SaaS）应用程序

#### 1.5.6 **高性能**、超大规模的宿主机部署



------



## **2. Docker的基本组成**



Docker Client 客户端

Docker Daemon 守护进程

Docker Image 镜像

Docker Container 容器

Docker Registry 仓库



### 2.1 C/**S**架构

![1606290103689](C:\Users\chenbolan\AppData\Roaming\Typora\typora-user-images\1606290103689.png)

Client通过接口与Server进程通信实现容器的构建，运行和发布

#### 2.1.1 Host（Docker 宿主机）

安装了Docker程序，并运行了Docker daemon的主机

#### **2.1.2 Docker 客户端/守护进程**

Docker命令行工具，用户是用Docker Client与Docker daemon进行通信并返回结果给用户

本地/远程

运行在宿主机上，Docker守护进程，用户通过Docker client(Docker命令)与Docker daemon交互

#### 2.1.3 Docker Image 镜像

容器的基石，一个镜像可以创建多个容器（容器基于镜像启动和运行）

层叠的只读文件系统（bootfs）

联合加载（union mount）

##### 2.1.3.1 镜像分层结构：

![1606290715764](C:\Users\chenbolan\AppData\Roaming\Typora\typora-user-images\1606290715764.png)

位于下层的镜像称为父镜像(Parent Image)，最底层的称为基础镜像(Base Image)。 

仅最上层为可视层即可读写

#### **2.1.4Docker Container 容器**

通过镜像启动

启动和执行阶段

写时复制（copy on write）

#### **2.1.5Docker Registry 仓库**

保存用户构建的镜像

公有 Docker Hub

私有 





------



## **3. Docker 容器的相关技术**



### 3.1 Docker依赖的Linux内核特性

Namespaces 命名空间

Control groups （cgroups）控制组



#### **3.1.1 Namespaces 命名空间**

如：编程语言

​				封装  →  代码隔离

​		操作系统

​				系统资源的隔离

​				进程、网络、文件系统等



PID （Process ID)    进程隔离

NET （Network)      管理网络接口

IPC  （InterProcess Communication)      管理跨进程通信的访问

MNT （Mount)      管理挂载点

UTS （Unit Timesharing System)     隔离内核和版本标识



#### 3.1.2 Control groups （cgroups）控制组

用来分配资源

来源于google

Linux kernel2.6.24@2007

为实现容器而生

功能：

资源限制

优先级设定

资源计量

资源控制



#### **3.1.3 Docker 容器的能力**

文件系统隔离：每个容器都有自己的root文件系统

进程隔离：每个容器都运行在自己的进程环境中

网络隔离：容器间的虚拟网络接口和IP地址都是分开的

资源隔离和分组：使用cgroups将CPU和内存之类的资源独立分配给每个docker容器







------

## **4. Docker基于windows10 home的安装**

### 4.1 安装**Hyper**-V

若打开此电脑此界面没有Hyper-V，则将下面的代码保存到一bat文件中，然后以管理员身份运行，重启系统后再打开此界面就会出现Hyper-V

~~~pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
~~~

![1606306194350](C:\Users\chenbolan\AppData\Roaming\Typora\typora-user-images\1606306194350.png)

### **4.2 官网下载docker desktop for windows**

地址：https://www.docker.com/products/docker-desktop

![1606306736914](C:\Users\chenbolan\AppData\Roaming\Typora\typora-user-images\1606306736914.png)

双击安装，完成后启动

启动docker desktop,如果报错：WSL 2 installation is incomplete.，则执行3

### **4.3 更新WSL 2**

参考网址：https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel

重启docker desktop









------



## **5. Docker 网络**

Docker允许通过外部访问容器或容器互联的方式来访问网络

安装Docker时，会自动安装一块Docker网卡称为docker0，用于Docker各容器及宿主机的网络通信，网段为172.0.0.1

Docker网络中有三个核心概念：沙盒（Sandbox）、网络（Network）、端点（Endpoint）

沙盒，提供了容器的虚拟网络栈，也即端口套接字、IP路由表、防火墙等内容。隔离容器网络与宿主机网络形成了完全独立的容器网络环境

网络，可以理解为Docker内部的虚拟子网，网络内的参与者相互可见并能够进行通讯。Docker的虚拟网络和宿主机网络是存在隔离关系的，其母的主要是形成容器间的安全通讯环境

断电，位于容器或网络隔离墙之上的洞，主要目的是形成一个可以控制的突破封闭的网络环境的出入口。当容器的断电与网络的端点形成配对后，就如同在这两者之间搭建了桥梁，便能够进行数据传输了

### **5.1 四类网络模式**

![1606398075699](C:\Users\chenbolan\AppData\Roaming\Typora\typora-user-images\1606398075699.png)

#### **5.1.1 host模式**

如果启动容器的时候使用host模式，那么这个容器将不会获得一个独立的Network Namespace，而是和宿主机共用一个Network Namespace。容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口。但是，容器的其他方面，如文件系统、进程列表等还是和宿主机隔离的。

使用host模式的容器可以直接使用宿主机的IP地址与外界通信，容器内部的服务端口也可以使用宿主机的端口，不需要进行NAT，host最大的优势就是网络性能比较好，但是docker host上已经使用的端口就不能再用了，网络的隔离性不好。

 ![img](https://upload-images.jianshu.io/upload_images/13618762-a892da42b8ff9342.png?imageMogr2/auto-orient/strip|imageView2/2/w/698/format/webp) 

#### **5.1.2 container模式**

这个模式指定新创建的容器和已经存在的一个容器共享一个 Network Namespace，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的 IP，而是和一个指定的容器共享 IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。两个容器的进程可以通过 lo 网卡设备通信。

 ![img](https://upload-images.jianshu.io/upload_images/13618762-790a69a562a5b358.png?imageMogr2/auto-orient/strip|imageView2/2/w/695/format/webp) 

#### **5.1.3 none模式**

使用none模式，Docker容器拥有自己的Network Namespace，但是，并不为Docker容器进行任何网络配置。也就是说，这个Docker容器没有网卡、IP、路由等信息。需要我们自己为Docker容器添加网卡、配置IP等。

这种网络模式下容器只有lo回环网络，没有其他网卡。none模式可以在容器创建时通过--network=none来指定。这种类型的网络没有办法联网，封闭的网络能很好的保证容器的安全性。

 ![img](https://upload-images.jianshu.io/upload_images/13618762-3fd41778faebcef5.png?imageMogr2/auto-orient/strip|imageView2/2/w/723/format/webp) 

#### **5.1.4 bridge模式**

当Docker进程启动时，会在主机上创建一个名为docker0的虚拟网桥，此主机上启动的Docker容器会连接到这个虚拟网桥上。虚拟网桥的工作方式和物理交换机类似，这样主机上的所有容器就通过交换机连在了一个二层网络中。

从docker0子网中分配一个IP给容器使用，并设置docker0的IP地址为容器的默认网关。在主机上创建一对虚拟网卡veth pair设备，Docker将veth pair设备的一端放在新创建的容器中，并命名为eth0（容器的网卡），另一端放在主机中，以vethxxx这样类似的名字命名，并将这个网络设备加入到docker0网桥中。可以通过brctl show命令查看。

bridge模式是docker的默认网络模式，不写--net参数，就是bridge模式。使用docker run -p时，docker实际是在iptables做了DNAT规则，实现端口转发功能。可以使用iptables -t nat -vnL查看。

 ![img](https://upload-images.jianshu.io/upload_images/13618762-f1643a51d313a889.png?imageMogr2/auto-orient/strip|imageView2/2/w/1083/format/webp) 

------

## **6. Docker常用操作**

输入docker可以查看Docker的命令用法，输入docker COMMAND --help可以查看指定命令详细用法

### **6.1 镜像常用操作**

#### **查找镜像**：

~~~docker search 关键词			#搜索docker hub网站镜像的详细信息
docker search 关键词			#搜索docker hub网站镜像的详细信息
~~~

#### **下载镜像：**

~~~docker pull 镜像名：TAG			#Tag表示版本，有些镜像的版本显示为latest，为最新版本
docker pull 镜像名：TAG			#Tag表示版本，有些镜像的版本显示为latest，为最新版本
~~~

#### **删除镜像：**

~~~
docker rmi -f 镜像ID或者镜像名：TAG			#获取镜像的元信息，详细信息
~~~

### **6.2容器常用操作：**

#### **运行：**

~~~
docker run --name 容器名 -i -t -p 主机端口：容器端口 -d -v 主机目录：容器目录:ro 镜像ID或镜像名：TAG
# --name 指定容器名，可自定义，不指定自动命名
# -i 以交互模式运行容器
# -t 分配一个伪终端，即命令行，通常-it组合来使用
# -p 指定映射端口，讲主机端口映射到容器内的端口
# -d 后台运行容器
# -v 指定挂在主机目录到容器目录，默认为rw读写模式，ro表示只读
~~~

#### **容器列表：**

~~~
docker ps -a -q
# docker ps查看正在运行的容器
# -a 查看所有容器（运行中、未运行）
# -q 之查看容器的ID
~~~

#### **启动容器：**

~~~
docker start 容器ID或容器名
~~~

#### **停止容器：**

~~~
docker stop 容器ID或容器名
~~~

#### **删除容器：**

~~~
docker rm -f 容器ID或容器名
# -f 表示强制删除
~~~

#### **查看日志：**

~~~
docker logs 容器ID或容器名
~~~

#### **进入正在运行的容器：**

~~~
docker exec -it 容器ID或容器名 /bin/bash
# 进入正在运行的容器并且开启交互模式
# /bin/bash是固有写法，作用是因为docker后台必须运行一个进程，否则容器就会退出，在这里表示启动容器后启动bash
# 也可以用docker exec在运行中的容器执行命令
~~~

#### **拷贝文件：**

~~~
docker cp 主机文件路径 容器ID或容器名:容器路径
# 主机中文件拷贝到容器中
docker cp 容器ID或容器名:容器路径 主机文件路径
# 容器中文件拷贝到主机中
~~~

#### **获取容器元信息：**

~~~
docker inspect 容器ID或容器名
~~~



### **6.3 docker desktop工具介绍**

#### **6.3.1 一般 General**

 在“ 设置”对话框的General选项卡上，可以配置何时启动和更新Docker

- **登录时启动Docker** - 在Windows系统登录时自动启动Docker Desktop for Windows应用程序
- **自动检查更新** - 默认情况下，Docker Desktop for Windows会自动检查更新并在更新可用时通知，单击“ **确定”**接受并安装更新（或取消以保留当前版本）。可以通过从Docker主菜单中选择**Check for Updates**来手动更新
- **发送使用情况统计信息** - 默认情况下，Docker Desktop for Windows会发送诊断，崩溃报告和使用情况数据。此信息有助于Docker改进应用程序并对其进行故障排除。取消选中此选项。Docker有时也可能会提示提供更多信息

#### **6.3.2 代理 Resources Proxies**

 Docker Desktop for Windows允许配置HTTP / HTTPS代理设置并自动将这些设置传播到Docker和容器。例如，如果将代理设置设置为`http://proxy.example.com`，则Docker在提取容器时会使用此代理。 

#### **6.3.3 实验模式Experimental Features: Docker Engine**

Docker Desktop for Windows Stable和Edge版本都启用了Docker Engine的实验版本，在GitHub上的[Docker实验特性README中](https://github.com/docker/cli/blob/master/experimental/README.md)有所描述。

实验性功能不适用于生产环境或工作负载。它们是用于新想法的沙盒实验。一些实验性功能可能会合并到即将发布的稳定版本中，但其他版本可能会从后续Edge版本中修改或删除，并且永远不会在Stable上发布。

在Edge和Stable版本上，可以打开和关闭**实验模式**。如果将其关闭，Docker Desktop for Windows将使用当前常用的Docker Engine版本。

运行`docker version`以查看是否处于实验模式。

#### **6.3.4 Kubernetes**

 [适用于Windows的Docker Desktop上的Kubernetes](https://docs.docker.com/docker-for-windows/kubernetes/) 在 [18.02 Edge（win50）](https://docs.docker.com/docker-for-windows/edge-release-notes/#docker-community-edition-18020-ce-rc1-win50-2018-01-26)及更高版本以及[18.06 Stable（win70）](https://docs.docker.com/docker-for-windows/edge-release-notes/#docker-community-edition-18060-ce-win70-2018-07-25)及更高版本中[可用](https://docs.docker.com/docker-for-windows/edge-release-notes/#docker-community-edition-18060-ce-win70-2018-07-25)。 

 有关使用Kubernetes与Docker Desktop for Windows集成的更多信息，[参阅在Kubernetes上部署](https://docs.docker.com/docker-for-windows/kubernetes/)。 



------

## **7. Docker生成镜像的两种方式**

### **7.1 更新镜像**

先使用基础镜像创建一个容器，然后对容器内容进行更改，然后使用dockers commit 命令提交为一个新的镜像（以tomcat为例）

#### **7.1.1 根据基础镜像，创建容器**

~~~
docker run --name mytomcat -p 80:8080 -d tomcat
~~~

#### **7.1.2 修改容器内容**

~~~
docker exec -it mytomcat /bin/bash
cd weapps/ROOT
rm -f index.jsp
echo hello world > index.html
exit
~~~

#### **7.1.3 提交为新镜像**

~~~
docker commit -m="描述消息" -a="作者" 容器ID或容器名 镜像名：TAG
# 例：
# docker commit -m="修改了首页" -a="曾玉林" mytomcat zyl/tomcat:latest
~~~

#### **7.1.4 使用新镜像运行容器**

~~~
docker run --name tom -p 8080:8080 -d zyl/tomcat:latest
~~~

### **7.2 使用Dockerfile构建镜像**

#### **7.2.1 什么是Dockerfile？**

~~~
Dockfile是一种被Docker程序解释的脚本，Dockerfile由一条一条的指令组成，每条指令对应Linux下面的一条命令。Docker程序将这些Dockerfile指令翻译真正的Linux命令。Dockerfile有自己书写格式和支持的命令，Docker程序解决这些命令间的依赖关系，类似于Makefile。Docker程序将读取Dockerfile，根据指令生成定制的image。相比image这种黑盒子，Dockerfile这种显而易见的脚本更容易被使用者接受，它明确的表明image是怎么产生的。有了Dockerfile，当我们需要定制自己额外的需求时，只需在Dockerfile上添加或者修改指令，重新生成image即可，省去了敲命令的麻烦
~~~

#### **7.2.2 Dockerfile的书写规则及指令使用方法**

 Dockerfile的指令是忽略大小写的，建议使用大写，使用`#`作为注释，每一行只支持一条指令，每条指令可以携带多个参数。 

Dockerfile的指令根据作用可以分为两种，**构建指令和设置指令**。构建指令用于构建image，其指定的操作不会在运行image的容器上执行；设置指令用于设置image的属性，其指定的操作将在运行image的容器中执行。

##### **FROM（指定基础image）**

 构建指令，必须指定且需要在Dockerfile其他指令的前面。后续的指令都依赖于该指令指定的image。FROM指令指定的基础image可以是官方远程仓库中的，也可以位于本地仓库。 

~~~
FROM <image>
或者
FROM <image>:<tag>
~~~

##### **MAINTAINER（用来指定镜像创建者的信息）**

~~~
MAINTAINER <name>
~~~

##### **RUN（安装软件用）** 

构建指令，RUN可以运行任何被基础image支持的命令。如基础image选择了ubuntu，那么软件管理部分只能使用ubuntu的命令。 

~~~
RUN <command> (the command is run in a shell - /bin/sh -c)
RUN ["executable", "param1", "param2" ... ] (exec form)
~~~

##### **CMD（设置container启动时执行的操作）** 

设置指令，用于container启动时指定的操作。该操作可以是执行自定义脚本，也可以是执行系统命令。该指令只能在文件中存在一次，如果有多个，则只执行最后一条。 

~~~
CMD ["executable","param1","param2"] (like an exec, this is the preferred form)
CMD command param1 param2 (as a shell)
~~~

 当Dockerfile指定了ENTRYPOINT，那么使用下面的格式： 

~~~
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
~~~

##### **ENTRYPOINT（设置container启动时执行的操作）** 

设置指令，指定容器启动时执行的命令，可以多次设置，但是只有最后一个有效。 

~~~
ENTRYPOINT ["executable", "param1", "param2"] (like an exec, the preferred form)
ENTRYPOINT command param1 param2 (as a shell)
~~~

##### **USER（设置container容器的用户）** 

设置指令，设置启动容器的用户，默认是root用户

~~~
# 指定memcached的运行用户  
ENTRYPOINT ["memcached"]  
USER daemon  
或  
ENTRYPOINT ["memcached", "-u", "daemon"]  
~~~

##### **EXPOSE（指定容器需要映射到宿主机器的端口）** 

设置指令，该指令会将容器中的端口映射成宿主机器中的某个端口。当你需要访问容器的时候，可以不是用容器的IP地址而是使用宿主机器的IP地址和映射后的端口。要完成整个操作需要两个步骤，首先在Dockerfile使用EXPOSE设置需要映射的容器端口，然后在运行容器的时候指定-p选项加上EXPOSE设置的端口，这样EXPOSE设置的端口号会被随机映射成宿主机器中的一个端口号。也可以指定需要映射到宿主机器的那个端口，这时要确保宿主机器上的端口号没有被使用。EXPOSE指令可以一次设置多个端口号，相应的运行容器的时候，可以配套的多次使用-p选项。

~~~
# 映射一个端口  
EXPOSE port1  
# 相应的运行容器使用的命令  
docker run -p port1 image  
  
# 映射多个端口  
EXPOSE port1 port2 port3  
# 相应的运行容器使用的命令  
docker run -p port1 -p port2 -p port3 image  
# 还可以指定需要映射到宿主机器上的某个端口号  
docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image  
~~~

##### **ENV（用于设置环境变量）** 

ENV指令可以用于为docker容器设置环境变量

ENV设置的环境变量，可以使用docker inspect命令来查看。同时还可以使用docker run --env <key>=<value>来修改环境变量

~~~
ENV JAVA_HOME /path/to/java/dirent
~~~

##### **ADD（从src复制文件到container的dest路径）** 

构建指令，所有拷贝到container中的文件和文件夹权限为0755，uid和gid为0；如果是一个目录，那么会将该目录下的所有文件添加到container中，不包括目录；如果文件是可识别的压缩格式，则docker会帮忙解压缩（注意压缩格式）；如果<src>是文件且<dest>中不使用斜杠结束，则会将<dest>视为文件，<src>的内容会写入<dest>；如果<src>是文件且<dest>中使用斜杠结束，则会<src>文件拷贝到<dest>目录下

~~~
ADD <src> <dest>
~~~

##### **VOLUME (指定挂载点)** 

 创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。 

~~~
FROM base  
VOLUME ["/tmp/data"] 
~~~

#####  **LABEL(定义标签)** 

 定义一个 image 标签 Owner，并赋值，其值为变量 Name 的值。 

~~~
LABEL Owner=$Name
~~~

#### **7.2.3创建Dockerfile，构建运行环境（实例演示）**

##### **在jar包路径下创建Dockerfile文件Dockerfile**

~~~
# 指定基础镜像，本地没有会从dockerHub pull下来
FROM java:8
# 作者
MAINTAINER zyl
# 把可执行jar包复制到基础镜像的根目录下
ADD luban.jar /luban.jar
# 镜像要包路的端口，如要使用端口，在执行docker run命令时使用-p生效
EXPOSE 80
# 在镜像运行为容器后执行的命令
ENTRYPOINT ["java","-jar","/luban.jar"]
~~~

##### **使用docker bulid命令构建镜像，基本语法**

~~~
docker bulid -t  zyl/mypro:latest
# -f指定Dockerfile文件的路径
# -t指定镜像名字和TAG
# .之当前目录，这里实际上需要一个上下文路径
~~~

##### **运行**

运行自己的SpringBoot镜像

~~~
docker run --name pro -p 80:80 -d 镜像名:TAG
~~~