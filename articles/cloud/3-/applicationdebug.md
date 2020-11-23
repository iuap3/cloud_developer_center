## 应用远程调试
### 以JAVA应用为例
#### 操作步骤
1. 应用属性增加环境变量\
```CATALINA_OPTS = -Xdebug  -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n```\
如图:\
![image](/articles/cloud/3-/images/debug/env.png)
2. 应用增加调试端口，需和步骤1中的属性```address=8000```一致，如8000\
如图:\
![image](/articles/cloud/3-/images/debug/ports.png)
3. 保存并重启实例\
![image](/articles/cloud/3-/images/debug/restart.png)
4. 实例重启完毕后,点击需要远程调试的实例控制台\
5. 在实例控制台中输入```echo $PORT0,$PORT1```,输出信息为应用放开的端口对应该实例的数组，```PORT0```对应8080端口，```PORT1```对应实例的8000端口（即远程调试使用的端口号）\
![image](/articles/cloud/3-/images/debug/terminal.png)
6. 查看实例运行的主机\
![image](/articles/cloud/3-/images/debug/host.png)
7. 配置开发工具IDE的远程调试信息为步骤6的IP地址和步骤5获得的端口\
![image](/articles/cloud/3-/images/debug/debug.png)