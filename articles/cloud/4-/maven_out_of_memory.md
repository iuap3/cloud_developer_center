# maven编译内存溢出解决办法

- [maven编译内存溢出解决办法](#maven编译内存溢出解决办法)
  - [问题现象](#问题现象)
    - [现象1](#现象1)
    - [现象2](#现象2)
  - [解决办法](#解决办法)
    - [dockerfile类型](#dockerfile类型)
    - [非dockerfile类型](#非dockerfile类型)
  - [参考](#参考)


流水线 dockerfile 在使用 maven 编译的时候，有时候会遇到内存溢出的现象。

## 问题现象

### 现象1

>[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.5.1:compile (default-compile) on project test-maven: Fatal error compiling: java.lang.OutOfMemoryError: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos23653: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24051: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24054: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24745: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24760: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24823: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24834: RegularFileObject[/app/test-maven/src/main/java/com/dst/dl/download/service/channel/vip/VipDownload.java]@pos24829: GC overhead limit exceeded -> [Help 1]  
>[ERROR]  
>[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.  
>[ERROR] Re-run Maven using the -X switch to enable full debug logging.  
>[ERROR]  
>[ERROR] For more information about the errors and possible solutions, please read the following articles:  
>[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException  
>[ERROR]  
>[ERROR] After correcting the problems, you can resume the build with the command  
>[ERROR]   mvn <goals> -rf :test-maven

### 现象2

> [ERROR] Failed to execute goal org.apache.maven.plugins:maven-war-plugin:3.2.2:war (default-war) on project test-mavn: Error assembling WAR: Problem creating war: Execution exception: Java heap space -> [Help 1]  
> [ERROR]  
> [ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.  
> [ERROR] Re-run Maven using the -X switch to enable full debug logging.  
> [ERROR]  
> [ERROR] For more information about the errors and possible solutions, please read the following articles:  
> [ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException  
> [ERROR]  
> [ERROR] After correcting the problems, you can resume the build with the command  
> [ERROR]   mvn <goals> -rf :test-maven

## 解决办法

### dockerfile类型

解决办法是增加 maven 内存：`[[ -f /etc/mavenrc ]] && sed -i 's/ -Xmx512m//g' /etc/mavenrc; export MAVEN_OPTS="-Xmx2048m"`，具体的 dockerfile 如下：

```dockerfile
FROM ycr.yonyoucloud.com/base/tomcat:8-jdk8-alpine-appupload
WORKDIR /app
RUN \
git clone http://git.yonyou.com/test-maven.git -b master \
&& cd /app/test-maven \
&& [[ -f /etc/mavenrc ]] && sed -i 's/ -Xmx512m//g' /etc/mavenrc; export MAVEN_OPTS="-Xmx2048m" \
&& mvn clean install -U -Dmaven.test.skip=true
```

### 非dockerfile类型

在 pom.xml 里面增加：

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                  <fork>true</fork>
                  <meminitial>512m</meminitial>
                  <!-- 如果不够读者可以加大 -->
                  <maxmem>4096m</maxmem>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## 参考

- http://note.youdao.com/s/IUj4m3UF
- https://stackoverflow.com/questions/18744351/maven-assembly-plugin-gives-java-heap-space-error