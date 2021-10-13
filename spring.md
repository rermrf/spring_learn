## 介绍

> Spring是一个分层的(一站式) 轻量级开源框架 Spring的核心是控制反转（IoC）和面向切面（AOP）



#### ioc

> IoC -- Inverse of Control，控制反转，将对象的创建权反转给Spring！！ 使用IOC可以解决的程序耦合性高的问题！



#### 控制反转（DI:依赖注入）

>  假设我需要做一个功能，在这个功能当中我需要调用servic层，然后再调用dao层，去取数据。在传统的javaEE开发中我就直接去new一个service 然后再new一个dao。但是在spring框架中，我们吧new service和new dao的权利交个spring框架，假设我需要使用我就直接去spring框架中寻找。等于说我的资源创建的权利交给了spring框架，这就叫做控制反转。



#### 解耦

> 刚刚我们说资源创建交给了sring，我们需要什么就找spring。这过程就像是工厂模式。但是在spring框架中它需要创建哪些对象，它需要一个配置文件。这个配置文件告诉spring，需要创建哪些资源。

例如：假设我需要去数据库查询数据显示页面

> 程序启动，spring框架去找配置文件创建资源，把资源放置再一个容器中，开始运行，前端请求数据，在spring中找controller层，再找service层，再找dao层要数据，最后数据原路返回controller，再显示到页面上。其中service被spring注入到controlller层，dao层被spring注入到service层。这个过程分工明确。每一层各司其职。传统的一个开发，在servlet中直接new然后去查数据，然后数据返回到界面上。万一操作一多所有的判断，查询不同的表，这个servlet的代码变得十分的臃肿。不说开发慢，你开发完了看代码也费劲。            所以说控制反转可以用来解耦





1. 搭建Spring环境

开发spring至少需要使用的jar：

> spring-aop.jar		//开发AOP特性时需要的jar
>
> spring-beans.jar	//处理Bean的jar
>
> spring-context.jar	//处理spring上下文的jar
>
> spring-core.jar		//spring核心jar
>
> spring-expression.jar	//spring表达式

以及三方提供的日志jar

> commons-logging.jar	//日志





2. 编写配置文件

资源目录创建appliactioncontext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```





3. 开发spring程序(ioc)

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--  该文件中所产生的所有对象，被spring放入了一个 称之为 spring ioc容器的地方  -->
    <bean id="student" class="com.springdemo.demo.entity.Student">
        <property name="id" value="2"></property>
        <property name="name" value="ls"></property>
        <property name="age" value="20"></property>
        <property name="address" value="guangdong"></property>
    </bean>

</beans>
```

Test.java

```java
package com.springdemo.demo.test;

import com.springdemo.demo.entity.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        //以往创建对象的方式
        Student student = new Student(1,"zs",20,"jiangxi");
        System.out.println(student.toString());

        //ioc创建对象的方式
        ApplicationContext context = new  ClassPathXmlApplicationContext("applicationContext.xml");     //获取spring上下文对象
        Student student1 = (Student) context.getBean("student");   //执行从springIOC容器中获取一个id为student的对象
        System.out.println(student1.toString());


    }
}

```

IOC(控制反转)也可以称之为DI(依赖注入)

> 控制反转:将 创建对象，属性赋值 的方式进行了反转，从new,setXXX() 反转为了 从springIOC容器getBean()
>
> 依赖注入: 将属性值注入给了属性， 将属性注入给了bean, 将bean注入给了ioc容器

 总结：ioc/di，无论要什么对象，都可以去spring ioc 容器中获取，而不需要自己操作(new/setXXX())

> 
>
> IOC容器赋值:如果是简单类型(8个基本+String)用value，如果是复杂类型用ref="需要引用的id值"





常见的依赖注入方式：

1. set方式的依赖注入：

   ```xml
   <bean id="student" class="com.springdemo.demo.entity.Student">
       <property name="id" value="2"></property>
       <property name="name" value="ls"></property>
       <property name="age" value="20"></property>
       <property name="address" value="guangdong"></property>
   </bean>
   ```

