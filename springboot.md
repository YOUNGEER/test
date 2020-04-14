# springboot

## 是什么

- 整合了所有的框架，相对于springmvc来说基本是零配置，遵循约定大于配置思想
- 核心是自动装配
- 优点
  - 开箱即用，提供各种默认配置来简化项目配置
  - 内嵌式容器简化web项目
  - 没有冗余代码生成和XML配置的要求

## 如何编写yml

### 语法

- Key-value键值对的形式,注意，冒号后面一定要加一个空格

- 可以编写对象，数组，set等形式的数据，比如

  ```xml
  person:
    name: wy
    age: 3
    happ:true
    birth: 2019-10-1
    maps: {k1:v1,k2:v2}
    list:
      - code
      - music
      - girl
    dogs:
      name: hha
      age: 3
  ```

- yml可以给java对象赋值

  ```java
  @ConfigurationProperties(prefix = "person")
  public class Person {
      private String name;
      private Integer age;
      private Boolean happy;
      private Date birth;
      private HashMap maps;
      private List list;
      private Dog dog;
      
      class Dog{
          private String name;
          private Integer age;
      }
  }
  ```

  

## 自动装配原理

- Pom.xml

  	- spring-boot-dependencies配置了核心依赖
  	- 我们在引入一些依赖的时候不需要指定版本，就是因为有了这些版本仓库

- 启动器

  ```xml
  <dependency>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

  	-  说白了，启动器就是springboot的启动场景
  	-  比如，spring-boot-starter-web，表示会自动导入web项目所需要的依赖
  	-  springboot会将所有的功能场景，拆分成一个个启动器
  	-  我们需要什么功能，只需要找到对应的启动器即可

- 注解

  ```java
  @SpringBootApplication =>标注该类是一个springboot的应用
  		@SpringBootConfiguration =>springboot的配置
  				@Configuration =>spring配置类
  						@Component =>说明着也是spring的一个组件
  
  		@EnableAutoConfiguration =>自动配置
  				@AutoConfigurationPackage =>自动导配置包
  						@Import(AutoConfigurationPackages.Registrar.class) =>自动配置包注册
  				@Import(AutoConfigurationImportSelector.class) =>自动配置导入选择器
  						//获取所有候选的配置
  						List<String> configurations = getCandidateConfigurations(...);
  
  		@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class), =>扫描当前主启动同级的包
  		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
  	
  ```

   - 获取所有候选的配置

     ```java
     protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
     		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
     				getBeanClassLoader());
     		Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
     				+ "are using a custom packaging, make sure that file is correct.");
     		return configurations;
     	}
     ```

       - META-INF/spring,核心配置文件

         ![image-20191225160553053](/Users/wangyang/Library/Application Support/typora-user-images/image-20191225160553053.png)

         ![image-20191225161818015](/Users/wangyang/Library/Application Support/typora-user-images/image-20191225161818015.png)

         

     ```xml
     @ConditionalOnClass:spring.factories配置的包，通过该注解判断是否生效
     ```

     **结论**：springboot所有自动配置都是在启动的时候扫描并加载，spring.factories所有的自动配置类都是在这里，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有了对应的启动器，有了启动器，我们的自动装配就会生效，然后就配置成功！

     **步骤**

     1.springboot在启动的时候，从类路径下/META-INF/spring.factories获取指定的值

     2.将这些自动配置的类导入容器，自动配置就会生效，帮我们进行自动配置！

     3.以前我们需要自动配置的东西，现在springboot帮我们做了！

     4.整合javaee，解决方案和自动配置的东西都在spring-boot-autoconfigure-2.2.0RELEASE.jar这个包下

     5.它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器

     6.容器中也会存在非常多的xxxAutoConfiguration的文件，这些类给容器中导入了这个场景需要的所有组件，并自动配置

     7.有了自动配置类，免去了我们手动编写配置文件的工作！

### run方法



## 集成web开发

## 集成数据库druid



## 分布式：dubbo+zookeeper

## swagger

## 任务调度

## SpringSecurity/Shiro

## linux







