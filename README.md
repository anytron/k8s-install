# ***\*Kubernetes\****

 

## [Kubernetes 文档 | Kubernetes](https://kubernetes.io/zh/docs/home/)

## ***\*k8s是什么\*******\*:\****

Kubernetes是一个开源的，用于管理云平台中多个主机上的容器化的应用，Kubernetes的目标是让部署容器化的应用简单高效，Kubernetes提供了应用部署、规划、更新、维护的一种机制。 在Kubenetes中，所有的容器均在Pod中运行，一个Pod可以承载一个或者多个相关的容器。同一个Pod中的容器会部署在同一个物理机器上并且能够共享资源。一个Pod也可以包含0个或者多个磁盘卷组（volumes）,这些卷组将会以目录的形式提供给一个容器，或者被所有Pod中的容器共享为什么要用k8s集群

## ***\*K8S实现了什么\*******\*:\****

从架构设计层面，我们关注的可用性，伸缩性都可以结合k8s得到很好的解决，如果你想使用微服务架构，搭配k8s，再从部署运维层面，服务部署，服务监控，应用扩容和故障处理，k8s都提供了很好的解决方案。

具体来说，主要包括以下几点：

l 服务发现与调度

l 负载均衡

l 服务自愈

l 服务弹性扩容

l 横向扩容

l 存储卷挂载

总而言之，k8s可以使我们应用的部署和运维更加方便

## ***\*K8s架构\****

### ***\*整体结构图\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps1.jpg) 

k8s集群由Master节点和Node（Worker）节点组成。

#### ***\*Master节点\****

Master节点指的是集群控制节点，管理和控制整个集群，基本上k8s的所有控制命令都发给它，它负责具体的执行过程。在Master上主要运行着：

l Kubernetes Controller Manager（kube-controller-manager）：k8s中所有资源对象的自动化控制中心，维护管理集群的状态，比如故障检测，自动扩展，滚动更新等。

l Kubernetes Scheduler（kube-scheduler）： 负责资源调度，按照预定的调度策略将Pod调度到相应的机器上。

l etcd：保存整个集群的状态。

#### ***\*Node节点\****

除了master以外的节点被称为Node或者Worker节点，可以在master中使用命令 kubectl get nodes查看集群中的node节点。每个Node都会被Master分配一些工作负载（Docker容器），当某个Node宕机时，该节点上的工作负载就会被Master自动转移到其它节点上。在Node上主要运行着：

l kubelet：负责Pod对应的容器的创建、启停等任务，同时与Master密切协作，实现集群管理的基本功能

l kube-proxy：实现service的通信与负载均衡

l docker（Docker Engine）：Docker引擎，负责本机的容器创建和管理

 

### ***\*组件间的协议\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps2.jpg) 

### ***\*master与\****[***\*node\****](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020)***\*架构图\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps3.jpg) 

 

### ***\*分层架构图\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps4.jpg) 

### ***\*核心组件\****

l etcd 保存了整个集群的状态

etcd是构建一个高可用的分布式键值(key-value)数据库。etcd内部采用raft协议作为一致性算法，etcd基于Go语言实现 ;主要用于共享配置和服务发现 ;

原理动画演示：http://thesecretlivesofdata.com/raft/

l apiserver 提供了资源操作的唯一入口，并提供认证、授权、访问控制、API 注册和发现等机制；

l controller manager 负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；

l scheduler 负责资源的调度，按照预定的调度策略将 Pod 调度到相应的机器上；

l kubelet 负责维护容器的生命周期，同时也负责 Volume（CSI）和网络（CNI）的管理；

l Container runtime 负责镜像管理以及 Pod 和容器的真正运行（CRI）；

l kube-proxy 负责为 Service 提供 cluster 内部的服务发现和负载均衡；

### ***\*其他重要组件：\****

l coredns：可以为集群中的SVC创建一个域名IP的对应关系解析

l dashboard：给 K8S 集群提供一个 B/S 结构访问体系

l ingress controller：官方只能实现四层代理，INGRESS 可以实现七层代理

l federation：提供一个可以跨集群中心多K8S统一管理功能

l prometheus ：提供K8S集群的监控能力

l elk：提供 K8S 集群日志统一分析介入平台

### ***\*分层介绍\****

\1. 核心层：Kubernetes 最核心的功能，对外提供 API 构建高层的应用，对内提供插件式应用执行环境；

\2. 应用层：部署（无状态应用、有状态应用、批处理任务、集群应用等）和路由（服务发现、DNS 解析等）、Service Mesh（部分位于应用层）；

\3. 管理层：系统度量（如基础设施、容器和网络的度量），自动化（如自动扩展、动态 Provision 等）以及策略管理（RBAC、Quota、PSP、NetworkPolicy 等）、Service Mesh（部分位于管理层）；

\4. 接口层：kubectl 命令行工具、客户端 SDK 以及集群联邦；

\5. 生态系统：在接口层之上的庞大容器集群管理调度的生态系统，可以划分为两个范畴；

(1) Kubernetes 外部：日志、监控、配置管理、CI/CD、Workflow、FaaS、OTS 应用、ChatOps、GitOps、SecOps 等

(2) Kubernetes 内部：CRI、CNI、CSI、镜像仓库、Cloud Provider、集群自身的配置和管理等

## ***\*K8s核心技术概念\****

### ***\*API对象\****

API对象是K8S的集群中管理操作单元；每个API对象都有3大类属性：元数据metadata，规范spec和状态status；元数据：标识API对象；

 

API对象至少含有3个元数据：namespace,name,uid;

 

API对象通过spec去设置配置； 用户通过配置系统的理想状态来改变系统 ， 所有的操作都是声明（Declarative）的而不是命令式（Imperative）的 ； 声明式操作在分布式系统中的好处是稳定，不怕丢操作或运行多次

### ***\*Pod\****

Pod 的设计理念是支持多个容器在一个 Pod 中共享网络地址和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。Pod 对多容器的支持是 K8 最基础的设计理念 ；

 

Pod 是 Kubernetes 集群中所有业务类型的基础，可以看作运行在 Kubernetes 集群中的小机器人，不同类型的业务就需要不同类型的小机器人去执行。

 

目前 Kubernetes 中的业务主要可以分为

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps5.jpg) 

### ***\*RC：副本控制器 Replication Controller\****

保证 Pod 高可用的 API 对象； 通过监控运行中的 Pod 来保证集群中运行指定数目的 Pod 副本。指定的数目可以是多个也可以是 1 个；少于指定数目，RC 就会启动运行新的 Pod 副本；多于指定数目，RC 就会杀死多余的 Pod 副本。即使在指定数目为 1 的情况下，通过 RC 运行 Pod 也比直接运行 Pod 更明智，因为 RC 也可以发挥它高可用的能力，保证永远有 1 个 Pod 在运行。RC 是 Kubernetes 较早期的技术概念，只适用于长期伺服型的业务类型，比如控制小机器人提供高可用的 Web 服务。

### ***\*RS：副本集 Replica Set\****

RS 是新一代 RC，提供同样的高可用能力，区别主要在于 RS 后来居上，能支持更多种类的匹配模式。副本集对象一般不单独使用，而是作为 Deployment 的理想状态参数使用。

 

RC 和 RS 主要是控制提供无状态服务的，其所控制的 Pod 的名字是随机设置的，一个 Pod 出故障了就被丢弃掉，在另一个地方重启一个新的 Pod，名字变了。名字和启动在哪儿都不重要，重要的只是 Pod 总数；

对于 RC 和 RS 中的 Pod，一般不挂载存储或者挂载共享存储，保存的是所有 Pod 共享的状态

### ***\*部署：Deployment\****

部署表示用户对 Kubernetes 集群的一次更新操作。部署是一个比 RS 应用模式更广的 API 对象，可以是创建一个新的服务，更新一个新的服务，也可以是滚动升级一个服务。滚动升级一个服务，实际是创建一个新的 RS，然后逐渐将新 RS 中副本数增加到理想状态，将旧 RS 中的副本数减小到 0 的复合操作；这样一个复合操作用一个 RS 是不太好描述的，所以用一个更通用的 Deployment 来描述。以 Kubernetes 的发展方向，未来对所有长期伺服型的的业务的管理，都会通过 Deployment 来管理。

### ***\*服务：service\****

RC、RS 和 Deployment 只是保证了支撑服务的微服务 Pod 的数量，但是没有解决如何访问这些服务的问题。一个 Pod 只是一个运行服务的实例，随时可能在一个节点上停止，在另一个节点以一个新的 IP 启动一个新的 Pod，因此不能以确定的 IP 和端口号提供服务。要稳定地提供服务需要服务发现和负载均衡能力。服务发现完成的工作，是针对客户端访问的服务，找到对应的的后端服务实例。在 K8 集群中，客户端需要访问的服务就是 Service 对象。每个 Service 会对应一个集群内部有效的虚拟 IP，集群内部通过虚拟 IP 访问一个服务。在 K8s 集群中微服务的负载均衡是由 Kube-proxy 实现的。Kube-proxy 是 K8s 集群内部的负载均衡器。它是一个分布式代理服务器，在 K8s 的每个节点上都有一个；这一设计体现了它的伸缩性优势，需要访问服务的节点越多，提供负载均衡能力的 Kube-proxy 就越多，高可用节点也随之增多。与之相比，我们平时在服务器端做个反向代理做负载均衡，还要进一步解决反向代理的负载均衡和高可用问题。

### ***\*任务：Job\****

Job 是 Kubernetes 用来控制批处理型任务的 API 对象。批处理业务与长期伺服业务的主要区别是批处理业务的运行有头有尾，而长期伺服业务在用户不停止的情况下永远运行。Job 管理的 Pod 根据用户的设置把任务成功完成就自动退出了。成功完成的标志根据不同的 spec.completions 策略而不同：单 Pod 型任务有一个 Pod 成功就标志完成；定数成功型任务保证有 N 个任务全部成功；工作队列型任务根据应用确认的全局成功而标志成功。

### ***\*后台支撑服务集：DaemonSet\****

长期伺服型和批处理型服务的核心在业务应用，可能有些节点运行多个同类业务的 Pod，有些节点上又没有这类 Pod 运行；而后台支撑型服务的核心关注点在 Kubernetes 集群中的节点（物理机或虚拟机），要保证每个节点上都有一个此类 Pod 运行。节点可能是所有集群节点也可能是通过 nodeSelector 选定的一些特定节点。典型的后台支撑型服务包括，存储，日志和监控等在每个节点上支持 Kubernetes 集群运行的服务。

### ***\*有状态服务集：StatefulSet\****

是用来控制有状态服务，StatefulSet 中的每个 Pod 的名字都是事先确定的，不能更改。 StatefulSet 中的 Pod，每个 Pod 挂载自己独立的存储，如果一个 Pod 出现故障，从其他节点启动一个同样名字的 Pod，要挂载上原来 Pod 的存储继续以它的状态提供服务。

 