赋值，默认使用的是set方法()；

依赖注入底层是通过反射实现



2.构造器注入(首先得有构造方法):

```xml
<bean id="student" class="com.springdemo.demo.entity.Student">
    <constructor-arg value="3" index="0"></constructor-arg>
    <constructor-arg value="ww" name="name"></constructor-arg>
    <constructor-arg value="21"></constructor-arg>
    <constructor-arg value="jiangxi"></constructor-arg>
</bean>
```

> 构造器注入的顺序需和构造方法一致，如不一致时可通过index=”参数下标“(从0开始)，name="形参名"或type(”数据类型“)(参数不得重复时可使用)进行指定



3.p命名空间注入

> 引入p命名空间
>
> xmlns:p="http://www.springframework.org/schema/p"

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"	p命名空间
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean id="student" class="com.springdemo.demo.entity.Student" p:id="4" p:name="zl" p:age="23" p:address="广西"></bean>

</beans>
```

> p命名空间注入如果是简单类型则直接写属性名=”属性值“ ，若是复杂类型则写字段名-ref="beanID"

> 
>
> 注意
>
> 注入的属性值默认为字符串，所以在注入时没有指定type或者name时，优先赋值给字符串类型的属性







#### 各种集合类的注入方式

1.set方式注入

set、list、数组   各自都有自己的标签<set> <list> <array>,但是也可以混着用。

```xml
 <bean class="com.springdemo.demo.entity.AllCollectionType" id="allCollectionType">
        <!--        1.set方式注入-->
        <property name="listElement">
            <list>
                <value>足球</value>
                <value>篮球</value>
                <value>羽毛球</value>
                <value>乒乓球</value>
            </list>
        </property>

        <property name="arrayElement">
            <array>
                <value>唱</value>
                <value>跳</value>
                <value>rap</value>
                <value>篮球</value>
            </array>
        </property>

        <property name="setElement">
            <set>
                <value>java</value>
                <value>c语言</value>
                <value>html</value>
            </set>
        </property>

        <property name="mapElement">
            <map>
                <entry>
                    <key><value>foot</value></key>
                    <value>足球</value>
                </entry>
                <entry>
                    <key><value>basket</value></key>
                    <value>篮球</value>
                </entry>
            </map>
        </property>
        <property name="propertiesElement">
            <props>
                <prop key="name">wen</prop>
                <prop key="zs">张三</prop>
                <prop key="ls">李四</prop>
            </props>
        </property>
    </bean>
```

```java
public static void collectionDemo(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        AllCollectionType type = (AllCollectionType) context.getBean("allCollectionType");
        System.out.println(type.toString());
    }
```





#### 特殊符号的处理

大于号小于号使用\&lt;  	\&rt;

```xml
<property name="name" value="l&lt;s"></property>
<property name="name"><value>l&lt;s</value></property>
```

或者使用value标签才有的	\<![CDATA[特殊字符]]>

```xml
<property name="name"><value>ls<![CDATA[特殊字符]]></value></property>
```





**处理空值**

使用	\<null/>标签

```xml
<property name="name">
	<null/>
</property>
```







#### 自动装配（只对ref复杂类型有效）

**autowire="byName"**	：	Course类中有一个ref属性teacher(属性名)，并且该ioc容器中恰好有一个bean的id也是teacher

其他bean的id值=类的属性名 就会自动注入到类的属性中,不需要再通过set，构造方法或者p空间去赋值



**autowire="byType"**	：	其他bean的类型(class) 是否与 该Course的ref属性类型一致，注意此种方式 必须满足：当前Ioc容器中 只能有一个Bean满足条件

**autowire="byConstructor"**	：	其他bean的类型(class) 是否与 该Course的构造方法参数的类型一致，此种方法的本质就是Type方法

```xml
<bean id="teacher" class="com.springdemo.demo.entity.Teacher" p:name="zl" p:age="23">
</bean>

