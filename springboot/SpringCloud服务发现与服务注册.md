# 服务发现与服务注册

# 服务发现的方式

## 客户端发现
> Eureka
* Eureka是Netflix开发的服务发现框架，本身是一个基于REST的服务，主要用于定位运行在AWS域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。Spring Cloud将它集成在其子项目spring-cloud-netflix中，以实现Spring Cloud的服务发现功能。
* Eureka 项目相当活跃，代码更新相当频繁，目前最新的版本是1.5.5。Eureka 2.0的版本也正在紧锣密鼓地开发中，2.0将会带来更好的扩展性，并且使用细粒度的订阅模型取代了基于拉取的模型，目前还没有Release。

> Zk 

## 服务器端发现
* Consul + nginx

# 服务发现
![image](https://github.com/csyeva/eva/blob/master/img/springcloud/fwfxx.png)

## 服务发现组件的功能
> 服务注册表
* 服务注册表是一个记录当前可用服务实例的网络信息的数据库，是服务发现机制的核心。服务注册表提供查询API和管理API，使用查询API获得可用的服务实例，使用管理API实现注册和注销；

> 服务注册
* 服务注册:服务启动时，将服务的网络地址注册到服务注册表中；


> 健康检查
* 服务发现组件会通过一些机制定时检测已注册的服务，如果发现某服务无法访问了（可能是某几个心跳周期后），就将该服务从服务注册表中移除。

# 服务发现组件：Eureka

* Eureka来自生产环境
* Spring Cloud对Eureka支持很好

## Eureka 原理

![image](https://github.com/csyeva/eva/blob/master/img/springcloud/er1.png)
![image](https://github.com/csyeva/eva/blob/master/img/springcloud/er2.png)

> 上图是来自Eureka官方的架构图，大致描述了Eureka集群的工作过程。

* Application Service 就相当于本书中的服务提供者（用户微服务），Application Client就相当于本书中的服务消费者（电影微服务）；
* Make Remote Call，可以简单理解为调用RESTful的接口；
* us-east-1c、us-east-1d等是zone，它们都属于us-east-1这个region；
 
> 由图可知，Eureka包含两个组件：Eureka Server 和 Eureka Client。
 
* Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册，这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。
* Eureka Client是一个Java客户端，用于简化与Eureka Server的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。
* 在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除（默认90秒）。
* Eureka Server之间将会通过复制的方式完成数据的同步。
* Eureka还提供了客户端缓存的机制，即使所有的Eureka Server都挂掉，客户端依然可以利用缓存中的信息消费其他服务的API。
 
> 综上，Eureka通过心跳检测、健康检查、客户端缓存等机制，确保了系统的高可用性、灵活性和可伸缩性。



