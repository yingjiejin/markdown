# 一、SpringBoot

## 1、HelloWorld探究

### 1、POM文件

#### 1、父项目

```java
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>

他的父项目是

<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>1.5.9.RELEASE</version>
	<relativePath>../../spring-boot-dependencies</relativePath>
</parent>

他来真正管理SpringBoot应用里面所有的依赖版本
```

> SpringBoot的版本仲裁中心；

#### 2、启动器

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

spring-boot-starter-==web==：

> spring-boot-starter: spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

> SpringBoot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景启动器。

### 2、主程序类、主入口类

```java
@SpringBootApplication
public class HelloWorldMainApplication{

	public static void main(String[] args){
		
		//Spring应用启动起来
		SpringBootApplication.run(HelloWorldMainApplication.class,args);
	}
}
```

> **@SpringBootApplication**：SpringBoot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
  @Filter( type = FilterType.CUSTOM, classes = {TypeExcludeFilter.class}), 
  @Filter( type = FilterType.CUSTOM, classes = {AutoConfigurationExcludeFilter.class})})
public @interface SpringBootApplication{}
```

**@SpringBootConfiguration:**SpringBoot配置类；

​			   标注在某个类上，表示这是一个SpringBoot的配置类；

​			  **@Configuration:**配置类上来标注这个注解；

​                     配置类 ------- 配置文件；配置类也是容器中的一个组件；@Component

**@EnableAutoConfiguration：**开启自动配置功能；

​			 以前我们需要配置的东西，SpringBoot帮我们自动配置；**@EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

**@AutoConfigurationPackage:**自动配置包

​	@**Import**(AutoConfigurationPackages.Registrar.class):

​    Spring底层注解@Import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class

​    ==将主配置类(@SpringBootApplication标注的类)的所在包及下面所有子包里面的所有组件扫描到Spring容器中；==

@**Import**({EnableAutoConfigurationImportSelector.class})

​    给容器中导入**EnableAutoConfigurationImportSelector**：导入哪些组件的选择器

   将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

  会给容器中导入非常多的自动配置类（xxxAutoConfiguration）：就是给容器中导入这个场景需要的所有组件，并配置好这些组件；

![image-20190916212856621](F:\markdown\SpringBoot\images/image-20190916212856621.png)

有了自动配置，免去了我们手动编写配置和注入功能组件等的工作；

​		SpringFactoriestLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)；

==**SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己指定配置的东西，自动配置类都帮我们**==

J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9-RELEASE.jar 

## 2、使用Spring Initializer快速创建SpringBoot项目

IDE都支持使用Spring的项目创建向导快速创建一个SpringBoot项目；

选择我们需要的模块；向导会联网创建SpringBoot项目；

默认生成的SpringBoot项目；

- 主程序已经生成好了，我们只需要我们自己的逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源；js、css、images；
  - templates：保存所有的模板页面；（SpringBoot默认jar包，使用嵌入式的tomcat，默认不支持JSP页面）；可以使用模板引擎（Freemarker、thymeleaf）；
  - application.properties：SpringBoot应用的配置文件；可以修改一些默认设置；

# 二、配置文件

## 1、配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

- application.properties
- application.yml



配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；



YAML（YAML Ain't Markup Language）

​	YAML A Markup Language：是一个标记语言

​    YAML isn't Markup Language：不是一个标记语言

标记语言：

​	以前的配置文件：大多都使用的是 **xxx.xml**文件；

​    YAML ：**以数据为中心**，比json，xml等更适合做配置文件；

​    YAML ：配置例子

```yaml
server:
 port: 8081
```

   XML ：配置例子

```xml
<server>
    <port>8081</port>
</server>
```

## 2、YAML语法

### 1、基本语法

k:(空格)v：表示一对键值对（空格必须有）;

以空格的缩进来控制层级关系；只要左对齐的一列数据都是同一层级的；

```yaml
server:
    port: 8081
    path: /hello
