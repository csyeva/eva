# Spring Cloud Feign使用

* Feign 本身支持 SpringMvc的部分注解

# 代码实现

* 配置依赖
```xml
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-feign</artifactId>
	</dependency>
```

* 入口应用程序
* @EnableFeignClients 增加feign的支持
```java
@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients
public class ConsumerMovieFeignApplication {
  public static void main(String[] args) {
    SpringApplication.run(ConsumerMovieFeignApplication.class, args);
  }
}

```

* 编写接口
* feign 不支持 @GetMapping
* @PathVariable 必须得设置
* 参数是复杂对象，即使指定了是GET方法，feign依然会以POST方法进行发送请求

```java

@FeignClient("microservice-provider-user")
public interface UserFeignClient {
  @RequestMapping(value = "/simple/{id}", method = RequestMethod.GET)
  public User findById(@PathVariable("id") Long id); 

  @RequestMapping(value = "/user", method = RequestMethod.POST)
  public User postUser(@RequestBody User user);
}

```

controller

```java
@RestController
public class MovieController {

  @Autowired
  private UserFeignClient userFeignClient;

  @GetMapping("/movie/{id}")
  public User findById(@PathVariable Long id) {
    return this.userFeignClient.findById(id);
  }

  @GetMapping("/test")
  public User testPost(User user) {
    return this.userFeignClient.postUser(user);
  }

}
```



