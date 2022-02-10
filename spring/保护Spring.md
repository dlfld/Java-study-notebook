# 保护Spring

##  **Spring Security**

添加依赖

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

通过将security starter添加到项目的构建文件中，我们得知了以下的安全特性：

+ 所有的HTTP请求路径都需要认证
+ 不需要特定的角色和权限
+ 没有登陆页面
+ 认证过程是通过HTTP basic认证对话框实现的
+ 系统只有一个用户，用户名为user

