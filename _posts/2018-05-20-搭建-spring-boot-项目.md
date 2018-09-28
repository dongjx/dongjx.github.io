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

### 搭建spring-boot项目基本框架
1. 通过[Spring Initializr](https://start.spring.io/)来生成，dependencies选择`web`
`spring-boot-starter-web` 里面包含了 `spring-boot-starter`
2. 自动生成的resources里面默认使用application.properties，这个文件是spring boot工程的主要配置文件，支持application.yml和application.properties两种形式，喜欢yml的请把文件修好成application.yml
3. `build.gradle`文件中，`apply plugin: 'eclipse'`改成`apply plugin: 'idea'`
4. `./gradlew idea`生成IntelliJ IDEA工程文件
5. `gradle build`  or 使用项目下面的gradle `./gradlew build`
使用`./gradlew` 会在项目目录的`.gradle/`下载一个gradle, 并且在`gradle/wrapper/gradle-wrapper.properties`中设置了下载的gradle的地址
6. `./gradlew tasks` 查看所有gradle的task
7. 项目的xxxApplication.java文件是项目入口  
```
@SpringBootApplication
public class CatteryApplication {

	public static void main(String[] args) {
		SpringApplication.run(CatteryApplication.class, args);
	}
}
```

**此刻项目目录**
```
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── src
    ├── main
    │   └── java
    │       └── com.cattery
    │           └── CatteryApplication.java
    └── test
        └── java
```

### 实现一个controller
1. `com.cattery`目录下创建home目录，在home目录下创建`HomeController.java`
```
@RestController("/home")
public class HomeController {

    @GetMapping
    public String index() {
        return "hello";
    }
}
```

**此刻项目目录**
```
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
└── src
    ├── main
    │   └── java
    │       └── com.cattery
    |           ├── home
    |                 └── HomeController.java
    │           └── CatteryApplication.java
    └── test
        └── java
```
2. `./gradlew bootRun` 
3. 访问 `http://localhost:8080/home`

### 项目debug
1. `./gradlew bootRun --debug-jvm` 启动debug，默认监听端口5005
2. ideal打开项目，`edit configrations` 添加一个`remote`的configration
![img](https://dongjx.github.io/img/posts/java-debug.png)
3. 启动remote的debug模式

### 参考资料
[用Gradle构建Spring Boot项目](http://www.cnblogs.com/davenkin/p/gradle-spring-boot.html)













