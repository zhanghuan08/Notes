# SpringSecurity（安全）

## 1.1  简介

Spring Security是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大
的Web安全控制，对于安全控制，我们仅需要引入spring-boot-starter-security模块,进行少量的配置，即可实现强大的安全管理!



记住这几个类：

- WebSecurityConfigurerAdapter: 自定义Security策略
- AuthenticationManagerBuilder:自定义认证策略
- @EnableWebSecurity: 开启WebSecurity模式

Spring Security的两个主要目标是"认证”和“授权”(访问控制)。

“认证”(Authentication)

"授权”(Authorization)

这个概念是通用的,而不是只在Spring Security中存在。



##1.2 配置SpringSecurity

###1.2.1 导入SpringSecurity

```XML
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.zh</groupId>
    <artifactId>springboot_04_security</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot_04_security</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--security-thymleaf 整合包-->
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-springsecurity5</artifactId>
        </dependency>

        <!--security-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <!--Web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--Thymleaf-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

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

### 1.2.2 创建配置类 SecurityConfig（实现认证，授权）

```java
package com.zh.config;

import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

/**
 * @ClassName SecurityConfig
 * @Description: TODO
 * @Author ZhangHuan
 * @Date 2020/3/31
 * @Version V1.0
 **/
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
       //首页 所有人都可以访问，功能页有对应权限的人可以访问
        //请求授权的规则
        http.authorizeRequests ()
                .antMatchers ( "/" ).permitAll ()
                .antMatchers ( "/level1/**" ).hasRole ( "vip1" )
                .antMatchers ( "/level2/**" ).hasRole ( "vip2" )
                .antMatchers ( "/level3/**" ).hasRole ( "vip3" );

        //没有权限进入登录页面
        http.formLogin ();
        //http.csrf ().disable ();//关闭cors

        //注销
//        http.logout ();
        //注销之后跳转到首页
        http.logout ().logoutSuccessUrl ( "/" );

        //开启记住我功能
        http.rememberMe ();
    }

    //认证 springboot2.1.X 可以直接使用
    //密码编码： passwordEncoder
    //在Spring Security 新增了很多的加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //开发 从数据库获取
        auth.inMemoryAuthentication ().passwordEncoder ( new BCryptPasswordEncoder ())
                .withUser ( "zs" ).password (new BCryptPasswordEncoder ().encode ( "zs" )).roles ( "vip1","vip2","vip3" ).and ()
                .withUser ( "ls" ).password ( new BCryptPasswordEncoder ().encode ( "ls" ) ).roles ( "vip1","vip2" ).and ()
                .withUser ( "ww" ).password ( new BCryptPasswordEncoder ().encode ( "ww" ) ).roles ( "vip1" );
    }
}

```

###1.2.3 配置RouterController（跳转页面）

```java
package com.zh.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @ClassName RouterController
 * @Description: TODO
 * @Author ZhangHuan
 * @Date 2020/3/31
 * @Version V1.0
 **/
@Controller
public class RouterController {

    @RequestMapping({"./","index"})
    public String index(){
        return "index";
    }

    @RequestMapping("toLogin")
    public String toLogin(){
        return "views/login";
    }

    @RequestMapping("/level1/{id}")
    public String level1(@PathVariable("id") int id){
        return "views/level1//"+id;
    }
    @RequestMapping("/level2/{id}")
    public String level2(@PathVariable("id") int id){
        return "views/level2//"+id;
    }
    @RequestMapping("/level3/{id}")
    public String level3(@PathVariable("id") int id){
        return "views/level3//"+id;
    }

}

```

### 1.2.4 页面中使用thymleaf+security 控制状态（登录，注销）

```html
<div class="right menu">

                <!--未登录-->
                <div sec:authorize="!isAuthenticated()">
                    <a class="item" th:href="@{/toLogin}">
                        <i class="address card icon"></i> 登录
                    </a>
                </div>
                <!--已登录：用户名，注销-->
                <div sec:authorize="isAuthenticated()">
                    <a class="item" th:href="@{/logout}">
                        用户名：<span sec:authentication="name"></span>
                        角色：<span sec:authentication="principal.authorities"></span>
                    </a>
                </div>
                <div sec:authorize="isAuthenticated()">
                    <a class="item" th:href="@{/logout}">
                        <i class="sign-out card icon"></i> 注销
                    </a>
                </div>
            </div>
        </div>
```



注意:

- 使用hymeleaf-extras-springsecurity4整合的时候在HTML页面中解析sec 标签没有效果，将pom的hymeleaf-extras-springsecurity4改成hymeleaf-extras-springsecurity5，同时页面的头标签也需要修改

  ```html
  <html xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
  ```

- 或者将SpringBoot的版本切换到2.0.9/2.0.7



### 1.2.5 定制登录页面和开启记住我

```java
//设置自定义的登录页面,并且自定义 前台接收的 登录的 用户名密码的参数（默认为username，password）
http.formLogin ().loginPage ( "/toLogin" ) //定制登录页面
    .usernameParameter ( "user" )  //自定义用户名参数
    .passwordParameter ( "pwd" ) //自定义密码参数
    .loginProcessingUrl ( "/login" ); //表单提交请求的地址，登录认证的url

//http.csrf ().disable ();//关闭cors

//注销
//        http.logout ();
//注销之后跳转到首页
http.logout ().logoutSuccessUrl ( "/" );

//开启记住我功能
//        http.rememberMe ();
//开启记住我功能，自定义参数(默认保存两周)
http.rememberMe ().rememberMeParameter ( "remember" );
```

前台的登录页面

![1585725225562](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1585725225562.png)