```

属性和值也是大小写敏感；



### 2、值的写法

##### 字面量：普通的值（数字、字符串、布尔）；

k: v：字面量直接来写；

​	字符串默认不用加单引号或双引号；

​    ""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​					name: "zhangsan \n lisi"；输出：zhangsan 换行 lisi

​    ''：单引号；会转义字符串里面的特殊字符；特殊字符最终只是一个普通的字符串数据

​					name: 'zhangsan \n lisi'；输出：zhangsan \n lisi

#####  对象、Map(属性和值)(键值对)：

​	k: v：在下一行来写对象的属性和值的关系；注意缩进

​			对象还是k: v的方式

           ```yaml
friends：

	lastname：zhangsan

  	age：20
           ```

行内写法：

```yaml
friends: {lastname: zhangsan,age: 20}
```

##### 数组（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法：

```yaml
pets: [cat,dog,pig]
```

### 3、配置文件值注入

Persion.java

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties,告诉SpringBoot将本类中的所有的属性和配置文件中相关的配置进行绑定，
 * prefix = "persion"，配置文件中哪个下面的所有属性进行一一映射
 *
 * 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能
 */
@Component
@ConfigurationProperties(prefix = "persion")
public class Person{
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

Dog.java

```java
public class Dog{
    private String name;
    private Integer age;
}
```

application.yml

```yaml
persion:
  lastName: zhangsan
  age: 18
  boss: false
  birth: 2017/12/12
  maps:{k1: v1,k2: 12}
  lists:
   - lisi
   - zhaoliu
  dog:
   name: 小狗
   age: 2
```

pom.xml

```xml
<!--导入配置文件处理器，配置文件进行绑定就会有提示-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

#### 1、properties配置文件在idea中默认utf-8，可能会乱码

application.properties

```properties
# idea，properties配置文件使用的是utf-8
# 配置person的值
person.lastname=张三
person.age=18
person.boss=false
person.birth=2017/12/12
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
penson.dog.age=15
```

Persion.java

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties,告诉SpringBoot将本类中的所有的属性和配置文件中相关的配置进行绑定，
 * prefix = "persion"，配置文件中哪个下面的所有属性进行一一映射
 *
 * 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能
 */
@Component
// @ConfigurationProperties(prefix = "persion")
public class Person{
    
    /**
     * <bean class="Person">
     *		<property name="lastName" value="字面量/${key}从环境变量、配置文件中获取值/#{SpEL}"></property>
     * </bean>
     */
    @Value("${pension.lastname}")
    private String lastName;
    @Value("#{11*2}")
    private Integer age;
    @Value("true")
    private Boolean boss;
    private Date birth;
    
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

#### 2、@Value获取值和@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

```java
@Component
@ConfigurationProperties(prefix = "persion")
@Validated
public class Person{
    //lastName必须是邮箱格式
    @Email
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

配置文件yml还是properties他们都能获取到值；

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value;

如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties

HelloController.java

```java
@RestController
public class HelloController{
    
    @Value("${person.last-name}")
    private String name;
    
    @RequestMapping("/sayHello")
    public String sayHello(){
        return "Hello" + name;
    }
}
```

#### 3、配置文件注入值数据校验

```java
@Component
@ConfigurationProperties(prefix = "persion")
@Validated
public class Person{
    //lastName必须是邮箱格式
    @Email
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

#### 4、@PropertySource & @ImportResource

**@PropertySource**：加载指定的配置文件

Person.java

```java
/**
 * @ConfigurationProperties(prefix = "persion")默认从全局配置文件中获取值
 * prefix = "persion"，配置文件中哪个下面的所有属性进行一一映射
 *
 */
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "persion")
public class Person{
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

person.properties

```properties
person.lastname=李四
person.age=18
person.boss=false
person.birth=2017/12/12
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
penson.dog.age=15
```

@**ImportResource**：导入Spring配置文件，让配置文件里面的内容生效；

SpringBoot里面没有Spring配置文件，我们自己编写的配置文件也不能自动识别；

想让Spring的配置文件生效，加载进来；@**ImportResource**标注在一个配置类上

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloService" class="com.example.demo.service.HelloService"></bean>
</beans>
```

DemoApplication.java

```java
@ImportResource(locations = {"classpath:beans.xml"})
// 导入Spring的配置文件让其生效
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```



不来编写Spring的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloService" class="com.example.demo.service.HelloService"></bean>
</beans>
```

Springboot推荐给容器中添加组件的方式：推荐使用全注解的方式

1、配置类 == Spring配置文件

2、使用@Bean给容器中添加组件

```java
/**
 * @Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
 * 在配置文件中用<bean></bean>标签添加组件
 */
@Configuration
public class MyAppConfig {