<bean id="course" class="com.springdemo.demo.entity.Course"  autowire="byName">
        <constructor-arg value="c语言"></constructor-arg>
        <constructor-arg value="200"></constructor-arg>
        <!--<constructor-arg ref="teacher"></constructor-arg>-->
</bean>
```





一次性设置所有的bean自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"
        default-autowire="byName"
       >
</beans>
```

标签内的属性 可以 覆盖 全局的自动装配

自动装配虽然可以减低代码量，但是会降低可读性







**Spring 中常用的注解如下。**

**1）@Component**

可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。

**2）@Repository**

用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**3）@Service**

通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**4）@Controller**

通常作用在控制层（如 Struts2 的 Action、SpringMVC 的 Controller），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**5）@Autowired**

可以应用到 Bean 的属性变量、属性的 setter 方法、非 setter 方法及构造函数等，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配。

**6）@Resource**

作用与 Autowired 相同，区别在于 @Autowired 默认按照 Bean 类型装配，而 @Resource 默认按照 Bean 实例名称进行装配。

**@Resource 中有两个重要属性：name 和 type**。

Spring 将 name 属性解析为 Bean 的实例名称，type 属性解析为 Bean 的实例类型。如果指定 name 属性，则按实例名称进行装配；如果指定 type 属性，则按 Bean 类型进行装配。如果都不指定，则先按 Bean 实例名称装配，如果不能匹配，则再按照 Bean 类型进行装配；如果都无法匹配，则抛出 NoSuchBeanDefinitionException 异常。

**7）@Qualifier**

与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定。





#### 注解定义Bean

通过注解的形式，将Bean以及相应的属性值放入ioc容器



1.配置扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--  配置扫描器  -->
    <context:component-scan base-package="com.demo2.wen.Dao"></context:component-scan>

</beans>
```

\<context:component-scan>	Spring在启动的时候，会根据 base-package 在该包中扫描所有类，查找这些类是否有注解 @Component("***")，如果有，则将该类加入Spring Ioc容器

```java
package com.demo2.wen.Dao;

import com.demo2.wen.entity.Student;
import org.springframework.stereotype.Component;

@Component("StudentDao")    //<bean id="StudentDao" class="com.demo2.wen.Dao.StudentDaoImpl"></bean>
public class StudentDaoImpl {

    public void addStudent(Student student){
        System.out.println("新增学生....");
    }
}
```

@Component细化：

dao层注解：**@Repository**

service层注解：**@Service**

控制层注解：**@Controller**




注解定义Bean例子：
Dao：
```java
package com.example.demo.Dao.impl;

import com.example.demo.Dao.UserDao;
import org.springframework.stereotype.Repository;

@Repository("UserDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void addUser() {
        System.out.println("新增用户...");
    }

    @Override
    public void updateUser() {
        System.out.println("修改用户...");
    }
}
```

Service：
```java
package com.example.demo.Service.impl;

import com.example.demo.Dao.UserDao;
import com.example.demo.Service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service("UserService")
public class UserServiceImpl implements UserService {

    private UserDao userDao;

    @Autowired
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void addUser() {
        userDao.addUser();
    }

    @Override
    public void updateUser() {
        userDao.updateUser();
    }
}
```

test:
```java
package com.example.demo.Test;

import com.example.demo.Service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void TestService(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        UserService service = (UserService) context.getBean("UserService");
        service.addUser();
    }

    public static void main(String[] args) {
        TestService();
    }
}
```





#### 注解注入参数属性

1.字段注入 （不推荐使用字段注入）：
```java
@Autowired
private UserDao userDao;
```

字段注入可能会导致：
1). 对象的外部可见性

2). 可能导致循环依赖

3). 无法设置注入的对象为final，也无法注入静态变量


2.构造器注入
```java
 @Autowired
