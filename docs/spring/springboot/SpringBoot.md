# spring boot

[25个经典的Spring面试问答](https://www.ctolib.com/topics-35589.html)

## [Spring Boot Starter](http://www.voidcn.com/article/p-riakvctb-boa.html)
#### 常见的starter会包几个方面的内容？分别是什么？
```
常见的starter会包括下面四个方面的内容
- 自动配置文件，根据classpath是否存在指定的类来决定是否要执行该功能的自动配置。
- spring.factories，非常重要，指导Spring Boot找到指定的自动配置文件。
- endpoint：可以理解为一个admin，包含对服务的描述、界面、交互(业务信息的查询)。
- health indicator:该starter提供的服务的健康指标。

两个需要注意的点：

  1. @ConditionalOnMissingBean的作用是：只有对应的bean在系统中都没有被创建，它修饰的初始化代码块才会执行，【用户自己手动创建的bean优先】。

 2. Spring Boot Starter找到自动配置文件(xxxxAutoConfiguration之类的文件)的方式有两种：

 spring.factories:由Spring Boot触发探测classpath目录下的类，进行自动配置；

 @EnableXxxxx:有时需要由starter的用户触发*查找自动配置文件的过程
```

#### Spring Boot Starter的工作原理
1. Spring Boot 在启动时扫描项目所依赖的JAR包，寻找包含spring.factories文件的JAR   2. 根据spring.factories配置加载AutoConfigure类   
3. 根据 @Conditional注解的条件，进行自动配置并将Bean注入Spring Context    

## SpringBoot到底是怎么做到自动配置的？
[如何做到自动配置](https://juejin.im/post/5b679fbc5188251aad213110) 

@SpringbootApplication注解：    
- @Configuration  
- @EnableAutoConfiguration   
- @ComponentScan   

最终会用到 spring.factories文件

```
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}


////////////////////////////////////////////
// @EnableAutoConfiguration可以帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IoC容器。该配置模块的主要使用到了SpringFactoriesLoader
////////////////////////////////////////////

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration //相当于 @Configuration：用于定义 @Bean
@EnableAutoConfiguration //自动配置：重点
@ComponentScan(          //组件扫描
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    //省略....
}


@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    //省略...
}
```