适合于 StatefulSet 的业务包括数据库服务 MySQL 和 PostgreSQL，集群化管理服务 ZooKeeper、etcd 等有状态服务。StatefulSet 的另一种典型应用场景是作为一种比普通容器更稳定可靠的模拟虚拟机的机制；

 

使用 StatefulSet，Pod 仍然可以通过漂移到不同节点提供高可用，而存储也可以通过外挂的存储来提供高可靠性，StatefulSet 做的只是将确定的 Pod 与确定的存储关联起来保证状态的连续性。

 

### ***\*Volume：存储卷\****

Kubernetes 的存储卷的生命周期和作用范围是一个 Pod。每个 Pod 中声明的存储卷由 Pod 中的所有容器共享；

### ***\*Node：节点\****

Kubernetes 集群中的计算能力由 Node 提供，最初 Node 称为服务节点 Minion，后来改名为 Node。Kubernetes 集群中的 Node 也就等同于 Mesos 集群中的 Slave 节点，是所有 Pod 运行所在的工作主机，可以是物理机也可以是虚拟机。不论是物理机还是虚拟机，工作主机的统一特征是上面要运行 kubelet 管理节点上运行的容器。

### ***\*Secret：密钥对象\****

Secret 是用来保存和传递密码、密钥、认证凭证这些敏感信息的对象。使用 Secret 的好处是可以避免把敏感信息明文写在配置文件里。在 Kubernetes 集群中配置和使用服务不可避免的要用到各种敏感信息实现登录、认证等功能，例如访问 AWS 存储的用户名密码。为了避免将类似的敏感信息明文写在所有需要使用的配置文件中，可以将这些信息存入一个 Secret 对象，而在配置文件中通过 Secret 对象引用这些敏感信息。这种方式的好处包括：意图明确，避免重复，减少暴漏机会。

### ***\*namespace：命名空间\****

命名空间为 Kubernetes 集群提供虚拟的隔离作用，Kubernetes 集群初始有两个命名空间，分别是默认命名空间 default 和系统命名空间 kube-system，除此以外，管理员可以可以创建新的命名空间满足需要。

### ***\*开放接口\****

#### ***\*CRI\****

容器运行时接口(Container Runtime Interface)； CRI中定义了容器和镜像的服务的接口，因为容器运行时与镜像的生命周期是彼此隔离的，因此需要定义两个服务。该接口使用Protocol Buffer，基于gRPC ；

 

Container Runtime实现了CRI gRPC Server，包括RuntimeService和ImageService。该gRPC Server需要监听本地的Unix socket，而kubelet则作为gRPC Client运行。

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps6.jpg) 

l RuntimeService：容器和Sandbox运行时管理。

l ImageService：提供了从镜像仓库拉取、查看、和移除镜像的RPC。

#### ***\*CNI\****

Container Network Interface的是一个标准的通用的接口 ； 由一组用于配置 Linux 容器的网络接口的规范和库组成，同时还包含了一些插件。CNI 仅关心容器创建时的网络分配，和当容器被删除时释放网络资源。

#### ***\*CSI\****

Container Storage Interface代表容器存储接口； 借助 CSI 容器编排系统（CO）可以将任意存储系统暴露给自己的容器工作负载 ； 部署 CSI 兼容卷驱动后，用户可以使用 csi 作为卷类型来挂载驱动提供的存储

### ***\*资源对象\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps7.jpg) 

## ***\*集群资源管理\****

### ***\*Node\****

描述：

是K8S的集群工作节点，可以是物理机也可以是虚拟机；

 

Node状态：

 

l Address

Ø HostName：可以被 kubelet 中的 --hostname-override 参数替代。

Ø ExternalIP：可以被集群外部路由到的 IP 地址。

Ø InternalIP：集群内部使用的 IP，集群外部无法访问。

l Condition

Ø OutOfDisk：磁盘空间不足时为 True

Ø Ready：Node controller 40 秒内没有收到 node 的状态报告为 Unknown，健康为 True，否则为 False。

Ø MemoryPressure：当 node 有内存压力时为 True，否则为 False。

Ø DiskPressure：当 node 有磁盘压力时为 True，否则为 False。

l Capacity

Ø CPU

Ø 内存

Ø 可运行的最大 Pod 个数

l SystemInfo：节点的一些版本信息，如 OS、kubernetes、docker 等

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps8.jpg) 

 

**# 查看节点信息** kubectl describe node NODENAME 

**# 禁止Pod调度到该节点上** kubectl corndon NODENAME 

**# 驱逐该节点上的所有 Pod** kubectl drain NODENAME

### ***\*Namespace\****

描述：

简写ns。在集群中，我们可以使用namespace划分出多个“虚拟集群”，这些ns之间可以完全隔离，也可以跨ns，让一个ns中的service访问到其他ns的服务； 比如 Traefik ingress 和 kube-systemnamespace 下的 service 就可以为整个集群提供服务，这些都需要通过 RBAC 定义集群级别的角色来实现；

 

注意：

不是所有的资源对象都对应ns，其中node和persistentVolume就不属于任何ns；

### ***\*Label\****

描述：

label是资源上的标识，用来对它们进行区分和选择；

 

特点：

\1. 一个label会以kv形式附加到各种对象上；Node，Pod，Service

\2. 一个资源对象可以定义人一多数量的Label，同一个Label也可以被添加到任意数量的资源对象上去；

\3. label通常再资源对象确定时定义，也可以在资源创建后动态添加或删除；

 

可以通过label实现资源的多维度分组，以便灵活，方便的进行资源分配，调度，配置，部署等管理工作；

 

常用的标签如下：

 

版本标签：“version”:“release”,

环境标签："environment": "dev", "environment": "qa", "environment": "production"

架构标签："tier": "frontend" ,"tier": "backend","tier": "cache"

 

标签选择器：用于查询和筛选拥有某些标签的资源对象

l 等式选择器 equality-based

可以使用=、==、!=操作符，可以使用逗号分隔多个表达式

name=slave:选择所有包含k=name，v=slave的对象

env!=prod:选择所有包括k=env且v!=prod的对象

l 集合选择器 set-based

可以使用in、notin、!操作符，另外还可以没有操作符，直接写出某个label的key，表示过滤有某个key的object而不管该key的value是何值，!表示没有该label的object

name in (master, slave):选择所有包含k=name且v=master或者v=slave对象

name not in (frontend):选择所有包含k=name且v不等于frontend的对象

 

标签的选择条件可以使用多个，此时将多个标签选择器进行组合，使用，进行分隔即可；

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps9.jpg) 

#### ***\*Label key的组成\****

l 不得超过63个字符

l 可以使用前缀，使用/分隔，前缀必须是DNS子域，不得超过253个字符，系统中的自动化组件创建的label必须指定前缀，kubernetes.io/由kubernetes保留

l 起始必须是字母（大小写都可以）或数字，中间可以有连字符、下划线和点

#### ***\*Label value的组成：\****

l 不得超过63个字符

l 起始必须是字母（大小写都可以）或数字，中间可以有连字符、下划线和点

### ***\*污点和容忍\****

#### ***\*描述：\****

 

Taint（污点）和 Toleration（容忍）可以作用于 node 和 pod 上，其目的是优化 pod 在集群间的调度，这跟节点亲和性类似，只不过它们作用的方式相反，具有 taint 的 node 和 pod 是互斥关系，而具有节点亲和性关系的 node 和 pod 是相吸的。另外还有可以给 node 节点设置 label，通过给 pod 设置 nodeSelector 将 pod 调度到具有匹配标签的节点上。

 

Taint 和 toleration 相互配合，可以用来避免 pod 被分配到不合适的节点上。每个节点上都可以应用一个或多个 taint ，这表示对于那些不能容忍这些 taint 的 pod，是不会被该节点接受的。如果将 toleration 应用于 pod 上，则表示这些 pod 可以（但不要求）被调度到具有相应 taint 的节点上。

#### ***\*污点\****

通过node上添加污点属性，来决定是否允许Pod调度过来；

 

Node被设置上污点之后，就和Pod之间存在了一种相斥的关系，进而拒绝Pod调度过来，甚至可以将已存在的Pod驱逐出去；

 

污点格式：key=value:effect，key和value是污点的标签，effect描述污点的作用，支持如下三个选项：

l PreferNoSchedule: k8s尽量不把pod调度到该污点Node上，除非没有其他节点可调度；

l NoSchedule: k8s将不会把Pod调度到该污点Node上，已存在的Pod将继续运行；

l NoExecute：k8s将不会把Pod调度到具有该污点的Node上，同时也会将Node上已存在的Pod驱逐；

 

命令:

 

\# 设置污点

kubectl taint nodes node1 key=value:effect

\# 去除污点

kubectl taint nodes node1 key:effect-

\# 去除所有污点

kubectl taint nodes node1 key-

 

\# 例如：

\# 为node1设置污点

kubectl taint nodes node1 tag=test:PreferNoSchedule

\# 为node1取消污点

kubectl taint nodes node1 tag:PreferNoSchedule-

\# 为node1删除去除污点

kubectl taint nodes node1 tag-

\# 查看node1 污点

kubectl describe node node1

#### ***\*容忍\****

若将pod调度到一个有污点的node上去，需要用到容忍；

 

污点就是拒绝，容忍就是忽略，Node通过污点拒绝pod调度上去，pod通过容忍忽略拒绝

 

pod的配置

 

spec:

​	containers:

​	- name: nginx

 

​	tolerations: # 添加容忍

​	- key: "tag" # 对应要容忍的污点的键，空着意味匹配所有的键

​	 operator: "Equal" # 操作符，支持Equal和Exists(默认)

​	 value: "tests" # 容忍的污点的值

​	 effect: "NoExecute" # 添加容忍规则，要个污点规则一致

​	 tolerationSeconds: # 容忍时间, 当effect为NoExecute时生效，表示pod在Node上的停留时间

### ***\*垃圾回收\****

描述：

对于 ReplicaSet、StatefulSet、DaemonSet、Deployment、Job、 CronJob 等创建或管理的对象，会自动为其设置 ownerReferences字段的值；K8s的垃圾回收机制需要依赖于ownerReferences；他标记了从属对象的所有者

 

目前有两大类GC：

 

l 级联删除：所有者删除，从属对象也被删除；

分为前台级联删除Foreground和后台级联删除Background模式；

\1. Foreground前台级联删除：所有从属对象删除完毕后删除所有者对象；当所有者删除时会进入“正在删除”状态，仍可以通过RestApi查询到当前对象；

\2. Background后台级联删除： 立即删除所有者的对象，并由垃圾回收器在后台删除其从属对象 ； 用等待删除从属对象的时间；

l 孤儿删除orphan：所有者删除，从属对象变成孤儿；

直接删除所有者对象，并将从属对象中的 ownerReference 元数据设置为默认值。之后垃圾回收器会确定孤儿对象并将其删除；

 

垃圾回收器的工作流程

由Scanner、Garbage Processor 和 Propagator 组成；