public UserServiceImpl(UserDao userDao) {
    this.userDao = userDao;
}
```

当依赖的对象很多时，需要严格按照构造器的顺序去填写依赖的对象，这将导致代码可读性和可维护性变得很差。这时候可以引入Setter方法进行注入，Setter方法和构造器注入很像，不过Setter更具有可读性。



3.set注入

```java
@Autowired
@Qualifier("UserDao")   //默认使用ByType,想要使用ByName时加上@Qualifier()注解并写bean的id值
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```








#### 使用注解实现事务(声明式事务) 

通过事务实现一些方法要么全成功，要么全失败，事务一般在service层写

**1.导入jar包：**
spring事务jar:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
```

数据库jar：
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.25</version>
</dependency>
```

commons-dbcp连接池使用的数据源jar：
```xml
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>
```

连接池jar:
```xml
<dependency>
    <groupId>commons-pool</groupId>
    <artifactId>commons-pool</artifactId>
    <version>1.5.4</version>
</dependency>
```

spring-jdbc jar:
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
```

aop jar：
```xml
 <dependency>
    <groupId>aopalliance</groupId>
    <artifactId>aopalliance</artifactId>
    <version>1.0</version>
</dependency>
```

**2.配置**
jdbc/mybatis/spring

配置对事务的支持：
(1.)增加事务tx的命名空间
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd"
>
</beans>
```

(2.)配置spring处理数据库的信息:
```xml
<!--   1. 配置数据库相关 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/user"></property>
    <property name="username" value="root"></property>
    <property name="password" value="wen1106."></property>
    <!-- 连接池的最大连接时间：-->
    <property name="maxActive" value="10"></property>
    <!--连接池最大空闲时间-->
    <property name="maxIdle" value="6"></property>
</bean>

<!--   2. 配置事务管理器-->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--   3. 增加对事务的支持（主要）-->
<tx:annotation-driven transaction-manager="txManager"/>
```

**3.使用**

将需要 成为事物的方法前 增加注解

```java
@Transactional()
```

@Transactional的属性：

1).readOnly	：	是否只读

2).





例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd"
>
  
    <!--   1. 配置数据库相关 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/user"></property>
        <property name="username" value="root"></property>
        <property name="password" value="wen1106."></property>
        <!-- 连接池的最大连接时间：-->
        <property name="maxActive" value="10"></property>
        <!--连接池最大空闲时间-->
        <property name="maxIdle" value="6"></property>
    </bean>

    <!--   2. 配置事务管理器-->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--   3. 增加对事务的支持-->
    <tx:annotation-driven transaction-manager="txManager"/>

</beans>
```

service

```java
@Transactional(readOnly = false,propagation = Propagation.REQUIRED,rollbackFor = {SQLException.class,ArithmeticException.class})    //事务注解：readOnly是否只读
    @Override
    public void addUser() {
        userDao.addUser();
    }
```











## aop

#### **什么是AOP?**

AOP是Aspect Oriented Programming的缩写，意思是：面向切面编程，它是通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

可以认为AOP是对OOP(Object Oriented Programming 面向对象编程)的补充，主要使用在日志记录，性能统计，安全控制等场景，使用AOP可以使得业务逻辑各部分之间的耦合度降低，只专注于各自的业务逻辑实现，从而提高程序的可读性及维护性。

比如，我们需要记录项目中所有对外接口的入参和出参，以便出现问题时定位原因，在每一个对外接口的代码中添加代码记录入参和出参当然也可以达到目的，但是这种硬编码的方式非常不友好，也不够灵活，而且记录日志本身和接口要实现的核心功能没有任何关系。

此时，我们可以将记录日志的功能定义到1个切面中，然后通过声明的方式定义要在何时何地使用这个切面，而不用修改任何1个外部接口。

在讲解具体的实现方式之前，我们先了解几个AOP中的术语。





#### 常用术语

**1 通知(Advice)**

在AOP术语中，切面要完成的工作被称为通知，通知定义了切面是什么以及何时使用。

Spring切面有5种类型的通知，分别是：

- **前置通知(Before**)：在目标方法被调用之前调用通知功能

- **后置通知(After)**：在目标方法完成之后调用通知，此时不关心方法的输出结果是什么

