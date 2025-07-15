可以通过minikube（一个轻量级的工具可在本地运行 k8s 集群）开发、测试和学习而无需在云端或生产环境中部署完整的 k8s 集群

kubectl get pod -A

kubectl get nodes



- node
工作单元，k8s 会根据资源需求和调度策略，将 pod 分配到合适的 node上，node这一层是管理资源的；

- namespace
逻辑隔离机制，不同命名空间中的资源默认无法直接互相访问；
default：默认命名空间，未指定命名空间的资源会被创建在这里。
kube-system：存放 Kubernetes 系统组件（如 kube-dns、etcd）

- pod
最小部署单元，每个 Pod 包含一个或多个容器（ip是相同的），pod只能管理自己内部的应用程序