l Scanner：它会检测 K8s 集群中支持的所有资源，并通过控制循环周期性地检测。它会扫描系统中的所有资源，并将每个对象添加到"脏队列"（dirty queue）中。

l Garbage Processor：它由在"脏队列"上工作的 worker 组成。每个 worker 都会从"脏队列"中取出对象，并检查该对象里的 OwnerReference 字段是否为空。如果为空，那就从“脏队列”中取出下一个对象进行处理；如果不为空，它会检测 OwnerReference 字段内的 owner resoure object 是否存在，如果不存在，会请求 API 服务器删除该对象。

l Propagator ：用于优化垃圾回收器，它包含以下三个组件：

 

\1. EventQueue：负责存储 k8s 中资源对象的事件；

\2. DAG（有向无环图）：负责存储 k8s 中所有资源对象的 owner-dependent 关系；

\3. Worker：从 EventQueue 中取出资源对象的事件，并根据事件的类型会采取操作;

在有了 Propagator 的加入之后，我们完全可以仅在 GC 开始运行的时候，让 Scanner 扫描系统中所有的对象，然后将这些信息传递给 Propagator 和“脏队列”。只要 DAG 一建立起来之后，那么 Scanner 其实就没有再工作的必要了

## ***\*总结\****

从 Kubernetes 的系统架构、技术概念和设计理念，我们可以看到 Kubernetes 系统最核心的两个设计理念：一个是 容错性，一个是 易扩展性。容错性实际是保证 Kubernetes 系统稳定性和安全性的基础，易扩展性是保证 Kubernetes 对变更友好，可以快速迭代增加新功能的基础

### ***\*功能\****

l 具备完善的集群管理能力；

l 透明的服务注册和服务发现机制；

l 内建负载均衡，故障发现和自我修复；

l 服务滚动升级和在线扩容；

l 可扩展的自动调度机制；

l 资源配额管理

## ***\*k8s安装步骤\****

### ***\*安装准备\****

准备两台服务器 (ip仅为示例)：192.168.26.227，192.168.26.228

一主一从：

master机：192.168.26.227

node机：192.168.26.228

**master主机上必须要有的组件：**

etcd：提供分布式数据存储的数据库吧，用于持久化存储k8s集群的配置和状态

kube-apiserver：api service提供了http rest接口，是整个集群的入口，K8s其它组件之间不直接通信，而是通过API server通信的。（只有API server连接了etcd，即其它组件更新K8s集群的状态时，只能通过API server读写etcd中的数据）

kube-scheduler：scheduler负责资源的调度

kube-controller-manager：整个集群的管理控制中心，此组件里面是由多个控制器组成的，如：Replication Manager（管理ReplicationController 资源），ReplicaSet Controller，PersistentVolume controller。主要作用用来复制组件、追踪工作结点状态、处理失败结点

**node节点机上必须要有的组件：**

flannel：好像是用来支持网络通信的吧

kube-proxy：用来负载均衡网络流量

kubelet：用来管理node节点机上的容器

docker：运行项目镜像容器的组件

 

\1. 所有机器上执行以下命令，准备安装环境：(注意是所有机器，主机master，从机node都要安装)

(1) 安装epel-release源

​	yum -y install epel-release

(2) 所有机器关闭防火墙(生产环境跳过这一步)

sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

setenforce 0

systemctl stop firewalld

systemctl disable firewalld

\#查看防火墙状态

firewall-cmd --state

(3) 禁用SELINUX

***\*最大限度地减小系统中服务进程可访问的资源(最小权限原则)\****

vi /etc/selinux/config 

SELINUX=disabled

(4) 设置时区

timedatectl set-timezone Asia/Shanghai 

systemctl restart rsyslog

(5) 设置主机名称

​	hostnamectl set-hostname master  #mater主机执行

​	hostnamectl set-hostname node1   #node1主机执行

​	hostnamectl set-hostname node2   #node2主机执行

(6) 添加hosts网络主机配置

三台虚拟机都要设置

vim /etc/hosts

192.168.124.131 master

192.168.124.129 node1

192.168.124.130 node2

保持ip跟主机名能够对应,这样做的目的是为了方便使用主机名进行交互

 

(7) 设置 iptables 可以看到 bridged traffic

modprobe br_netfilter

#### ***\*yum库版本太旧了,安装的是1.5版本的,现在最新的1.25,差了20个版本,更推荐离线安装1.20左右版本的比较稳定,1.25太新了避免出现未知问题\****

#### ***\*安装kubernetes\**** ***\*admin\****

K8s admin不是k8s的核心而是简化安装k8s集群

以下步骤所有节点包括master都要执行一遍

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps10.jpg) 

kube114-rpm.tar 		集群管理工具的安装压缩包

docker-ce-18.09.tar 	docker的安装压缩包

k8s-114-images.tar K8S		镜像本身的压缩包

flannel-dashboard.tar		仪表盘,用于监控集群状态

 

\1. 将镜像包上传至服务器每个节点

mkdir /usr/local/k8s-install

cd /usr/local/k8s-install

XFTP上传安装文件

 

\2. 按每个Centos上安装Docker 

tar -zxvf docker-ce-18.09.tar.gz 

cd docker 

yum localinstall -y *.rpm

或者从阿里库进行安装也可以

启动docker

systemctl start docker

设置docker为自动启动

systemctl enable docker

 

\3. 确保从cgroups均在同一个从groupfs

\#cgroups是control groups的简称，它为Linux内核提供了一种任务聚集和划分的机制，通过一组参数集合将一些任务组织成一个或多个子系统。  

\#cgroups是实现IaaS虚拟化(kvm、lxc等)，PaaS容器沙箱(Docker等)的资源管理控制部分的底层基础。

\#子系统是根据cgroup对任务的划分功能将任务按照一种指定的属性划分成的一个组，主要用来实现资源的控制。

\#在cgroup中，划分成的任务组以层次结构的形式组织，多个子系统形成一个数据结构中类似多根树的结构。cgroup包含了多个孤立的子系统，每一个子系统代表单一的资源

 

docker info | grep cgroup 

 

如果不是groupfs,执行下列语句

 

cat << EOF > /etc/docker/daemon.json

{

 "exec-opts": ["native.cgroupdriver=cgroupfs"]

}

EOF

systemctl daemon-reload && systemctl restart docker

 

\4. 安装kubeadm

\# kubeadm是集群部署工具(不是K8S本身)

 

cd /usr/local/k8s-install/kubernetes-1.14

tar -zxvf kube114-rpm.tar.gz

cd kube114-rpm

yum localinstall -y *.rpm

 

\5. 关闭交换区

swapoff -a

vi /etc/fstab 

\#swap一行注释

 

\6. 配置网桥

 

cat <<EOF >  /etc/sysctl.d/k8s.conf

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

EOF

sysctl --system

含义:

在容器间进行网络通信也要遵循ip table的规则进行的处理,这样做可以提供我们系统在网络传输间的安全性

 

\7. 通过镜像安装k8s

 

cd /usr/local/k8s-install/kubernetes-1.14

docker load -i k8s-114-images.tar.gz

docker load -i flannel-dashboard.tar.gz

加载本地镜像文件来在本地docker中创建一系列镜像

#### ***\*使用阿里云镜像安装\****

一、配置软件源

1、配置yum源base repo为阿里云的yum源

cd /etc/yum.repos.d

mv CentOS-Base.repo CentOS-Base.repo.bak

mv epel.repo epel.repo.bak

curl https://mirrors.aliyun.com/repo/Centos-7.repo -o CentOS-Base.repo

sed -i 's/gpgcheck=1/gpgcheck=0/g' /etc/yum.repos.d/CentOS-Base.repo

curl https://mirrors.aliyun.com/repo/epel-7.repo -o epel.repo

gpkcheck=0 表示对从这个源下载的rpm包不进行校验

2、配置docker repo

wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O/etc/yum.repos.d/docker-ce.repo

3、配置kubernetes源为阿里的yum源

cat > /etc/yum.repos.d/kubernetes.repo << EOF

[kubernetes]

name=Kubernetes

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64

enabled=1

gpgcheck=0

repo_gpgcheck=0

gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg

EOF

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64  Kubernetes源设为阿里

gpgcheck=0：表示对从这个源下载的rpm包不进行校验

repo_gpgcheck=0：某些安全性配置文件会在 /etc/yum.conf 内全面启用 repo_gpgcheck，以便能检验软件库的中继数据的加密签署

如果gpgcheck设为1，会进行校验，就会报错如下，所以这里设为0

repomd.xml signature could not be verified for kubernetes

4、update cache 更新缓存

yum clean all

yum makecache

yum repolist

二、安装docker

查看可安装的版本

yum list docker-ce --showduplicates | sort -r

安装

yum install -y docker-ce #(这里最好自定义版本)

yum install docker-ce-18.09.9 docker-ce-cli-18.09.9 containerd.io

启动docker

systemctl enable docker && systemctl start docker

查看docker版本

docker version

设置docker的镜像源，这里我设置为国内的docker源https://www.docker-cn.com

vim /etc/docker/daemon.json

{

"exec-opts": ["native.cgroupdriver=systemd"],

"log-driver": "json-file",

"log-opts": {

"max-file": "3",

"max-size": "100m"

},

"storage-driver": "overlay2",

"storage-opts": [

"overlay2.override_kernel_check=true"

],

"registry-mirrors": ["https://www.docker-cn.com"]

}

注意：native.cgroupdriver=systemd 官方推荐此配置，地址 https://kubernetes.io/docs/setup/cri/

重新加载daemon，重启docker

systemctl daemon-reload

systemctl restart docker

如果docker重启失败提示start request repeated too quickly for docker.service

 

应该是centos的内核太低内核是3.x，不支持overlay2的文件系统，移除下面的配置

"storage-driver": "overlay2",

"storage-opts": [

"overlay2.override_kernel_check=true"

]

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps11.jpg) 

\#下载kubeadm相关组件

yum install --downloadonly --downloaddir=/root/k8sOfflineSetup/packages kubelet-1.20.4 kubeadm-1.20.4 kubectl-1.20.4#查看最新的组件镜像版本，可参考

kubeadm config images list#在有docker环境的服务器上下载K8S镜像，然后传输到模板机

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.13-0

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.7.0# 重新tag镜像，该步骤是因为K8S配置清单中yaml用的是默认镜像地址

docker images \

  | grep registry.cn-hangzhou.aliyuncs.com/google_containers \

  | sed 's/registry.cn-hangzhou.aliyuncs.com\/google_containers/k8s.gcr.io/' \

  | awk '{print "docker tag " $3 " " $1 ":" $2}' \

  | sh#导出镜像

docker save -o kube-apiserver-v1.20.4.tar k8s.gcr.io/kube-apiserver:v1.20.4

docker save -o kube-controller-manager-v1.20.4.tar k8s.gcr.io/kube-controller-manager:v1.20.4

docker save -o kube-scheduler-v1.20.4.tar k8s.gcr.io/kube-scheduler:v1.20.4

docker save -o kube-proxy-v1.20.4.tar k8s.gcr.io/kube-proxy:v1.20.4

