# dockerfile示例

- [dockerfile示例](#dockerfile示例)
  - [如何使用dockerfile构建](#如何使用dockerfile构建)
  - [通过curl或者wget下载nginx服务器上的文件](#通过curl或者wget下载nginx服务器上的文件)
  - [利用git下载源码并编译](#利用git下载源码并编译)
    - [注意：如果密码中包含@符号，需要转义为%40](#注意如果密码中包含符号需要转义为40)
  - [利用多阶段构建(multi-stage build)实现java应用编译环境和运行环境隔离](#利用多阶段构建multi-stage-build实现java应用编译环境和运行环境隔离)
  - [利用多阶段构建编译前端项目并将编译产物放在nginx镜像里面](#利用多阶段构建编译前端项目并将编译产物放在nginx镜像里面)
  - [增加环境变量-解决maven编译内存溢出的问题](#增加环境变量-解决maven编译内存溢出的问题)
  - [使用dockerfile构建gitbook](#使用dockerfile构建gitbook)

## 如何使用dockerfile构建

编辑流水线，如下图所示：

![dockerfile_demo_01](./images/dockerfile_demo_01.png)

选择【生成镜像】，并点击【编辑】，如下图所示：

![dockerfile_demo_02](./images/dockerfile_demo_02.png)

输入 dockerfile 内容，并点击【保存】，如下图所示：

![dockerfile_demo_03](./images/dockerfile_demo_03.png)

勾选【使用 dockerfile 构建】，并点击【完成配置】，如下图所示：

![dockerfile_demo_04](./images/dockerfile_demo_04.png)

执行流水线即可。

## 通过curl或者wget下载nginx服务器上的文件

```dockerfile
FROM ycr.yonyoucloud.com/base/tomcat:8-jdk8-alpine

RUN \
    cd /usr/local/tomcat/webapps && \
    wget http://you_nginx_server_ip:port/path/to/download/file.war
```

## 利用git下载源码并编译

### 注意：如果密码中包含@符号，需要转义为%40

```dockerfile
# 设置构建应用的镜像
FROM ycr.yonyoucloud.com/base/java:8-jdk-alpine
# 设置工作目录
WORKDIR /app
# 构建应用
RUN \
	# 拉取 project-a 项目的代码
	git clone http://username:password@github.com/project/project-a.git -b master \
	# 拉取 project-b 项目的代码
	&& git clone http://username:password@github.com/project/project-b.git -b master \
	# 构建 project-a
	&& cd project-a && mvn install -Dmaven.test.skip=true -Pdev \
	# 构建 project-b
	&& cd ../project-b && mvn install -Dmaven.test.skip=true -Pdev
```

## 利用多阶段构建(multi-stage build)实现java应用编译环境和运行环境隔离

```dockerfile
# 设置构建应用的镜像
FROM ycr.yonyoucloud.com/base/java:8-jdk-alpine
# 设置工作目录
WORKDIR /app
# 构建应用
RUN \
	# 拉取 project-a 项目的代码
	git clone http://username:password@github.com/project/project-a.git -b master \
	# 拉取 project-b 项目的代码
	&& git clone http://username:password@github.com/project/project-b.git -b master \
	# 构建 project-a
	&& cd project-a && mvn install -Dmaven.test.skip=true -Pdev \
	# 构建 project-b
	&& cd ../project-b && mvn install -Dmaven.test.skip=true -Pdev

# 设置运行环境镜像
FROM ycr.yonyoucloud.com/base/java:8-jdk-alpine
# 设置工作目录
WORKDIR /app
# 将上一步构建的jar包，拷贝到运行环境的镜像里面
COPY --from=0 /app/project-b/project-b-service/target/mdf.jar /app
```

## 利用多阶段构建编译前端项目并将编译产物放在nginx镜像里面

```dockerfile
# 编译环境
FROM ycr.yonyoucloud.com/base/node:11-alpine
WORKDIR /design/op-fe
ADD . /design/op-fe
RUN npm install && npm run build

# 运行环境
FROM ycr.yonyoucloud.com/base/nginx:1.15-alpine-perl
COPY --from=0 /design/op-fe/dist /usr/share/nginx/html
RUN mkdir -p /data/nginx/cache/proxy_cache/tmp /data/log/nginx
```

## 增加环境变量-解决maven编译内存溢出的问题

参考：[maven内存溢出解决办法](maven_out_of_memory.md)

## 使用dockerfile构建gitbook

```dockerfile
FROM ycr.yonyoucloud.com/base/gitbook:3.2.3-buster

WORKDIR /app

RUN \
    git clone https://github.com/dunwu/gitbook-templates.git --depth 1 && \
    cd gitbook-templates && \
    echo 'nameserver 114.114.114.114' > /etc/resolv.conf && \
    gitbook install && \
    gitbook build

CMD ["gitbook", "serve"]
```
