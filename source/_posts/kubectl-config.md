---
title: kubectl配置
date: 2019-11-08 10:47:18
excerpt: kubectl配置
---

### 参考文档：
1. [Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
2. [kubectl-commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
3. [Organizing Cluster Access Using kubeconfig Files](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/)

### 配置文件(kubeconfig file)
配置文件有什么用呢？
用来组织集群的信息，包括集群(cluster)，用户(user)，命名空间(namespace)和授权机制(authentication mechanisms)
`kubectl`使用`kubeconfig`配置文件来发现一些信息，这些信息帮助它选择集群和帮助它与集群中的API server进行通信

>我们通常将这个配置文件叫为`kubeconfig file`，但是这并不意味有一个名为`kubeconfig`的文件存在。

一般有三种方式指定配置
##### 1. 默认
默认在`$HOME/.kube`目录下查找一个名字为`config`的配置文件
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    server: https://192.168.88.75:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/client.crt
    client-key: /root/.minikube/client.key

```

##### 2. 通过环境变量设置
设置 `KUBECONFIG` 环境变量，指定`config`的文件位置

##### 3. 指定`--kubeconfig`选项
在使用`kubectl`命令时指定`--kubeconfig`选项指定配置文件位置。
这个选项可以通过`kubectl options`命令查询出来，描述如下
```
--kubeconfig='': Path to the kubeconfig file to use for CLI requests.
```