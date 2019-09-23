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

