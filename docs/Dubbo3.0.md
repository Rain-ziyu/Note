# Dubbo入门使用

传统的微服务项目之间的相互调用使用的是发送Http请求或者是Feign等，原理上都是使用REST请求调用服务方接口得到json数据进行反序列化。

# 服务提供方改造

## 将一个传统的服务方的的项目，改造成Dubbo项⽬，有⼏件事情要做： 

1. 添加dubbo核⼼依赖 
2. 添加要使⽤的注册中⼼依赖
3. 添加要使⽤的协议的依赖 
4. 配置dubbo相关的基本信息 
5. 配置注册中⼼地址 
6. 配置所使⽤的协议

### 增加依赖

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.7.14</version>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>3.2.4</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.10.2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.dubbo</groupId>
            <artifactId>dubbo-rpc-triple</artifactId>
            <version>3.2.4</version>

        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
            <version>2021.0.4.0</version>
        </dependency>

        <dependency>
            <groupId>asia.huayu</groupId>
            <artifactId>common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
```

### 配置yml

```yml
dubbo:
  application:
    name: consumer
    # 作为服务提供方我们使用的协议以及dubbo服务端口
  protocol:
    name: dubbo
    port: -1
  registry:
    #  配置注册中心 无论是提供方还是调用方都需要去配置
    address: nacos://huayu.asia:8848
    use-as-config-center: false  #dubbo 不使用nacos作为配置中心

server:
  port: 9999

spring:
  application:
    name: consumer
  #配置中心
  cloud:
    # 注册中心配置
    nacos:
      discovery:
        server-addr: huayu.asia:8848
```

### 改造服务

consumer和provider中都⽤到了User类，所以可以单独新建⼀个maven项⽬⽤来存consumer和provider 公⽤的⼀些类，新增⼀个common模块，把User类转移到这个模块中改造成Dubbo，得先抽象出来服务，⽤接⼝表示。 像UserService就是⼀个服务，不过我们得额外定义⼀个接⼝，我们把之前的UserService改为 UserServiceImpl，然后新定义⼀个接⼝UserService，该接⼝表示⼀个服务，UserServiceImpl为该服务 的具体实现。

服务接口

```java
public interface UserService {
    public User getUser(String uid);
}
```

服务实现

```java
@DubboService
public class UserServiceImpl implements UserService {
    public User getUser(String uid) {
        User zhouyu = new User(uid, "zhouyu");
        return zhouyu;
   }
}
```

注意：要把Spring中的@Service注解替换成Dubbo中的@DubboService注解。 然后把UserService接⼝也转移到common模块中去，在provider中依赖common。 改造之后的provider为：

![](https://prod.huayu.asia:9000/picgo/2023/08/03/20230803154605.png)

### 开启Dubbo（提供方）

在ProviderApplication上加上@EnableDubbo，表示 Dubbo会去扫描某个路径下的@DubboService，从⽽对外提供该Dubbo服务。

```java
package asia.huayu;

import org.apache.dubbo.config.spring.context.annotation.EnableDubbo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @Description: ${description}
 * @Author: ZiYu
 * @Created on: 2023/08/03 8:55
 * @Since:
 */
@EnableDubbo
@SpringBootApplication
public class ProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class,args);
    }
}
```

此时就可以启动该Provider了，注意先启动注册中心，无需controller保留http接口即可让调用方完成调用

# 调⽤Dubbo服务

## 将一个传统的服务调用方的的项目，改造成Dubbo项⽬，有⼏件事情要做： 

在consumer中如果想要调⽤Dubbo服务，也要引⼊相关的依赖： 

1. 引⼊common，主要是引⼊要⽤调⽤的接⼝ 
2. 引⼊dubbo依赖
3. 引⼊需要使⽤的协议的依赖 
4. 引⼊需要使⽤的注册中⼼的依赖

### 引入依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.7.14</version>
    </dependency>

    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.10.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>3.2.4</version>
    </dependency>
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-rpc-dubbo</artifactId>
        <version>3.2.4</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        <version>2021.0.4.0</version>
    </dependency>

    <dependency>
        <groupId>asia.huayu</groupId>
        <artifactId>common</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

### 引⼊服务

通过@DubboReference注解来引⼊⼀个Dubbo服务。

```java
package asia.huayu.service;

