---
layout:     post
title:      java & spring各种注解
date:       2017-12-29
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - java
    - spring
    - annotation
    
---
作为一名ruby转java的程序员，首先就是懵在各种java&spring注解上...

---

## java注解
### 运行机制分类
1. 源码注解：注解只在源码中存在，编译成.class文件就不存在了   
2. 编译时注解：注解在源码和.class文件中都存在。（例如：JDK的三个注解）  
3. 运行时注解：在运行阶段还起作用，甚至会影响运行逻辑的注解   

### 来源分类
1. 来自JDK的注解：三个@Override  @Deprecated  @SuppressWarning  
2. 来自第三方的注解：比如Spring框架、Mybatis框架的注解等等  
3. 我们自己定义的注解  

### JDK自带注解
1. @Override 表示当前方法覆盖了父类的方法  
2. @Deprecation 表示方法已经过时,方法上有横线，使用时会有警告。  
3. @SuppviseWarnings 表示关闭一些警告信息(通知java编译器忽略特定的编译警告)  

### 自定义注解 - 元注解
java.lang.annotation提供了四种元注解，专门注解其他的注解（在自定义注解的时候，需要使用到元注解）：  
1. @Retention – 定义该注解的生命周期, 什么时候使用该注解  
- RetentionPolicy.SOURCE : 在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解。  
- RetentionPolicy.CLASS : 在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式  
- RetentionPolicy.RUNTIME : 始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式。  
2. Target – 表示该注解用于什么地方。默认值为任何元素，表示该注解用于什么地方。可用的ElementType参数包括  
- ElementType.CONSTRUCTOR:用于描述构造器  
- ElementType.FIELD:成员变量、对象、属性（包括enum实例）  
- ElementType.LOCAL_VARIABLE:用于描述局部变量  
- ElementType.PACKAGE:用于描述包  
- ElementType.PARAMETER:用于描述参数  
- ElementType.TYPE:用于描述类、接口(包括注解类型) 或enum声明  
3. @Documented – 一个简单的Annotations标记注解，表示是否将注解信息添加在java文档中  
4. @Inherited – 定义该注释和子类的关系, 是否允许子类继承该注解  
@Inherited元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。  

### 自定义注解类编写规则
1. Annotation型定义为@interface, 所有的Annotation会自动继承java.lang.Annotation这一接口,并且不能再去继承别的类或是接口.  
2. 参数成员只能用public或默认(default)这两个访问权修饰  
3. 参数成员只能用基本类型byte,short,char,int,long,float,double,boolean八种基本数据类型和String、Enum、Class、annotations等数据类型,以及这一些类型的数组.  
4. 要获取类方法和字段的注解信息，必须通过Java的反射技术来获取 Annotation对象,因为你除此之外没有别的获取注解对象的方法  
5. 注解也可以没有定义成员, 不过这样注解就没啥用了   
**PS:自定义注解需要使用到元注解**   

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {
    String value() default "";
}
```   

## spring常用注解
### 声明bean注解
1. @Component - 注解在类上，可以作用在任何层次, 泛化的概念  
2. @Service - 注解在类上，用于标注业务层组件  
3. @Controller - 注解在类上，用于标注控制层组件  
4. @Repository - 注解在类上，用于标注数据访问组件，即DAO组件
**@Component、@Repository、@Service、@Controller实质上属于同一类注解，用法相同，功能相同，区别在于标识组件的类型**  
```
@Service(value="SecUserService")
public class SecUsersServiceImpl implements SecUsersService {
    @Autowired
    private SecUsersDao secUsersDao;
    @Autowired
    private SecRoleUserService secRoleUserService;
    public void doBusiness(){
        //do some business
    }
 }