- **返回通知(After-returning)**：在目标方法成功执行之后调用通知

- **异常通知(After-throwing)**：在目标方法抛出异常后调用通知

- **环绕通知(Around)**：通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为

- **最终通知(After FinallyAdvice)**：在目标方法执行完毕后。插入的通知(不论是正常返回还是异常退出)

  

**2 连接点(Join point)**

连接点是在应用执行过程中能够插入切面的一个点，这个点可以是调用方法时、抛出异常时、修改某个字段时。



**3 切点(Pointcut)**

切点是为了缩小切面所通知的连接点的范围，即切面在何处执行。我们通常使用明确的类和方法名称，或者利用正则表达式定义所匹配的类和方法名称来指定切点。



**4 切面(Aspect)**

切面是通知和切点的结合。通知和切点共同定义了切面的全部内容：它是什么，在何时和何处完成其功能。



**5 引入(Introduction)**

引入允许我们在不修改现有类的基础上，向现有类添加新方法或属性。



**6 织入(Weaving)**

织入是把切面应用到目标对象并创建新的代理对象的过程。

切面在指定的连接点被织入到目标对象中，在目标对象的生命周期里，有以下几个点可以进行织入：

- 编译期：切面在目标类编译时被织入。这种方式需要特殊的编译器。AspectJ的织入编译器就是以这种方式织入切面的。
- 类加载期：切面在目标类加载到JVM时被织入。这种方式需要特殊的类加载器(ClassLoader)，它可以在目标类被引入应用之前增强该目标类的字节码。
- 运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。Spring AOP就是以这种方式织入切面的。







#### 定义切面：

1.jar包：

spring-aop.jar



2.配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

</beans>
```



#### 定义通知：

**实现接口的方式定义通知：**

<table>
    <tr>
    	<td>通知类型</td>
        <td>需要实现的接口</td>
        <td>接口中的方法</td>
        <td>执行时机</td>
    </tr>
    <tr>
    	<td>前置通知</td>
        <td>org.springframework.aop.MethodBeforeAdvice</td>
        <td>before()</td>
        <td>目标方法执行前</td>
    </tr>
    <tr>
    	<td>后置通知</td>
        <td>org.springframework.aop.AfterReturningAdvice</td>
        <td>afterReturning()</td>
        <td>目标方法执行后</td>
    </tr>
    <tr>
    	<td>异常通知</td>
        <td>org.springframework.aop.ThrowsAdvice</td>
        <td>无</td>
        <td>目标方法发生异常时</td>
    </tr>
    <tr>
    	<td>环绕通知</td>
        <td>org.aopalliance.intercept.MethodInterceptor</td>
        <td>invoke()</td>
        <td>目标方法调用之前和之后</td>
    </tr>
</table>



**一 通过spring api实现**

①添加前置后置方法



```dart
public class AopBeforeConfigLog implements MethodBeforeAdvice {

    /**
     * method : 要执行的目标对象的方法
     * args : 被调用的方法的参数
     * target : 目标对象
     */
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass()+"===="+method.getName()+"被执行了");
    }
}
```



```dart
public class AopAfterConfigLog implements AfterReturningAdvice {

    /**
     * method : 要执行的目标对象的方法
     * args : 被调用的方法的参数
     * result : 返回值
     * target : 被调用的目标对象
     */

    @Override
    public void afterReturning(Object result, Method method, Object[] args, Object target){
        System.out.println(target.getClass()+"===="+method.getName()+"被执行了"+"===返回值"+result);
    }
}
```

②beans.xml



```xml
    <!--注册bean-->
    <bean id="userService" class="service.UserServiceImpl"/>
    <bean id="afterLog" class="aoptest.AopAfterConfigLog"/>
    <bean id="beforeAop" class="aoptest.AopBeforeConfigLog"/>

    <aop:config>
        <!--切入点  expression:表达式匹配要执行的方法-->
        <aop:pointcut id="cut" expression="execution(* service.UserServiceImpl.*(..))"/>

        <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
        <aop:advisor advice-ref="afterLog" pointcut-ref="cut"/>
        <aop:advisor advice-ref="beforeAop" pointcut-ref="cut"/>
    </aop:config>
