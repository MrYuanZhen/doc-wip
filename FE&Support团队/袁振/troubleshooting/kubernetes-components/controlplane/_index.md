---
title: Troubleshooting Controlplane Nodes
---

本节适用于具有 `control` 角色的节点.

## 检查Controlplane容器是否正在运行

在具有`control`角色的节点上启动了三个特定的容器

- `kube-apiserver`
- `kube-controller-manager`
- `kube-scheduler`

这三个容器的正常情况应该是 **Up** 状态。 并且 **Up** 状态应该是长时间运行，通过下面命令可以进行检查：

```
docker ps -a -f=name='kube-apiserver|kube-controller-manager|kube-scheduler'
```

输出示例：

```
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS              PORTS               NAMES
26c7159abbcc        rancher/hyperkube:v1.11.5-rancher1   "/opt/rke-tools/en..."   3 hours ago         Up 3 hours                              kube-apiserver
f3d287ca4549        rancher/hyperkube:v1.11.5-rancher1   "/opt/rke-tools/en..."   3 hours ago         Up 3 hours                              kube-scheduler
bdf3898b8063        rancher/hyperkube:v1.11.5-rancher1   "/opt/rke-tools/en..."   3 hours ago         Up 3 hours                              kube-controller-manager
```

## Controlplane 容器日志

> **注意:** 如果是 `control` 角色的节点， `kube-controller-manager` 和 `kube-scheduler` 会通过选举选举出leader节点。 只有leader节点会记录当前操作的日志信息。查看 [Kubernetes leader 选举](/docs/troubleshooting/kubernetes-resources/#kubernetes-leader-election) 可以知道如何查看当前的leader节点

通过下面命令查看容器日志信息可以查看到可能包含的错误信息：

```
docker logs kube-apiserver
docker logs kube-controller-manager
docker logs kube-scheduler
```
