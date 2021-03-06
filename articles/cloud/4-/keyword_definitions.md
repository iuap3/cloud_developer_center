# 常见关键字释义

- [常见关键字释义](#常见关键字释义)
  - [应用资源配置-CPU和内存](#应用资源配置-cpu和内存)
    - [CPU最大值](#cpu最大值)
    - [内存最大值](#内存最大值)
    - [CPU最小值](#cpu最小值)
    - [内存最小值](#内存最小值)
    - [应用最大CPU和内存推荐配置](#应用最大cpu和内存推荐配置)
  - [健康检查](#健康检查)
    - [开始检查时间](#开始检查时间)
    - [检查周期](#检查周期)
    - [超时](#超时)
    - [最多失败次数](#最多失败次数)
    - [端口类型](#端口类型)
    - [TCP协议](#tcp协议)
    - [HTTP协议](#http协议)
    - [COMMAND协议](#command协议)

## 应用资源配置-CPU和内存

### CPU最大值

CPU最大值是应用实例能够使用的CPU的最大值，比如将CPU最大值设置为：2，代表应用实例能够使用的CPU最大值为 2 个CPU。

### 内存最大值

内存最大值是应用实例能够使用的内存最大值，比如将内存最大值设置为：2048，代表应用实例能够使用的内存最大值为 2048 M。

### CPU最小值

CPU最小值是资源池节点要为应用实例预留的CPU资源。假设某台资源池节点 node1 的CPU为 4核，应用的CPU最小值为 0.2核，则该应用在 node1 上启动一个实例之后，node1 的CPU预留资源减少 0.2核。如果 node1 上的CPU预留资源小于 0.2核，则该应用不能部署到 node1 上。

### 内存最小值

内存最小值是资源池节点要为应用实例预留的内存资源。假设某台资源池节点 node1 的内存为 8192M，应用的CPU最小值为 2048M，则该应用在 node1 上启动一个实例之后，node1 的内存预留资源减少 2048M。如果 node1 上的内存预留资源小于 2048M，则该应用不能部署到 node1 上。

### 应用最大CPU和内存推荐配置

推荐配置：

| 应用类型 | CPU 最大值 | 内存最大值 |
| :---: | :---: | :---: |
| java | 2 | 2048 |
| node | 2 | 2048 |
| nginx | 2 | 1024 |
| python | 4 | 2048 |
| 其他 | 2 | 2048 |

## 健康检查

### 开始检查时间

开始检查时间是应用的状态变成【运行中】之后，多久开始对应用进行健康检查。

### 检查周期

检查周期是第一次健康检查之后，间隔多久对应用进行一次健康检查。

### 超时

超时是对应用进行健康检查的时候，应用响应的超时时间，如果超过了超时时间没有返回相应的结果，则认为健康检查失败。

### 最多失败次数

最多失败次数是连续失败多少次之后，认为该实例不健康。

### 端口类型

端口类型有两种：端口索引和端口号。其中：端口索引是【端口】-【容器端口】里面索引，从 0 开始；端口号是容器内指定端口。

### TCP协议

TCP协议是检查容器内指定端口，健康的标准为：指定端口存在且能够连通。

### HTTP协议

HTTP协议是，通过 GET 方法，检查容器内指定端口的【路径】是否可访问，健康的标准为：指定端口的【路径】返回小于 400 的状态码。

### COMMAND协议

COMMAND协议是在容器内执行【命令】，健康标准：能够正常执行命令。