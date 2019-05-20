# SpringBoot-Note
SpringBoot 学习笔记 V1.0
# 一、Spring Boot 入门

## 1、Spring Boot 简介

> 简化 Spring 应用开发的一个框架；
>
> 整个 Spring 技术栈的一个大整合；
>
> J2EE开发的一站式解决方案；

## 2、微服务

2014，Martin Flwler

微服务：架构风格

一个应用应该是一组小型服务；可以通过 HTTP 的方式进行互通；每个功能元素应该独立出来；每一个功能元素都是一个可独立替换的和独立升级的软件单元；部署和运维的挑战

单体应用：ALL IN ONE；不相涉多个应用之间的互联互调，开发/测试/部署/扩展简单；但是牵一发而动全身；日益增长的软件需求

必须掌握以下内容：

- Spring 框架的使用经验
- 熟练使用 Maven 进行项目构建和依赖管理
- 熟练使用 Eclipse 或者 IDEA

## 3、环境约束

- JDK1.8
- Maven 3.x
- IntelliJ IDEA 2017
- Spring Boot 1.5.9.RELEASE

## 4、Spring Boot HelloWorld

一个功能：

浏览器发生 hello 请求，服务器接受请求并处理，响应 Hello World 字符串；

###  1、创建一个maven工程（jar）

> IDEA
>
> create New Poject
>
> 进入 Maven
>
> 配置 Project SDK：JDK1.8 的安装路径
>
> 一路 Next 配置下去，直到 Finish
>
> 选择 Enable Auto-Import



### 2、导入 Spring Boot 相关的依赖