import asia.huayu.User;
import asia.huayu.UserRequest;
import lombok.extern.slf4j.Slf4j;
import org.apache.dubbo.common.stream.StreamObserver;
import org.apache.dubbo.config.annotation.DubboReference;
import org.apache.dubbo.config.annotation.DubboService;
import org.springframework.stereotype.Service;

/**
 * @Description: 订单服务
 * @Author: ZiYu
 * @Created on: 2023/08/03 9:18
 * @Since:
 */
@Slf4j
@Service
public class OrderService {
    @DubboReference(version = "2.0")
    private UserService userService;
    @DubboReference
    private asia.huayu.UserService userService1;
    public String getOrder() {
        return userService.getUser();
    }
}
```

这样就不需要⽤RestTemplate或者Feign了。

### 配置yml

```yml
dubbo:
  application:
    name: consumer
  protocol:
    name: dubbo
    port: -1
  registry:
    #  配置注册中心
    address: nacos://huayu.asia:8848
    use-as-config-center: false  #dubbo 不使用nacos作为配置中心

server:
  port: 9999

spring:
  application:
    name: consumer
  #配置中心
  cloud:
    # 注册中心配置
    nacos:
      discovery:
        server-addr: huayu.asia:8848

```

### 调用

开启Dubbo，如果User没有实现Serializable接⼝，则会报错。

## 总结

⾃此，Dubbo的改造就完成了，总结⼀下： 

1. 添加pom依赖
2. 配置dubbo应⽤名、协议、注册中⼼
3.  定义服务接⼝和实现类 
4.  使⽤@DubboService来定义⼀个Dubbo服务 
5.  使⽤@DubboReference来使⽤⼀个Dubbo服务 
6.  使⽤@EnableDubbo开启Dubbo

# Dubbo3.0新特性介绍

## 注册模型的改变

在服务注册领域，市⾯上有两种模型，⼀种是应⽤级注册，⼀种是接⼝级注册，在Spring Cloud中，⼀ 个应⽤是⼀个微服务，⽽在Dubbo2.7中，⼀个接⼝是⼀个微服务。 

所以，Spring Cloud在进⾏服务注册时，是把应⽤名以及应⽤所在服务器的IP地址和应⽤所绑定的端⼝注 册到注册中⼼，相当于key是应⽤名，value是ip+port，⽽在Dubbo2.7中，是把接⼝名以及对应应⽤的IP 地址和所绑定的端⼝注册到注册中⼼，相当于key是接⼝名，value是ip+port。 

所以在Dubbo2.7中，⼀个应⽤如果提供了10个Dubbo服务，那么注册中⼼中就会存储10对keyvalue，⽽ Spring Cloud就只会存⼀对keyvalue，所以以Spring Cloud为⾸的应⽤级注册是更加适合的。 

所以Dubbo3.0中将注册模型也改为了应⽤级注册，提升效率节省资源的同时，通过统⼀注册模型，也为 各个微服务框架的互通打下了基础。

## 新⼀代RPC协议

定义了全新的 RPC 通信协议 – Triple，⼀句话概括 Triple：

​	它是基于 HTTP/2 上构建的 RPC 协 议，完全兼容 gRPC，并在此基础上扩展出了更丰富的语义。

使⽤ Triple 协议，⽤户将获得以下 能⼒ 

**·**	更容易到适配⽹关、Mesh架构，Triple 协议让 Dubbo 更⽅便的与各种⽹关、Sidecar 组件配 合⼯作。 

**·**	多语⾔友好，推荐配合 Protobuf 使⽤ Triple 协议，使⽤ IDL 定义服务，使⽤ Protobuf 编码业 务数据。

**·**	流式通信⽀持。Triple 协议⽀持 Request Stream、Response Stream、Bi-direction Stream 。

当使⽤Triple协议进⾏RPC调⽤时，⽀持多种⽅式来调⽤服务，只不过在服务接⼝中要定义不同的⽅法， ⽐如：

```java
package asia.huayu.service;

import org.apache.dubbo.common.stream.StreamObserver;

/**
 * @Description: UserService接口
 * @Author: ZiYu
 * @Created on: 2023/08/03 9:13
 * @Since:
 */
public interface UserService {
    /**
     * 方法getUser作用为：
     * api形式调用 UNRAY
     *
     * @param
     * @return java.lang.String
     * @throws
     * @author RainZiYu
     */
    String getUser();