```
5. @Bean - 注解在方法上, 声明当前方法的返回值为一个Bean   

```
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
```  

6. @Primary - 自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常, **与@Component or @Bean 一起使用**   
7. @Scope - 注解在类或方法上，描述的是Spring容器如何新建Bean的实例的，**通常与@Component or @Bean 一起使用**  
- Singleton，一个spring容器中只要一个Bean的实例，此为Spring的默认配置，全容器共享一个实例  
- Prototype，每次调用新建一个Bean实例  
- Request，Web项目中，给每一个http request新建一个Bean实例   
- Session，Web项目中，给每一个http session新建一个Bean实例   
- GlobalSession，这个只在portal应用中有用，给每一个global http session 新建一个Bean实例   
  
```
@Controller("demo")
@Scope("Prototype")
public class demo {
 
}
```
  
### 注入Bean注解
1. @Autowired - 可以对成员变量、方法、构造函数、类，进行注释, 默认按类型装配
注解在set方法上或属性上
如果我们想使用按名称装配，可以结合@Qualifier注解一起使用。   
  
```
@Autowired @Qualifier("staff")
private People people;
```

```
@Autowired
public PeopleService(@Qualifier("staff") People people) {
 ...
}
```
  
2. @Resource - 来自于java EE规范的一个annotation, 有一个name属性, 在默认情况下，spring将这个值解释为需要被注入的Bean实例的名字   

### 配置注解
1. @Configuration - 注解在类上，声明当前类是一个配置类，相当于一个Spring配置的xml文件  
把一个类作为一个IoC容器 和@Bean搭配使用，某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean  
2. @ComponentScan - 注解在类上，自动扫描包名下所有使用@Service、@Compent、@Repository、@Controller的类注册为Bean  
3. @Lazy(true) - 表示延迟初始化  

### 资源调用
1. @Value - 注解在变量上, 调用资源（普通文件，网址，配置文件，系统环境变量等）
- 默认值   
```
@Value("true")
private boolean defaultBoolean;
```    
- Spring Environment Property  
```
@Value("${APP_NAME_NOT_FOUND}")
private String defaultAppName;
```    
- 系统环境变量  
```
@Value("${java.home}")
private String javaHome;
```  
- java home system property  
```
@Value("#{systemProperties['java.home']}")
private String javaHome;
```  
- 注释在方法上   
```
@Value("Test")
public void printValues(String s, String v){} //both 's' and 'v' values will be 'Test' 

@Value("Test")
public void printValues(String s, @Value("Data") String v){}
// s=Test, v=Data
```   

2. @PropertySource - 注解在类上, 目的是加载指定的属性文件   
```
@PropertySource("classpath:conf/config.properties")
public class SystemConfig {
  ...
}
```    
```
@PropertySources({@PropertySource("classpath:conf/config.properties"), @PropertySource("classpath:conf/config1.properties")})
public class SystemConfig {
  ...
}
```   

3. @Profile - 作用在类、方法上，在不同情况下选择激活不懂的config。

```
@Profile("Development") //指定profile为Development
@Configuration
public class DevDatabaseConfig implements DatabaseConfig {
 
    @Override
    @Bean
    public DataSource createDataSource() {
        System.out.println("Creating DEV database");
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        /*
         * Set MySQL specific properties for Development Environment
         */
        return dataSource;
    }
 
} 
```  
```
@ActiveProfiles("Development") //激活Development profile
public class TestActiveProfile {

    @Autowired
    private HelloService hs;
    
    @Test
    public void testProfile() throws Exception {
        String value = hs.sayHello();
        System.out.println(value);
    }
}
```   
4. `@ImportResource("classpath:ws-client.xml")` - 加载xml配置   

### Bean的初始化和销毁
1. @PostConstruct - 注解在方法上,在构造函数执行后执行   
2. @PreDestroy- 注解在方法上,在Bean销毁之前执行   
```
@Component
public class Chinese
{
...
  @PostConstruct
  public void init()
  {
    System.out.println("正在执行初始化的init方法...");
  }
  
  @PreDestroy
  public void close()
  {
    System.out.println("正在执行销毁之前的close方法...");
  }
}
```
3. @DependsOn - 注解在方法上,定义Bean初始化及销毁时的顺序  
```
@Configuration
public class AppConfig {

   @Bean("beanOne")
   @DependsOn(value = { "beanTwo", "beanThree" }) //beanTwo, beanThree初始化后才初始化beanOne
   public BeanOne getBeanOne() {
      return new BeanOne();
   }

   @Bean("beanTwo")
   public BeanTwo getBeanTwo() {
      return new BeanTwo();
   }

   @Bean("beanThree")
   public BeanThree getBeanThree() {
      return new BeanThree();
   }
}
```  

### spring AOP注解
1. @Aspect - 注解在类上, 声明是一个切面  
2. @After - 注解在方法上, 在目标方法返回或抛出异常后调用    
3. @PointCut - 注解在方法上, 定义拦截规则，声明切点  
4. @AfterReturning - 注解在方法上, 通常方法会在目标方法返回后调用    
5. @AfterThrowing	- 注解在方法上,通知方法会在目标方法抛出异常后调用  
6. @Around - 注解在方法上, 通知方法将目标方法封装起来  
7. @Before - 注解在方法上, 通知方法会在目标方法执行之前执行   
8. @EnableAspectJAutoProxy - JavaConfig类上使用注解@EnableAspectJAutoProxy注解启动自动代理功能，@Aspect才能生效  

```
@Aspect
public class Audience {

    /**
     * 定义一个公共的切点
     */
    @Pointcut("execution(** com.spring.aop.service.Perfomance.perform(..))")
    public void performance() {
    }

