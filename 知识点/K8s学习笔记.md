#一.Kubernetes架构图
![](https://i.imgur.com/kyRW1Mj.png)
##1.1.Pod:包含一组容器和卷。同一个Pod里的容器共享同一个网络命名空间，可以使用localhost互相通信。Pod是短暂的，不是持续性实体。
###1.1.1.Pod是短暂的，那么我怎么才能持久化容器数据使其能够跨重启而存在呢？ 是的，Kubernetes支持卷的概念，因此可以使用持久化的卷类型。
###1.1.2.是否手动创建Pod，如果想要创建同一个容器的多份拷贝，需要一个个分别创建出来么？可以手动创建单个Pod，但是也可以使用Replication Controller使用Pod模板创建出多份拷贝
###1.1.3.如果Pod是短暂的，那么重启时IP地址可能会改变，那么怎么才能从前端容器正确可靠地指向后台容器呢？这时可以使用Service
##1.2.Label:一个Label是attach到Pod的一对键/值对，用来传递用户定义的属性。
##1.3.Replication Controller:确保任意时间都有指定数量的Pod“副本”在运行。如果为某个Pod创建了Replication Controller并且指定3个副本，它会创建3个Pod，并且持续监控它们。如果某个Pod不响应，那么Replication Controller会替换它，保持总数为3.
###1.3.1.创建Replication Controller需要指定两个东西（Pod模板：用来创建Pod副本的模板；Label:Replication Controller需要监控的Pod的标签）
##1.4.Service:是定义一系列Pod以及访问这些Pod的策略的一层抽象。Service通过Label找到Pod组。因为Service是抽象的，所以在图表里通常看不到它们的存在。
##1.5.Node.节点是物理或者虚拟机器，作为Kubernetes worker，通常称为Minion。每个节点都运行如下Kubernetes关键组件
###1.5.1.Kubelet：是主节点代理。
###1.5.2.Kube-proxy：Service使用其将链接路由到Pod
###1.5.3.Docker或Rocket：Kubernetes使用的容器技术来创建容器
##1.6.Kubernetes Master:Kubernetes Master提供集群的独特视角，并且拥有一系列组件，比如Kubernetes API Server。API Server提供可以用来和集群交互的REST端点。master节点包括用来创建和复制Pod的Replication Controller。

#二.一些抽象概念的理解
##2.1.Service的关键特征
###2.1.1.拥有一个唯一指定的名字
###2.1.2.拥有一个虚拟IP和端口号
###2.1.3.能够提供某种远程服务能力
###2.1.4.被映射到了提供这种服务能力的一组容器应用上

#三.基本概念与术语
##3.1.Master节点（注意高可用）：Master节点上需要启动一个etcd服务，因为kubernetes里的所有资源对象的数据全部保存在etcd中。
###3.1.1.Kubernetes API Server ( kube叩 iserver ） ： 提供了 HTTP Rest 接口的关键服务进程，是Kubernetes 里所有资源 的增、删、改、 查等操作的唯一入口，也是集群控制的入口进程。
###3.1.2.Kubernetes Controller Manager ( kube-controller-manager ) : Kubernetes 里所有资源对象的自动化控制中心，可以理解为资源对象的“大总管”。
###3.1.3.Kubernetes Scheduler C!rube-scheduler ）： 负责资源调度（ Pod 调度〉的进程，相当于公交公司的“调度室"

##3.2.Node:工作负载节点
###3.2.1.kubelet：负责 Pod 对应的容器的创建、启 停等任务 ，同时与 Master 节点密切协作， 实现集群管理的基本功能。
###3.2.2.kube-proxy ： 实现 Kubernetes Service 的通信与负载均衡机制的重要组件 。
###3.2.3.Docker Engine ( docker ) : Docker 引 擎，负责本机的容器创建和管理工作。

##3.3.Pod
![](https://i.imgur.com/lEib1IU.png)
### Pause 容器对应的镜像属于 Kubemetes平台的一部分

##3.4.Label与Label Selector

##3.5.Replication Controller
###3.5.1.Pod期待的副本数
###3.5.2.用于筛选目标Pod的Label Selector
###3.5.3.当Pod的副本数量小于预期数量时，用于创建Pod的Pod模板

##3.6.Deployment

##3.7.Horizontal Pod Autoscaler(Pod横向自动扩容)
###3.7.1.CPUUtilizationPercentage
###3.7.2.应用程序自定义的度量指标，比如服务在每秒内的相应请求数

##3.8.StatefulSet:针对一些有状态的服务

##3.9.Service
![](https://i.imgur.com/2X4elaU.png)
###3.9.2.Kubernetes的服务发现机制：DNS系统
###3.9.3.外部系统访问Service的问题
###Node IP :是 Kubemetes 集群中每个节点 的物理网卡的 IP 地址，这是一个真实存在的物理网络，所有属于这个网络的服务器之间都能通过这个网络直接通信，不管它们中是否有部分节点不属于这个 Kubemetes 集群。这也表明了 Kubemetes 集群之外的节点访问 Kubemetes 集群之内的某个节点或者 TCP/IP 服务时，必须要通过 Node IP 进行通信。
###Pod IP:是每个 Pod 的 IP 地址，它是 Docker Engine 根据 dockerO 网桥的 IP 地址段进行分配的，通常是一个虚拟的二层网络，前面我们说过， Kubemetes 要求位于不同 Node 上的Pod 能够彼此直接通信，所以 Kubemetes 里一个 Pod 里的容器访问另外一个 Pod 里的容器，就是通过 Pod IP 所在的虚拟二层网络进行通信的，而真实的 TCP/IP 流量则是通过 Node IP所在的物理网卡流出的 。
###Cluster IP:是一个虚拟的 IP，但更像是一个 “伪造”的 IP网络,属于 Kubemetes 集群内部的地址，无法在集群外部直接使用这个地址。

##3.10.Volume(存储卷)
###3.10.1.emptyDir:是在 Pod 分配到 Node 时创建的(临时空间，例如用于某些应用程序运行时所需的临时目录，且无须永久保留 ;长时间任务的中间过程 Check:Point 的临时保存目录 ;一个容器需要从另一个容器中获取数据的目录（多容器共享目录〉)
###3.10.2.hostPath:在 Pod 上挂载宿主机上的文件或目录(容器应用程序生成的日志文件需要永久保存时，可以使用宿主机的高速文件系统进行存储;需要访问宿主机上 Docker 引 擎内部数据结构的容器应用 时，可以通过定义 hostPath 为宿主机／var/lib/docker 目录，使容器内部应用可以直接访问 Docker 的文件系统)


##3.11.Namespace:多租户资源隔离
##3.12.Annotation:用户任意定义的“附加”信息，以便于外部工具进行查找