docker save -o pause-3.2.tar k8s.gcr.io/pause:3.2

docker save -o etcd-3.4.13-0.tar k8s.gcr.io/etcd:3.4.13-0

docker save -o coredns-1.7.0.tar k8s.gcr.io/coredns:1.7.0

docker save -o ingress.tar quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.23.0#calico相关镜像下载可通过官方yaml内提取：https://docs.projectcalico.org/manifests/calico.yaml#默认下载calico镜像是最新的，我这里使用3.16.3，需要将yaml版本替换

docker save -o calico-node-v3.16.3.tar calico/node:v3.16.3

docker save -o calico-pod2daemon-flexvol-v3.16.3.tar calico/pod2daemon-flexvol:v3.16.3

docker save -o calico-cni-v3.16.3.tar calico/cni:v3.16.3

docker save -o calico-kube-controllers-v3.16.3.tar calico/kube-controllers:v3.16.3

 

## ***\*利用k8s构建集群\****

### ***\*1. master主服务器配置\****

kubeadm init --kubernetes-version=v1.14.1 --pod-network-cidr=10.244.0.0/16

运行此命令至少要有两个处理器核心

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps12.jpg) 

以下必须管理员账户手动执行

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps13.jpg) 

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

 

admin.conf:有关于当前集群的核心配置文件

有关于安全的授权数据以及集群本身的信息

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps14.jpg) 

这里的join不是由master来执行而是由子节点来运行,只要在其他节点运行则与当前master形成集群关系,将其复制保存,以便在节点运行

 

kubectl  k8s管理命令行工具

 

kubectl get nodes

\#查看存在问题的pod

kubectl get pod --all-namespaces

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps15.jpg) 

如果status是CrashLoopBackOff 表示重试启动,一般不用管,自己会启动

Pending则是等待状态,这里的基础网络应用缺少组件需要安装组件以便继续

 

\#设置全局变量

\#安装flannel网络组件 K8S最底层的网络协议,用于在多个pod直接彼此进行通信

kubectl create -f kube-flannel.yml

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps16.jpg) 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps17.png)

再次运行kubectl get pod --all-namespaces可能发现没有running (可能处于ContainerCreating状态)稍微等会,可能处理的时间有点长

### ***\*2. 加入\*******\*node\*******\*节点\****

kubeadm join 192.168.4.130:6443 --token 911xit.xkp2gfxbvf5wuqz7 \

  --discovery-token-ca-cert-hash sha256:23db3094dc9ae1335b25692717c40e24b1041975f6a43da9f43568f8d0dbac72

​	

如果忘记

在master 上执行kubeadm token list 查看 ，在node上运行

kubeadm join 192.168.124.131:6443 --token l5u5n5.g3p0m8oet0mv10jy --discovery-token-unsafe-skip-ca-verification

 

执行后则当前node节点则加入到master

 

如保存...已存在先去对应目录清除对应文件

如报ipv4转发应该设置为1去对应目录对应文件设置值为1

 

kubectl get nodes

 

### ***\*一些错误解决办法\****

#### ***\*k8s集群在节点运行kubectl出现的错误：The connection to the server localhost:8080 was refused - did you specify the right host or port?\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps18.jpg) 

出现这个问题的原因是kubectl命令需要使用kubernetes-admin来运行

 

将主节点（master节点）中的【/etc/kubernetes/admin.conf】文件拷贝到从节点相同目录下:

scp -r /etc/kubernetes/admin.conf ${node1}:/etc/kubernetes/admin.conf

 

配置环境变量：

echo “export KUBECONFIG=/etc/kubernetes/admin.conf” >> ~/.bash_profile

 

立即生效：

source ~/.bash_profile

 

#### ***\*k8s中node节点出现NotReady状态的解决方法\****

kubectl describe nodes node1 中发现

 

cni config uninitialized KubeletNotReady runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized

这个时候也许是因为你的node节点中没有安装相应的cni模块。这个时候需要做如下操作:

 

在新增节点上有flannel容器运行，但是没有cni网络相关配置，导致新增节点的网络没法初始化，接入到集群，所以通过网络搜索出相关解决方案，增加cni配置：

\1.  查取正常集群节点所使用网段(在master执行)

cat /run/flannel/subnet.env

FLANNEL_NETWORK=10.244.0.0/16 **#记录下该网段**

FLANNEL_SUBNET=10.244.0.1/24 **#记录下该网段**

FLANNEL_MTU=1450

FLANNEL_IPMASQ=true

\2.   创建cni网络相关配置文件：(node执行)

 

mkdir -p /etc/cni/net.d/

 

cat <<EOF> /etc/cni/net.d/10-flannel.conf

{"name":"cbr0","type":"flannel","delegate": {"isDefaultGateway": true}}

EOF

mkdir /usr/share/oci-umount/oci-umount.d -p

mkdir /run/flannel/

cat <<EOF> /run/flannel/subnet.env

FLANNEL_NETWORK=10.244.0.0/16

FLANNEL_SUBNET=10.244.0.1/24

FLANNEL_MTU=1450

FLANNEL_IPMASQ=true

EOF

 

#### ***\*kubeadm join\**** ***\*无法加入超时问题\****

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps19.jpg) 

可能中途改过ip啥的,所有节点都要kubeadm reset才可以,

 

#### ***\*Unable to connect to the server: x509: certificate signed by unknown authority\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps20.jpg) 

报错原因，之前在这台机器使用kubeadm搭建过k8s集群，之前为了可以使当前用户可以使用kubectl命令，做了如下操作。

 mkdir -p $HOME/.kube

 sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

 sudo chown $(id -u):$(id -g) $HOME/.kube/config

解决

删除$HOME/.kube

rm -rf $HOME/.kube

重新执行mk

重新安装flannel网络组件

#### ***\*The connection to the server localhost:8080 was refused - did you specify the right host or port?\****

环境变量 原因：kubernetes master没有与本机绑定，集群初始化的时候没有绑定，此时设置在本机的环境变量即可解决问题。

解决方式

步骤一：设置环境变量

具体根据情况，此处记录linux设置该环境变量

方式一：编辑文件设置

vim /etc/profile

在底部增加新的环境变量 

export KUBECONFIG=/etc/kubernetes/admin.conf

方式二:直接追加文件内容

echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /etc/profile

步骤二：使生效

source /etc/profile

## ***\*重新启动服务\****

假设我们的服务器因为各种原因重启了,那如何恢复我们的k8s服务

 

### ***\*kubeadm/kubelet/kubectl区别\****

 

kubeadm是kubernetes集群快速构建工具

kubelet运行在所有节点上,负责启动POD和容器,以系统服务形式出现

kubectl:kubectl是kubenetes命令行工具,提供指令

### ***\*启动节点命令\****

启动节点的K8S服务

systemctl start kubelet

建议以下方式

设置开机启动

systemctl enable kubelet

即使服务器重启也会跟着一起启动,在加入节点完成后就设置上开机启动

 

## ***\*启用Web Ui Dashboard\****

### ***\*Master开启仪表盘\****

flannel-dashboard.tar.gz 这个压缩包是Web Ui Dashboard的镜像,在前面安装过程已经使用docker导入了

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps21.jpg) 

kubernetes-dashboard.yaml 仪表盘的核心配置,web ui最基础的配置,提供了一些默认的设置选项

#### ***\*简化安装\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps22.png)

admin-role.yaml 说明了管理员的角色以及职能

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps23.png)

kubernetes-dashboard-admin.rbac.yaml 基于角色进行访问控制,定义了系统应用的权限所在

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps24.png)

以上配置用户名和密码进行了简化,无需用户名密码登录

kubectl apply -f kubernetes-dashboard.yaml

kubectl apply -f admin-role.yaml

kubectl apply -f kubernetes-dashboard-admin.rbac.yaml

kubectl -n kube-system get svc #获取系统命名空间

http://[192.168.124.131](http://192.168.124.131:32000/):32000 访问 比较慢可能...

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps25.jpg) 

#### ***\*非简化安装\****

默认的token的方式非简化

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps26.png)

#### ***\*非简化账户密码安装\****

在默认的token的方式的基础上配置

设置用户名密码登录

l 备份kube-apiserver.yaml（重要）

cp /etc/kubernetes/manifests/kube-apiserver.yaml  /etc/kubernetes/manifests/kube-apiserver.yaml-bake-20211129

 

l 新增密码

账户admin密码admin，唯一id是1

echo "admin,admin,1" > /etc/kubernetes/pki/basic_auth_file

echo "yanpj,ypj@123,2" >> /etc/kubernetes/pki/basic_auth_file

每行写一个账号，id不能重复 

 

l 修改apiserver.yaml

vim /etc/kubernetes/manifests/kube-apiserver.yaml

\#加入这一行

\- --token-auth-file=/etc/kubernetes/pki/basic_auth_file

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps27.jpg) 

\#保存退出

 

l 查看状态

apiserver.yaml被修改后会自动重启（十秒左右），查看状态有报错

 

l 为admin/yanpj用户绑定权限

\# admin绑定权限

kubectl create clusterrolebinding login-on-dashboard-with-cluster-admin --clusterrole=cluster-admin --user=admin

\# 查看绑定结果

kubectl get clusterrolebinding login-on-dashboard-with-cluster-admin

 

l 修改dashboard.yaml

dashboard.yaml 是dashboard相关部署文件。

\- --token-ttl=21600

\- --authentication-mode=basic

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps28.jpg) 

 

l 浏览器查看结果

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps29.jpg) 

 

##### ***\*报错：\****

configmaps is forbidden: User  system:anonymous cannot list resource configmaps in API g_wangmiaoyan

 

解决：

kubectl create clusterrolebinding test:anonymous --clusterrole=cluster-admin --user=system:anonymous

 

解决：

kubectl create clusterrolebinding gitlab-cluster-admin --clusterrole=cluster-admin --group=system:serviceaccounts --namespace=dev

 

## ***\*D\*******\*ashboard\*******\*部署Tomcat集群\****

***\*此方式一般在生产情况下不会使用,一般是采用命令方式进行部署\****

 

在能够访问外网的情况下,其他内网方式看具体情况是进行开放还是离线

因为要下载的一些镜像默认是访问国外的,直连会下载不了

所以需要使用阿里云镜像加速器功能

登录阿里云官网（https://www.aliyun.com/）——点击控制台——产品与服务——容器镜像服务——镜像中心——镜像加速器

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps30.jpg) 

 

 

sudo mkdir -p /etc/docker sudo tee /etc/docker/daemon.json <<-'EOF' { "registry-mirrors": ["https://rap3r6zx.mirror.aliyuncs.com"] } EOF sudo systemctl daemon-reload sudo systemctl restart docker

 

访问dashbord  - 工作负载

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps31.jpg) 

工作负载-在集群中创建容器,选择右上角创建-创建应用

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps32.jpg) 

比如我们创建一个Tomcat镜像

填写应用名称,容器镜像(tomcat:latest 表示最新的版本Tomcat)

