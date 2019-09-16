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

spring-boot-starter-web：

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

​    <u>将主配置类(@SpringBootApplication标注的类)的所在包及下面所有子包里面的所有组件扫描到Spring容器中；</u>

@**Import**({EnableAutoConfigurationImportSelector.class})

​    给容器中导入**EnableAutoConfigurationImportSelector**：导入哪些组件的选择器

   将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

  会给容器中导入非常多的自动配置类（xxxAutoConfiguration）：就是给容器中导入这个场景需要的所有组件，并配置好这些组件；

![image-20190916212856621](/Users/jinyingjie/Documents/Typora/SpringBoot/images/image-20190916212856621.png)

有了自动配置，免去了我们手动编写配置和注入功能组件等的工作；

​		SpringFactoriestLoader.loadFactoryNames(EnableAutoConfiguration.class,classLoader)；

**SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己指定配置的东西，自动配置类都帮我们**

J2EE的整体整合解决方案和自动配置都在spring-boot-autoconfigure-1.5.9-RELEASE.jar 

