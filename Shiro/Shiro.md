# 一、Shiro

## 1.Shiro是什么？

 它是一个功能强大且易于使用的Java安全框架，可以执行身份验证、授权、加密和会话管理。使用Shiro易于理解的API，您可以快速且轻松地保护任何应用程序——从最小的移动应用程序到最大的web和企业应用程序。 

## 2. 功能简介

1. **Authentication**：身份认证 / 登录，验证用户是不是拥有相应的身份；
2. **Authorization**：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；
3. **Session Manager**：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的；
4. **Cryptography**：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
5. **Web Support**：Web 支持，可以非常容易的集成到 Web 环境；
6. **Caching**：缓存，比如用户登录后，其用户信息、拥有的角色 / 权限不必每次去查，这样可以提高效率；
7. **Concurrency**：shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去;
8. **Testing**：提供测试支持;
9. **Run As**：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；
10. **Remember Me**：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

# 二、Shiro架构

## 2.1 工作流程

![1584343348890](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1584343348890.png)

```
shiro运行流程中，三个核心的组件：
Subject、SecutityManager、Realm
```

1. **Subject**：主体，代表了当前 “用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是 Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者；
2. **SecurityManager**：安全管理器；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet 前端控制器；
3. **Realm**：域，Shiro 从从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色 / 权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。

## 2.2 POM

```XM
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-web</artifactId>
  <version>1.5.1</version>
</dependency>
```

![1584433306797](C:\Users\ZhangHuan\AppData\Roaming\Typora\typora-user-images\1584433306797.png)

# 三、Shiro使用

1. 导入依赖

2. 配置文件

3. HelloWorld


```java
Subject currentUser = SecurityUtils.getSubject();
Session session = currentUser.getSession();
currentUser.isAuthenticated()
currentUser.hasRole("schwartz")
currentUser.isPermitted("lightsaber:wield")
currentUser.logout();
```

Shiro内置过滤器：

```java
anon:无需认真就可以访问
authc:必须认真了才能访问
user:必须拥有记住我功能才能访问
perms :拥有对某个资源的权限才能访问
role:拥有某个角色权限才能访问

Map<String, String> filterMap = new LinkedHashMap<>();
filterMap.put("/admin/index","perms[1,2,3,4,5]");
filterMap.put("/admin/login","anon");
//静态资源需要释放，springboot默认把所有的静态资源都映射到static目录了，所以我们需要映射到img、css、js下面
filterMap.put("/admin/*","authc");
filterMap.put("/img/**","anon");
filterMap.put("/css/**","anon");
filterMap.put("/js/**","anon");
filterMap.put("/*","authc");
filterMap.put("/**","authc");
```

##  3.1 用户授权

1、首先获取到当前的对象,在拿到User对象

```java
 //拿到当前的对象
Subject subject =SecurityUtils.getSubject ();
User user=(User) subject.getPrincipal (); 
```

2、拿到用户的Id在数据库的Role表中进行查询相应的权限，将其便利给User对象，在将info返回。

```java
 //授权
@Override
protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
    System.out.println ("执行了授权：doGetAuthorizationInfo");
    List<Role> roleList = roleService.findUserRoleByUserId(user.getId());
    SimpleAuthorizationInfo info = new SimpleAuthorizationInfo ();
    //设置当前用户的权限
    HashSet<String> roles =new HashSet<>();
    //遍历User 的权限，放到roles 集合中
	for (Role role : roleList) {
        roles.add(role.getRole());
    //info.addStringPermission(role.getRole()); 循环赋值权限 一个一个赋值，太慢
    }
   // 将roles 赋值给 info
    info.addStringPermissions(roles);
    return info;
}
```



###3.1.1 addStringPermissions 和 addStringPermission

查询官网API的文档如下：

`addStringPermission`

```
public void addStringPermission(String permission)

	Adds (assigns) a permission to those directly associated with the account. If the account doesn't yet have any direct permissions, a new permission collection (a Set<String>) will be created automatically.
	
Parameters: 参数:
	permission - the permission to add to those directly assigned to the account.
```

意思大致就是:

`向与帐户直接关联的用户添加（分配）权限。如果帐户尚未具有任何直接权限，则将自动创建新的权限集合（集合<String>）`。



`addStringPermissions `

```
public void addStringPermissions(Collection<String> permissions)

	 Adds (assigns) multiple permissions to those associated directly with the account. If the 	account doesn't yet have any string-based permissions, a new permissions collection (a Set<String>) will be created automatically.
        
Parameters:参数
	permissions - the permissions to add to those associated directly with the account.
```

`将多个权限添加（分配）到与帐户直接关联的权限。如果帐户尚未具有任何基于字符串的权限，则将自动创建新的权限集合（集合<string>）。`







#四、遇到的问题：

1、在提交登录请求之后继续提交，出现 `org.springframework.web.HttpRequestMethodNotSupportedException: Request method 'GET' not supported`