容器镜像可以选公有镜像源或者docker hub

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps33.jpg) 

容器个数:现在我们准备在两个节点都进行部署,所以创建2个

服务:是否对外暴露我们的端口,一般我们都选对外,不然只能在本服务器内了,只有对外开放其他服务器才能够正常访问

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps34.jpg) 

点击部署后一段时间进行部署,可以看到两个容器分别部署在node1和node2上(这一步跟网络有很大关系,网不好下载时间太长会报错,需要重置)

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps35.jpg) 

右侧![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps36.jpg)可以查看当前节点Tomcat日志

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps37.jpg) 

 

部署原理:

Dashbord运行在master主服务器上,在运行集群创建的时候由master向两个节点发送创建容器的请求,两个容器在接收到请求后开始下载镜像,docker images可以看到下载了Tomcat镜像

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps38.jpg) 

对node节点进行镜像删除后,K8S对整体状态进行监控,并重新自动创建容器

 

## ***\*deployment脚本部署Tomcat\****

### ***\*deployment脚本格式说明\****

部署是指Kubernetes向Node节点发送指令，创建容器的过程

Kubernetes支持yml格式的部署脚本

kubectl create -f 部署yml文件 #创建部署

 

apiVersion: extensions/v1beta1

kind: Deployment

metadata:

 name: tomcat-deploy

spec:

 replicas: 2

 template:

  metadata:

   labels:

​    app: tomcat-cluster #pod名称

  spec:

   containers:

   \- name: tomcat-cluster #容器名称

​    image: tomcat:latest

​    ports:

​    \- containerPort: 8080

 

第一部分:部署的元信息

apiVersion api版本号

Kind 当前yaml类型

matedata : name 部署文件名称

第二部分:

spec 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps39.jpg)部署模板基本信息

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps40.jpg)创建容器命名规则以及镜像来源,以及对外暴露的端口

containers 是容器的设置

### ***\*与部署相关的常用命令\****

kubectl create -f 部署yml文件 			#创建部署

kubectl apply -f 部署yml文件 			#更新部署配置

kubectl get pod [-o wide] 				#查看已部署pod

kubectl describe pod pod名称 			#查看Pod详细信息

kubectl logs [-f] pod名称 				#查看pod输出日志 -f是否实时更新

 

## ***\*外部访问Tomcat集群\****

通过deployment部署初步只能使服务内部进行访问,需要通过service来达到外部访问

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps41.jpg) 

类似nginx , service就可以说是k8s中的内置的负载均衡,是访问内置容器的统一入口

所有的数据都要交给service,由又它来进行转发

设置服务的时候有一个便利,就是可以设置外侧宿主机的对外暴露端口,如果这么设置并进行宿主机+制定ip进行访问则使得service失去了负载均衡的功能

### ***\*创建服务\****

同样需要设置yml脚本

apiVersion: v1

kind: Service

metadata:

 name: tomcat-service

 namespace: default

 labels:

  app: tomcat-service

spec:

 type: NodePort #节点端口

 selector:

  app: tomcat-cluster #之前部署的容器那个labels,对其进行绑定

 ports:

  \- port: 8000 #pod暴露的端口

   protocol: TCP #

   targetPort: 8080 #宿主机暴露的端口

   nodePort: 32500 #service 对外暴露的端口

 

kubectl get service #查看service信息

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps42.jpg) 

## ***\*持久化存储-\*******\*基于NFS文件集群共享\****

Network File System - NFS

NFS,是由SUN公司研制的文件传输协议

NFS主要是采用远程过程调用RPC机制实现文件传输

yum install -y nfs-utils rpcbind 

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps43.jpg) 

如图 如果两个node的tomcat实例的两边内部应用都进行修改或者发布,这种情况下非常复杂而且容易出错不统一

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps44.jpg) 

那么在NFS中额外增加一个节点,假设叫node3,它的职责就是提供要共享的文件,可以看成是一台文件共享服务器,保存了这个集群中所有要共享的数据

只要共享服务器的文件发生变化则对应的集群服务器都发生变化

比如/www-data/目录对应/mnt目录,那么tomcat的应用目录挂载在/mnt目录即可

 

### ***\*文件提供方\****

这里把master作为文件共享服务器来用

yum install -y nfs-utils rpcbind 

 

cd /usr/local

mkdir data

cd data

mkdir www-data

cd www-data

vim /etc/exports

 

/usr/local/data/www-data 192.168.124.131/24(rw,sync)

\#master的ip地址以及掩码(rw 通过网络共享的这个目录是可读可写的,sync同步写入)

 

\#保存后启动服务

systemctl start nfs.service

systemctl start rpcbind.service

\#开机启动

systemctl enable nfs.service

systemctl enable rpcbind.service

exportfs #查看设置是否完成

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps45.jpg) 

### ***\*文件接收方\****

yum install -y nfs-utils #使用者只需要nfs-utils即可

showmount -e 192.168.124.131 #这里指向的是主服务器的地址

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps46.jpg) 

能够进行下一步挂载

mount 192.168.124.131:/usr/local/data/www-data /mnt

\#挂载主服务器地址以及对应目录 空格后为本地映射目录

使用方仅仅是远程连接到主服务器并不是真正下载,所以是不具备修改删除功能的

systemctl start nfs.service

\#开机启动

systemctl enable nfs.service

 

### ***\*部署配置挂载点\****

将容器应用目前映射到每个节点的/mnt目录

清除之前部署的deployment 和service 重新编辑deployment来说明挂载目录的信息

 

apiVersion: extensions/v1beta1

kind: Deployment

metadata:

 name: tomcat-deploy

spec:

 replicas: 2

 template:

  metadata:

   labels:

​    app: tomcat-cluster #pod名称

  spec:

   volumes: #数据卷

   \- name: web-app  #挂载数据卷的别名,自定义

​    hostPath: #设置宿主机的原始目录是哪个

​     path: /mnt #通知容器有mnt目录挂载

   containers:

   \- name: tomcat-cluster #容器名称

​    image: tomcat:latest

​    ports:

​    \- containerPort: 8080

​    volumeMounts: #挂载点

​    \- name: web-app #这里的挂载点名字要与上方挂载数据卷别名相同

​     mountPath: /usr/local/tomcat/webapps

 

启动后进入docker内部路径

docker exec -it 5415bb335655 /bin/bash

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps47.jpg) 

或者在master执行:

kubectl exec -it pod名称 /bin/bash

进入webapps目录即可验证文件是否同步

## ***\*持久化存储-(PV和PVC)\****

NFS的缺陷是你需要知道文件服务器的地址以及路径

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps48.jpg) 

PV:持久化存储,对存储资源进行抽象,对外提供可以调用的地方(生产者)

PVC:持久化存储调用,用户调用不需要关系内部实现细节(消费者)

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps49.jpg) 

apiVersion: apps/v1

kind: Deployment

metadata:

 name: nginx-dep1

spec:

 replicas: 2

 selector:

  matchLabels:

   app: nginx

 template:

  metadata:

   labels:

​    app: nginx

  spec:

   containers:

   \- name: nginx

​    image: nginx

​    volumeMounts:

​    \- name: wwwroot

​     mountPath: /usr/share/nginx/html

​    ports:

​    \- containerPort: 80

   volumes:

   \- name: wwwroot

​    persistentVolumeClaim:

​     claimName: my-pvc

 

\---

 

apiVersion: v1

kind: PersistentVolumeClaim

metadata:

 name: my-pvc

spec:

 accessModes:

  \- ReadWriteMany

 resources:

  requests:

   storage: 5Gi

\---

 

apiVersion: v1

kind: PersistentVolume

metadata:

 name: my-pv

spec:

 capacity:

  storage: 5Gi

 accessModes:

  \- ReadWriteMany

 nfs:

  path: /mnt

server: 192.168.124.200

 

\# kubectl get pc,pvc

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps50.jpg) 

 

## ***\*利用Rinetd实现Service负载均衡\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps51.jpg) 

外部访问我们的容器的时候每次都要确定具体的ip这样很不方便,不能把ip选择交给客户

所以所有对外暴露的出口都使用service来作为前置利用其端口向node侧进行信息转发

通过K8S的service这个机制来实现容器间的负载均衡

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps52.jpg) 

注释掉type : NodePort 对应的nodePort也注释掉

即所有的节点不再暴露32500端口,一切数据的端口都由service提供的端口8000来接收

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps53.jpg) 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps54.jpg) 

在主服务器共享目录写个jsp文件打印ip

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps55.jpg) 

内部对其进行访问后则可以看到两个node都会被随机访问到

但我们也发现10.109.145.184是一个内部虚拟的ip并不是我们本机的ip外侧并不能直接访问到

所以我们需要对其虚拟ip与本机ip进行映射,端口转发

我们可以用端口转发组件来帮助我们进行端口转发rinetd

 

### ***\*端口转发工具-Rinetd\****

Rinetd是Linux操作系统中为重定向传输控制协议工具

可将源IP端口数据转发至目标IP端口

在Kubernetes中用于将service服务对外暴露

 

rinetd.tar.gz 这个软件可能不太好下载,需要从国内找下载源

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps56.png)

tar -zxvf rinetd.tar.gz

cd rinetd

sed -i 's/65536/65535/g' rinetd.c

\#这里修改的内容是我们要调整允许映射的端口范围

mkdir -p /usr/man/

yum install -y gcc

make && make install

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps57.jpg) 

当看到 install -m 700 644则说明安装成功

这时候还没完成因为还不知道如何进行映射

vim /etc/rinetd.conf

0.0.0.0 8000 10.109.145.184 8000

\#0.0.0.0 8000 表示我们允许哪些ip对当前服务器进行访问发送请求 0.0.0.0表示所有ip

\#也就是每当宿主机8000端口收到请求则把请求转发给后边的service的ip端口

\#后边的10.109.145.184 8000是我们的service的ip端口

 

rinetd -c /etc/rinetd.conf

\#对刚才创建的配置进行应用加载

 netstat -tulpn

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps58.jpg) 

可以发现对应的配置ip端口已被监听

网页直接访问master节点ip:8000

### ***\*其他\****

如果不想使用此方式负载均衡比如想要用nginx方式等则使用之前的NodePort类型来使各个节点对外暴露,在其他服务器配置负载均衡工具来完成

## ***\*集群配置调整与资源限定\****

### ***\*K8S部署调整命令\****

更新集群配置

kubectl apply -f yml文件路径

删除部署(Deployment)|服务(Service)

kubectl delete deployment|service 部署|服务名称

 

apply和create的区别是apply拥有更新部署同时也拥有create的创建性质,create仅仅创建

### ***\*资源限定\****

对部署的容器到底使用多少CPU或者内存资源,主要设置resources

   containers:

   \- name: tomcat-cluster

​    image: tomcat:latest

​    resources:

​     requests:

​      cpu: 1  #至少一个是空闲的

​      memory: 500Mi #至少需要500M空闲内存

​     limits:

