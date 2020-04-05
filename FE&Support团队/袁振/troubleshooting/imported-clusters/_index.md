---
title: Imported clusters
---

此页面上列出的命令/步骤可用于检查要导入的群集或在Rancher中导入的群集。

确保您配置了正确的kubeconfig (示例, `export KUBECONFIG=$PWD/kubeconfig_from_imported_cluster.yml`)

#### Rancher agents

通过Rancher cluster-agent与群集进行通信（通过`cows-cluster-agent`调用Kubernetes API与集群通讯）和通过`cows-node-agent`与节点进行通信。

如果cowle-cluster-agent无法连接到已有的rancher server 也就是`server-url`, 群集将保留在 **Pending**状态, 错误显示为 `Waiting for full cluster configuration`.

##### cattle-node-agent

检查是否每个节点都正常运行了cattle-node-agent, 正确运行的状态应该是 **Running** 并且重启的次数应该不多；

```
kubectl -n cattle-system get pods -l app=cattle-agent -o wide
```

输出示例：

```
NAME                      READY     STATUS    RESTARTS   AGE       IP                NODE
cattle-node-agent-4gc2p   1/1       Running   0          2h        x.x.x.x           worker-1
cattle-node-agent-8cxkk   1/1       Running   0          2h        x.x.x.x           etcd-1
cattle-node-agent-kzrlg   1/1       Running   0          2h        x.x.x.x           etcd-0
cattle-node-agent-nclz9   1/1       Running   0          2h        x.x.x.x           controlplane-0
cattle-node-agent-pwxp7   1/1       Running   0          2h        x.x.x.x           worker-0
cattle-node-agent-t5484   1/1       Running   0          2h        x.x.x.x           controlplane-1
cattle-node-agent-t8mtz   1/1       Running   0          2h        x.x.x.x           etcd-2
```

检查特定节点上cattle-node-agent或者所有节点上cattle-node-agent pods的日志是否有错误:

```
kubectl -n cattle-system logs -l app=cattle-agent
```

##### cattle-cluster-agent

检查集群中是否正确运行了cattle-cluster-agent, 正确运行的状态应该是 **Running** 并且重启的次数应该不多；

```
kubectl -n cattle-system get pods -l app=cattle-cluster-agent -o wide
```

输出示例：

```
NAME                                    READY     STATUS    RESTARTS   AGE       IP           NODE
cattle-cluster-agent-54d7c6c54d-ht9h4   1/1       Running   0          2h        x.x.x.x      worker-1
```

检查cattle-cluster-agent pod的日志是否有错误:

```
kubectl -n cattle-system logs -l app=cattle-cluster-agent
```