> 进入 [SpringBoot官网](<https://projects.spring.io/spring-boot> )
>
> 找到 [maven例子](<https://docs.spring.io/spring-boot/docs/1.5.20.RELEASE/reference/htmlsingle/#using-boot-maven> )并且复制如下代码到 pom.xml
>
> ```xml
>     <parent>
>         <groupId>org.springframework.boot</groupId>
>         <artifactId>spring-boot-starter-parent</artifactId>
>         <version>1.5.20.RELEASE</version>
>     </parent>
> 
>     <!-- Add typical dependencies for a web application -->
>     <dependencies>
>         <dependency>
>             <groupId>org.springframework.boot</groupId>
>             <artifactId>spring-boot-starter-web</artifactId>
>         </dependency>
>     </dependencies>
> ```
>
> 

### 3、编写一个主程序：启动 Spring Boot 应用

```java
package com.zrl.gu;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * Created by 16114 on 2019/5/7.
 */
//@SpringBootApplication来标注一个主程序类，说明这是一个 SPringBoot 应用
@SpringBootApplication
public class HelloWorldMainApplication {
    public static void main(String[] args) {
        //Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```



### 4、编写相关的业务逻辑 Controller、Service等

```java
package com.zrl.gu.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * Created by 16114 on 2019/5/7.
 */

@Controller
public class HelloController {

    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "Hello World";
    }
}
```

### 5、运行主程序测试

### 6、简化部署

>进入[maven例子](<https://docs.spring.io/spring-boot/docs/1.5.20.RELEASE/reference/htmlsingle/#using-boot-maven> )导入 Maven 插件
>
>```xml
><build>
>    <plugins>
>        <plugin>
>            <groupId>org.springframework.boot</groupId>
>            <artifactId>spring-boot-maven-plugin</artifactId>
>        </plugin>
>    </plugins>
></build>
>```
>在右侧边栏调出 Maven Projects
>在 target 目录 找到对应 jar 包
>复制出来此 jar 包
>在 jar 包所在路径 cmd 运行 [ java -jar jar包名 ]
>

##  5、Hello World 探究

### 1、POM文件

#### 1）父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.20.RELEASE</version>
</parent>
```

SpringBoot 的版本仲裁中心；

以后我们导入依赖是不需要写版本；（没有在 dependencies 里面管理的依赖自然需要声明版本号）

#### 2）导入的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

**spring-boot-starter**-web：

spring-boot-starter 是 SpringBoot 场景启动器，帮我们导入了 web 模块正常运行所依赖的组件

SpringBoot 将所有的功能场景都抽取出来，做成一个个的 starters（启动器），只需要在项目里面引用这些 starter 相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景启动器

**主程序类、主入口类**

```java
//@SpringBootApplication来标注一个主程序类，说明这是一个 SPringBoot 应用
@SpringBootApplication
public class HelloWorldMainApplication extends SpringBootServletInitializer{
    public static void main(String[] args) {
        //Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }

    //继承SpringBootServletInitializer ，重写configure
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application){
        return application.sources(HelloWorldMainApplication.class);
    }
}
```

**@SpringBootApplication**：SpringBoot 应用标注在某个类上说明这个类是 SpringBoot 的主配置类，SpringBppt

就应该运行这个类的 main 方法来启动这个 SpringBoot 应用

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {        @Filter(
            type = FilterType.CUSTOM,
            classes = {TypeExcludeFilter.class}
        ),         @Filter(
            type = FilterType.CUSTOM,
            classes = {AutoConfigurationExcludeFilter.class}
        )}
)
public @interface SpringBootApplication {
```

**@SpringBootConfiguration**：SpringBoot 的配置类；

* 标注在某个类上，标识这是一个 SpringBoot 的配置类；
* **@Configuration**：配置类上来标注这个注解；
* 配置类——配置文件；配置类也是容器中的一个组件；@Component

**@EnableAutoConfiguration**：开启自动配置功能；

以前我们需要配置的东西，SpringBoot 帮我们自动配置，@EnableAutoConfiguration 告诉 SpringBoot 开启自动配置功能，这样自动配置才能生效

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

**@AutoConfigurationPackage**：自动配置包

* @Import({Registrar.class})：Spring 的底层注解，给容器中导入一个组件

<u>将主配置类（@SpringBootApplication 标注的类）的所在包及下面的所有子包里面的所有组件都扫描到 Spring 容器</u>

**@Import({EnableAutoConfigurationImportSelector.class})**：给容器中导入组件

* EnableAutoConfigurationImportSelector：导入哪些组件的选择器
* 将所有需要导入的组件以全类名的方式返回，这些组件就会被添加到容器中；
* 会给容器中导入非常多的自动配置类（xxxAutoConfiguration）：就是给容器中导入这个场景需要的所有组件，并配置好这些组件；
* 有了自动配置类，免去了我们手动编写配置主入功能组件等的工作

**SpringBoot 在启动的时候从类路径下 META-INF/spring-factories 中获取 EnableAutoConfiguration 指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；我们需要自己配置的，自动配置类都做了**

J2EE 的整体整合解决方案和自动配置都在 [spring-boot-01-helloworld-1.0-SNAPSHOT.jar](E:\maven\apache-maven-3.0.5-bin\apache-maven-3.0.5\repository\org\springframework\boot\spring-boot-autoconfigure\1.5.20.RELEASE\spring-boot-autoconfigure-1.5.20.RELEASE.jar!\org\springframework\boot\autoconfigure)

## 6、使用 Spring Initializer 快速创建 Spring Boot 项目

IDE 都支持使用 Spring 的项目创建向导创建一个  Spring Boot 项目；

选择我们需要的模块；向导会**联网**创建 Spring Boot 项目；

默认生成得 Spring Boot 项目：

* 主程序已经生成好了，我们只需要我们自己得逻辑
* resources 文件夹中目录结构
  * static ：保存所有静态资源（图片/JS/CSS）
  * templates：保存所有得模板页面（Spring Boot 默认 jar 包使用嵌入式得 Tomcat，默认不支持 JSP 页面）可以使用模板引擎（freemarker、thymeleaf）
  * application.properties：Spring Boot 应用得配置文件；可以修改一些默认设置，比如server.port=8081



# 二、配置文件

## 1、配置文件

Spring Boot 使用一个全局得配置文件，配置文件名字是固定得

* applicatio.properties
* applicatio.yml

配置文件得作用：修改 Spring Boot 自动配置的默认值；Spring Boot 在底层都给我们自动配置好

.yml 是 YAML 语言的文件，以数据为中心，比 json、 xml 更适合做配置文件

YAML（YAML Ain't Markup Language）

​	YAML A Markup Language：是一个标记语言

​	YAML  isn't Markup Lanuage：不是一个标记语言

标记语言： 

​	以前的配置文件，大多使用的是 xxx.xml 文件

​	YAML：以数据为中心，比 json、 xml 更适合做配置文件

YAML:

```yaml
server:
  port: 8081
```

XML:

```xml
<server>
    <port>8081</port>
</server>
```



## 2、YAML 语法

### 1）基本语法

k:(空格)  v:标识一对键值对（空格必须有）

以空格的缩进来控制层级关系，只要是左对齐的一列数据，都统一层级的

属性和值大小写敏感



### 2）值的写法

#### 字面量：普通的值（数字、字符串、布尔）:

​	k:v 字面量直接来写

​		字符串默认不用加上单引号或者双引号

​		""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​			name:"zahngsan\blisi" 输出：zhangsan 换行 lisi

​		''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​			name:"zhangsan \n lisi" 输出：

zhangsan  \n lisi

#### 对象、Map（属性和值）（键值对）:

​		k:v 	：在下一行来写对象的属性和值的关系；注意缩进

​			对象还是k:v的方式；

```yam
friends:
	lastName:zhangsan
	age:20
```

行内写法：

```yaml
firends:{lastName: zhangsan,age: 18}
```

#### 数组（List、Set）

用-值表示数组中的一个元素

```yaml
pets:
	-cat
	-dog
	-pig
```

行内写法：

```yaml
pets: [cat,dog,pig]
```

### 3）获取配置文件值注入

```yaml
server:
  port: 8081

person:
  lastName: zhangsan
  age: 18
  boss: false
  birth: 2017/12/12
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - zhaoliu
  dog:
    name: 小狗
    age: 2
```

javaBean:

```java
package com.example.demo.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.Date;
import java.util.List;
import java.util.Map;

/**
 * Created by 16114 on 2019/5/8.
 */

/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * ConfigurationProperties告诉springboot将本类中的所有属性和配置文件中相关的配置进行绑定
 * (prefix = "person"):配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件才能容器提供的功能
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
 	private Dog dog;
    @Override
    public String toString() {
        return "Person{" +
                "lastName='" + lastName + '\'' +
                ", age=" + age +
                ", boss=" + boss +
                ", birth=" + birth +
                ", maps=" + maps +
                ", lists=" + lists +
                ", dog=" + dog +
                '}';
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Boolean getBoss() {
        return boss;
    }

    public void setBoss(Boolean boss) {
        this.boss = boss;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }

    public Map<String, Object> getMaps() {
        return maps;
    }

    public void setMaps(Map<String, Object> maps) {
        this.maps = maps;
    }

    public List<Object> getLists() {
        return lists;
    }

    public void setLists(List<Object> lists) {
        this.lists = lists;
    }

  
}
```

我们可以导入配置文件处理器，以后编写配置就有提示了

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
   <optional>true</optional>
</dependency>
```

|                                          | @ConfigurationProperties | @Value     |
| ---------------------------------------- | ------------------------ | ---------- |
| 功能                                     | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法last-name<->lastName） | 支持                     | 不支持     |
| SpEL（eg: #{11*2}  表达式求值）          | 不支持                   | 支持       |
| JSR303数据校验 （如下所示）              | 支持                     | 不支持     |
| 复杂类型封装                             | 支持                     | 不支持     |

```java
@Validated
...
@Email
private String lastName;
```

配置文件 yml 还是 properties 都能获取到值；

如果说，只是在某个业务逻辑中需要获取一下配置文件中的某项值——>@Value

如果说，我们专门编写了一个 JavaBean 来个配置文件进行映射——>@ConfigurationProperties

#### 3、配置文件注入值数据校验

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    /**
     * <bean class="Person">
     *     <property name="lastName value="字面量/ ${环境变量、配置文件中获取值}/ #{SpEl}{}"></>
     * </bean>
     */
   // @Value("${person.last-name}")

    //lastName必须填上邮箱
    @Email
    private String lastName;
   // @Value("#{11*2}")
    private Integer age;
   //@Value("true")
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

#### 4、@PropertySource & @ImportResource

@PropertySource：加载指定配置文件

```java
@PropertySource(value = "classpath:person.properties")
@Component
@ConfigurationProperties(prefix = "person")
//@Validated
public class Person {
    /**
     * <bean class="Person">
     *     <property name="lastName value="字面量/ ${环境变量、配置文件中获取值}/ #{SpEl}{}"></>
     * </bean>
     */
   // @Value("${person.last-name}")

    //lastName必须填上邮箱
    //@Email
    private String lastName;
   // @Value("#{11*2}")
    private Integer age;
   //@Value("true")
    private Boolean boss;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
```

**@ImportResource**：导入 Spring 的配置文件，让配置文件里面的内容生效

Spring Boot 里面没有 Spring 的配置文件，我们自己编写的配置文件，也不能自动识别

想让 Spring 配置文件生效，加载进来；**@ImportResource**标注在一个配置类上

```java
@ImportResource(locations = {"classpath:beans.xml"})
//导入 Spring 的配置文件，让其生效
```

不来编写 Spring 的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="helloService" class="com.example.demo.service.HelloService"></bean>
    
</beans>
```

Spring Boot 推荐给容器中添加组件的方式：推荐使用全注解的方式

1、配置类<——>Spring 配置文件

2、使用 @Bean 给容器中添加组件

```java
/**
 * @Configuration 指出当前类是一个配置类，替代之前的 Spring 配置文件
 * 以前在配置文件加<bean><bean/>标签添加组件
 */
@Configuration
public class MyAppConfig {

    //将方法的返回值添加到容器中，容器中这个组件的默认 id 就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("配置类@Bean给容器中添加组件了");
        return new HelloService();
    }
}
```

## 4、配置文件占位符

### 1、随机数

```properties
${random.uuid}
${random.int}
${random.value}
${random.long}
${random.int(10)}
${random.int[1024,65536]}
```

### 2、占位符获取之前配置的值，如果没有可以用:指定默认值

```properties
person.last-name=张三${random.uuid}
person.age=${random.int}
person.birth=2019/05/06
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=${person.hello:hello}_dog
person.dog.age=2
```

## 5、Profile

### 1）多 profile 文件

我们在主配置文件编写的时候，文件名可以实 application-{profile}.properties/yml

默认使用 application.properties 的配置

### 2）yml 支持多文档块方式

```yaml
server:
  port: 8081
spring:
  profiles:
    active: prod
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
    profiles: prod
```



### 3）激活指定 profile

1、在配置文件  application.properties 中指定激活 

```properties
spring.profiles.active=dev  
```

2、配置参数：在 edit configuration ——> programmer arguments

```properties
--spring.profiles.active=dev
```

3、命令行：直接在测试的时候 maven pro 的 package 后 打包 去 target 下 show in explod 运行 cmd 如下命令

```properties
java -jar demo-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
```

4、JVM参数：

```properties
-Dspring.profiles.active=dev
```



## 6、配置文件加载位置

-file../config

-file../

-classpath:/config/

-classpath:/

优先级由高到低，高优先级的配置会覆盖低优先级的配置

Spring Boot 会从这四个位置全部加载主配置文件：**互补配置**

## 7、外部配置加载顺序

* **命令行参数**

```properties
使用指令：
java -jar xxxxx.jar --server.port=8082
多个参数时用空格隔开,如：
java -jar coco-0.0.1-SNAPSHOT.jar --server.port=8082 --server.servlet./context-path=/coco
```

* 来自java:comp/env的JNDI属性
* 使用“spring.config.location”改变默认的配置文件位置
* Java系统属性（System.getProperties()）
* 操作系统环境变量
* RandomValuePropertySource配置的random.*属性值

优先加载带 profile 的

* **jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**
* **jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

再来加载不带 profile 的

* **jar包外部的application.properties或application.yml(不带spring.profile)配置文件**
* **jar包内部的application.properties或application.yml(不带spring.profile)配置文件**
* 通过SpringApplication.setDefaultProperties指定的默认属性

## 8、自动配置原理

配置文件到底能写什么？怎么写？自动配置原理：

[配置文件能配置的属性参照](<https://docs.spring.io/spring-boot/docs/2.1.4.RELEASE/reference/htmlsingle/#common-application-properties> )

**自动配置原理：**

1）Spring Boot 启动的时候加载主配置类，开启了自动配置功能**@EnableAutoConfiguration**

2）@EnableAutoConfiguration作用：

* 利用AutoConfigurationImportSelector给容器中导入一些组件
* 可以查看 selectImports() 方法的内容

```java
@Override
public String[] selectImports(AnnotationMetadata annotationMetadata) {
   if (!isEnabled(annotationMetadata)) {
      return NO_IMPORTS;
   }
   AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
         .loadMetadata(this.beanClassLoader);
   AutoConfigurationEntry autoConfigurationEntry = getAutoConfigurationEntry(
         autoConfigurationMetadata, annotationMetadata);
   return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
} 
```

```java
/**
	 * Return the {@link AutoConfigurationEntry} based on the {@link AnnotationMetadata}
	 * of the importing {@link Configuration @Configuration} class.
	 * @param autoConfigurationMetadata the auto-configuration metadata
	 * @param annotationMetadata the annotation metadata of the configuration class
	 * @return the auto-configurations that should be imported
	 */
	protected AutoConfigurationEntry getAutoConfigurationEntry(
			AutoConfigurationMetadata autoConfigurationMetadata,
			AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return EMPTY_ENTRY;
		}
		AnnotationAttributes attributes = getAttributes(annotationMetadata);
		List<String> configurations = getCandidateConfigurations(annotationMetadata,
				attributes);
		configurations = removeDuplicates(configurations);
		Set<String> exclusions = getExclusions(annotationMetadata, attributes);
		checkExcludedClasses(configurations, exclusions);
		configurations.removeAll(exclusions);
		configurations = filter(configurations, autoConfigurationMetadata);
		fireAutoConfigurationImportEvents(configurations, exclusions);
		return new AutoConfigurationEntry(configurations, exclusions);
	}
```

* List<String> configurations = getCandidateConfigurations(annotationMetadata,attributes);

获取候选的配置

```java
SpringFactoriesLoader.loadFactoryNames
扫描所有 jar 包类路径下 "META-INF/spring.factories"
然后把扫描到的这些文件的内容包装成 properties 对象
然后从 properties 获取到 EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加到容器中
```

**将类路径下 META-INF/spring.factories 里面配置的所有 EnableAutoConfiguration 的值加入到了容器中**

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudServiceConnectorsAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.rest.RestClientAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
org.springframework.boot.autoconfigure.reactor.core.ReactorCoreAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityRequestMatcherProviderAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration

```

每一个这样的 xxxAutoConfiguration 都是容器中的一个组件，都加入到容器中，用他们来做自动配置

3）每一个自动配置类进行自动配置功能

4）以 **HttpEncodingAutoConfiguration** 为例解释自动配置原理：

```java
@Configuration	//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties(HttpProperties.class)	//启用指定类的 ConfigurationProperties 功能，将配置文件中对应的值和 HttpProperties 绑定起来了
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET) //spring 底层@Conditional注解（Spring 注解版），根据不同的条件，如果满足指定的条件，整个配置就会生效，判断当前应用是否是 web 应用，如果是，当前配置生效
@ConditionalOnClass(CharacterEncodingFilter.class) //判断当前项目有没有这个类
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled",
		matchIfMissing = true) //判断配置文件中是否存在某个配置，如果不存在，判断也是成立的
public class HttpEncodingAutoConfiguration {
```

根据当前不同的条件判断，决定这个配置类是否生效？

一旦这个配置类生效，这个配置类就会给容器中添加各种组件，这些组件的属性是从对应的 properties 类中获取的，这些类里面的每一个属性又是和配置文件绑定的

5）所有在配置文件中能被配置的属性都是在 xxxxProperties 类中封装着，配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "spring.http")	//从配置文件中获取指定的值和 bean 的属性进行绑定
public class HttpProperties {

```



精髓：

* SpringBoot启动会加载大量的自动配置类
* 看我们需要的功能有没有SpringBoot默认写好的自动配置类
* 再来看这个自动配置类中到底配置了哪些组件（只要我们要用的组件有，我们就不需要配了，如果没有需要自己写）
* 给容器中自动配置类添加组件的时候，会从 properties 获取某些属性，我们就可以在配置文件中指定这些属性的值

xxxAutoConfiguration：自动配置类，给容器中添加组件

xxxProperties：封装配置文件中相关属性

## 细节

**1、@Conditional派生注解（Spring注解版原生的@Conditional作用）** 
作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![img](https://img-blog.csdnimg.cn/20181220003410913.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p6aHVhbl8x,size_16,color_FFFFFF,t_70) 

自动配置类必须在一定的条件下生效：

我们怎么知道哪些自动配置类生效？

我们可以通过 debug=true 属性；来让控制台打印自动配置报告 CONDITIONS EVALUATION REPORT ，这样就知道哪些自动配置类生效了

```java
Positive matches:	//自动配置类启用的
-----------------

   CodecsAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.http.codec.CodecConfigurer' (OnClassCondition)

   CodecsAutoConfiguration.JacksonCodecConfiguration matched:
      - @ConditionalOnClass found required class 'com.fasterxml.jackson.databind.ObjectMapper' (OnClassCondition)
          
          ...
          
Negative matches:	//自动配置类没启用、没匹配成功的
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'javax.jms.ConnectionFactory' (OnClassCondition)

   AopAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'org.aspectj.lang.annotation.Aspect' (OnClassCondition)

```



# 三、日志

## 1、日志框架

张三：开发一个大型系统

​	1、System.out.println("") :将关键数据在控制台打印，老板想去掉？写在一个文件里？

​	2、框架记录系统的一些运行时信息，日志框架

​	3、高大上的功能？异步模式？自动归档？

​	4、将以前框架卸下来？换上新的框架。重新修改之前相关的API

​	5、JDBC-数据库驱动

​		写了一个统一的接口时：日志门面（日志的一个抽象层）

​		给项目导入日志实现即可，我们之前的日志框架都是实现日志抽象层

市面上的日志框架：

JUL、JCL、jboss-logging、logback、log4j、jog4j2、slf4j...

| 日志门面（日志的抽象层）          | 日志实现                        |
| --------------------------------- | ------------------------------- |
| ~~JCL~~、SLF4j、~~jboss-logging~~ | log4j、JUL、log4j2、**logback** |



左边选一个门面（抽象层）、右边来选一个实现

日志门面：SLF4j

日志实现：logback

SpringBoot：底层时 spring 框架，spring框架默认使用 JCL；

​	springboot选用SLF4j和logback

## 2、SLF4j使用

### 1、如何在系统中使用SLF4j

以后开发的时候，日志记录方法的调用，不应该直接调用日志的实现类，而是调用日志抽象层的方法；

给系统导入 slf4j 的 jar 包和 logback的实现 jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```





![click to enlarge](https://www.slf4j.org/images/concrete-bindings.png) 

​	每一个实现框架都有自己的配置文件，使用slf4j后，配置文件还是做成日志实现框架自己本身的配置文件

### 2、遗留问题

统一日志记录，即使是别的框架和我一起统一使用slf4j进行输出？





![click to enlarge](https://www.slf4j.org/images/legacy.png) 



**如何让系统中所有的日志都统一到slf4j**

1、将系统中其他日志框架先排除出去

2、用中间包来替换原有的日志框架

3、我们导入slf4j其他的实现

## 3、SpringBoot日志关系

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.1.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

总结：

1）SpringBoot也使用 slf4j + logback 的方式进行日志记录

2）SpringBoot也把其他日志都替换成了slf4j

3）中间替换包去 lib查看

4）如果我们要引入其他框架。一定要把这个框架的默认日志依赖移除掉

SpringBoot能自动适配所有的日志，而且底层使用 slf4j + logback 记录日志，引入 其他框架时，只要把这个框架依赖的日志框架排除掉

## 4、日志使用

### 1、默认配置

SpringBoot默认帮我们配置好了日志

```java
@SpringBootTest
public class SpringBoot03LoggingApplicationTests {

	//记录器
	Logger logger = LoggerFactory.getLogger(getClass());

	@Test
	public void contextLoads() {
		//日志级别由低到高 trace<debug<info<warn<error
		//可以调整输出的日志级别：日志只会在这个级别及以后的高级别生效
		logger.trace("这是trace日志...");
		logger.debug("这是debug日志...");
		//springboot默认使用info级别，只输出info及以后,没有指定级别的用默认级别的
		logger.info("这是info日志");
		logger.warn("这是warn日志...");
		logger.error("这是error日志");
	}

}


pro调整：
logging.level.com.atguigu=trace
```





```properties
logging.level.com.atguigu=trace
# 不指定路径 当前项目下生成spring.log日志
# 可以指定完整的路径
#logging.path=D:/springboot.log
#在当前磁盘的根路径下创建sprig文件夹和里面的log文件夹，然后这个文件夹使用spring.log 作为默认文件
logging.path=/spring/log

#在控制台输出的日志格式
logging.pattern.console=%d{yyyy-MM-dd} {%thread} %-5level %logger{50} - %msg%n

#指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === {%thread} === %-5level === %logger{50} === %msg%n
```



**日志输出格式**

%d ：日期时间

%thread：线程名

%-5level：级别从左显示5个字符宽度

%logger{50}：表示logger名字最长50个字符，否则按照句点分割

%msg：日志消息

%n：换行符



### 2、指定配置

去看官方文档。给类路径下放上每个日志框架自己的配置文件即可；SPringBoot就不适用默认配置了

logback.xml：直接被日志框架识别了

**logback.spring.xml**（推荐）：日志框架不直接加载日志的配置项，由sboot加载，某个配置只在某个环境下加载，比如测试环境输出什么什么，详见官方文档



## 5、切换日志框架

可以按照，SLF4J日志适配图，进行切换   

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180902092805350?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25hbmdlYWxp/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) slf4j+log4j

 ```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐web</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>logback‐classic</artifactId>
            <groupId>ch.qos.logback</groupId>
        </exclusion>
        <exclusion>
            <artifactId>log4j‐over‐slf4j</artifactId>
            <groupId>org.slf4j</groupId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j‐log4j12</artifactId>
</dependency>
 ```

### Spring boot日志

spring-boot-starter-log4j2 
支持log4j2

spring-boot-starter-logging 
支持slf4j+logback

默认，使用的logging 
切换为，log4j2

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐web</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>spring‐boot‐starter‐logging</artifactId>
            <groupId>org.springframework.boot</groupId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐log4j2</artifactId>
</dependency>
```



# 四、Web 开发

## 1、简介

使用 Spring Boot：

1、创建一个 Spring Boot 应用，选中我们需要的模块

2、Spring Boot 已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来

3、自己编写业务代码

**自动配置原理**：

这个场景 Spring Boot 帮我们配置了什么？能不能修改？能修改哪些配置？能不能扩展？

xxxAutoConfiguration：帮我们容器中自动配置组件

xxxProperties：配置类来封装配置文件的内容



## 2、Spring Boot 对静态资源的映射规则



```java
	@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
			CacheControl cacheControl = this.resourceProperties.getCache()
					.getCachecontrol().toHttpCacheControl();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(registry
						.addResourceHandler("/webjars/**")
						.addResourceLocations("classpath:/META-INF/resources/webjars/")
						.setCachePeriod(getSeconds(cachePeriod))
						.setCacheControl(cacheControl));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(
						registry.addResourceHandler(staticPathPattern)
								.addResourceLocations(getResourceLocations(
										this.resourceProperties.getStaticLocations()))
								.setCachePeriod(getSeconds(cachePeriod))
								.setCacheControl(cacheControl));
			}
		}

```

1）、所有 "/webjars/**" ，都去 "classpath:/META-INF/resources/webjars/" 找资源

​	webjars：以 jar 包的方式引入静态资源

<https://www.webjars.org/> 

localhost:8080/webjars/jquery/3.4.1/jquery.js

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.4.1</version>
</dependency>
```

在访问时只需写 webjars 下面资源的名称即可

2、/** ：访问当前项目的任何资源，（静态资源的文件夹）

```xml
classpath:/META-INF/resources/webjars/
classpath:/resources/
calsspath:/static/
classpath:/public/
/：当前项目的根路径
```

3、欢迎页：静态资源文件夹下的所有 index.html 页面：/**

localhost:8080/ 找 index 页面

```java
//配置欢迎页映射
final class WelcomePageHandlerMapping extends AbstractUrlHandlerMapping {

	private static final Log logger = LogFactory.getLog(WelcomePageHandlerMapping.class);

	private static final List<MediaType> MEDIA_TYPES_ALL = Collections
			.singletonList(MediaType.ALL);

	WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
			ApplicationContext applicationContext, Optional<Resource> welcomePage,
			String staticPathPattern) {
		if (welcomePage.isPresent() && "/**".equals(staticPathPattern)) {
			logger.info("Adding welcome page: " + welcomePage.get());
			setRootViewName("forward:index.html");
		}
		else if (welcomeTemplateExists(templateAvailabilityProviders,
				applicationContext)) {
			logger.info("Adding welcome page template: index");
			setRootViewName("index");
		}
	}

```



4、所有的 **/favicon.ico 都是在静态资源文件夹下找



```java
//配置喜欢的头标	
@Configuration
		@ConditionalOnProperty(value = "spring.mvc.favicon.enabled",
				matchIfMissing = true)
		public static class FaviconConfiguration implements ResourceLoaderAware {

			private final ResourceProperties resourceProperties;

			private ResourceLoader resourceLoader;

			public FaviconConfiguration(ResourceProperties resourceProperties) {
				this.resourceProperties = resourceProperties;
			}

			@Override
			public void setResourceLoader(ResourceLoader resourceLoader) {
				this.resourceLoader = resourceLoader;
			}

			@Bean
			public SimpleUrlHandlerMapping faviconHandlerMapping() {
				SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
				mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
				mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
						faviconRequestHandler()));
				return mapping;
			}

			@Bean
			public ResourceHttpRequestHandler faviconRequestHandler() {
				ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
				requestHandler.setLocations(resolveFaviconLocations());
				return requestHandler;
			}

			private List<Resource> resolveFaviconLocations() {
				String[] staticLocations = getResourceLocations(
						this.resourceProperties.getStaticLocations());
				List<Resource> locations = new ArrayList<>(staticLocations.length + 1);
				Arrays.stream(staticLocations).map(this.resourceLoader::getResource)
						.forEach(locations::add);
				locations.add(new ClassPathResource("/"));
				return Collections.unmodifiableList(locations);
			}

		}

	}

```



## 3、模板引擎

JSP、Velocity、Freemarker、**Thymeleaf**

Spring Boot 推荐的模板引擎**Thymeleaf**：语法简单、功能强大

#### 1、引入 Thymeleaf

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```



#### 2、Thymeleaf 的使用和语法

```java

@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";
//前后缀
```

只要把HTML页面放在classpath:/templates/下，它会自动渲染

1、导入 thymeleaf的名称空间

```xml
 <html lang="en" xmlns:th="http://www.thymeleaf.org"">
```

2、使用语法



#### 3、语法规则

1、th:text：改变当前文本内容

th:任意html属性来替换原生属性



```properties
Simple expressions:（表达式语法）
    Variable Expressions: ${...}  //获取变量值：OGNL
    1）获取对象的属性、调用方法
    2）使用内置基本对象
            #ctx: the context object.
            #vars: the context variables.
            #locale: the context locale.
            #request: (only in Web Contexts) the HttpServletRequest object.
            #response: (only in Web Contexts) the HttpServletResponse object.
            #session: (only in Web Contexts) the HttpSession object.
            #servletContext: (only in Web Contexts) the ServletContext object.	
    3）内置的一些工具对象
            #execInfo: information about the template being processed.
            #messages: methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
            #uris: methods for escaping parts of URLs/URIs
            #conversions: methods for executing the configured conversion service (if any).
            #dates: methods for java.util.Date objects: formatting, component extraction, etc.
            #calendars: analogous to #dates, but for java.util.Calendar objects.
            #numbers: methods for formatting numeric objects.
            #strings: methods for String objects: contains, startsWith, prepending/appending, etc.
            #objects: methods for objects in general.
            #bools: methods for boolean evaluation.
            #arrays: methods for arrays.
            #lists: methods for lists.
            #sets: methods for sets.
            #maps: methods for maps.
            #aggregates: methods for creating aggregates on arrays or collections.
            #ids: methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).
    Selection Variable Expressions: *{...} //选择表达式，和${}在功能上一样，补充：配合th.object使用
      <div th:object="${session.user}">
        <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
        <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
        <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
      </div>
      
    Message Expressions: #{...} //获取国际化内容
    Link URL Expressions: @{...} //定义URL链接
    Fragment Expressions: ~{...} //片段引用表达式	
Literals //字面量	
    Text literals: 'one text', 'Another one!',…
    Number literals: 0, 34, 3.0, 12.3,…
    Boolean literals: true, false
    Null literal: null
    Literal tokens: one, sometext, main,…
Text operations: //文本操作
    String concatenation: +
    Literal substitutions: |The name is ${name}|
Arithmetic operations:  //数学运算
    Binary operators: +, -, *, /, %
    Minus sign (unary operator): -
Boolean operations: //布尔运算
    Binary operators: and, or	
    Boolean negation (unary operator): !, not
Comparisons and equality: //比较运算
    Comparators: >, <, >=, <= (gt, lt, ge, le)
    Equality operators: ==, != (eq, ne)
Conditional operators: //条件运算
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
Special tokens: //特殊操作
	No-Operation: _
```

#### 4、SpringMVC自动配置

#### 5、如何修改SpringBoot的默认配置

模式：

​	1、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置（@Bean、@Component）如果有就用用户自己配置的，没有才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置和自己默认的组合起来

编写一个配置类（@Configuration），是WebMvcConfigurationAdapter类型，不能标注@EnableWebMvc

```java
public class MyMvcConfig extends WebMvcConfigurerAdapter {
	//override
}
```

既保留了所有的自动配置，也用到了扩展配置

全面接管 SpringMVC：

SpringBoot对SpringMVCの 自动配置不需要了，所有都是我们自己配置，我们需要在配置类中添加@EnableWebMvc即可——>所有SpeingMVC的自动配置类都失效了

不推荐全面接管（不方便高级功能）