​      cpu: 2 #最多使用的CUP数量

​      memory: 1024Mi #最多使用的内存大小

 

\#requests表示当前容器需要的系统要求,即最低需求

\#limits 表示资源上限

kubectl describe deployment tomcat-deploy

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps59.jpg) 

## ***\*kubernetes-\*******\*Ingress\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps60.jpg) 

kubectl create deployment web --image=nginx#创建deploy

kubectl expose deployment web --port=80 --target-port=80 --type=NodePort#创建service

### ***\*部署ingress controller\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps61.png)

如果无法联网需要提前安装镜像 镜像从阿里云镜像下载打包成对应名称

k8s.gcr.io/ingress-nginx/controller:v1.2.1

k8s.gcr.io/ingress-nginx/kube-webhook-certgen:v1.1.1

 ![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps62.jpg)

需要注意的地方hostNetwork: true 表示对外暴露pods网络

查看ingress controller状态

kubectl get pod -n ingress-nginx

### ***\*创建ingress规则\****

每个service的端口以及名字需要进行配置才能让ingress找到访问入口

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps63.jpg) 

apiVersion: networking.k8s.io/v1beta1

kind: Ingress

metadata:

 name: example-ingress #ingress名称

spec:

 rules:

 \- host: example.ingredemo.com #ingress域名

  http:

   paths:

   \- path: /

​    backend:

​     serviceName: web #service的名称

​     servicePort: 80 #service的端口

 

如果之前安装过删除了应用还需要删除：

kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission

kubectl get pod -n ingress-nginx -owide#查看部署在哪个节点

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps64.jpg) 

可以看到部署在node1节点下 在node1查看80端口是否监听

netstat -antp | grep 80#查看80端口是否监听

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps65.jpg) 

修改window系统host模拟域名访问

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps66.jpg) 

 

 

 

## ***\*kubernetes-Helm\****

K8S 上的应用对象，都是由特定的资源描述组成，包括 deployment、service 等。都保存各自文件中或者集中写到一个配置文件。然后 kubectl apply –f 部署。如果应用只由一个或几个这样的服务组成，上面部署方式足够了。而对于一个复杂的应用，会有很多类似上面的资源描述文件，例如微服务架构应用，组成应用的服务可能多达十个，几十个。如果有更新或回滚应用的需求，可能要修改和维护所涉及的大量资源文件，而这种组织和管理应用的方式就显得力不从心了。且由于缺少对发布过的应用版本管理和控制，使Kubernetes 上的应用维护和更新等面临诸多的挑战，主要面临以下问题：

（1）如何将这些服务作为一个整体管理 

（2）这些资源文件如何高效复用 

（3）不支持应用级别的版本管理

### ***\*Helm 介绍\****

Helm 是一个 Kubernetes 的包管理工具，就像 Linux 下的包管理器，如 yum/apt 等，可以很方便的将之前打包好的 yaml 文件部署到 kubernetes 上。

Helm 有 3 个重要概念：

（1） helm：一个命令行客户端工具，主要用于 Kubernetes 应用 chart 的创建、打包、发布和管理。

（2） Chart：应用描述，一系列用于描述 k8s 资源相关文件的集合。

（3） Release：基于 Chart 的部署实体，一个 chart 被 Helm 运行后将会生成对应的一个release；将在 k8s 中创建出真实运行的资源对象。

### ***\*Helm v3 变化\****

2019 年 11 月 13 日， Helm 团队发布 Helm v3 的第一个稳定版本。

该版本主要变化如下：

架构变化：

1、最明显的变化是 Tiller 的删除

2、Release 名称可以在不同命名空间重用

3、支持将 Chart 推送至 Docker 镜像仓库中

4、使用 JSONSchema 验证 chart values

5、其他

### ***\*部署\**** ***\*helm\**** ***\*客户端\****

Helm 客户端下载地址：

[https://github.com/helm/helm/releases ](https://github.com/helm/helm/releases)解移动到/usr/bin/目录即可。

wget https://get.helm.sh/helm-v3.8.2-linux-amd64.tar.gz tar zxvf helm-v3.8.2-linux-amd64.tar.gz

mv linux-amd64/helm /usr/bin/

### ***\*helm 常用命令\****

| **命令**   | **描述**                                                     |
| ---------- | ------------------------------------------------------------ |
| create     | 创建一个 chart 并指定名字                                    |
| dependency | 管理 chart 依赖                                              |
| get        | 下载一个 release。可用子命令：all、hooks、manifest、notes、values |
| history    | 获取 release 历史                                            |
| install    | 安装一个 chart                                               |
| list       | 列出 release                                                 |
| package    | 将 chart 目录打包到 chart 存档文件中                         |
| pull       | 从远程仓库中下载 chart 并解压到本地 # helm pull stable/mysql -- untar |
| repo       | 添加，列出，移除，更新和索引 chart 仓库。可用子命令：add、index、list、remove、update |
| rollback   | 从之前版本回滚                                               |
| search     | 根据关键字搜索 chart。可用子命令：hub、repo                  |
| show       | 查看 chart 详细信息。可用子命令：all、chart、readme、values  |
| status     | 显示已命名版本的状态                                         |
| template   | 本地呈现模板                                                 |
| uninstall  | 卸载一个 release                                             |
| upgrade    | 更新一个 release                                             |
| version    | 查看 helm 客户端版本                                         |

### ***\*配置国内 chart 仓库\****

微软仓库（http://mirror.azure.cn/kubernetes/charts/）这个仓库推荐，基本上官网有的 chart 这里都有。

阿里云仓库（https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts	）

官方仓库（https://hub.kubeapps.com/charts/incubator）官方 chart 仓库，国内有点不好使。

#### ***\*添加存储库\****

helm repo add stable http://mirror.azure.cn/kubernetes/charts

helm repo add aliyun https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts 

helm repo update

helm repo list

#### ***\*查看配置的存储库\****

helm repo list

helm search repo stable

#### ***\*删除存储库：\****

helm repo remove aliyun

### ***\*helm 基本使用\****

#### ***\*主要介绍三个命令：\****

chart install

chart upgrade

chart rollback

#### ***\*使用 chart 部署一个应用\****

\#查找 chart 

\# helm search repo weave

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps67.jpg) 

\#查看 chrt 信息 

\# helm show chart stable/weave-scope 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps68.jpg) 

\#安装包 

\# helm install ***\*ui\**** stable/weave-scope

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps69.jpg) 

\#查看发布状态 

\# helm list

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps70.jpg) 

\#helm status ui

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps71.jpg) 

\#修改 service Type: NodePort 即可访问 ui

\# kubectl edit svc ui-weave-scope

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps72.jpg) 

#### ***\*安装前自定义 chart 配置选项\****

自定义选项是因为并不是所有的 chart 都能按照默认配置运行成功，可能会需要一些环境 

依赖，例如 PV。 

所以我们需要自定义 chart 配置选项，安装过程中有两种方法可以传递配置数据： 

· --values（或-f）：指定带有覆盖的 YAML 文件。这可以多次指定，最右边的文件 

优先

· --set：在命令行上指定替代。如果两者都用，--set 优先级高

### ***\*自定义chart\****

\# helm create mycart

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps73.jpg) 

Chart.yaml：用于描述这个 Chart 的基本信息，包括名字、描述信息以及版本等。 

values.yaml ：用于存储 templates 目录中模板文件中用到变量的值。 

Templates： 目录里面存放所有 yaml 模板文件。 

charts：目录里存放这个 chart 依赖的所有子 chart。 

NOTES.txt ：用于介绍 Chart 帮助信息， helm install 部署后展示给用户。

例如：如何使用这个 Chart、列出缺省的设置等。 

_helpers.tpl：放置模板助手的地方，可以在整个 chart 中重复使用

kubectl create deployment nginx-app --image=nginx -o yaml --dry-run > deployment.yaml

kubectl expose deployment nginx-app --port=80 --target-port=80 --type=NodePort -o yaml --dry-run > service.yaml

cd ../..

#### ***\*安装自定义chart\****

helm install [chart名称] [chart目录]

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps74.jpg) 

#### ***\*应用升级\****

如果更改了yaml文件则需要升级更新 helm upgrade [chart名称] [chart目录]

#### ***\*应用卸载\****

helm uninstall [chart名称] [chart目录]

### ***\*helm\*******\*高效复用\****

通过传递参数动态渲染模板

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps75.jpg) 

values.yaml ：用于存储 templates 目录中模板文件中用到变量的值(yaml文件全局变量)

yaml文件大体的几个不同点:

image

tag

label

port

replicas

 

l 在values.yaml定义变量和值

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps76.jpg) 

l 在具体yaml获取定义变量值

通过表达式的形式来使用全局变量

{{ .Values.变量名}}  {{ .Release.Name}}

V大写

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps77.jpg) 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps78.jpg) 

## ***\*kubernetes-集群资源监控\****

监控指标:

节点资源利用率

节点数

运行pods

pod监控

容器指标

应用程序

### ***\*监控平台搭建\****

搭建监控的组件有很多,此处选择prometheus(普罗米修斯)+***\*grafana\****

prometheus:

开源的

监控,报警,数据库

以http协议周期性抓取被监控组件状态

不需要复杂的集成过程.使用http接口接入就可以

***\*grafana:\****

开源的数据分析和可视化工具

支持多种数据源

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps79.jpg) 

### ***\*部署\****

#### ***\*部署守护进程yaml\****

kubectl create -f node-exporter.yaml

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps80.png)

#### ***\*部署prometheus\****

kubectl create -f rbac-setup.yaml

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps81.png)

kubectl create -f configmap.yaml 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps82.png)

kubectl create -f prometheus-deployment.yaml 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps83.png)

#### ***\*部署grafana\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps84.png)

#### ***\*配置\****

打开grafana配置prometheus数据源,导入显示模板

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps85.jpg) 

访问对应节点的grafana服务

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps86.jpg) 

默认账号密码admin/admin

 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps87.jpg) 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps88.jpg) 

此处的IP填写SVC查出来的IP

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps89.jpg) 

#### ***\*设置显示数据模板\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps90.jpg) 

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps91.jpg) 

如果下载不了使用离线json导入

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps92.png)

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps93.jpg) 

## ***\*kubernetes-master集群高可用\****

### ***\*介绍\****

单master节点问题在于如果maser节点挂了则无法有效控制管理node节点

Kubernetes 作为容器集群系统，通过健康检查+重启策略实现了 Pod 故障自我修复能力， 通过调度算法实现将 Pod 分布式部署，监控其预期副本数，并根据 Node 失效状态自动在正常 Node 启动 Pod，实现了应用层的高可用性。

 

针对 Kubernetes 集群，高可用性还应包含以下两个层面的考虑：Etcd 数据库的高可用性和 Kubernetes Master 组件的高可用性。 而 Etcd 我们已经采用 3 个节点组建集群实现高可用，本节将对 Master 节点高可用进行说明和实施。

 

