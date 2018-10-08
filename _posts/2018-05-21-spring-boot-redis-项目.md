---
layout:     post
title:      spring-boot + redis
subtitle:   
date:       2018-05-21
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - spring-boot
    - redis

---

**github  example repo**: [https://github.com/dongjx/cattery-spring-boot-jpa-druid](https://github.com/dongjx/cattery-spring-boot-jpa-druid) 

在上一篇spring-boot的搭建基础上，加入redis  

---

## spring-boot 提供的cache功能    
1. 在`application.yml`里面加入CacheType, 默认是NONE   



```
spring:
  cache:
    type: redis
```

2. 在`XXXApplication.java`里开启缓存功能   


```
@SpringBootApplication
**@EnableCaching**
public class CatteryApplication {
	public static void main(String[] args) {
		SpringApplication.run(CatteryApplication.class, args);
	}
}

```

## spring-boot + redis

### spring-boot + 单机redis

1. `build.gradle`加入redis相关的插件   



```
compile('org.springframework.boot:spring-boot-starter-data-redis')
compile('redis.clients:jedis')
```

2. 在`resources`目录下加入`redis.properties`并设置对应的配置信息    


```
spring.redis.database=0
spring.redis.host=localhost
spring.redis.port=6379
spring.redis.password=
```
  
3. 在`configration`中建立`RedisConfig.java`并引入`redis.properties`     


```
@SpringBootConfiguration
@PropertySource("classpath:redis.properties")
public class RedisConfig {
}
```
  
4. 使用`RedisTemplate` 来操作redis    


```
//RedisTemplate 已经通过spring-boot-starter-data-redis的RedisAutoConfiguration自动注入了
ValueOperations<String, String> ops = redisTemplate.opsForValue();
ops.set("test1", "123");  //RedisTemplate操作redis时key会默认有前缀，所以这个例子redis里面的key是xxxtest1
String value = ops.get("test1");
```
  
5. 使用spring-boot的cachable, `@EnableCaching`时才能生效   


```
@Service
public class RoleService {
    @Autowired
    private RoleRepository roleRepository;

    @Cacheable(value = "roles")
    public List<Role> findRoles() {
        return StreamSupport.stream(roleRepository.findAll().spliterator(), false).collect(Collectors.toList());
    }
}
```

### spring-boot + 集群redis

1. `redis.properties` 加入 
`spring.redis.cluster.nodes=127.0.0.1:7000,127.0.0.1:7001,127.0.0.1:7002,127.0.0.1:7003,127.0.0.1:7004,127.0.0.1:7005`    

2. 创建`RedisClusterProperties.java`     


```
@Component
@PropertySource("classpath:redis.properties")
@ConfigurationProperties(prefix = "spring.redis.cluster")
public class RedisClusterProperties {
    List<String> nodes;

    public List<String> getNodes() {
        return nodes;
    }

    public void setNodes(List<String> nodes) {
        this.nodes = nodes;
    }
}
```

3. 修改`redisConfig.java`    


```
@SpringBootConfiguration
public class RedisConfig {
    @Autowired
    RedisClusterProperties clusterProperties;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory(new RedisClusterConfiguration(clusterProperties.getNodes()));
    }
}
```



