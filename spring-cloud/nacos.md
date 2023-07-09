# Nacos 配置中心

https://juejin.cn/book/7180604185786712075/section/7181705133464977468

## 服务发现

### 客户端

#### 服务注册

* **客户端怎么进行服务注册的？**

  1. spring读取nacos提供的`spring.factories`文件，创建其中的配置类`NacosServiceRegistryAutoConfiguration`
  2. 配置类会注册两个Bean，其中一个Bean——`NacosServiceRegistry`，其`register`方法包含实际注册的代码逻辑。另一个 Bean——`NacosAutoServiceRegistration` 会依赖`NacosServiceRegistry`
  3. 利用 **Spring 事件监听**(`ApplicationListener.onApplicationEvent`) ，在监听方法中最终会调用的 `register` 方法，从而完成自动注册。
  4. 在 register 方法会发送http请求给nacos服务端，完成服务注册（同时会启动心跳定时任务，定时发送心跳）。

  ![image-20230708173228498](./pic/image-20230708152444653.png)

  