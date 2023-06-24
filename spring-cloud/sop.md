# Maven

https://blog.csdn.net/MSDCP/article/details/127680844

# Nacos

服务发现、配置中心

## 部署

https://nacos.io/zh-cn/docs/quick-start-docker.html

## 服务发现

* 在启动类上添加 @EnableDiscoveryClient

* application.yml

  ```yml
  spring:
    cloud:
      nacos:
        discovery:
          server-addr: localhost:8848 #配置Nacos地址
  ```

## 配置中心

* application.yml

  ```yml
  spring:
    profiles:
      active: dev
  ```

* bootstrap.yml

  ```yaml
  spring:
    cloud:
      nacos:
        config:
          server-addr: localhost:8848 #Nacos地址
          file-extension: yaml #这里我们获取的yaml格式的配置
  ```

  

* ```java
  @RefreshScope // 动态刷新配置
  ```

# 熔断

配合 feign使用：

```java
@FeignClient(value = "user-service",fallback = UserFallbackService.class)
public interface UserService {
}
```

fallback指定服务降级逻辑。

指定熔断算法：

```yaml
feign: # 下面两者二选一
  hystrix:
    enabled: true #在Feign中开启Hystrix
  sentinel:
    enabled: true #在Feign中开启Sentinel
```

# Spring

https://blog.csdn.net/qq_43377749/article/details/115442050

* 打包成 jar 包的形式，不建议选 war ，因为 spring 已经整合了 Tomcat 等服务器，打包 jar 包更方便运行
* 配置文件优先级application.yml：https://blog.csdn.net/wangxinyao1997/article/details/103232944

## 坑

* mybatis的依赖引入之后需要使用，如果暂时不使用就先关掉。否则就会报错"Failed to determine a suitable driver class"

  ```xml
      <!-- <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>2.3.1</version>
      </dependency> -->
  ```

* Missing artifact com.alibaba.cloud:spring-cloud-starter-alibaba-nacos-discovery

  https://blog.csdn.net/BigCandidate/article/details/115526516

  正确配置（**type、scope 不能少，否则报错。 todo原因？**）：

  ```xml
  		<dependency>
  			<groupId>com.alibaba.cloud</groupId>
  			<artifactId>spring-cloud-alibaba-dependencies</artifactId>
  			<version>2.1.0.RELEASE</version>
  			<type>pom</type>
  			<scope>import</scope>
  		</dependency>
  		
  		<dependency>
  			<groupId>com.alibaba.cloud</groupId>
  			<artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
  			<version>2.1.4.RELEASE</version>
  			<type>pom</type>
  			<scope>import</scope>
  		</dependency>
  ```

   