# kubernetes介绍

## kubernetes组件

一个kubernetes集群主要是由**控制节点（master）**、**工作节点（node）**构成，每个节点上都会安装不同的组件。

**master：集群的控制平面，负责集群的决策和管理**

> APIserver：资源操作的唯一入口，接受用户输入的命令，提供认证、授权、API注册和发现等机制
>
> Scheduler：负责集群资源调度，按照预定的调度策略，将POD调度到相应的node节点上
>
> Controler-Manager：负责维护集群的状态，比如程序部署安排、故障检测、自动扩展、滚动更新等
>
> Etcd：负责存储集群中各种资源对象的信息

**node：集群的数据平面，负责为容器提供运行环境**

> Kubelet：负责维护容器的生命周期，即通过控制docker，来创建、更新、销毁容器
>
> KuberProxy：负责提供集群内部的服务发现和负载均衡
>
> Docker：负责节点上容器的各种操作



## kubernetes组件调用关系

下面，以部署一个nginx服务来说明kubernetes系统各个组件的调用关系

1、首先要明确，一旦kubernetes环境启动之后，master和node都会将自身的信息存储到etcd数据库中

2、一个nginx服务的安装请求会首先被发送到master节点的apiserver组件

3、apiserver组件会调用scheduler组件来决定到底应该把这个服务安装到哪个node节点上

​	  在此时，它会从etcd中读取各个node节点信息，然后按照一定的算法进行选择，并将结果返回apiserver

4、apiserver调用controler-manager去调度node节点安装nginx服务

5、kubelet接收到指令后，会通知docker，然后由docker来启动一个nginx的pod

​	  pod是kubernetes的最小操作单元，容器必须泡在pod中

6、由此，一个nginx服务就运行了，如果需要访问nginx，还需要通过kube-proxy来对pod产生访问的代理

​	  这样，外界用户就可以访问集群中的nginx服务了



## kubernetes概念

**master**：集群的控制节点，每个集群需要至少一个master节点负责集群的管控

**node**：工作负载节点，由master分配容器到这些node工作节点上，然后node节点上的docker负责容器的运行

**pod**：kubernetes的最小控制单元，容器都是运行在pod中的，一个pod中可以有一个或者多个容器

**controler**：控制器，通过它来实现对pod的管理，比如启动pod、停止pod、伸缩pod的数量等等

**service**：pod对外服务的统一入口，虾米那可以维护着同一类的多个pod

**label**：标签，用于对pod的进行分类管理，同一类的pod会拥有相同的标签

**namespace**：命名空间，用来隔离pod的运行环境