Master 节点扮演着总控中心的角色，通过不断与工作节点上的 Kubelet 和 kube-proxy 进行通信来维护整个集群的健康工作状态。如果 Master 节点故障，将无法使用 kubectl 工具或者 API 任何集群管理。

 

Master 节点主要有三个服务 kube-apiserver、kube-controller-mansger 和 kube- scheduler，其中 kube-controller-mansger 和 kube-scheduler 组件自身通过选择机制已经实现了高可用，所以 Master 高可用主要针对 kube-apiserver 组件，而该组件是以 HTTP API 提供服务，因此对他高可用与 Web 服务器类似，增加负载均衡器对其负载均衡即可， 并且可水平扩容。

### ***\*多 Master 架构图：\****

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps94.png) 

VIP指的是虚拟IP(同网段)

 

keepalived + haproxy (或者Nginx也可以)来实现多master集群

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps95.jpg) 

keepalived：配置虚拟ip，检查节点的状态

haproxy：负载均衡服务【类似于nginx】

apiserver：

controller：

manager：

scheduler：

### ***\*初始化操作\*******\*(\*******\*文中出现的IP根据自己的情况来修改\*******\*)\****

这里我们假设是有俩台master一台node,实际一般会是3master(2+)node

我们需要在这三个节点上进行操作

\# 关闭防火墙(注意:生产环境不要做这步)

systemctl stop firewalld

systemctl disable firewalld

 

\# 关闭selinux

\# 永久关闭

sed -i 's/enforcing/disabled/' /etc/selinux/config  

\# 临时关闭

setenforce 0  

 

\# 关闭swap

\# 临时

swapoff -a 

\# 永久关闭

sed -ri 's/.*swap.*/#&/' /etc/fstab

 

\# 根据规划设置主机名【master1节点上操作】

hostnamectl set-hostname master1

\# 根据规划设置主机名【master2节点上操作】

hostnamectl set-hostname master2

\# 根据规划设置主机名【node1节点操作】

hostnamectl set-hostname node1

 

\# r添加hosts

cat >> /etc/hosts << EOF

192.168.44.158 master.k8s.io k8s-vip

192.168.44.155 master01.k8s.io master1

192.168.44.156 master02.k8s.io master2

192.168.44.157 node01.k8s.io node1

EOF

 

\# 将桥接的IPv4流量传递到iptables的链【3个节点上都执行】

cat > /etc/sysctl.d/k8s.conf << EOF

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

EOF

 

\# 生效

sysctl --system  

 

\# 时间同步

yum install ntpdate -y

ntpdate time.windows.com

### ***\*部署keep\*******\*keepAlived\****

下面我们需要在所有的master节点【master1和master2】上部署keepAlive

\# 安装相关工具

yum install -y conntrack-tools libseccomp libtool-ltdl

\# 安装keepalived

yum install -y keepalived

### ***\*配置master节点\****

#### ***\*添加master1的配置\****

cat > /etc/keepalived/keepalived.conf <<EOF 

! Configuration File for keepalived

 

global_defs {

  router_id k8s

}

 

vrrp_script check_haproxy {

  script "killall -0 haproxy"

  interval 3

  weight -2

  fall 10

  rise 2

}

 

vrrp_instance VI_1 {

  state MASTER 

  interface ens33 

  virtual_router_id 51

  priority 250

  advert_int 1

  authentication {

​    auth_type PASS

​    auth_pass ceb1b3ec013d66163d6ab

  }

  virtual_ipaddress {

​    192.168.142.204

  }

  track_script {

​    check_haproxy

  }

 

}

EOF

#### ***\*添加master2的配置\****

 

cat > /etc/keepalived/keepalived.conf <<EOF 

! Configuration File for keepalived

 

global_defs {

  router_id k8s

}

 

vrrp_script check_haproxy {

  script "killall -0 haproxy"

  interval 3

  weight -2

  fall 10

  rise 2

}

 

vrrp_instance VI_1 {

  state BACKUP 

  interface ens33 

  virtual_router_id 51

  priority 200

  advert_int 1

  authentication {

​    auth_type PASS

​    auth_pass ceb1b3ec013d66163d6ab

  }

  virtual_ipaddress {

​    192.168.142.204

  }

  track_script {

​    check_haproxy

  }

 

}

EOF

### ***\*启动和检查\****

在两台master节点都执行

\# 启动keepalived

systemctl start keepalived.service

\# 设置开机启动

systemctl enable keepalived.service

\# 查看启动状态

systemctl status keepalived.service

 

启动后查看master的网卡信息

ip a s ens33

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps96.jpg) 

### ***\*部署haproxy\****

haproxy主要做负载的作用，将我们的请求分担到不同的node节点上

\# 安装haproxy

yum install -y haproxy

\# 启动 haproxy

systemctl start haproxy

\# 开启自启

systemctl enable haproxy

 

启动后，我们查看对应的端口是否包含 16443

netstat -tunlp | grep haproxy

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps97.jpg) 

### ***\*配置haproxy\****

两台master节点的配置均相同，配置中声明了后端代理的两个master节点服务器，指定了haproxy运行的端口为16443等，因此16443端口为集群的入口

cat > /etc/haproxy/haproxy.cfg << EOF

\#---------------------------------------------------------------------

\# Global settings

\#---------------------------------------------------------------------

global

  \# to have these messages end up in /var/log/haproxy.log you will

  \# need to:

  \# 1) configure syslog to accept network log events.  This is done

  \#   by adding the '-r' option to the SYSLOGD_OPTIONS in

  \#   /etc/sysconfig/syslog

  \# 2) configure local2 events to go to the /var/log/haproxy.log

  \#  file. A line like the following can be added to

  \#  /etc/sysconfig/syslog

  \#

  \#   local2.*            /var/log/haproxy.log

  \#

  log     127.0.0.1 local2

  

  chroot    /var/lib/haproxy

  pidfile   /var/run/haproxy.pid

  maxconn   4000

  user     haproxy

  group    haproxy

  daemon 

​    

  \# turn on stats unix socket

  stats socket /var/lib/haproxy/stats

\#---------------------------------------------------------------------

\# common defaults that all the 'listen' and 'backend' sections will

\# use if not designated in their block

\#---------------------------------------------------------------------  

defaults

  mode           http

  log           global

  option          httplog

  option          dontlognull

  option http-server-close

  option forwardfor    except 127.0.0.0/8

  option          redispatch

  retries         3

  timeout http-request   10s

  timeout queue      1m

  timeout connect     10s

  timeout client      1m

  timeout server      1m

  timeout http-keep-alive 10s

  timeout check      10s

  maxconn         3000

\#---------------------------------------------------------------------

\# kubernetes apiserver frontend which proxys to the backends

\#--------------------------------------------------------------------- 

frontend kubernetes-apiserver

  mode         tcp

  bind         *:16443

  option        tcplog

  default_backend    kubernetes-apiserver   

\#---------------------------------------------------------------------

\# round robin balancing between the various backends

\#---------------------------------------------------------------------

backend kubernetes-apiserver

  mode     tcp

  balance   roundrobin

  server    master01.k8s.io  192.168.142.200:6443 check

  server    master02.k8s.io  192.168.142.203:6443 check

\#---------------------------------------------------------------------

\# collection haproxy statistics message

\#---------------------------------------------------------------------

listen stats

  bind         *:1080

  stats auth      admin:awesomePassword

  stats refresh     5s

  stats realm      HAProxy\ Statistics

  stats uri       /admin?stats

EOF

 

### ***\*安装Docker、Kubeadm、kubectl\****

所有节点安装Docker/kubeadm/kubelet ，Kubernetes默认CRI（容器运行时）为Docker，因此先安装Docker

#### ***\*安装Docker\****

l 首先配置一下Docker的阿里yum源

cat >/etc/yum.repos.d/docker.repo<<EOF

[docker-ce-edge]

name=Docker CE Edge - \$basearch

baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/7/\$basearch/edge

enabled=1

gpgcheck=1

gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg

EOF

l 然后yum方式安装docker

\# yum安装

yum -y install docker-ce

 

\# 查看docker版本

docker --version  

\# 启动docker

systemctl enable docker

systemctl start docker

l 配置docker的镜像源

cat >> /etc/docker/daemon.json << EOF

{

 "registry-mirrors": ["https://b9pmyelo.mirror.aliyuncs.com"]

}

EOF

l 然后重启docker

systemctl restart docker

#### ***\*添加kubernetes软件源\****

然后我们还需要配置一下yum的k8s软件源

cat > /etc/yum.repos.d/kubernetes.repo << EOF

[kubernetes]

name=Kubernetes

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64

enabled=1

gpgcheck=0

repo_gpgcheck=0

gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg

EOF

#### ***\*安装kubeadm，kubelet和kubectl\****

由于版本更新频繁，这里指定版本号部署：

\# 安装kubelet、kubeadm、kubectl，同时指定版本

yum install -y kubelet-1.20.4 kubeadm-1.20.4 kubectl-1.20.4

\# 设置开机启动

systemctl enable kubelet

### ***\*部署Kubernetes Master【master节点】\****

#### ***\*创建kubeadm配置文件\****

在具有vip的master上进行初始化操作，这里为master1

\# 创建文件夹

mkdir /usr/local/kubernetes/manifests -p

\# 到manifests目录

cd /usr/local/kubernetes/manifests/

\# 新建yaml文件

vi kubeadm-config.yaml

yaml内容如下所示：

apiServer: certSANs:  - master1  - master2  - master.k8s.io  - 192.168.44.158  - 192.168.44.155  - 192.168.44.156  - 127.0.0.1 extraArgs:  authorization-mode: Node,RBAC timeoutForControlPlane: 4m0sapiVersion: kubeadm.k8s.io/v1beta1certificatesDir: /etc/kubernetes/pkiclusterName: kubernetescontrolPlaneEndpoint: "master.k8s.io:16443"controllerManager: {}dns:  type: CoreDNSetcd: local:     dataDir: /var/lib/etcdimageRepository: registry.aliyuncs.com/google_containerskind: ClusterConfigurationkubernetesVersion: v1.16.3networking:  dnsDomain: cluster.local   podSubnet: 10.244.0.0/16 serviceSubnet: 10.1.0.0/16scheduler: {}

 

然后我们在 master1 节点执行

kubeadm init --config kubeadm-config.yaml

 

执行完成后，就会在拉取我们的进行了【需要等待...】

 

按照提示配置环境变量，使用kubectl工具

# 执行下方命令mkdir -p $HOME/.kubesudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/configsudo chown $(id -u):$(id -g) $HOME/.kube/config# 查看节点kubectl get nodes# 查看podkubectl get pods -n kube-system

 

按照提示保存以下内容，一会要使用：

kubeadm join master.k8s.io:16443 --token jv5z7n.3y1zi95p952y9p65 \

  --discovery-token-ca-cert-hash sha256:403bca185c2f3a4791685013499e7ce58f9848e2213e27194b75a2e3293d8812 \

--control-plane 

