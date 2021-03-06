# **Spring Boot DevTools**

顾名思义，DevTools为Spring开发人员提供了一些便利的开发期工具，其中包括：

+ 代码变更后应用会自动重启
+ 当面向浏览器的资源（如模板、js、css）等发生变化时，会自动刷新浏览器；
+ 自动禁用模板缓存；
+ 如果使用h2数据库的话，还内置了h2控制台；

DevTools并不是IDE插件，他也不需要你使用特定的IDE。另外他能够智能的在生产环境中把自己禁用掉。

## 应用自动重启

​		当DevTools运行的时候，应用程序会被加载到Java虚拟机两个独立的类加载器中。其中一个类加载器会加载Java代码、属性文件以及项目中“s r c/main/“路径下几乎所有的内容。这些条目很可能会经常发生变化。另一个类加载器会加载依赖的库，这些库不太可能经常发生变化

​		当检测到变更的时候，DevTools只会重新加载包含项目代码的类加载器，并重启Spring的应用上下文，在这个过程中另外一个类加载器和JVM会原封不动。这个策略非常精细，但是他能够减少应用启动的时间。	