    /**
     * 方法sayHelloStream作用为：
     * 双向流(BIDIRECTIONAL_STREAM)
     *
     * @param response
     * @return org.apache.dubbo.common.stream.StreamObserver<java.lang.String>
     * @throws
     * @author RainZiYu
     */
    StreamObserver<String> sayHelloStream(StreamObserver<String> response);

    /**
     * 方法sayHelloServerStream作用为：
     * 服务端流调用  流式接口 SERVER_STREAM(服务端流)
     *
     * @param request
     * @param response
     * @return void
     * @throws
     * @author RainZiYu
     */
    void sayHelloServerStream(String request, StreamObserver<String> response);
}

```

### UNARY

unary，就是正常的调⽤⽅法

服务实现类对应的⽅法：

```java
@DubboService(version = "1.0")
public class UserServiceImpl implements UserService {
    public String getUser(){
        return "wwl";
    }
}
```

服务消费者调⽤⽅式：

```java
return userService.getUser();
```

### SERVER_STREAM

服务实现类对应的⽅法：

```java
  @Override
    public void sayHelloServerStream(String request, StreamObserver<String> response) {
        response.onNext("接收到了:" + request+"1");
        response.onNext("接收到了:" + request+"2");
        response.onNext("接收到了:" + request);
        response.onCompleted();
    }

```

服务消费者调⽤⽅式：

```java
        userService.sayHelloServerStream("hhh", new StreamObserver<String>() {
            @Override
            public void onNext(String data) {
                log.info("接收到消息:" + data);
            }

            @Override
            public void onError(Throwable throwable) {
                log.info("调用出错");
            }

            @Override
            public void onCompleted() {
                log.info("调用完成");
            }
        });
```

### CLIENT_STREAM

服务实现类对应的⽅法：

```
    @Override
    public StreamObserver<String> sayHelloStream(StreamObserver<String> response) {
        return new StreamObserver<String>() {
            @Override
            public void onNext(String data) {
                log.info("服务端接受的客户端的内容："+data);
                response.onNext("响应结果:"+data);
            }

            @Override
            public void onError(Throwable throwable) {

            }

            @Override
            public void onCompleted() {
                log.info("接收完成");
            }
        };
    }
```

服务消费者调⽤⽅式：

```java
 // 双向流调用
        StreamObserver<String> stringStreamObserver = userService.sayHelloStream(new StreamObserver<String>() {
            @Override
            public void onNext(String data) {
                log.info("双向流接收到消息:" + data);
            }

            @Override
            public void onError(Throwable throwable) {

            }

            @Override
            public void onCompleted() {
                log.info("双端流调用完成");
            }
        });
        // 向服务端发送消息
        stringStreamObserver.onNext("1");
        stringStreamObserver.onNext("2");
        stringStreamObserver.onNext("3");
        stringStreamObserver.onCompleted();
```

### BI_STREAM

和CLIENT_STREAM⼀样

# Dubbo3.0跨语⾔调⽤

在⼯作中，我们⽤Java语⾔通过Dubbo提供了⼀个服务，另外⼀个应⽤（也就是消费者）想要使⽤这个 服务，如果消费者应⽤也是⽤Java语⾔开发的，那没什么好说的，直接在消费者应⽤引⼊Dubbo和服务 接⼝相关的依赖即可。 

但是，如果消费者应⽤不是⽤Java语⾔写的呢，⽐如是通过python或者go语⾔实现的，那就⾄少需要满 ⾜两个条件才能调⽤Java实现的Dubbo服务： 

1. Dubbo⼀开始是⽤Java语⾔实现的，那现在就需要⼀个go语⾔实现的Dubbo框架，也就是现在的 dubbo-go，然后在go项⽬中引⼊dubbo-go，从⽽可以在go项⽬中使⽤dubbo，⽐如使⽤go语⾔去暴露和使⽤Dubbo服务。 
1. 我们在使⽤Java语⾔开发⼀个Dubbo服务时，会把服务接⼝和相关类，单独抽象成为⼀个Maven项 ⽬，实际上就相当于⼀个单独的jar包，这个jar能被Java项⽬所使⽤，但不能被go项⽬所使⽤，所以 go项⽬中该如何使⽤Java语⾔所定义的接⼝呢？直接⽤是不太可能的，只能通过间接的⽅式来解决 这个问题，除开Java语⾔之外，那有没有其他技术也能定义接⼝呢？并且该技术也是Java和go都⽀ 持，这就是protobuf。

## protobuf

我们可以通过protobuf来定义接⼝，然后通过protobuf的编译器将接⼝编译为特定语⾔的实现。 

在common项⽬中定义⼀个新的userservice.proto⽂件，路径为**src/main/proto/userservice.proto**

```protobuf
syntax = "proto3";
package api;
option go_package = "./;api";
option java_multiple_files = true;
option java_package = "asia.huayu";
option java_outer_classname = "UserServiceProto";

