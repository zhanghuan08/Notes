# JPA

>简介

`JPA`(`Java Persistence API`)意即Java持久化API，是Sun官方在JDK5.0后提出的Java持久化规范（JSR 338，这些接口所在包为`javax.persistence`。JPA的出现主要是为了简化持久层开发以及整合ORM技术，结束Hibernate、TopLink、JDO等ORM框架各自为营的局面。JPA是在吸收现有ORM框架的基础上发展而来，易于使用，伸缩性强。


## 1. 使用

### 1.1 创建SpringBoot项目

### 1.2 导入相关依赖
```pom
<dependencies>
        <!--JPA-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!--Mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!--JDBC-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
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
```


### 1.3 配置`application.yml`
```yml
spring:
  datasource:
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: root
  jpa:
    hibernate:
      # 更新或创建数据库表
      ddl-auto: update
      # 控制台显示 Sql
    show-sql: true
```

### 1.4 创建实体类`User`
```java
@Data
@Entity
@Table(name = "t_user")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY,generator = "uuid")
    private Integer id;
    private String username;
    private String password;
}
```

### 1.5 `UserRepository`
```java
public interface UserRepository extends JpaRepository<User,Integer> {
    
}
```

### 1.6 `UserController`
```java
@RestController
public class UserController {

    @Resource
    private UserRepository userRepository;

    /**
     * 根据 id 查询用户
     * @param id
     * @return
     */
    @GetMapping("/user/{id}")
    public Optional<User> getUser(@PathVariable("id") Integer id)
    {
        return  userRepository.findById(id);
    }

    /**
     * 添加用户
     * @param user
     * @return
     */
    @GetMapping("/user")
    public User insertUser(User user)
    {
        return userRepository.save(user);
    }

    /**
     * 查询所有用户
     * @param user
     * @return
     */
    @GetMapping("/user/findAllUser")
    public List<User> findAllUser(User user)
    {
        return userRepository.findAll();
    }

    /**
     * 根据id 删除用户
     * @param id
     * @param
     * @return
     */
    @GetMapping("/user/deleteUserById/{id}")
    public String deleteUserById(@PathVariable("id") Integer id)
    {
        System.out.println("删除成功");
       userRepository.deleteById(id);
        return "success";
    }

    /**
     * 修改用户
     * @param user
     * @return
     */
    @GetMapping("/user/updateUserById")
    public User updateUserById(User user)
    {
        System.out.println("修改成功");
        return userRepository.save(user);
    }
}
```