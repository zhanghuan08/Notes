# SpringBoot-Shiro-thymeleaf

1. 导入依赖

2. 配置文件

3. HelloWord



## 1.导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.6.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.zh</groupId>
    <artifactId>shiro-springboot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>shiro-springboot</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <!--
           Subject: 用户
           SecurityManager  管理所有用户
           Realm 连接数据
        -->
        <!--thymeleaf-shiro-->
        <!--security-thymleaf 整合包-->
        <dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>

        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>
        <!--Mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!--druid-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
        <!--mybatis -springboot-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>

        <!--shiro 整合spring5包-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.5.1</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--thymleaf-->
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
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
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

## 2.创建config包，创建Shiro的配置类

```java
/**
 * @ClassName ShiroConfig
 * @Description: TODO
 * @Author ZhangHuan
 * @Date 2020/4/1
 * @Version V1.0
 **/
@Configuration
public class ShiroConfig {
//ShiroFilterFactoryBean
@Bean
public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager){
    ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
    //设置安全管理器
    bean.setSecurityManager (defaultWebSecurityManager);

    //添加Shiro的内置过滤器
    /*
            anon:无需认真就可以访问
            authc:必须认真了才能访问
            user:必须拥有记住我功能才能访问
            perms :拥有对某个资源的权限才能访问
            role:拥有某个角色权限才能访问
         */
    //拦截
    Map<String, String> filterMap = new LinkedHashMap<>();

    //给用户授权
    filterMap.put ( "/user/add","perms[user:add]" );
    filterMap.put ( "/user/update","perms[user:update]" );

    //设置认证，认真了才能访问
    filterMap.put ( "/user/*","authc" );
    bean.setFilterChainDefinitionMap ( filterMap );

    //设置登录请求
    bean.setLoginUrl ( "/toLogin" );
    //未授权页面
    bean.setUnauthorizedUrl ( "/unauth" );
    
    return bean;
}

//DefaultWebSecurityManager 2.
@Bean(name="securityManager")
public DefaultWebSecurityManager getDeafaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
    DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
    //关联UserRealm
    securityManager.setRealm (userRealm);
    return securityManager;
}

//创建 realm 对象  1.
@Bean(name="userRealm")
public UserRealm userRealm(){
    return new UserRealm ();
};

//整合ShiroDialect：用来整合shiro-thymeleaf
@Bean
public ShiroDialect getShiroDialect(){
    return new ShiroDialect ();
	}
}

```



### 分析：

#### 三大核心对象（固定的配置）

1. ShiroFilterFactoryBean 

   - 处理拦截资源文件问题,

   - 初始化ShiroFilterFactoryBean的时候需要注入：SecurityManager 

2. DefaultWebSecurityManager

3. UserRealm  需要自定义类，创建UserRealm 类，需要继承 AuthorizingRealm类，实现里面的授权和认证两个方法

==注意==：配置的时候到着配置，3-2-1的顺序配置

##### 1.创建自定义UserRealm 类

    //自定义的Realm
    public class UserRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        System.out.println ("执行了授权：doGetAuthorizationInfo");
        return null;
    }
    
    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println ("执行了认证：doGetAuthenticationInfo");
    
    	}
    }
#####2.将配置的UserRealm 类放在在ShiroConfig中

```
@Bean(name="userRealm")	//自定义的name名称，可以不用写，默认为方法名
public UserRealm userRealm(){ 
	return new UserRealm ();
};
```

配置完之后需要我们加上@Bean注解，将它交给Spring进行托管，完成之后再我们的ShiroConfig中继续配置我们的 DefaultWebSecurityManager对象

#####3.配置 DefaultWebSecurityManager

```java
@Bean(name="securityManager")							
public DefaultWebSecurityManager getDeafaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
    DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
    //关联UserRealm
    securityManager.setRealm (userRealm);
    return securityManager;
}
```

DefaultWebSecurityManager方法写完之后，需要将配置的UserRealm关联起来，用来管理UserRealm



#####4.配置 ShiroFilterFactoryBean 

```java
//ShiroFilterFactoryBean
@Bean
public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager){
    ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
    //设置安全管理器
    bean.setSecurityManager (defaultWebSecurityManager);

    //添加Shiro的内置过滤器
    /*
   		anon:无需认真就可以访问
    	authc:必须认真了才能访问
   		user:必须拥有记住我功能才能访问
   	 	perms :拥有对某个资源的权限才能访问
    	role:拥有某个角色权限才能访问
     */
    //登录拦截
    Map<String, String> filterMap = new LinkedHashMap<>();

    //给用户授权
    filterMap.put ( "/user/add","perms[user:add]" );
    filterMap.put ( "/user/update","perms[user:update]" );

    //设置认证，认真了才能访问
    filterMap.put ( "/user/*","authc" );
    bean.setFilterChainDefinitionMap ( filterMap );

    //设置登录请求
    bean.setLoginUrl ( "/toLogin" );
    //未授权页面
    bean.setUnauthorizedUrl ( "/unauth" );

    return bean;
}
```

