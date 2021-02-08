# 创建SpringBoot项目

## 一、创建SpringBoot

### 1.1 使用IDEA一个MavenWeb项目

### 1.2 在pom.xml文件中引入parent标签 配置SpringBoot

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <!--引入SpringBoot的父类-->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.2.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
  <groupId>com.hopu</groupId>
  <artifactId>spring-boot-helloworld</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>SpringBoot_01</name>
  <description>Demo project for Spring Boot</description>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--springboot 进行单元测试的模块-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
      <exclusions>
        <exclusion>
          <groupId>org.junit.vintage</groupId>
          <artifactId>junit-vintage-engine</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
  </dependencies>

  <!--可以将应用打成一个jar包-->
  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

</project>

```

### 1.3 创建SpringBoot的启动类

![1585221888491](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1585221888491.png)

```java
package com.zh.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @ClassName SpringBootApplication
 * @Description: TODO
 * @Author ZhangHuan
 * @Date 2020/3/26
 * @Version V1.0
 **/
@SpringBootApplication
public class SpringBoot_01Application {
    public static void main(String[] args) {
        SpringApplication.run ( SpringBoot_01Application.class,args );
    }
}

```

1. 创建完成之后，创建controller 测试 

   ```java
   package com.zh.springboot.controller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   /**
    * @ClassName HelloController
    * @Description: TODO
    * @Author ZhangHuan
    * @Date 2020/3/26
    * @Version V1.0
    **/
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String Hello(){
           return "Hello,SpringBoot";
       }
   }
   
   ```

2. 启动之后 访问 localhost:8080/hello,页面显示下面的情况就已经成功![](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1585222246737.png)

注意：

- 如果出现报错，检查SpringBoot的启动类是不是在最外层，要将 **Application**启动类放在最外层，springboot会自动加载启动类所在包下及其子包下的所有组件![1585222703453](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1585222703453.png)