    // 将方法的返回值添加到容器中，容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService() {
        return new HelloService();
    }
}
```

### 4、配置文件占位符

#### 1、随机数

```java
random、value、(random.int)、${random.long}、random.int(10)、{(random.int[1024,65536])}
```

#### 2、占位符获取之前配置的值，如果没有可以使用：指定默认值

```properties
person.lastname=zhangsan${random.uuid}
person.age=${random.int}
person.boss=false
person.birth=2017/12/12
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=${person.hello:hello}_dog
penson.dog.age=15
```

### 5、Profile

#### 1、多Profile文件

我们在主配置文件编写时，文件名可以是application-{profile}.properties/yml

默认使用application.properties的配置

#### 2、yml支持多文档快方式

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod  #指定属于哪个环境
```

#### 3、激活指定Profile

​		1、在配置文件中指定  spring.profiles.active=dev

​		2、命令行：

​			java -jar springboot-02-config-0.0.1.SNAPSHOT.jar --spring.profiles.active=dev

 		  可以直接在测试的时候，配置传入命令行参数

​      3、虚拟机参数：

​			-Dspring.profile.active=dev

### 6、配置文件加载位置

SpringBoot启动会扫描以下位置的application.properties或者application.yml文件作为SpringBoot的默认配置文件

- file: ./config/

- file: ./

- classpath: /config/

- classpath: /

  **优先级由高到低**，高优先级的配置会覆盖低优先级的配置；

  SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；

==我们还可以通过spring.config.location来改变默认的配置文件位置==

项目打包以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的位置；指定的配置文件和默认加载的这些配置文件会共同起作用，形成互补配置；

### 7、外部配置加载顺序

==**SpringBoot也可以从以下位置加载配置；优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置**==

1. **命令行参数**

   java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc

   多个配置用空格分开；--配置项=值

2. 来自java:comp/env的NDI属性

3. java系统属性(System.getProperties())

4. 操作系统环境变量

5. RandomValuePropertySource配置的random.*属性值

   ==**由jar包外向jar包内进行寻找；**==

   ==**优先加载带profile**==

6. **jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

7. **jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件**

   ==**再来加载不带profile**==

8. **jar包外部的application.properties或application.yml(不带spring.profile)配置文件**

9. **jar包内部的application.properties或application.yml(不带spring.profile)配置文件**

10. @Configuration注解类上的@PropertySource

11. 通过SpringApplication.setDefaultProperties指定的默认属性

### 8、自动配置原理

#### 1、**自动配置原理：**

1）SpringBoot启动的时候加载主配置类，开启了自动配置功能==**@EnableAutoConfiguration**==

2）@EnableAutoConfiguration作用：

	- @EnableAutoConfigurationImportSelector给容器中导入一些组件？
	
	- 可以查看selectImports()方法的内容：

```java
List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
// 获取候选的配置
```

```java
SpringFactoriesLoader.loadFactoryNames()
扫描所有jar包类路径下 META-INF/spring.factories
把扫描到的这些内容包装成properties对象
从properties中获取到EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加在容器中
```



==**将类路径下META-INF/spring.factories里面配置的所有EnableAutoConfiguration的值加入到容器中；**==

```properties
# Auto Configure
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

每一个这样的 xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；

3）每一个自动配置类进行自动配置功能；

4）以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理：

```java
@Configuration    //表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件

@EnableConfigurationProperties(HttpProperties.class)  //启用指定类的ConfigurationProperties功能；将配置文件中对应的值和HttpProperties绑定起来；并把HttpProperties加入到IOC容器中

@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)//Spring底层@Conditional注解，根据不同的条件，如果满足指定的条件，整个配置类，里面的配置就会生效；判断当前应用是否为web应用，如果是，当前配置类生效

