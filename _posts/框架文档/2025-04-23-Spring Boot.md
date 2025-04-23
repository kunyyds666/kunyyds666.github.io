





## 





## 自动装配原理

### pom.xml

父工程 `spring-boot-starter-parent`

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.3.1</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

父工程`spring-boot-dependencies`

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>3.3.1</version>
</parent>
```

- `spring-boot-dependencies`：核心依赖在父工程中
- 引入Spring Boot依赖时不需要指定版本，是因为有这些仓库

### 启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```

- 就是Spring Boot的启动场景
- 比如spring-boot-starter-web，就会帮我们自动导入web环境的所有依赖
- 我们需要什么功能，就只需要找到对应的启动器`starter`

### 主程序

- 注解

![](.images\1L2UfpPw3Xo7B.png)

![](.images\WvgoNskZ1QAt7.png)

- ### 深入解析 @SpringBootApplication 注解

  `@SpringBootApplication` 是 Spring Boot 提供的一个关键注解，用于简化 Spring Boot 应用程序的配置和启动。本文将深入解析 `@SpringBootApplication` 注解，特别是 `@EnableAutoConfiguration` 的工作机制。

#### @SpringBootApplication 概述

  `@SpringBootApplication` 是一个组合注解，它结合了以下三个重要注解：

  - `@SpringBootConfiguration`
  - `@EnableAutoConfiguration`
  - `@ComponentScan`

  这些注解共同作用，简化了 Spring Boot 应用的配置和启动过程。

#### @SpringBootConfiguration

  - **作用**：标识一个 Spring 配置类，替代传统的 XML 配置文件。
  - **功能**：这是 `@Configuration` 注解的特化版本，允许在类中定义一个或多个 `@Bean` 方法来创建和配置 Spring 容器中的 Bean。

#### @ComponentScan

  - **作用**：启用组件扫描。
  - **功能**：
- 自动扫描和注册使用 `@Component`、`@Service`、`@Repository`、`@Controller` 等注解标注的类为 Spring Bean。
    - 默认扫描当前包及其子包中的所有类。


### 深入解析 @EnableAutoConfiguration

  `@EnableAutoConfiguration` 是 Spring Boot 自动配置的核心注解。它的作用是根据项目的依赖和配置，自动配置 Spring 应用上下文。以下是对 `@EnableAutoConfiguration` 详细机制的解析：

#### @EnableAutoConfiguration 的作用

  - **启用自动配置机制**：`@EnableAutoConfiguration` 启用 Spring Boot 的自动配置机制，它会自动配置应用程序所需的各种 Spring Bean。
  - **加载自动配置类**：自动配置机制通过扫描类路径中的 `META-INF/spring.factories` 文件，加载其中定义的自动配置类。

#### 自动配置的详细机制

  1. **`AutoConfigurationImportSelector`**    

     `@EnableAutoConfiguration` 通过 `AutoConfigurationImportSelector` 类来选择和导入自动配置类。

     - **选择自动配置类**：`AutoConfigurationImportSelector` 实现了 `DeferredImportSelector` 接口，它会在所有 `@Configuration` 配置类处理完之后执行，选择自动配置类并将它们导入到 Spring 容器中。

  2. **`SpringFactoriesLoader`**

     `SpringFactoriesLoader` 是 Spring 框架中的一个工具类，它负责从 `META-INF/spring.factories` 文件中加载工厂类。

     - **读取** **`spring.factories`** **文件**：`SpringFactoriesLoader` 从类路径中读取 `META-INF/spring.factories` 文件，该文件包含了所有自动配置类的列表。
     - **解析工厂类**：将这些工厂类名解析为相应的类，并返回一个包含这些类的列表。

  3. **自动配置类的加载**

     - **加载配置**：`AutoConfigurationImportSelector` 会根据 `spring.factories` 文件中的配置，加载所有定义的自动配置类。
     - **条件加载**：自动配置类通常使用 `@Conditional` 注解，根据条件（如类路径中是否存在特定的类，配置文件中是否设置了特定的属性）决定是否加载相应的配置。


#### `META-INF/spring.factories` 文件

  `META-INF/spring.factories` 文件是一个关键的配置文件，它定义了各种自动配置类。例如：

  ````
  运行过程
  ````

  这个文件中的键是工厂类接口或注解（如 `EnableAutoConfiguration`），值是具体的实现类。Spring Boot 根据这些配置类自动配置应用程序所需的 Bean。

### 运行过程

  1. **启动类**：应用程序的主类通常使用 `@SpringBootApplication` 注解标注。
  2. **SpringApplication.run()**：调用 `SpringApplication.run()` 方法启动 Spring Boot 应用。
  3. **初始化 Spring 上下文**：创建并初始化 Spring 应用上下文（ApplicationContext）。
  4. **自动配置**：`EnableAutoConfiguration` 自动配置 Spring 上下文，加载必要的配置类和 Bean。
  5. **组件扫描**：`ComponentScan` 扫描和注册所有符合条件的组件（如控制器、服务等）。
  6. **应用启动**：完成所有配置和组件注册后，Spring Boot 应用启动并开始运行。

### 示例

  ````
  总结
  ````

  在这个示例中，`@SpringBootApplication` 注解启用了自动配置和组件扫描功能。`SpringApplication.run` 方法启动 Spring Boot 应用，自动配置机制会根据类路径中的依赖和配置文件中的设置，自动配置 Spring 上下文。

### 总结

  `@SpringBootApplication` 注解通过结合 `@SpringBootConfiguration`、`@EnableAutoConfiguration` 和 `@ComponentScan` 注解，极大地简化了 Spring Boot 应用的配置和启动过程。特别是 `@EnableAutoConfiguration` 注解，通过 `AutoConfigurationImportSelector` 和 `SpringFactoriesLoader` 等组件，实现了自动配置的强大功能，使得开发者可以专注于业务逻辑，而不必手动配置大量的 Spring Bean。这种自动化配置机制是 Spring Boot 高效、简洁、易用的关键所在。

  

  



### **面试回答建议**

如果面试官问 **`@Autowired` 和 `@Resource` 的区别**，可以这样回答：

> `@Autowired` 是 **Spring 提供**的，默认 **按类型（byType）注入**，如果有多个匹配的 Bean，可以使用 `@Qualifier` 指定具体实现。`@Resource` 是 **J2EE 规范**提供的，默认 **按名称（byName）注入**，找不到匹配 Bean ID 时才会按类型（byType）注入，并且可以使用 `name` 和 `type` 进行控制。
>
> `@Autowired` 需要确保 Bean 存在，否则会报错，而 `@Resource` 通过 `name` 或 `type` 严格匹配，不匹配时也会报错。

如果面试官追问 **如何解决多个实现类导致的注入冲突**，可以回答：

- **`@Autowired` 的解决方案**
  1. **修改变量名**，使其与 Bean ID 一致。
  2. **使用 `@Qualifier` 显式指定 Bean 名称**。
- **`@Resource` 的解决方案**
  1. **使用 `name` 属性**指定 Bean ID。
  2. **使用 `type` 属性**指定 Bean 类型。

这样回答既清晰又展现了你的实际开发经验！💡