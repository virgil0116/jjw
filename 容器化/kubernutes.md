# 介绍

**生产级别的容器编排系统： 自动化的容器部署、扩展和管理。**

[Kubernetes](https://kubernetes.io/zh/docs/concepts/overview/what-is-kubernetes/) 是用于自动部署，扩展和管理容器化应用程序的开源系统。

它将组成应用程序的容器组合成逻辑单元，以便于管理和服务发现。Kubernates源自Google15年的生产运维经验。

一个K8s系统通常称为一个**K8S集群（Cluster）**。

这个集群主要包含两部分：

* 一个Master节点（主节点：主要负责管理和控制。）
* 一群Node节点（工作计算节点：是工作负载节点，里面是具体的容器。）

容器技术部署应用就像**金刚葫芦娃变成葫芦七兄弟一样**。

### 特性

* 自动上线和回滚
* 服务发现和负载均衡
* 服务拓扑（Service Topology）
* 存储编排
* Secret和配置管理
* 自动装箱
* 批量执行
* IPv4和IPv6双协议栈
* 水平扩缩
* 自我修复



### 节点

##### Master节点（指挥官）

包括API Server、Scheduler、Controller manager、etcd。

* API Server是整个系统的对外接口，供客户端和其他组件调用，相当于“营业厅”。
* Scheduler负责对集群内部的资源进行调度，相当于“调度室”。
* Controller manager负责管理控制器，相当于“大总管”。
* etcd 是一个kubernates集群的数据库，所有持久化的信息都存储在etcd中。

##### Node节点（干活的）

包括Docker、kubelet、kube-proxy、Fluentd、kube-dns（可选），还有**Pod**。

> Pod是Kubernetes最基本的操作单元。一个Pod代表着集群中运行的一个进程，它内部封装了一个或多个紧密相关的容器。除了Pod之外，K8S还有一个Service的概念，一个Service可以看作一组提供相同服务的Pod的对外访问接口。

* Kubelete 主要负责监视指派到它所在Node上的**Pod**，包括创建、修改、监控、删除等。（对pod的增删改查）
* Kube-proxy 负责为**Pod对象**提供代理。
* Fluentd 负责日志收集、存储和查询。
* 



