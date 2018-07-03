### Docker-Kubernetes

#### 介绍
Kubernetes是Google团队发起并维护的基于Docker的开源容器集群管理系统，它不仅支持常见的云平台，而且支持内部数据中心。

[什么是Docker](https://yeasy.gitbooks.io/docker_practice/content/introduction/what.html)


##### 安装minikube和kubectl
[教程](https://www.imooc.com/article/details/id/23785)
[Rails on Kubernetes](https://blog.cosmocloud.co/rails-on-kubernetes-part-1/)

Linux
```
1、安装minikube
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
$ minikube version

2、安装kubectl
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl 
$ chmod +x ./kubectl 
$ sudo mv ./kubectl /usr/local/bin/kubectl
$ kubectl version

3、安装Virtualbox
[教程](https://www.cnblogs.com/wangshuyi/p/6927113.html)
$ yum clean all
$ yum makecache
$ yum install VirtualBox-5.2
```


##### 配置k8s
```
$ minikube version		# 查看版本
$ kubectl version  		# 查看kubectl版本

$ minikube start			# 创建k8s集群
$ minikube ssh			# 进入k8s查看信息
$ kubectl cluster-info	# 连接k8s api server
$ kubectl get nodes 	# 浏览node的个数

$ kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080				# 创建部署
$ kubectl get deployments	# 列出所有的部署
$ kubectl proxy			# 运行代理
$ curl http://localhost:8001/version		# 查看运行代理后的api
$ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')		# 获取pod名称，然后在环境变量中存储
$ curl http://localhost:8001/api/v1/proxy/namespaces/default/pods/$POD_NAME/	# 对刚才的pod进行网路请求
```

##### 浏览Pods和Nodes
Pod是Kubernetes平台上的原子单元。 K8s -> Nodes -> Pods -> Containers

```
$ kubectl get 
$ kubectl describte
$ kubectl logs
$kubectl exec


```























