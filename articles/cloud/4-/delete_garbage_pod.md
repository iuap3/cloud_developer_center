# 删除注册中心垃圾实例

根据垃圾实例的 IP，通过 kubernetes master 上的 kubectl 搜索，命令如下：

```shell
kubectl get po -o wide --all-namespaces | grep garbage_ip
```

将搜索出来的 POD 名称删除，命令如下：

```shell
kubectl delete garbage_pod_name -n garbage_pod_namespace --force --grace-period=0
```