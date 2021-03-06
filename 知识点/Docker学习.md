#一.基本概念
##1.1.定义：软件以及它运行安装所需的一切文件（代码、运行时、系统工具、系统库）打包到一起，这就保证了不管是在什么样的运行环境，总是能以相同的方式运行。

##1.2.优点
###1.2.1.轻量级：所有容器在一台机器上共享同一个操作系统内核，这样他们立即开始，并更有效地利用内存。Image 是从分层文件系统的构建，这样他们能够共享公共文件，使得磁盘使用率和 Image 的下载更加高效
###1.2.2.开放：Docker 容器是基于开发的标准，允许容器运行在主流的 Linux 发布版和 Microsoft 操作系统作为所有的基础设施。
###1.2.3.安全：容器使得应用程序彼此隔离，而基础架构同时为应用程序提供了额外的保护层。

##1.3.作用
###1.3.1.开发更加敏捷：Docker 让开发人员可以自由定义环境，创建和部署的应用程序更快、更容易，IT 运维人员快速应对变化也更加灵活性。
###1.3.2.更加可控：Docker 使得开发人员保存从基础设施到应用的代码，帮助 IT 运维人管理拥有标准的、安全的、可扩展的操作环境。
###1.3.3.高可移植性：Docker 允许自由选择，可以是从笔记本电脑到一个团队，从私人基础设施到公共云提供商。

#二.架构
##2.1.虚拟机架构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%9E%B6%E6%9E%84.png)
##2.2.容器架构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E5%AE%B9%E5%99%A8%E6%9E%B6%E6%9E%84.png)

#三.常见术语
##3.1.Docker Image镜像：容器的基石；层叠的只读文件系统；联合加载；一个镜像可以放到另一个镜像的底部   对于下面的镜像成为父镜像 依次类推 直到最下面的镜像称为基础镜像
##3.2.Docker Container容器：通过镜像启动   容器中可以运行客户的一个或多个镜像；启动和执行阶段 #加载一个读写层；写时复制
##3.3.Docker Registry仓库：公有与私有

#四.Docker命令结构图
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Docker%E5%91%BD%E4%BB%A4%E7%BB%93%E6%9E%84%E5%9B%BE.png)
##4.1.Docker容器的生命周期
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/docker%E5%AE%B9%E5%99%A8%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

#五.Docker内核知识
##5.1.数据卷：一个可供容器使用的特殊目录，他将主机操作系统目录直接映射到容器
- 数据卷可以在容器间共享和重用，容器间传递数据高效方便
- 对数据卷内数据的修改会立马生效，无论是容器内操作还是本地操作
- 对数据卷的更新不会影响镜像，解耦开应用和数据
- 卷会一直存在，直到没有容器使用，可以安全地卸载他

##5.2.网络核心管理
###5.2.1.host模式：如果启动容器的时候使用host模式，那么这个容器将不会获得一个独立的Network Namespace 而是和宿主机器共用一个Network Namespace 容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的ip和端口，但是容器的其他方面，如文件系统，进程列表等还是和宿主机器隔离的
###5.2.2.container模式：Container模式指定新创建的容器和已经存在的一个容器共享一个Network Namespace，而不是和宿主机器共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP，端口范围等。同样，两个容器除了网络方面，其他的如文件系统，进程列表等还是隔离的。两个容器的进程可以通过localhost网卡设备通信
###5.2.3.none模式：  使用none模式，docker容器拥有自己的Network Namespace，但是，并不为docker容器进行任何网络配置，也就是说，这个docker容器没有网卡，ip，路由等信息。需要我们自己为docker容器添加网卡，配置ip等
###5.2.4.bridge模式：此模式会为每一个容器分配Network Namespace，设置ip等，并将一个主机上的docker容器连接到一个虚拟网桥上

#六.Docker组件
##6.1.Docker客户端和服务器，也称为Docker引擎
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Docker%E6%9E%B6%E6%9E%84.png)
##6.2.Docker镜像
##6.3.Regisry
##6.4.Docker容器
- 一个镜像容器
- 一系列标准的操作
- 一个执行环境
##6.5.Docker技术组件
- 一个原生的Linux容器格式，Docker中称为libcontainer
- Linux内核的命名空间，用于隔离文件系统、进程和网络
- 文件系统隔离：每个容器都有自己的root文件系统
- 进程隔离：每个容器都运行在自己的进程环境中
- 网络隔离：容器间的虚拟网络接口和IP地址分开
- 资源隔离和分组：使用cgroups将CPU和内存之类的资源独立分配给每个Docker容器
- 写时复制：文件系统都是通过写时复制创建的，文件系统是分层的、快速的、占用空间小
- 日志：日志分析和故障排错
- 交互式shell
#七.核心概念
##7.1.镜像：一堆可读层的统一视角（统一文件系统）
- 一个特殊的文件系统，除了提供容器运行时需要的程序、库、资源、配置文件等，还包含一些为运行时准备的一些配置参数（匿名卷、环境变量、用户等）
- 镜像不包含任何动态数据，内容在构建之后不会被改变
##7.2.仓库：集中存放镜像文件的场所
##7.3.容器：镜像创建的运行实例（容器最上面的那一层可读可写）
- 一个运行态容器被定义为一个可读写的统一文件系统加上隔离的进程空间和包含其中的进程
- 容器=镜像+可读层