service UserService{
  rpc GetUser(UserRequest) returns (User){}
}

message UserRequest{
  string uid = 1;
}
// The response message containing the greetings
message User {
  string uid = 1;
  string username = 2;
}
```

相当于定义了⼀个UserService服务，并且定义了⼀个getUser⽅法，接收UserRequest类型的参数，返 回User类型的对象。

## 编译成Java

在项⽬中的pom⽂件中添加相关maven插件：

```xml
  <build>
        <extensions>
            <extension>
                <groupId>kr.motd.maven</groupId>
                <artifactId>os-maven-plugin</artifactId>
                <version>1.6.1</version>
            </extension>
        </extensions>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.xolstice.maven.plugins</groupId>
                <artifactId>protobuf-maven-plugin</artifactId>
                <version>0.6.1</version>
                <configuration>
                    <protocArtifact>com.google.protobuf:protoc:3.7.1:exe:${os.detected.classifier}</protocArtifact>
                    <outputDirectory>build/generated/source/proto/main/java</outputDirectory>
                    <clearOutputDirectory>false</clearOutputDirectory>
                    <protocPlugins>
                        <protocPlugin>
                            <id>dubbo</id>
                            <groupId>org.apache.dubbo</groupId>
                            <artifactId>dubbo-compiler</artifactId>
                            <version>0.0.3</version>
                            <mainClass>org.apache.dubbo.gen.dubbo.Dubbo3Generator</mainClass>
                        </protocPlugin>
                    </protocPlugins>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>test-compile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>build-helper-maven-plugin</artifactId>
                <version>3.0.0</version>
                <executions>
                    <execution>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>add-source</goal>
                        </goals>
                        <configuration>
                            <sources>
                                <source>build/generated/source/proto/main/java</source>
                            </sources>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

然后运⾏模块中lifecycle的compile，就会进⾏编译。

并且会编译出对应的接⼝等信息，编译完成后，会⽣成⼀些类：