    /**
     * 目标方法执行之前调用
     */
    @Before("performance()")
    public void silenceCellPhone() {
        System.out.println("Silencing cell phones");
    }

    /**
     * 目标方法执行之前调用
     */
    @Before("performance()")
    public void takeSeats() {
        System.out.println("Taking seats");
    }

    /**
     * 目标方法执行完后调用
     */
    @AfterReturning("performance()")
    public void applause() {
        System.out.println("CLAP CLAP CLAP");
    }

    /**
     * 目标方法发生异常时调用
     */
    @AfterThrowing("performance()")
    public void demandRefund() {
        System.out.println("Demanding a refund");
    }
    
    /**
    * 环绕通知
    * @param jp 通过它调用目标方法
    */
    @Around("perforance()")
    public void watchPerformance(ProceedingJoinPoint jp) {
        try {
            System.out.println("Silencing cell phones");
            System.out.println("Taking seats");
            jp.proceed();
            System.out.println("CLAP CLAP CLAP!!!");
        } catch (Throwable e) {
            System.out.println("Demanding a refund");
        }
    }
}
```

### spring事务注解
1. @Transactional - 可以作用于接口、接口方法、类以及类方法上  
当作用于类上时，该类的所有 public 方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义。   
- propagation 
PROPAGATION_REQUIRED	如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是最常见的选择  
PROPAGATION_SUPPORTS	支持当前事务，如果当前没有事务，就以非事务方式执行  
PROPAGATION_MANDATORY	使用当前的事务，如果当前没有事务，就抛出异常  
PROPAGATION_REQUIRES_NEW	新建事务，如果当前存在事务，把当前事务挂起  
PROPAGATION_NOT_SUPPORTED	以非事务方式执行操作，如果当前存在事务，就把当前事务挂起  
PROPAGATION_NEVER	以非事务方式执行，如果当前存在事务，则抛出异常  
PROPAGATION_NESTED	如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类 似的操作  
- readOnly - 事务的读写属性，取true或者false，true为只读、默认为false  
- rollbackFor - 回滚策略，当遇到指定异常时回滚。譬如上例遇到异常就回滚  
- timeout -  （补充的） 设置超时时间，单位为秒  
- isolation - 设置事务隔离级别，枚举类型，一共五种  
DEFAULT	采用数据库默认隔离级别  
READ_UNCOMMITTED	读未提交的数据（会出现脏读取） 
READ_COMMITTED	读已提交的数据（会出现幻读，即前后两次读的不一样）  
REPEATABLE_READ	可重复读，会出现幻读  
SERIALIZABLE	串行化（对资源消耗较大，一般不使用）  

```
@Service
public class CompanyServiceImpl implements CompanyService {
  @Autowired
  private CompanyDAO companyDAO;

  @Transactional(propagation = Propagation.REQUIRED, readOnly = false, rollbackFor = Exception.class)
  public int deleteByName(String name) {

    int result = companyDAO.deleteByName(name);
    return company;
  }
  ...
}
```

### spring mvc注解
1. @RequestMapping - 类，方法上，用来映射Web请求(访问路径和参数)、处理类和方法的   
- @RequestMapping 既可以作用在类级别，也可以作用在方法级别。当它定义在类级别时，标明该控制器处理所有的请求都被映射到 /favsoft 路径下  
- @RequestMapping中可以使用 method 属性标记其所接受的方法类型，如果不指定方法类型的话，可以使用 HTTP GET/POST 方法请求数据，但是一旦指定方法类型，就只能使用该类型获取数据    
- @GetMapping, @PostMapping, @DeleteMapping, @PutMapping  
2. @ResponseBody - 类，返回值前，方法上, 将返回类型直接输入到HTTP response body中, 输出JSON格式的数据  
3. @RequestBody - 参数前, request的参数在请求体内  
```
@RequestMapping(value = "/something", method = RequestMethod.PUT)
public void handle(@RequestBody String body, Writer writer) throws IOException {
    writer.write(body);
}
```  
4. @PathVariable - 参数前,接收路径参数   
5. @RestController - 类上，组合注解 = @Controller+@ResponseBody   
6. @RequestParam - 参数中，将请求的参数绑定到方法中的参数上，如下面的代码所示。 
其实，即使不配置该参数，注解也会默认使用该参数。如果想自定义指定参数的话，如果将@RequestParam的 required 属性设置为false（@RequestParam（value="id",required=false））     

### spring-boot 注解
1. @SpringBootApplication - SpringBoot的核心注解，主要作用是开启自动配置  
@SpringBootApplication=@ComponentScan+@Configuration+@EnableAutoConfiguration   
关闭特定的自动配置：@SpringBootApplication注解的exclude参数。  
例如：`@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class} )`  