--control-plane ： 只有在添加master节点的时候才有

查看集群状态

\# 查看集群状态

kubectl get cs

\# 查看pod

kubectl get pods -n kube-system

 

### ***\*安装集群网络\****

从官方地址获取到flannel的yaml，在master1上执行

\# 创建文件夹

mkdir flannel

cd flannel

\# 下载yaml文件

wget -c https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

安装flannel网络

kubectl apply -f kube-flannel.yml 

检查

kubectl get pods -n kube-system

### ***\*master2节点加入集群\****

#### ***\*复制密钥及相关文件\****

从master1复制密钥及相关文件到master2

# ssh root@192.168.44.156 mkdir -p /etc/kubernetes/pki/etcd# scp /etc/kubernetes/admin.conf root@192.168.44.156:/etc/kubernetes# scp /etc/kubernetes/pki/{ca.*,sa.*,front-proxy-ca.*} root@192.168.44.156:/etc/kubernetes/pki# scp /etc/kubernetes/pki/etcd/ca.* root@192.168.44.156:/etc/kubernetes/pki/etcd

#### ***\*master2加入集群\****

执行在master1上init后输出的join命令,需要带上参数--control-plane表示把master控制节点加入集群

kubeadm join master.k8s.io:16443 --token ckf7bs.30576l0okocepg8b   --discovery-token-ca-cert-hash sha256:19afac8b11182f61073e254fb57b9f19ab4d798b70501036fc69ebef46094aba --control-plane

 

检查状态

kubectl get node

kubectl get pods --all-namespaces

### ***\*加入Kubernetes Node\****

在node1上执行

向集群添加新节点，执行在kubeadm init输出的kubeadm join命令：

kubeadm join master.k8s.io:16443 --token ckf7bs.30576l0okocepg8b   --discovery-token-ca-cert-hash sha256:19afac8b11182f61073e254fb57b9f19ab4d798b70501036fc69ebef46094aba

 

集群网络重新安装，因为添加了新的node节点

 

检查状态

kubectl get node

kubectl get pods --all-namespaces

### ***\*测试kubernetes集群\****

在Kubernetes集群中创建一个pod，验证是否正常运行：

\# 创建nginx deployment

kubectl create deployment nginx --image=nginx

\# 暴露端口

kubectl expose deployment nginx --port=80 --type=NodePort

\# 查看状态

kubectl get pod,svc

 

然后我们通过任何一个节点，都能够访问我们的nginx页面

 

## ***\*K8S集群部署项目\****

### ***\*容器交付流程\****

l 开发代码阶段

\1. 编写代码

\2. 编写Dockerfile【打镜像做准备】

l 持续交付/集成

\1. 代码编译打包

\2. 制作镜像

\3. 上传镜像仓库

l 应用部署

\1. 环境准备

\2. Pod

\3. Service

\4. Ingress

l 运维

\1. 监控

\2. 故障排查

\3. 应用升级

### ***\*k8s部署Java项目流程\****

l 制作镜像【Dockerfile】

l 上传到镜像仓库【Dockerhub、阿里云、网易】

l 控制器部署镜像【Deployment】

l 对外暴露应用【Service、Ingress】

l 运维【监控、升级】

### ***\*k8s部署Java项目\****

#### ***\*准备Java项目\****

第一步，准备java项目，把java进行打包【jar包或者war包】

#### ***\*依赖环境\****

在打包java项目的时候，我们首先需要两个环境

l java环境【JDK】

l maven环境

然后把java项目打包成jar包

mvn clean install

#### ***\*编写Dockerfile文件\****

Dockerfile 内容如下所示

FROM openjdk:8-jdk-alpineVOLUME /tmpADD ./target/demojenkins.jar demojenkins.jarENTRYPOINT ["java","-jar","/demojenkins.jar", "&"]

#### ***\*制作镜像\****

在我们创建好Dockerfile文件后，我们就可以制作镜像了

我们首先将我们的项目，放到我们的服务器上

然后执行下面命令打包镜像

docker build -t java-demo-01:latest .

等待一段后，即可制作完成我们的镜像

最后通过下面命令，即可查看我们的镜像了

docker images

#### ***\*启动镜像\****

在我们制作完成镜像后，我们就可以启动我们的镜像了

docker run -d -p 8111:8111 java-demo-01:latest -t

启动完成后，我们通过浏览器进行访问，即可看到我们的java程序

http://192.168.177.130:8111/user

#### ***\*推送镜像\****

下面我们需要将我们制作好的镜像，上传到镜像服务器中【阿里云、DockerHub】

首先我们需要到 阿里云 [容器镜像服务](https://cr.console.aliyun.com/cn-hangzhou/instances/repositories)，然后开始创建镜像仓库

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps98.jpg) 

然后选择本地仓库

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps99.jpg) 

我们点击我们刚刚创建的镜像仓库，就能看到以下的信息

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps100.jpg) 

##### ***\*登录镜像服务器\****

使用命令登录

docker login --username=XXXXXXX@163.com registry.cn-shenzhen.aliyuncs.com

然后输入刚刚我们开放时候的注册的密码

##### ***\*镜像添加版本号\****

下面为我们的镜像添加版本号

\# 实例

docker tag [ImageId] registry.cn-shenzhen.aliyuncs.com/mogublog/java-project-01:[镜像版本号]

\# 举例

docker tag 33f11349c27d registry.cn-shenzhen.aliyuncs.com/mogublog/java-project-01:1.0.0

 

操作完成后

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps101.jpg) 

##### ***\*推送镜像\****

在我们添加版本号信息后，我们就可以推送我们的镜像到阿里云了

docker push registry.cn-shenzhen.aliyuncs.com/mogublog/java-project-01:1.0.0

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps102.jpg) 

操作完成后，我们在我们的阿里云镜像服务，就能看到推送上来的镜像了

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps103.jpg) 

#### ***\*控制器部署镜像\****

在我们推送镜像到服务器后，就可以通过控制器部署镜像了

首先我们需要根据刚刚的镜像，导出yaml

\# 导出yaml

kubectl create deployment  javademo1 --image=registry.cn-

shenzhen.aliyuncs.com/mogublog/java-project-01:1.0.0 --dry-run -o yaml > javademo1.yaml

 

导出后的 javademo1.yaml 如下所示

apiVersion: apps/v1kind: Deploymentmetadata: creationTimestamp: null labels:  app: javademo1 name: javademo1spec: replicas: 1 selector:  matchLabels:   app: javademo1 strategy: {} template:  metadata:   creationTimestamp: null   labels:    app: javademo1  spec:   containers:   - image: registry.cn-shenzhen.aliyuncs.com/mogublog/java-project-01:1.0.0    name: java-project-01    resources: {}status: {}

然后通过下面命令，通过yaml创建我们的deployment

\# 创建

kubectl apply -f javademo1.yaml

\# 查看 pods

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps104.jpg) 

或者我们可以进行扩容，多创建几个副本

kubectl scale deployment javademo1 --replicas=3

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps105.jpg) 

然后我们还需要对外暴露端口【通过service 或者 Ingress】

\# 对外暴露端口

kubectl expose deployment javademo1 --port=8111  --target-port=8111 --type=NodePort

\# 查看对外端口号

kubectl get svc

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps106.jpg) 

然后通过下面的地址访问

\# 对内访问

curl http://10.106.103.242:8111/user

\# 对外访问

http://192.168.177.130:32190/user

#### ***\*运维\****

....

## ***\*相关命令:\****

查看kubelet状态

systemctl status kubelet.service

重启docker和kubelet服务

\# systemctl restart docker.service

\# systemctl restart kubelet.service

查看pod

kubectl get pod --all-namespaces

查看pod的yml配置 

kubectl get pod -n kube-system -o yaml kube-apiserver 

查看日志 后缀可变pod

kubectl logs -n kube-system kube-apiserver

查看kubelet日志

journalctl -xefu kubelet 或者journalctl -xeu kubelet

 

kubectl get pod -n ingress-nginx -owide#查看部署在哪个节点

 

netstat -antp | grep 80#查看80端口是否监听

 

docker ps #查看docker镜像信息 

docker rm -f CONTAINER ID  #强制移除镜像

 

查看pod镜像

kubectl get pods ingress-nginx-admission-patch-t9zbw -n ingress-nginx -o yaml | grep image:

 

获取登录token

kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token

 

允许master节点部署pod

kubectl taint nodes --all node-role.kubernetes.io/master-

如果不允许调度

kubectl taint nodes master节点名 node-role.kubernetes.io/master=:NoSchedule

污点可选参数

   NoSchedule: 一定不能被调度

   PreferNoSchedule: 尽量不要调度

   NoExecute: 不仅不会调度, 还会驱逐Node上已有的Pod

## ***\*版本说明：\****

版本，k8s版本与docker需要匹配一般情况18.09能匹配1.14-1.20版本k8s

推荐使用1.20版本k8s比如1.20.4，需要准备镜像，正常情况下k8s.gcr.io镜像源国内是下载不了的，需从dockerhub获取其他国内镜像源下载

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.20.4

Docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.20.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.2

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.4.13-0

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.7.0

docker pull kubernetesui/dashboard:v2.3.0

docker pull kubernetesui/metrics-scraper:v1.0.6

docker pull quay.io/coreos/flannel:v0.14.0-amd64

docker pull calico/node:v3.16.3

docker pull calico/pod2daemon-flexvol:v3.16.3

docker pull calico/cni:v3.16.3

docker pull calico/kube-controllers:v3.16.3 

 

\# 重新tag镜像，该步骤是因为K8S配置清单中yaml用的是默认镜像地址

docker images \

  | grep registry.cn-hangzhou.aliyuncs.com/google_containers \

  | sed 's/registry.cn-hangzhou.aliyuncs.com\/google_containers/k8s.gcr.io/' \

  | awk '{print "docker tag " $3 " " $1 ":" $2}' \

| sh#导出镜像

 

把镜像修改成k8s.gcr.io后进行上述镜像全打包

dashboard:v2.3.0 K8S1.20版本至少使用此版本dashboard

dashboard可有可无只是一个图形界面可用其他的平台替代，市面上有一些像kuborad之类但是收费的

 

其他资料

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps107.png)

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps108.png)

## ***\*K8S-额外笔记\****

### ***\*yaml\****

通常情况下手写yaml是非常困难的可以通过俩种方式简化

\1. 使用kubectl create 来创建yaml

kubectl create deployment nginx-app --image=nginx -o yaml --dry-run > nginx-app.yaml

\#deployment 来说明类型

\#nginx-app指明应用名称

\# --image来说明镜像

\#--dry-run 尝试运行，并不真的运行

\#nginx-app.yaml 生成的文件名称

生成之后在对其进行进一步编辑

![img](file:///C:\Users\anytron\AppData\Local\Temp\ksohtml10680\wps109.jpg) 

\2. 对已有的部署好的yaml导出kubectl get

kubectl get deploy [已部署的deploy名称] -o=yaml --export > [自定义导出文件名.yaml]

 

 