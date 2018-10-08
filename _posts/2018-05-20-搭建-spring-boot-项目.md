---
layout:     post
title:      搭建spring-boot项目
subtitle:   
date:       2018-05-20
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - spring-boot
    - java
    - gradle

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
@SpringBootApplication //= @Configuration + @ComponentScan + @EnableAutoConfiguration
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

### 连接数据库 postgreSql
1. 使用psql数据库，`build.gradle`文件加入`runtime('org.postgresql:postgresql')`
2. 使用阿里提供的 https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter 数据库连接池  
`build.gradle`文件加入`compile('com.alibaba:druid-spring-boot-starter:1.1.10')` 
[java各种数据库连接池对比](https://dongjx.github.io/2018/04/02/java%E5%90%84%E7%A7%8D%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5%E6%B1%A0%E5%AF%B9%E6%AF%94/)    
另外`build.gradle`文件在加入`compile("org.springframework.boot:spring-boot-starter-data-jpa")` 引入jpa    

3. `application.yml`中加入
```
spring:
  datasource:
    druid:
      url: jdbc:postgresql://127.0.0.1:5432/probation_production
      username: postgres
      password:
      driver-class-name: org.postgresql.Driver
```
4. `./gradlew bootRun`，访问 http://localhost:8080/druid/index.html 查看数据库监控 
5. 配置jpa和show sql log，`application.yml`如下
```
spring:
  datasource:
    druid:
      url: jdbc:postgresql://127.0.0.1:5432/probation_production
      username: postgres
      password:
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQL9Dialect
    database: postgresql
    properties:
      hibernate:
        ddl-auto: none
        temp:
          use_jdbc_metadata_defaults: false
logging:
  level:
    org:
      hibernate:
        type:
          descriptor:
            sql:
              BasicBinder: TRACE
        SQL: DEBUG
```
6. 创建实体
```
@Entity
@Table(name = "roles")
public class Role implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User [id=" + id + ", name=" + name + "]";
    }
}
```
7. 创建数据库操作接口
```
public interface RoleRepository extends CrudRepository<Role, Long> {
}
```

8. 创建对应的controller 

```  
package com.cattery.role;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/roles")
public class RoleController {
    @Autowired
    private RoleRepository roleRepository;

    @GetMapping
    public Object index() {
        Iterable<Role> roles = roleRepository.findAll();
        return roles;
    }

    @PutMapping
    public Object update(@RequestParam(name = "role") Role role) {
        roleRepository.save(role);
        return role;
    }

    @PostMapping
    public Object create(@RequestParam(name = "name") String name) {
        Role role = new Role();
        role.setName(name);
        roleRepository.save(role);
        return role;
    }

    @DeleteMapping
    public Object delete(@RequestParam(name = "id") Long id) {
        roleRepository.deleteById(id);
        return id;
    }
}
```  
9. `./gradlew build` & `./gradlew bootRun`, 访问 http://localhost:8080/roles

### 打包运行  

1. `./gradlew jar`  
根据`build.gradle`的配置（`group = 'com.java' version = '0.0.1-SNAPSHOT' sourceCompatibility = 1.8``）生成jar名    
2. `java -jar build/libs/xxx.jar`      


### 查看Druid监控 
1. `./gradlew bootRun`，访问 http://localhost:8080/druid/index.html 查看数据库监控 
2. 配置用户名，密码， 创建DruidConfig 

```
@SpringBootConfiguration
public class DruidConfig {

    @Bean
    public ServletRegistrationBean statViewServlet() {
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new StatViewServlet(),
                "/druid/*");
        servletRegistrationBean.addInitParameter("allow", "127.0.0.1");
        servletRegistrationBean.addInitParameter("deny", "192.168.0.1");
        servletRegistrationBean.addInitParameter("loginUsername", "admin");
        servletRegistrationBean.addInitParameter("loginPassword", "123456");
        servletRegistrationBean.addInitParameter("resetEnable", "false");
        return servletRegistrationBean;
    }

    @Bean
    public FilterRegistrationBean statFilter() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new WebStatFilter());
        filterRegistrationBean.addUrlPatterns("/*");
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return filterRegistrationBean;
    }

}
```
3. `./gradlew build` & `./gradlew bootRun`，访问http://localhost:8080/druid/index.html 输入用户名密码查看监控数据



**github  example repo**: [https://github.com/dongjx/cattery-spring-boot-jpa-druid](https://github.com/dongjx/cattery-spring-boot-jpa-druid) 

### 参考资料
[用Gradle构建Spring Boot项目](http://www.cnblogs.com/davenkin/p/gradle-spring-boot.html)
