---
layout:     post
title:      搭建-spring-boot-项目
subtitle:   
date:       2018-05-20
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - spring-boot
    - java
    - maven

---
spring-boot, spring-cloud....学java然后就是各种spring...学的都忘了spring居然是春天，这么美好的含义...  

---
### 先装java吧
下个1.8的jdk包，安装  
```
> java -version
java version "1.8.0_172"
Java(TM) SE Runtime Environment (build 1.8.0_172-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.172-b11, mixed mode)
```

### 管理工具 
1. maven or gradle, xml恐惧症的选了gradle  
2. `brew install gradle`  
```
> gradle --version
Welcome to Gradle 4.10.2!
```

### 搭建spring-boot
1. 通过[Spring Initializr](https://start.spring.io/)来生成
2. 自动生成的resources里面默认使用application.properties，这个文件是spring boot工程的主要配置文件，支持application.yml和application.properties两种形式，喜欢yml的请把文件修好成application.yml
3. `gradle build`  or 使用项目下面的gradle `./gradlew build`
使用`./gradlew` 会在项目目录的`.gradle/`下载一个gradle, 并且在`gradle/wrapper/gradle-wrapper.properties`中设置了下载的gradle的地址
4. `./gradlew tasks` 查看所有gradle的task
5. `build.gradle` 文件中把`compile('org.springframework.boot:spring-boot-starter')` 改为 `compile('org.springframework.boot:spring-boot-starter-web')`  
`spring-boot-starter-web` 里面包含了 `spring-boot-starter`
6. 项目的xxxApplication.java文件是文件入口  
```
@SpringBootApplication
public class CatteryApplication {

	public static void main(String[] args) {
		SpringApplication.run(CatteryApplication.class, args);
	}
}
```