![](https://prod.huayu.asia:9000/picgo/2023/08/03/20230803162239.png)

其中就包括了⼀个UserService接⼝，所以我们的UserServiceImpl就可以实现这个接⼝了：

```java
package asia.huayu.service.impl;

import asia.huayu.User;
import asia.huayu.UserRequest;
import asia.huayu.UserService;
import org.apache.dubbo.config.annotation.DubboService;

import java.util.concurrent.CompletableFuture;

/**
 * @Description: 继承自Proto生成的Java接口
 * @Author: ZiYu
 * @Created on: 2023/08/03 15:08
 * @Since:
 */
@DubboService
public class UserServiceImpl2 implements UserService {
    @Override
    public User getUser(UserRequest request) {
        return User.newBuilder().setUid("qqq").build();
    }

    @Override
    public CompletableFuture<User> getUserAsync(UserRequest request) {
        return UserService.super.getUserAsync(request);
    }
}
```

⽽对于想要调⽤UserService服务的消费者⽽⾔，其实也是⼀样的改造，只需要使⽤同⼀份 userservice.proto进⾏编译就可以了，⽐如现在有⼀个go语⾔的消费者。

## go消费者调⽤java服务

⾸先，新建⼀个go模块：

![](https://prod.huayu.asia:9000/picgo/2023/08/03/20230803162519.png)

然后把userservice.proto复制到go-consumer/proto下，然后进⾏编译，编译成为go语⾔对应的服务代 码，只不过go语⾔中没有maven这种东⻄可以帮助我们编译，我们只能⽤原⽣的protobuf的编译器进⾏ 编译。

这就需要⼤家在机器上下载、安装protobuf的编译器：protoc

1. 下载地址：https://github.com/protocolbuffers/protobuf/releases
2.  解压之后，把protoc-3.20.1-win64\bin添加到环境变量中去 
3.  在cmd中执⾏protoc --version，能正常看到版本号即表示安装成功

另外还需要安装go：

1. 下载地址：https://studygolang.com/dl
2. 然后直接下⼀步安装 
3. 在cmd中（新开⼀个cmd窗⼝）执⾏go version，能正常看到版本号即表示安装成功

然后在go-consumer下新建⽂件夹api，进⼊到go-consumer/proto下，运⾏：

```shell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
go get -u github.com/dubbogo/tools/cmd/protoc-gen-go-triple
go install github.com/golang/protobuf/protoc-gen-go
go install github.com/dubbogo/tools/cmd/protoc-gen-go-triple
protoc -I. userservice.proto --go_out=../api --go-triple_out=../api
```

这样就会在go-consumer/api下⽣成⼀个userservice.pb.go⽂件和userservice_triple.pb.go⽂件

然后就可以写go语⾔的服务消费者了，新建⼀个consumer.go，内容为：

```go
package main

import (
	"context"
	"dubbo.apache.org/dubbo-go/v3/common/logger"
	"dubbo.apache.org/dubbo-go/v3/config"
	_ "dubbo.apache.org/dubbo-go/v3/imports"
	"go-consumer/api"
)

var userServiceImpl = new(api.UserServiceClientImpl)

// export DUBBO_GO_CONFIG_PATH=conf/dubbogo.yml
func main() {
	config.SetConsumerService(userServiceImpl)
	config.Load()

	logger.Info("start to test dubbo")
	req := &api.UserRequest{
		Uid: "1",
	}

	user, err := userServiceImpl.GetUser(context.Background(), req)

	if err != nil {
		logger.Error(err)
	}

	logger.Infof("client response result: %v\n", user)
}
```

然后在go-consumer下新建conf/dubbogo.yml，⽤来配置注册中⼼：

```yml
dubbo:
  registries:
    demoZK:
      protocol: nacos
      address: huayu.asia:8848
  consumer:
    references:
      UserServiceClientImpl:
        protocol: tri
        interface: asia.huayu.UserService
```

注意这⾥配置的协议为tri，⽽不是dubbo，在provider端也得把协议改为tri：

在运行配置中修改使用以上yml文件

```
DUBBO_GO_CONFIG_PATH=conf/dubbogo.yml
```

![](https://prod.huayu.asia:9000/picgo/2023/08/03/20230803164104.png)

安装完之后，直接执行代码可能会报错，可以在go-consumer⽬录下执⾏命令下载依赖：

```shell
go mod tidy
```

运⾏成功：

![](https://prod.huayu.asia:9000/picgo/2023/08/03/20230803165456.png)

# Dubbo3.2.x解决JDK17问题

在Dubbo3.2.x之前使用dubbo-spring-boot-starter与JDK17时会出现因为JDK9之后的安全封装，导致java.lang包下的非公开符号不能被随意反射，启动或者运行中出现问题：

```shell
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'ServiceBean:com.platform.ahj.tenant.provider.TenantDetailProvider::': Invocation of init method failed; nested exception is java.lang.IllegalStateException: Failed to create adaptive instance: java.lang.IllegalStateException: Can't create adaptive extension interface org.apache.dubbo.rpc.Protocol, cause: Unable to make protected final java.lang.Class java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain) throws java.lang.ClassFormatError accessible: module java.base does not "opens java.lang" to unnamed module @1f2586d6
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1804) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:620) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:542) ~[spring-beans-5.3.22.jar:5.3.22]
```

Dubbo3.2.x之前使用--add-opens把需要的封装打破，允许反射即可继续使用，但是需要在启动命令中增加vm选项

```sh
--add-opens java.base/java.math=ALL-UNNAMED --add-opens java.base/java.lang=ALL-UNNAMED
```

但是在Dubbo3.2.x之后官方说可以原生支持JDK17并且支持native打包，一般我们**直接升级dubbo-spring-boot-starter依赖版本即可**。

如果升级之后仍存在冲突问题，可以查看一下依赖中实际使用的 **javassist**依赖到底是哪个版本是否是dubbo-spring-boot-starter所传递的新版，一般是因为该依赖冲突导致的。