#八.容器十诫
- 不要在容器内存储数据---容器可以被停止、销毁或者取代，如果要存储数据，将其存储在卷组中，在这种情况下，要小心两个容器向同一卷组写入数据的情况。确保自己的应用向共享的数据存储区填写数据
- 不要将应用分开发布。容器是不可变得
- 不要创建太大的镜像----镜像越大，越难以分发。确保只留有运行应用或进程所需的文件和库
- 不要使用单层镜像---为了有效利用分层的文件系统，总是为操作系统创建基础镜像，此外，分别为用户名定义、运行时安装、配置、以及自己的应用创建不同的镜像
- 不要为运行中的容器创建镜像
- 不要单独使用“latest”标签---因为容器本身的分层式文件系统，鼓励使用标签
- 不要在一个容器内运行多个进程---容器在运行单一进程使几乎无可挑剔，如果运行多个进程，可能在管理、检索日志以及独个更新进程时遇到麻烦
- 不要在镜像中存储凭证---使用环境变量。不要将镜像中的任何用户名或密码写死。使用环境变量从容器外部检索这些信息
- 不要以root用户运行进程---默认情况下，docker容器使用root权限运行
- 不要依赖IP地址---每个容器都有其内部IP地址，该地址可能因为启动或停止容器而发生改变，如果应用或者微服务需要与另一个容器交换消息，应该使用环境变量在容器间传送合适的主机名与端口号





#八.Dockerfile（[https://www.jianshu.com/p/12fed992c3eb](https://www.jianshu.com/p/12fed992c3eb)）
##8.1.四个部分
- 基础镜像（父镜像）信息指令FROM
- 维护着信息指令MAINTAINER
- 镜像操作指令RUN、EVN、ADD以及WORDDIR等
- 容器启动指令CMD、ENTRYPOINT和USER等
##8.2.常用命令
###8.2.1.FROM
- 功能为指定基础镜像，并且必须是第一条指令。
###语法
- FROM <image\>
- FROM <image\>:<tag\>
- FROM <image\>:<digest\> 
- 其中<tag\><digest\>为可选项，默认值为latest
###8.2.2.RUN
- 功能为运行指定命令
###语法
- RUN <command\>
- RUN ["executable","param1","param2"]
### 第一种后面直接跟shell命令
- 在Linux操作系统上默认/bin/sh -c
###第二种类似于函数调用
- 可将executable理解成为可执行文件，后面就是两个参数
###注意
- 多行命令不要写多个RUN，因为Dockerfile中每一个指令都会建立一层，多少个RUN就构建多少层镜像，会造成镜像臃肿
###8.2.3.CMD
- 功能为容器启动时要运行的命令
###语法
- CMD ["executable","param1","param2"]
- CMD ["param1","param2"]
- CMD command param1 param2
###RUN与CMD的区别
- RUN是构建容器是就运行的命令以及提交运行结果
- CMD是容器七佛那个时执行的命令，在构建时并不运行，构建时仅指定了这个命令到底是个什么样子的
###8.2.4.LABEL
- 功能是为镜像指定标签
###语法
- LABEL <key\>=<value\> <key\>=<value\> <key\>=<value\> ...
###注意
- LABEL会继承基础镜像种的LABEL，如遇到key相同，则值覆盖
###8.2.5.MAINTAINER
- 指定作者
###语法
- MAINTAINER <name\>
###8.2.6.EXPOSE
- 暴露容器运行时的监听端口给外部
- EXPOSE并不会使容器访问主机的端口
- 若想使得容器与主机的端口有映射关系，必须在容器启动时加上-P参数


###8.2.7.ENV
- 设置环境变量
###语法
- ENV <key\> <value\>
- ENV <key\>=<value\> ...


