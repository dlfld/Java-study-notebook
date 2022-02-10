# Spring 简介

Spring的核心是提供了一个容器（container），通常称为Spring应用上下文（Spring Application Context），它们会创建和管理应用组件，这些组件也称为bean。

在Spring技术中，自动配置起源于所谓的自动装配aotowiring和组件扫描component scanning，借助组件扫描技术，spring能够自动发现应用类路径下的组件，并将他们建成Spring应用上下文中的bean。

随着Springboot的引入，自动配置的能力已经远远超出了组件扫描和自动装配

SpringBoot 启动类的@SpringBootApplication是一个组合注解，他组合了三个注解

+ @SpringBootConfiguration将该类声明为配置类。
+ @EnableAutoConfiguration 启用springboot的自动配置
+ @ComponentScan启用组件扫描，这样就可以扫描到@Component、@Controller、@Service这样注解声明的其他类。Spring会发现它们并将它们注册为Spring应用上下文组件。

Spring旨在简化开发人员面临的挑战，比如创建Web应用程序、处理数据库、保护应用程序以及实现微服务。

Spring Boot构建在Spring上，通过简化依赖管理、自动配置和运行时洞察，使Spring更加易用。































