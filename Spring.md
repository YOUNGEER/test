# Spring

## Spring利用现有的技术，让开发更加简单

- 轻量级,非侵入式的框架

- Ioc控制反转/DI依赖注入：

  - 内容：对象和属性都由Spring容器来创建，传统的对象是由程序来创建的。

  - 反转：程序本身不去创建对象，而变成被动的接收对象。

  - ioc容器：BeanFactory

  - 1.传统写法是new出对象，改进版本是set对象，可以随便变动，解耦

    ```java
        PersonDao person=new PersonDaoImpl();
    
        public void setPerson(Person person) {
            this.person = person;
        }
    	//	通过set的方式，可以随便变动继承类对象，程序变成了被动的接受对象，这就是控制反转的思想
    ```

  - 2.beans容器管理

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
    
        <!--以下写法类似new出了一个对象，stuent对象名，给属性为name的变量赋值为Spring-->
        <bean id="student" class="com.wy.lucky.pojo.Student">
            <property name="name" value="Spring"/>
        </bean>
    		<!--这里的name类似别名，可以取多个-->
        <bean id="student2" class="com.wy.lucky.pojo.Student" name="student3,student4">
            <property name="name" value="Spring2"/>
        </bean>
      <!--import可以导入其他的beans文件-->
        <import resource="XXX.xml"/>
      
    </beans>
    ```

  - 3.对象调用

    ```java
         ApplicationContext applicationContext=new ClassPathXmlApplicationContext("beans.xml");
            Student student= (Student) applicationContext.getBean("student");
            System.out.println(student.toString());
    
    
            Student student2= (Student) applicationContext.getBean("student2");
            System.out.println(student2.toString());
    
    ```

  - 4.bean对象的作用域

    

    ```xml
        <!--以下写法类似new出了一个对象，stuent对象名，给属性为name的变量赋值为Spring-->
        <bean id="student" class="com.wy.lucky.pojo.Student" scope="singleton">
            <property name="name" value="Spring"/>
        </bean>
    
        <bean id="student2" class="com.wy.lucky.pojo.Student" name="student3,student4" scope="prototype">
            <property name="name" value="Spring2"/>
        </bean>
    ```

  默认是单例，即整个bean容器内只会有一个对象，scope="prototype"表示，每次都创建一个新的对象，如下：

  ```java
     ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");
          Student student = (Student) applicationContext.getBean("student");
          System.out.println(student.toString());
          Student student3 = (Student) applicationContext.getBean("student");
  
          Student student2 = (Student) applicationContext.getBean("student2");
          Student student4 = (Student) applicationContext.getBean("student2");
          System.out.println(student2.toString());
  
          System.out.println(student == student2);//false
          System.out.println(student == student3);//true
          System.out.println(student2 == student4);//false
  
  ```

- Bean自动装配

  - 自动装配是Spring满足bean依赖的一种方式

  - Spring会在上下文中自动寻找 ，并自动给bean装配属性

  - Spring中有三种自动装配方式

     - 在xml中配置

     - 在java中配置,在类的属性和直接使用即可，连set方法都省略了，前提是这个自动装配的属性在IOC容器内

       - @autowired//byType
       - @qualified(value=)
       - @resource(name=) //byName，找不到名字再通过byType找

     - 隐式的自动装配bean

       - byName，通过set方法后面的名字
       - byType，通过类型来查找，此时可能会类型冲突，这时候就需要指定唯一名字

       - ```xml
         autowire="byName/byType"
         ```

- Java注解自动装配

  - @Component：会将该类自动注册到bean容器中,下面三个注解都是相同的意思

    - @Resposity

    - @Service

    - @Controller

  - @Scope：设置作用域

- 完全不用xml配置，用javaConfig

  - @Configuration ：类注解，一个项目应该只有一个，其他可以通过@import导入，类似beans.xml导入其他xml
  - @Bean ：方法注解，类型bean容器中的bean

## Aop面向切面编程

### 代理模式

- 特点：代理类和实际业务类实现同一个接口（或继承同一父类），代理对象持有一个实际对象的引用，外部调用时操作的是代理对象，而在代理对象的内部实现中又会去调用实际对象的操作

- 静态代理
  - 角色分析
    - 抽象类
    - 真实角色，实现抽象类
    - 代理角色，实现抽象类，调用真实角色的方法，可以可以扩展自己的方法
    - 客户端，调用代理角色调用方法
  - 优点：
    - 可以使真实角色更加纯粹，可以不用关心一些公共的业务
    - 公共的交给代理，实现了业务的分工
    - 公共业务发生扩展的时候，方便集中管理
  - 缺点
    - 一个真实角色需要一个代理对象
- 动态代理
  - 优点：
  - 一个动态代理类对应的是一个接口，一般就是对应的一类业务
    - 一个动态代理类可以代理多个类，只要是实现了同一个接口即可！
  
- 对事务的支持
- 对框架的支持