###8.2.8.ADD
- 一个复制命令，将文件复制到镜像中
###语法
- ADD <src\>... <dest\>
- ADD ["<src\>",... "<dest\>"]
###注意
- <dest\>路径的填写可以是容器内的绝对路径，也可以是相对于工作目录的相对路径
- <src\>可以是一个本地文件或者是一个本地压缩文件，还可以是一个url
- 如果把<src\>写成一个url，那么ADD就类似于wget命令
- 尽量不要把<src\>写成一个文件夹，如果<src\>是一个文件夹了，复制整个目录的内容,包括文件系统元数据
###8.2.9.COPY
- 复制命令
###语法
- COPY <src\>... <dest\>
- COPY ["<src\>",... "<dest\>"]
###与ADD的区别
-  COPY的<src>只能是本地文件，其他用法一致
###8.2.10.ENTRYPOINT
- 启动时的默认命令
###语法
- ENTRYPOINT ["executable", "param1", "param2"]
- ENTRYPOINT command param1 param2

###与CMD比较说明
####相同点
- 只能写一条，如果写了多条，那么只有最后一条生效
- 容器启动时才运行，运行时机相同

####不同点
- ENTRYPOINT不会被运行的command覆盖，而CMD则会被覆盖
- 如果我们在Dockerfile种同时写了ENTRYPOINT和CMD，并且CMD指令不是一个完整的可执行命令，那么CMD指定的内容将会作为ENTRYPOINT的参数
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/ENTRYPOINT%20%E5%92%8CCMD%E4%B8%8D%E5%90%8C%E7%BB%84%E5%90%88%E7%9A%84%E6%89%A7%E8%A1%8C%E6%83%85%E5%86%B5.png)

###8.2.11.VOLUME
- 实现挂载功能，可以将本地文件夹或其他容器中的文件夹挂载到这个容器中
###语法
- VOLUME ["/data"]

###注意
- ["/data"]可以是一个JsonArray ，也可以是多个值。


#注意
- 容器技术是和宿主机共享硬件资源及操作系统，可以实现资源的动态分配
- 一般的使用场景为需要持久化存储数据时
- 容器使用的是AUFS，这种文件系统不能持久化数据，当容器关闭后，所有的更改都会丢失，所以数据需要持久化时用这个命令

###8.2.12.USER
- 设置启动容器的用户，可以是用户名或UID
###语法
- USER daemo
- USER UID
###注意
- 如果设置了容器以daemon用户去运行，那么RUN, CMD 和 ENTRYPOINT 都会以这个用户去运行

###8.2.13.WORKDIR
- 设置工作目录，对RUN,CMD,ENTRYPOINT,COPY,ADD生效。如果不存在则会创建，也可以设置多次。
###语法
- WORKDIR /path/to/workdir

###8.2.13.ARG
- 设置变量命令，ARG命令定义了一个变量，在docker build创建镜像的时候，使用--build-arg <varname\>=<value\>来指定参数

###8.2.14.ONBUILD
- 这个命令只对当前镜像的子镜像生效。
###语法
- ONBUILD [INSTRUCTION]

###8.2.15.STOPSIGNAL
- 当容器退出时给系统发送什么样的指令
###语法
- STOPSIGNAL signal



###8.2.16.HEALTHCHECK
- 容器健康状况检查命令
###语法
- HEALTHCHECK [OPTIONS] CMD command
	- [OPTIONS]的选项支持以下三中选项：	
		- interval=DURATION 两次检查默认的时间间隔为30秒
		- timeout=DURATION 健康检查命令运行超时时长，默认30秒
		- retries=N 当连续失败指定次数后，则容器被认为是不健康的，状态为unhealthy，默认次数是3
- HEALTHCHECK NONE
###注意
- 第一个的功能是在容器内部运行一个命令来检查容器的健康状况
- 第二个的功能是在基础镜像中取消健康检查命令
- HEALTHCHECK命令只能出现一次，如果出现了多次，只有最后一个生效


##8.3.优化
1. 选择合适的基础镜像：能满足运行应用所需要的最小的镜像，理论上是能用小的就不要用大的，能用轻量的就不要用重量级的，能用性能好的就不要用性能差的。这里有时候还需要考虑那些能够减少我们构建层数的基础镜像。
2. 优化指令顺序：比如，我们可以把WORKDIR、ENV等命令放前面，COPY、ADD放后面。
3. 合并指令：
4. 使用.dockerignore






#九.Docker持续开发工作流
##9.1.基于Docker容器的内部循环开发工作流
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E5%9F%BA%E4%BA%8EDocker%E5%AE%B9%E5%99%A8%E7%9A%84%E5%86%85%E9%83%A8%E5%BE%AA%E7%8E%AF%E5%BC%80%E5%8F%91%E6%B5%81.png)