@ConditionalOnClass(CharacterEncodingFilter.class) //判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；

@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing = true) //判断配置文件中是否存在某个配置spring.http.encoding.enabled;如果不存在判断也是成立的
// 即使我们配置文件中不配置spring.http.encoding.enabled=true,也是默认生效的
public class HttpEncodingAutoConfiguration {
    
    //他已经和SpringBoot的配置文件映射了
    private final HttpProperties.Encoding properties;
    
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
		this.properties = properties.getEncoding();
	}
    
    @Bean //给容器中添加一个组件，这个组件的某些值需要从properties中获取
	@ConditionalOnMissingBean //判断容器中没有这个组件
	public CharacterEncodingFilter characterEncodingFilter() {
		CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
		filter.setEncoding(this.properties.getCharset().name());
		filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
		filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
		return filter;
	}
    
}
```

根据当前不同的条件判断，决定这个配置类是否生效？

一旦这个配置类生效：这个配置就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；



5）所有在配置文件中能配置的属性都是在xxxProperties类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "spring.http") //从配置文件中获取指定的值和bean的属性进行绑定
public class HttpProperties {}
```



**精髓：**

​	**1）SpringBoot启动会加载大量的自动配置类；**

​	**2）我们看我们需要的功能有没有SpringBoot默认写好的自动配置类；**

​	**3）我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）**

​	**4）给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中指定这些属性的值；**



xxxxAutoConfiguration：自动配置类；

给容器中添加组件

xxxxProperties：封装配置文件相关的属性；

#### 2、细节

##### 1、@Conditional派生注解（Spring注解版原生的@Conditional作用）

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置类里面的所有内容才生效；

| @Conditional扩展注释            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的Java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定；                             |
| @ConditionalOnClass             | 系统中有指定的类；                               |
| @ConditionalOnMissingClass      | 系统中没有指定的类；                             |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

**自动配置类必须在一定的条件下才能生效；**

我们怎么知道哪些自动配置类生效？

我们可以通过启用debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

```java
=========================
AUTO-CONFIGURATION REPORT
=========================

Positive matches:(自动配置类启用的)
-----------------
    
    DispatcherServletAutoConfiguration matched：
    	- @ConditionalOnClass found required class
'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
    	- @ConditionalOnWebApplication (required) found StandardServletEnvironment (OnWebApplicationCondition)
    

Negative matches:(没有启动，没有匹配成功的自动配置类)
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)

   AopAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)
```

# 三、日志

## 1、日志框架

**市面上的日志框架：**

JUL、JCL、jboss-logging、logback、log4j、log4j2、slf4j...

| 日志门面（日志的抽象层）                                     | 日志实现                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| JCL（Jakarta Commons Logging）SLF4J（Simple Logging Facade for Java）**jboss-logging** | Log4j JUL (java.util.logging) Log4j2 Logback |

左边选择一个门面（抽象层）、右边来选一个实现；

日志门面：SLF4J

日志实现：Logback



SpringBoot：底层是Spring框架，Spring框架默认是用JCL

​		==**SpringBoot选用SLF4J和Logback；**==



## 2、SLF4j使用

### 1、如何在系统中使用SLF4j

以后开发的时候，日志记录的方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里的方法；

应该给系统里导入slf4j的jar和logback的实现jar

```java
import org.slf4j.Logger;
import org,slf4j.LoggerFactory;

public class HelloWorld {
	public static void main(String[] args){
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
    }
}
```

图示：

![slf4j](F:\markdown\SpringBoot\images\slf4j.png)

每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件**

### 2、遗留问题

a(slf4j+logback)：Spring（commons-logging）、Hibernate（jboss-logging）、Mybatis、xxxx

统一日志记录，即使是别的框架和我一起统一使用slf4j进行输出？