```

③测试类：



```cpp
public class TestAop {
    public static void main(String[] args) {
        ApplicationContext Context = new ClassPathXmlApplicationContext("ContextAplication.xml");
        UserService userService = (UserService) Context.getBean("userService");
        userService.add();
        userService.selete();
        userService.update();
        userService.delete();
    }
}
```

④总结：

- 切面：横切的点
- 通知：切面完成的工作，其实就是一个方法
- 目标：被通知的对象
- expression="execution(* service.UserServiceImpl.*(..))" 这是一套固定的公式 *代表所有 这句话的意思就是service.UserServiceImpl下的所有方法都被切面了！



**二 自定义类来实现**

①自定义被切入类



```csharp
/*
* 自定义类
* */
public class MyDIYAopCut {
    public void before(){
        System.out.println("方法执行前前前前前前前前前前");
    }
    public void after(){
        System.out.println("方法执行后后后后后后后后后后");
    }
}
```

②beans.xml



```xml
    <!--注册bean-->
    <bean id="userService" class="service.UserServiceImpl"/>
    <bean id="Mydiycut" class="diyaop.MyDIYAopCut"/>

    <aop:config>
        <!--这里的ref指定被 切入 的类是哪一个-->
        <aop:aspect ref="Mydiycut">
            <!--切入点-->
            <aop:pointcut id="cut" expression="execution(* service.UserServiceImpl.*(..))"/>

            <!--切入点之前执行,这里的方法名即是我们自定义类中的方法名-->
            <aop:before method="before" pointcut-ref="cut"/>
            <!--切入点之后执行,这里的方法名即是我们自定义类中的方法名-->
            <aop:before method="after" pointcut-ref="cut"/>
        </aop:aspect>
    </aop:config>
```

③总结：

- 测试类的方法即是xml中method的方法名

- 其他见xml中的注释！





**三 注解实现**

①自定义增强注解实现类

```java
@Component("ServiceAspect")  //定义是一个Bean
@Aspect //@Aspect注解表明ServiceAspect类是一个切面
public class ServiceAspect {

    //定义切点/切入点，该方法无方法体,主要为方便同类中其他方法使用此处配置的切入点
    @Pointcut("execution(public void com.example.demo.Service.impl.UserServiceImpl.updateUser()))")
    public void aspect(){
    }

    //@Before(value = "execution(public void com.example.demo.Service.impl.UserServiceImpl.addUser())")
    @Before("aspect()") //配置前置通知,使用在方法aspect()上注册的切入点
    public void before(){
        System.out.println("前置通知...");
    }

    @After("aspect()")
    public void after(){
        System.out.println("后置通知...");
    }
}

```



②xml

```xml
   <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"
>
    <!-- 配置扫描器 -->
    <context:component-scan base-package="com.example.demo"></context:component-scan>

    <!--声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面-->
    <aop:aspectj-autoproxy proxy-target-class="false"/>
</beans>
```

③java

```java
@Service("UserService")     //注解定义Bean
public class UserServiceImpl implements UserService {

    @Value("#{UserDao}")    //属性注入
    private UserDao userDao;

    @Autowired
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void updateUser() {
        userDao.updateUser();
    }
}
```



④总结：

- proxy-target-class="false" false 是以jdk实现动态代理 true 是以CGLib实现动态代理 这玩意我们一般使用默认机制，了解，知道有这么个玩意即可！
- @Aspect：把当前类标识为一个切面供容器读取
- @Before：方法执行前
- @After：方法执行后
- @Around：环绕式执行 ProceedingJoinPoint：环绕通知=前置+目标方法执行+后置通知。proceed方法就是用于启动目标方法执行的 就是必须得使用proceed(),相当于中间点，这样程序才能知道哪个是before哪个是after！

