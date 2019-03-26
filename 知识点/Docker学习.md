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


#五.Docker内核知识
##5.1.



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


#八.Dockerfile
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


#注意
- 容器技术是和宿主机共享硬件资源及操作系统，可以实现资源的动态分配