![legacy](F:\markdown\SpringBoot\images\legacy.png)

**如何让系统中所有的日志都统一到slf4j：**

==1、将系统中其他日志框架先排除出去；==

==2、用中间包来替换原有的日志框架；==

==3、我们导入slf4j其他的实现；==

## 3、SpringBoot日志关系

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
</dependency>
```

SpringBoot使用它来做日志功能：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-logging</artifactId>
</dependency>
```

底层依赖关系

![slf4jgroupnew](F:\markdown\SpringBoot\images\slf4jgroupnew.png)

总结：

​	1）、SpringBoot底层也是使用slf4j+logback的方式进行日志记录；

​	2）、SpringBoot也把其他的日志都替换成了slf4j；

​	3）、中间替换包？

```java
public abstract class LogFactory {
    static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";
    static LogFactory logFactory = new SLF4JLogFactory();
}
```

![transferpackage](F:\markdown\SpringBoot\images\transferpackage.png)

​	4）、如果我们要引入其他框架？一定要把这个框架的日志依赖移除掉？

​			Spring框架用的commons-logging：	

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-core</artifactId>
	<exclusions>
		<exclusion>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
		</exclusion>
	</exclusions>
</dependency>
```

**SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需把这个框架依赖的日志框架排除掉；**

## 4、日志使用

### 1、默认配置

SpringBoot默认帮我们配置好了日志；

```java
// 记录器
Logger logger = LoggerFactory.getLogger(getClass());

@Test
public void contextLoads() {

	// 日志级别：
	// 由低到高 trace<debug<info<warn<error
	// 可以调整输出的日志级别：日志就只会在这个级别以后的高级别生效
	logger.trace("这是trace日志...");
	logger.debug("这是debug日志...");
	// SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别，root级别
	logger.info("这是info日志...");
	logger.warn("这是warn日志...");
	logger.error("这是error日志...");
}
```





```xml
<!--
	日志输出格式：
		%d 表示日期时间，
		%thread 表示线程名，
		%-5level：级别从左显示5个字符宽度
		%logger{50} 表示logger名字最长50个字符，否则按照句点分割
		%msg：日志消息
		%n 是换行符
-->
%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
```

SpringBoot修改日志的默认配置

```properties
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
logging.file=E:/springboot.log
# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用spring.log作为默认文件
logging.path=/spring/log

# 在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd}[%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} === %msg%n
```

| logging.file | logging.path | Example  | Description                      |
| ------------ | ------------ | -------- | -------------------------------- |
| {none}       | {none}       |          | 只在控制台输出                   |
| 指定文件名   | {none}       | my.log   | 输出日志到my.log文件             |
| {none}       | 指定目录     | /var/log | 输出到指定目录的spring.log文件中 |

### 2、指定配置

给类路径下每个日志框架放上自己的配置文件即可；SpringBoot就不使用他默认配置了；

| Logging System           | Customization                                                |
| ------------------------ | ------------------------------------------------------------ |
| Logback                  | logback-spring.xml，logback-spring.groovy，logback.xml Or logback.groovy |
| Log4j2                   | log4j2-spring.xml Or log4j2.xml                              |
| JDK（java.util.Logging） | logging.properties                                           |

logback.xml：直接就被日志框架识别了；

logback-spring.xml：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
    可以指定某段配置只在某个环境下生效
</springProfile>
```

否则

```java
no applicable action for [springProfile]
```

## 5、切换日志框架

可以按照slf4j的日志适配图，进行相关的切换；

slf4j+log4j的方式；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
    	<exclusion>
        	<groupId>ch.qos.logback</groupId>
    		<artifactId>logback</artifactId>
        </exclusion>
        <exclusion>
        	<groupId>org.slf4j</groupId>
    		<artifactId>log4j-over-slf4j</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
	<groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
