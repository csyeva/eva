# SpringCloud Hystrix 实现

# 基于方法的融断

> pom.xml引入相应的依赖

```java
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
```

## 配置hystrix 默认超时时间

```xml
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds: 5000
```

## 应用入口

> 应用入口 引入@EnableCircuitBreaker 熔断器注解

```java

@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker
public class ConsumerMovieRibbonApplication {

  @Bean
  @LoadBalanced
  public RestTemplate restTemplate() {
    return new RestTemplate();
  }

  public static void main(String[] args) {
    SpringApplication.run(ConsumerMovieRibbonApplication.class, args);
  }
}

```

# 基于Feign的融断

> UserFeignClient

```java
@FeignClient(name = "microservice-provider-user", fallback = HystrixClientFallback.class)
public interface UserFeignClient {
  @RequestMapping(value = "/simple/{id}", method = RequestMethod.GET)
  public User findById(@PathVariable("id") Long id);
}
```

> HystrixClientFallback
```java
@Component
public class HystrixClientFallback implements UserFeignClient {

  @Override
  public User findById(Long id) {
    User user = new User();
    user.setId(0L);
    return user;
  }
}
```


## controller 中实现融短方法

> @HystrixCommand 指定融断方法
> 注意熔断器的方法一定和原方法的名称保持一致
> 配置@HystrixCommand(fallbackMethod = "findByIdFallback", commandProperties = @HystrixProperty(name = "execution.isolation.strategy", value = "SEMAPHORE"))
* 配置完上面isolation的表示findById 与 findByIdFallback 在同一线程。不配置表示默认两个方法在不同的线程,其中findByIdFallback在隔离线程。

```java
@RestController
public class MovieController {
  @Autowired
  private RestTemplate restTemplate;

  @GetMapping("/movie/{id}")
  @HystrixCommand(fallbackMethod = "findByIdFallback")
  public User findById(@PathVariable Long id) {
    return this.restTemplate.getForObject("http://microservice-provider-user/simple/" + id, User.class);
  }

  public User findByIdFallback(Long id) {
    User user = new User();
    user.setId(0L);
    return user;
  }
}
```

## Feign 关闭Hystrix

### 局部关闭Hystrix支持

* Feign 默认支持Hystrix。Feign.Builder: HystrixFeign.Builder
* 关闭Hystrix的支持只需要修改feignBuilder配置如下：

```java
  @Bean
  @Scope("prototype")
  public Feign.Builder feignBuilder() {
    return Feign.builder();
  }
```

### 全局关闭Hystrix支持
* feign.hystrix.enabled = false

## 采用FallbackFactory 实现熔断

> 为FeignClient 配置HystrixClientFactory

```java
@FeignClient(name = "microservice-provider-user", fallbackFactory = HystrixClientFactory.class)
public interface UserFeignClient {
  @RequestMapping(value = "/simple/{id}", method = RequestMethod.GET)
  public User findById(@PathVariable("id") Long id);
}

```
> HystrixClientFactory 
```java
@Component
public class HystrixClientFactory implements FallbackFactory<UserFeignClient> {

  private static final Logger LOGGER = LoggerFactory.getLogger(HystrixClientFactory.class);

  @Override
  public UserFeignClient create(Throwable cause) {
    HystrixClientFactory.LOGGER.info("fallback; reason was: {}", cause.getMessage());
    return new UserFeignClientWithFactory() {
      @Override
      public User findById(Long id) {
        User user = new User();
        user.setId(-1L);
        return user;
      }
    };
  }
}

```

## UserFeignClientWithFactory 实现了 UserFeignClient接口
```java
public interface UserFeignClientWithFactory extends UserFeignClient {

}
```

# 查看Hystrix状态
> http://localhost:8010/hystrix.stream 查看Hystrix信息
> http://localhost:8010/health 查看监控状态

# Hystrix DashBoard

pom.xml 

* 引入对DashBoard的支持


```java

		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
		</dependency>
```


> EnableHystrixDashboard 应用入口指定对HystrixDashboard的支持

```java
@EnableHystrixDashboard
@SpringBootApplication
public class EurekaApplication {
  public static void main(String[] args) {
    SpringApplication.run(EurekaApplication.class, args);
  }
}

```

* 访问地址 http://localhost:8030/hystrix
* 填写要检测的hystrix: http://localhost:8010/hystrix.stream 



