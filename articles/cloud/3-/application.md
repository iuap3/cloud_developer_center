# 概述
用友开发者中心的应用管理提供了对已发部署应用的管理，包括应用的暂停、销毁、扩缩等。
## 应用管理入口
在开发者中心菜单栏点击应用管理即可进入应用管理页面。

![image](/articles/cloud/3-/images/application/1.png)

## 应用列表
进入应用管理菜单后，可以查看到已部署的应用列表，应用可以显示为卡片和列表视图，支持通过资源池、主机等过滤。每一个应用都展示了一些基本信息，通过点击应用，可进入应用详情页；同时支持应用的销毁。

![image](/articles/cloud/3-/images/application/2_1.png)
![image](/articles/cloud/3-/images/application/2_2.png)

## 应用详情
1. 在应用详情页，显示了应用的域名、镜像、创建时间等；同时提供了暂停、重启、销毁、升级、回滚、上架等功能
> 暂停：将应用的暂停，这是应用的实例数变为了0

> 重启：将应用的所有实例重启，先启动相同的实例数，健康后销毁旧的实例，实现蓝绿切换

> 销毁：删除应用，将应用完全销毁

> 迁移：将应用迁移到其他资源池

> 升级：更新应用

> 回滚：由于某些原因，回滚到旧的版本

> 权限管理：可以将自己的应用授权给别人

> 上架：上架应用

![image](/articles/cloud/3-/images/application/3.png)

2. 扩缩

修改应用的实例数来应对不同的访问量

![image](/articles/cloud/3-/images/application/4.png)

3. ```实例```页签

实例页签列出了所有正在运行的实例，包括实例的运行状态、健康情况，以及运行的主机等；支持特定实例的销毁、日志查看、性能监控等功能。

![image](/articles/cloud/3-/images/application/5.png)

> ```销毁```实例

*选择实例后，点击```销毁```按钮，即可删除当前实例并重新启动一个新的实例*
> ```销毁并缩容```

*选择实例后，点击```销毁```按钮，即可删除当前实例，并不会再启动新的实例*
> 点击运行日志的图标，可以查看当前实例的运行日志

> 点击容器控制台的图标，会打开一个控制台页面，可以进入容器进行命令操作

4. ```属性```页签

属性页签可以修改部署应用的属性，包括实例个数、cpu、内存、镜像、健康检查等内容。

![image](/articles/cloud/3-/images/application/6.png)

5. ```日志与事件```页签

事件页签显示了最后修改、失败任务等。日志页签可以查看应用的日志，并可以暂停日志刷新。

![image](/articles/cloud/3-/images/application/7_1.png)

![image](/articles/cloud/3-/images/application/7_2.png)

![image](/articles/cloud/3-/images/application/7_3.png)

6. ```监控与报警```页签

监控页签显示了每个实例的cpu、内存、网络的图表信息，可以查看多个时间段的数据.
报警页签显示了应用变更、接口调用、域名检测等信息。

![image](/articles/cloud/3-/images/application/8_1.png)
![image](/articles/cloud/3-/images/application/8_2.png)

7. ```域名```页签

域名页签显示了应用的域名，并可以手动设置绑定新的域名。
 
![image](/articles/cloud/3-/images/application/9.png)

9. ```配置文件```页签

配置文件页签可以查看应用的配置文件。

![image](/articles/cloud/3-/images/application/10.png)

10. ```自动扩缩```页签

依据实际业务需求量情况，可以自行设置自动扩缩功能来实现应用的自动扩缩。

![image](/articles/cloud/3-/images/application/11.png)



 