</dependency>
```



切换为log4j2

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
    	<exclusion>
        	<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

# 四、Web开发

### 1、简介

**使用SpringBoot：**

**1）、创建SpringBoot应用，选中我们需要的模块**

**2）、SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来**

**3）、自己编写业务代码**



**自动配置原理？**

```java
xxxxAutoConfiguration：帮我们给容器中自动配置组件
xxxxProperties：配置类来封装配置文件的内容
```

 ### 2、SpringBoot对静态资源的映射规则

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties implements ResourceLoaderAware {
    //可以设置和静态资源有关的参数，缓存时间
}
```

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
    	logger.debug("Default resource handling disabled");
    } else {
    	Integer cachePeriod = this.resourceProperties.getCachePeriod();
    	if (!registry.hasMappingForPattern("/webjars/**")) {
    		 this.customizeResourceHandlerRegistration(registry.addResourceHandler(new 				String[]{"/webjars/**"}).addResourceLocations(new String[]								{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(cachePeriod));
        }
		String staticPathPattern = this.mvcProperties.getStaticPathPattern();
        if (!registry.hasMappingForPattern(staticPathPattern)) {
			this.customizeResourceHandlerRegistration(registry.addResourceHandler(new 
 String[{staticPathPattern}).addResourceLocations(this.resourceProperties.getStaticLocati	ons()).setCachePeriod(cachePeriod));
        }

   }
}

//配置欢迎页映射
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(
	ResourceProperties resourceProperties) {
	return new WelcomePageHandlerMapping(resourceProperties.getWelcomePage(),
	this.mvcProperties.getStaticPathPattern());
}
        
//配置喜欢的图标
@Configuration
@ConditionalOnProperty(value = "spring.mvc.favicon.enabled", matchIfMissing = true)
public static class FaviconConfiguration {

	private final ResourceProperties resourceProperties;

	public FaviconConfiguration(ResourceProperties resourceProperties) {
		this.resourceProperties = resourceProperties;
	}

	@Bean
	public SimpleUrlHandlerMapping faviconHandlerMapping() {
		SimpleUrlHandlerMapping mapping = new SimpleUrlHandlerMapping();
		mapping.setOrder(Ordered.HIGHEST_PRECEDENCE + 1);
        //所有**/favicon.ico
		mapping.setUrlMap(Collections.singletonMap("**/favicon.ico",
		faviconRequestHandler()));
		return mapping;
	}

	@Bean
    public ResourceHttpRequestHandler faviconRequestHandler() {
            ResourceHttpRequestHandler requestHandler = new ResourceHttpRequestHandler();
            requestHandler.setLocations(this.resourceProperties.getFaviconLocations());
            return requestHandler;
    }
}
```

==1）、所有/webjars/**，都去classpath:/META-INF/resources/webjars/找资源；==

​	webjars：以jar包的方式引入静态资源；

https://www.webjars.org/

![webjars](F:\markdown\SpringBoot\images\webjars.png)

localhost:8080/webjars/jquery/3.3.1/jquery.js

```xml
<!-- 引入jquery-webjar -->在访问的时候只需要写webjars下面资源的名称即可
<dependency>
	<groupId>org.webjars</groupId>
	<artifactId>jquery</artifactId>
	<version>3.3.1</version>
</dependency>
```

==2）、"/**"访问当前项目的任何资源，（静态资源的文件夹）==

```xml
"classpath:/META-INF/resources/", 
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/"
"/"：当前项目的根路径
```

localhost:8080/abc ==== 去静态资源文件夹里面找abc

==3）、欢迎页：静态资源文件夹下的所有index.html页面；被"/**"映射；==

localhost:8080/ 找index页面

==4）、所有的**/favicon.ico都是在静态资源文件下找；==

### 3、模板引擎

JSP、Velogcity、Freemarker、Thymeleaf

![Template](F:\markdown\SpringBoot\images\Template.png)

SpringBoot推荐的Thymeleaf；

语法更简单，功能更强大；

#### 1、引入thymeleaf

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

切换thymeleaf版本
<properties>
	<thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
    <!-- 布局功能的支持程序 thymeleaf3主程序 layout2以上版本 -->
    <thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>
</properties>
```

#### 2、Thymeleaf使用

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = Charset.forName("UTF-8");

	private static final MimeType DEFAULT_CONTENT_TYPE = MimeType.valueOf("text/html");

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";
    // 
}
```

只要我们把html页面放在classpath:/templates/，thymeleaf就能自动渲染；

1、导入thymeleaf的名称空间

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

2、使用thymeleaf语法；

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>成功！</h1>
    <!--th:text 将div里面的文本内容设置为-->
    <div th:text="${hello}"></div>
</body>
</html>
```

#### 3、语法规则

1）、th:text：改变当前元素里面的文本内容；

​		th：任意html属性；来替换原生属性的值

![th_grammar](F:\markdown\SpringBoot\images\th_grammar.png)

2）、表达式？

```properties
Simple expressions:(表达式语法)
    Variable Expressions: ${...}：获取变量值；OGNL；
    	1）、获取对象的属性、调用方法
    	2）、使用内置的基本对象
    		#ctx : the context object.
            #vars: the context variables.
            #locale : the context locale.
            #request : (only in Web Contexts) the HttpServletRequest object.
            #response : (only in Web Contexts) the HttpServletResponse object.
            #session : (only in Web Contexts) the HttpSession object.
            #servletContext : (only in Web Contexts) the ServletContext object.
            ${session.foo}
        3）、内置的一些工具对象
#execInfo : information about the template being processed.
#messages : methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
#uris : methods for escaping parts of URLs/URIs
#conversions : methods for executing the configured conversion service (if any).
#dates : methods for java.util.Date objects: formatting, component extraction, etc.
#calendars : analogous to #dates , but for java.util.Calendar objects.
#numbers : methods for formatting numeric objects.
#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
#objects : methods for objects in general.
#bools : methods for boolean evaluation.
#arrays : methods for arrays.
#lists : methods for lists.
#sets : methods for sets.
#maps : methods for maps.
#aggregates : methods for creating aggregates on arrays or collections.
#ids : methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

    Selection Variable Expressions: *{...}:选择表达式：和${}在功能上是一样的；
    	补充：配合 th:object="${session.user}"：
    	<div th:object="${session.user}">
            <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
            <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
            <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
        </div>
        
    Message Expressions: #{...}：获取国际化内容
    Link URL Expressions: @{...}：定义URL；
    	@{/order/process(execId=${execId},execType='FAST')}
    Fragment Expressions: ~{...}：片段引用表达式
    	<div th:insert="~{commons :: main}">...</div>
Literals: (字面量)
    Text literals: 'one text' , 'Another one!' ,…
    Number literals: 0 , 34 , 3.0 , 12.3 ,…
    Boolean literals: true , false
    Null literal: null
    Literal tokens: one , sometext , main ,…
Text operations:(文本操作)
    String concatenation: +
    Literal substitutions: |The name is ${name}|
Arithmetic operations:(数学运算)
    Binary operators: + , - , * , / , %
    Minus sign (unary operator): -
Boolean operations:(布尔运算)
    Binary operators: and , or
    Boolean negation (unary operator): ! , not
Comparisons and equality:(比较运算)
    Comparators: > , < , >= , <= ( gt , lt , ge , le )
    Equality operators: == , != ( eq , ne )
Conditional operators:(条件运算，支持三元运算符)
    If-then: (if) ? (then)
    If-then-else: (if) ? (then) : (else)
    Default: (value) ?: (defaultvalue)
Special tokens:
	No-Operation: _
```

### 4、SpringMVC自动配置

#### 1、SpringMVC autoConfigure

SpringBoot自动配置好了SpringMVC

以下是SpringBoot对SpringMVC的默认配置：

- Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.

  - 自动配置了ViewResolver(视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？）)
  - ContentNegotiatingViewResolver：组合所有的视图解析器的；
  - ==如何定制：我们可以自己给容器中添加一个视图解析器；自动的将其组合进来；==

- Support for serving static resources, including support for Webjars(see below). 静态资源文件夹路径webjars

- Static index.html support. 静态首页访问

- Custom Favicon support (see below). favicon.ico

- 自动注册了 Converter, GenericConverter, Formatter beans.

  - Converter：转换器；public String hello(User user)：类型转换使用Converter

  - Formatter：格式化器；2019-10-23===Date；

    ```java
    @Bean
    @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")//在文件中配置日期格式化的规则
    public Formatter<Date> dateFormatter() {
    	return new DateFormatter(this.mvcProperties.getDateFormat()); //日期格式化组件
    }
    ```

  ==自己添加的格式化器转换器，我们只需要放在容器中即可==

- Support for HttpMessageConverters (see below).

  - HttpMessageConverters：SpringMVC用来转换HTTP请求和响应的；User---json；

  - HttpMessageConverters 是从容器中确定；获取所有HttpMessageConverters；

    ==自己给容器中添加HttpMessageConverters，只需要将自己的组件注册在容器中（@Bean，@Component）==

- Automatic registration for MessageCodesResolver (see below).定义错误代码生成规则

- Automatic use of a ConfigurableWebBindingInitializer bean (see below).

  ==我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（添加到容器）==

```xml
初始化WebDataBinder:
请求数据===JavaBean；
```

**org.springframework.boot.autoconfigure.web：web的所有自动配置场景**

#### 2、扩展SpringMVC

```xml
<mvc:view-controller path="/hello" view-name="success">
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/hello"/>
        <bean></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

==**编写一个配置类（@Configuration），是WebMvcConfigurerAdapter类型；不能标注@EnableWebMvc**==

既保留了所有的自动配置，也能用我们扩展的配置；

```java
// 使用WebMvcConfigurerAdapter可以来扩展SpringMVC的功能
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // super.addViewControllers(registry);
        // 浏览器发送/jyj请求来到success
        registry.addViewController("/jyj").setViewName("success");
    }
}
```

原理：

​		1）、WebMvcAutoConfiguration是SpringMVC的自动配置类

​		2）、在做其他自动配置时会导入@Import(**EnableWebMvcConfiguration**.class)

```java
@Configuration
public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration {}

