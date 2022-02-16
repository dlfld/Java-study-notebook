+ @Configuration：配置类，告知Spring这是一个配置类，会为Spring应用上下文提供bean，配置类的方法使用@Bean注解进行标注，表明这些方法返回的对象会以bean的形式添加到Spring应用上下文中。
+ @RunWith(SpringRunner.class)是Junit注解，它提供一个测试运行器（runner）来指导junit来如何测试运行。
+ @WebMvcTest ,SpringBoot提供的特殊测试注解，它会让这个测试在SpringMvc 应用的上下文中执行，该注解同样会为测试SpringMvc应用提供Spring环境的支持，尽管我们可以启动一个服务器来测试，但是对于我们的场景来说，仿造一下SpringMvc的运行机制就可以。测试类被注入了一个MockMvc能够让实例实现mockUp。
+ @ConfigurationProperties https://www.cnblogs.com/tian874540961/p/12146467.html
+ @ResponseStatus 添加返回code和messagehttps://www.cnblogs.com/lvbinbin2yujie/p/10575101.html