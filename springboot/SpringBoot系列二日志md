# spring 日志

>* 支持日志框架：Java Util Logging, Log4J2 and Logback，默认是使用logback
>* 配置方式：默认配置文件配置和引用外部配置文件配置

# 一、Logback

## 默认配置文件配置

>* 不建议使用：不够灵活，对log4j2等不够友好

```xml

# 日志文件名，比如：roncoo.log，或者是 /var/log/roncoo.log
logging.file=roncoo.log 
# 日志级别配置，比如： logging.level.org.springframework=DEBUG
logging.level.*=info
logging.level.org.springframework=DEBUG

```

## 引用外部配置文件

>* spring boot默认会加载classpath:logback-spring.xml或者classpath:logback-spring.groovy
>* 使用自定义配置文件，配置方式为

```xml
logging.config=classpath:logback-roncoo.xml
注意：不要使用logback这个来命名，否则spring boot将不能完全实例化
```

## 使用基于spring boot的配置
```xml

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
<include resource="org/springframework/boot/logging/logback/base.xml"/>
<logger name="org.springframework.web" level="DEBUG"/>
</configuration>

```

# 二、Log4j


> 去除logback的依赖包，添加log4j2的依赖包

```xml
		<exclusions>
			<exclusion>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-starter-logging</artifactId>
			</exclusion>
		</exclusions>

		<!-- 使用log4j2 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-log4j2</artifactId>
		</dependency>

```

>* 在classpath添加log4j2.xml或者log4j2-spring.xml（spring boot 默认加载）

# 比较 Logback 与 Log4J2
>* 性能比较：Log4J2 和 Logback都优于 log4j（不推荐使用）
>* 配置方式：Logback最简洁，spring boot默认，推荐使用