private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
//从容器中获取所有的WebMvcConfigurer
@Autowired(required = false)
public void setConfigurers(List<WebMvcConfigurer> configurers) {
    if (!CollectionUtils.isEmpty(configurers)) {
    	this.configurers.addWebMvcConfigurers(configurers);
        // 一个参考实现：将所有的WebMvcConfigurer相关的配置都来一起调用；
        @Override
        //public void addViewControllers(ViewControllerRegistry registry) {
        //    for (WebMvcConfigurer delegate : this.delegates) {
        //    	delegate.addViewControllers(registry);
        //    }
        //}
    }
}
```

​		3）、容器中所有的WebMvcConfigurer都会一起起作用；

​		4）、我们的配置类也会被调用；

​		效果：SpringMVC的自动配置和我们的扩展配置都会起作用；

#### 3、全面接管SpringMVC

SpringBoot对SpringMVC的自动配置不需要了，所有都是我们自己配；所有的SpringMVC的自动配置都失效了

**我们需要在配置类中添加@EnableWebMvc即可；**

```java
//使用WebMvcConfigurerAdapter可以用来扩展SpringMVC的功能
@EnableWebMvc
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter{
    
    @Override
    public void addViewControllers(ViewControllerRegistry registry){
        //super.addViewControllers(registry);
        //浏览器发送/jyj请求来到success
        registry.addViewController("/jyj").setViewName("success");
    }
}
```

原理：

​	为什么@EnableWebMvc自动配置就失效了；

1）@EnableWebMvc的核心

```java
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc{}
```

2）

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport{}
```

3)

```java
@Configuration
@ConditionalOnWebApplication
@ConditionalOnClass({ Servlet.class,DispatcherServlet.class,WebMvcConfigurerAdapter.class })
// 容器中没有这个组件的时候，这个自动配置类才生效
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({DispatcherServletAutoConfiguration.class,ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {}
```

4）@EnableWebMvc将WebMvcConfigurationSupport组件导入进来；

5）导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

### 5、如何修改SpringBoot的默认配置

模式：

​		1）、SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

​		2）、在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置；

