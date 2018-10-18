---
layout:     post
title:      spring-boot debug
subtitle:   
date:       2018-05-20
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - spring-boot
    - debug
    - maven
    - gradle
---
以git来说，对比下现在讨论较多的git-flow 和 truncked-base的异同
帮助选择最合适自己team的开发流程

---

### 项目debug
1. `./gradlew bootRun --debug-jvm` or `mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005"`启动debug，默认监听端口5005   
2. ideal打开项目，`edit configrations` 添加一个`remote`的configration
![img](https://dongjx.github.io/img/posts/java-debug.png)
3. 启动remote的debug模式
