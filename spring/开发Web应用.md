# 开发Web应用

## 构建领域类

​		应用的领域指的是它所要解决的主题范围也就是会影响到对应用理解的理念和概念。

@Slf4j,这是lombok所提供的注解，在运行时他会在这个类中自动生成一个SLF4J Logger。这个简单的注解和在类中通过如下代码显示声明的效果是一样的。

```java
private static final org.slf4j.Logger log =
    org.slf4j.LoggerFactory.getLogger(DesignTacoController.class);
```



## Validation API -> JSR-303

Spring支持java的Bean校验API（Bean Validation API）被称为JSR-303，这样的话我们能够更容易的声明检验规则，而不必在应用程序代码中显示的声明逻辑。在SpringBoot中添加校验库不需要做任何操作，Validation API会作为Spring Boot Web Starter的传递性依赖自动添加到项目中。

Validation API提供了一些可添加到领域对象的注解，以便于声明校验规则.

<a href="https://blog.csdn.net/chennuo001/article/details/88244420">具体的校验注解</a>

在具体的Controller方法当中，使用<code>@Valid</code>注解添加到对应的参数前面，并可选择跟一个参数<code>Errors errors</code>来接收错误，<code>@Valid</code>注解会告诉Spring MVC要对提交的参数对象进行检验。

```java
public String processDesign(@Valid Taco design, Errors errors) {
  if (errors.hasErrors()) {
    return "design";
  }
}
```

## **JdbcTemplate**

```java
private JdbcTemplate jdbc;
@Override
public Ingredient findOne(String id) {
  return jdbc.queryForObject(
      "select id, name, type from Ingredient where id=?",
      this::mapRowToIngredient, id);
      }
private Ingredient mapRowToIngredient(ResultSet rs, int rowNum)
    throws SQLException {
  return new Ingredient(
      rs.getString("id"),
      rs.getString("name"),
      Ingredient.Type.valueOf(rs.getString("type")));
}
```

在项目中使用JdbcTemplate非常容易，只需要加入如下依赖就可以了。

```java
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```