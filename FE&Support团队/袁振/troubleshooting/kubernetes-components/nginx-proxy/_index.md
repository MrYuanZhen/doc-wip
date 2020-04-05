---
title: Troubleshooting nginx-proxy
---

`nginx-proxy` 容器部署在除了`control` 角色的所有节点上。他通过动态生成NGINX的配置，从而提供对`control` 角色节点的访问。

## 检查容器是否正在运行

nginx-proxy容器在正常情况应该是 **Up** 状态。 并且 **Up** 状态应该是长时间运行，通过下面命令可以进行检查：

```
docker ps -a -f=name=nginx-proxy
```

输出示例：

```
docker ps -a -f=name=nginx-proxy
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS               NAMES
c3e933687c0e        rancher/rke-tools:v0.1.15   "nginx-proxy CP_HO..."   3 hours ago         Up 3 hours                              nginx-proxy
```

## 检查动态生成的NGINX配置

生成的配置应包括具有`control`角色的节点的IP地址。 可以使用以下命令检查配置：

```
docker exec nginx-proxy cat /etc/nginx/nginx.conf
```

输出示例：

```
error_log stderr notice;

worker_processes auto;
events {
  multi_accept on;
  use epoll;
  worker_connections 1024;
}

stream {
        upstream kube_apiserver {

            server ip_of_controlplane_node1:6443;

            server ip_of_controlplane_node2:6443;

        }

        server {
            listen        6443;
            proxy_pass    kube_apiserver;
            proxy_timeout 30;
            proxy_connect_timeout 2s;

        }

}
```

## nginx-proxy 容器日志

通过下面命令查看容器日志信息可以查看到可能包含的`nginx-proxy`错误信息：

```
docker logs nginx-proxy
```
