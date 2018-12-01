# spring boot

[25个经典的Spring面试问答](https://www.ctolib.com/topics-35589.html)


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






