	**重要层级！！！！**

**DB -> entity -> DAO(REPO, jpa) -> service -> payload(DTO) -> controller**



## Exception -> @controllerAdvice的缺点
 **When @ControllerAdvice Might Not Be Beneficial:**

1. **Overhead for Simple Applications**:
   - **Scenario**: In small or simple applications with only a few controllers.
   - **Reason**: Introducing @ControllerAdvice might add unnecessary complexity. Managing exceptions locally within the controller can be simpler and more straightforward.
   - **Example**: A basic CRUD application with minimal business logic.
   ```java
   @RestController
   public class SimpleController {
       
       @GetMapping("/example")
       public ResponseEntity<String> example() {
           try {
               // Some logic
           } catch (Exception e) {
               return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error occurred");
           }
           return ResponseEntity.ok("Success");
       }
   }
   ```

2. **Controller-Specific Exception Handling**:
   - **Scenario**: When different controllers need to handle exceptions in very specific ways.
   - **Reason**: Using @ControllerAdvice might lead to overgeneralization, making it harder to implement specific handling logic for each controller.
   - **Example**: One controller might need to log additional context for security reasons, while another might need to sanitize outputs differently.
   ```java
   @RestController
   public class SpecificController {
       
       @GetMapping("/secure-data")
       public ResponseEntity<String> getSecureData() {
           try {
               // Secure logic
           } catch (SecurityException e) {
               // Special handling for security-related exceptions
               return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Unauthorized access");
           }
           return ResponseEntity.ok("Secure Data");
       }
   }
   ```

3. **Performance Concerns**:
   - **Scenario**: In high-performance applications where every millisecond counts.
   - **Reason**: The additional layer of global exception handling might introduce slight latency, which can be critical in performance-sensitive scenarios.
   - **Example**: Real-time trading systems or high-frequency transaction processing systems.
   ```java
   @RestController
   public class PerformanceController {
       
       @GetMapping("/high-performance-endpoint")
       public ResponseEntity<String> highPerformanceEndpoint() {
           try {
               // High performance logic
           } catch (Exception e) {
               // Minimal handling to maintain performance
               return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error occurred");
           }
           return ResponseEntity.ok("Fast Response");
       }
   }
   ```

4. **Complex Exception Handling Logic**:
   - **Scenario**: When the exception handling logic is very complex and specific to certain operations.
   - **Reason**: Using @ControllerAdvice can make it harder to follow the flow and understand the specific handling logic required for different operations.
   - **Example**: Complex workflows where each step may need different exception handling strategies.
   ```java
   @RestController
   public class ComplexController {
       
       @GetMapping("/complex-operation")
       public ResponseEntity<String> complexOperation() {
           try {
               // Complex logic
           } catch (FirstException e) {
               // Handling for the first part of the operation
               return ResponseEntity.status(HttpStatus.BAD_REQUEST).body("First part failed");
           } catch (SecondException e) {
               // Handling for the second part of the operation
               return ResponseEntity.status(HttpStatus.CONFLICT).body("Second part failed");
           }
           return ResponseEntity.ok("Operation Successful");
       }
   }
   ```

### Summary:

- **Use @ControllerAdvice**:
  - For centralizing common exception handling across multiple controllers.
  - When you need consistent and reusable handling logic.

- **Avoid @ControllerAdvice**:
  - In simple applications where local exception handling suffices.
  - When controller-specific handling is required.
  - For high-performance scenarios where minimal overhead is critical.
  - When dealing with complex and operation-specific exception handling logic.

By considering these factors, you can determine whether @ControllerAdvice is suitable for your application or if local exception handling is more appropriate.


## @ resource

**1.来源
2.byname
3.必须要有bean，如果找不到匹配的bean，会引发异常**

`@Resource` 和 `@Autowired` 都是用于在 Spring 框架中进行依赖注入的注解，它们的主要区别在于以下几点：

1. **来源**:
   - `@Autowired` 是Spring提供的注解，它是Spring自带的注解。
   - `@Resource` 是Java EE（Java Platform, Enterprise Edition）规范的一部分，它不是Spring独有的注解，可以在其他Java EE容器中使用。

2. **类型**:
   - `@Autowired` 通过类型（byType）来自动注入依赖，它会在Spring容器中查找匹配类型的bean，并注入到目标对象中。
   - `@Resource` 通过名称（byName）来自动注入依赖，它会根据指定的名称在Spring容器中查找匹配的bean，并注入到目标对象中。

3. **可选性**:
   - `@Autowired` 是非必需的，如果找不到匹配的bean，不会引发异常。你可以使用`@Autowired(required = false)`来标记它为非必需的。
   - `@Resource` 是必需的，如果找不到匹配的bean，会引发异常。但你可以通过`@Resource(name = "beanName", required = false)`来指定为非必需的。

4. **更多配置选项**:
   - `@Autowired` 提供了更多的配置选项，例如可以与`@Qualifier`注解一起使用，以指定要注入的bean的名称。
   - `@Resource` 的配置选项较少，主要用于指定要注入的bean的名称。

举个例子，假设有以下两个bean：

```java
@Service
public class UserService {
    // ...
}
```

```java
@Repository("customRepository")
public class UserRepository {
    // ...
}
```

使用 `@Autowired`：

```java
@Autowired
private UserService userService;

@Autowired
@Qualifier("customRepository")
private UserRepository userRepository;
```

使用 `@Resource`：

```java
@Resource
private UserService userService;

@Resource(name = "customRepository")
private UserRepository userRepository;
```

总之，`@Autowired` 和 `@Resource` 都是依赖注入的方式，但它们有不同的来源、类型匹配方式、可选性和配置选项。你可以根据项目的需要选择合适的注解来进行依赖注入。在大多数情况下，`@Autowired` 是更常用的方式，因为它提供了更多的配置选项和灵活性。但在一些特定的Java EE环境中，你可能需要使用 `@Resource`。

## restcontroller 返回ResoponseEntity<>()或者dto
```java
    @ApiOperation(value = "Get All Comments by Post ID REST API")
    @GetMapping("/posts/{postId}/comments")
    public List<CommentDto> getCommentsByPostId(@PathVariable(value = "postId") Long postId) {
        return commentService.getCommentsByPostId(postId);
    }

    @ApiOperation(value = "Get single Comment by ID REST API")
    @GetMapping("/posts/{postId}/comments/{id}")
    public ResponseEntity<CommentDto> getCommentsById(
            @PathVariable(value = "postId") Long postId,
            @PathVariable(value = "id") Long commentId) {

        CommentDto commentDto = commentService.getCommentById(postId, commentId);
        return new ResponseEntity<>(commentDto, HttpStatus.OK);
    }
```
在这个例子中，可以直接返回DTO是因为Spring Boot的自动序列化特性。Spring Boot内置了Jackson库，它可以将Java对象（包括DTO）自动转换为JSON格式（或其他格式，例如XML），从而使得返回DTO直接作为响应体成为可能。

在这里，`CommentDto`是一个自定义的数据传输对象（DTO），它可能有一些字段用于表示评论的信息。当调用这两个API时，Spring Boot会自动将`CommentDto`对象转换为JSON格式，并将其作为响应体发送给客户端。

Spring Boot的自动序列化工作是通过`@RestController`注解实现的，它实际上是一个组合注解，包含了`@Controller`和`@ResponseBody`两个注解。`@Controller`用于标记这个类是控制器，而`@ResponseBody`则告诉Spring Boot返回的对象将直接作为响应体，而不是解析为视图。
```java
    @ResponseBody
    // 有@RestController 就不用上面这个注释了
    public String getName() {
        return "John Doe";
    }
```

因此，当控制器使用`@RestController`注解标记时，Spring Boot会自动处理DTO对象的序列化，从而将DTO对象转换为JSON格式的响应体。这使得在控制器中直接返回DTO对象成为可能。


## GlobalExceptionHandler

application properties:
```xml
spring.web.resources.add-mappings=false
server.error.include-message=always
server.error.include-binding-errors=always
```


然后添加处理类：
```java
package com.example.demospring.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.NoHandlerFoundException;

@ControllerAdvice
public class GolbalExceptionHandler {
    @ExceptionHandler(NoHandlerFoundException.class)
    public ResponseEntity<?> handleNoHandlerFoundException(NoHandlerFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body("Incorrect Endpoint: " + e.getRequestURL());
    }

}
```

## bean scope
singelton: 计数器
prototype: 临时数据
request: http request。需要保存用户请求的信息，request holder类

### bean lifecycle
**实例化（Instantiation）**：在这个阶段，Spring IoC 容器会根据 Bean 的定义创建 Bean 的实例。这通常是通过**构造函数**（对于构造函数注入）或无参构造函数（对于默认构造函数注入）来完成的。

**属性注入（Dependency Injection**）：一旦 Bean 实例被创建，Spring IoC 容器将会注入 Bean 的依赖关系，包括其他 Bean、基本类型和属性。这是通过 setter 方法、字段注入或构造函数注入来完成的，具体取决于你的配置。

**初始化（Initialization**）：在初始化阶段，Spring IoC 容器会调用 Bean 的初始化方法，如果有的话。你可以使用 **@PostConstruct 注解或 InitializingBean 接口的 afterPropertiesSet 方法来定义初始化方法。**自定义的初始化逻辑，例如配置属性、建立数据库连接、启动定时任务等等

使用（In Use）：在初始化之后，Bean 就可以在应用程序中使用了。其他组件可以引用和调用这个 Bean。

销毁（Destruction）：当应用程序关闭时，Spring IoC 容器会销毁 Bean 实例。你可以使用 @PreDestroy 注解或 DisposableBean 接口的 destroy 方法来定义销毁方法。

## Spring 和spring boot优缺点
Spring是一个全功能的企业级Java应用程序开发框架，提供了大量的模块和功能，如依赖注入、AOP、事务管理、数据访问、Web开发等。「

1. Spring Boot是Spring Framework的一个子项目，旨在简化Spring应用程序的开发和部署。提供了自动配置功能，根据应用程序的依赖关系自动配置Spring的各个组件。
2. Spring Boot引入了"约定优于配置"的理念，通过默认约定来简化开发，并提供了大量的starter依赖，减少了对第三方库的手动集成。Convention over Configuration, reducing explicit configuration and relying on pre-defined conventions to simplify and streamline development processes. 
3. Spring Boot内置了一个嵌入式的Web服务器（通常是Tomcat），使得打包和运行应用程序变得更加容易。




## Spring task: 
application类上加上@EnableScheduling 或者专门在config包里新建一个SpringTaskConfig类，空白类，类前加上@EnableScheduling 
Spring Batch:

## Cookie and Session:
cookie: browser 
session: server


## Logging and Aspect(看小红书代码最好理解)

在上述示例中，LoggingAspect是一个切面类，使用@Aspect注解进行标记。在类中定义了两个通知方法：beforeMethodExecution和afterMethodExecution。这些通知方法分别在指定的切入点上执行相应的逻辑。在本例中，beforeMethodExecution方法在com.example.myapp.service包中的方法执行之前执行，afterMethodExecution方法在com.example.myapp.controller包中的方法执行之后执行。
```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.myapp.service.*.*(..))")
    public void beforeMethodExecution(JoinPoint joinPoint) {
        // 在方法执行之前执行的逻辑
        // 可以获取方法参数、目标对象等信息并执行相应操作
        System.out.println("Before method execution");
    }

    @After("execution(* com.example.myapp.controller.*.*(..))")
    public void afterMethodExecution(JoinPoint joinPoint) {
        // 在方法执行之后执行的逻辑
        System.out.println("After method execution");
    }

    // 其他通知的定义...

}

```
另外可以和pointcut 注释连用


## 测试：
`jacoco-maven-plugin`: code coverage
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
```

spring-boot-starter-test 依赖项提供了许多用于测试的库和工具，包括 JUnit、Mockito、Hamcrest 和 Spring Test 等。它还包括一些 Spring Boot 的测试支持类和注解，使测试变得更加方便和简化。

## Lombok和Logback

Lombok和Logback是两个不同的工具，它们在Java开发中有不同的用途。

1. Lombok:
Lombok是一个Java库，它通过自动生成样板代码（boilerplate code）来简化Java类的编写。使用Lombok，你可以通过在类上添加注解来自动生成构造方法、Getter/Setter方法、toString方法等常见的样板代码。它帮助减少了手动编写重复代码的工作量，提高了代码的可读性和可维护性。

Lombok提供了多个注解，包括`@Getter`、`@Setter`、`@NoArgsConstructor`、`@AllArgsConstructor`等。通过在类上添加这些注解，Lombok会根据注解生成相应的代码。例如，使用`@Getter`注解会自动生成相应字段的Getter方法。

使用Lombok需要将Lombok依赖添加到项目中，并在开发环境中安装Lombok插件，以使开发工具能够识别和处理Lombok注解。

2. Logback:
Logback是一个灵活且高性能的日志框架，它用于在Java应用程序中记录和管理日志信息。Logback是log4j框架的继承者，由同一个开发团队开发。它提供了可配置的日志记录器、输出器和格式化器，可以将日志记录到不同的目标，如控制台、文件、数据库等。

Logback具有灵活的配置选项，可以根据应用程序的需要进行配置和定制。它支持多种日志级别（如DEBUG、INFO、WARN、ERROR等），可以过滤和分类日志消息，并具有适应多线程环境的高性能特性。

在Java项目中，Logback通常与其他日志抽象库（如SLF4J）一起使用。通过在项目中配置合适的日志实现（如Logback），应用程序可以使用日志记录器接口编写日志代码，而具体的日志实现则由配置决定。

总结：Lombok用于简化Java类的编写，通过自动生成样板代码来减少重复工作量。而Logback是一个灵活和高性能的日志框架，用于在Java应用程序中记录和管理日志信息。它们在Java开发中扮演不同的角色，分别提供了简化类编写和强大的日志记录功能。


## Swagger


在Spring Boot项目中使用Swagger配置可以让你自动生成并展示API文档，方便API的查看和测试。下面是在`@Configuration`类中配置Swagger的简单示例：

首先，确保你的项目中引入了Swagger相关的依赖，包括`springfox-boot-starter`和`springfox-swagger-ui`，在`pom.xml`文件中添加以下依赖：

```xml
<!-- Swagger -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>3.0.0</version>
</dependency>
```

然后，在一个`@Configuration`类中添加Swagger的配置：

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.controller")) // 指定扫描的controller包
                .paths(PathSelectors.any())
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("My API Documentation") // 设置API文档的标题
                .description("API documentation for My Application") // 设置API文档的描述
                .version("1.0.0") // 设置API文档的版本号
                .build();
    }
}
```

上述配置会扫描指定的`com.example.controller`包下的所有Controller类，并将它们生成API文档。你可以根据自己的项目包结构和需求调整`basePackage`的值。

完成以上配置后，启动应用程序，访问`http://localhost:8080/swagger-ui/`即可查看自动生成的API文档。在Swagger UI页面上，你可以看到所有的API接口和相应的信息，包括请求和响应的参数、HTTP方法、接口描述等，方便开发和测试。

需要注意的是，Swagger提供了很多其他的配置选项，可以根据项目需求进行定制。以上示例只是一个简单的配置，更多的功能和配置可以在Swagger官方文档中找到。

## Runner
在Spring Boot中，`ApplicationRunner` 和 `CommandLineRunner` 是接口，用于在Spring Boot应用程序启动时执行特定的代码块。这些接口允许你在Spring应用程序启动后执行一些初始化或其他任务。

1. **ApplicationRunner**：这是一个函数式接口，它定义了一个名为`run`的方法，该方法在Spring应用程序完全启动后执行。`ApplicationRunner`的主要参数是`ApplicationArguments`，它提供了应用程序启动时传递的命令行参数。你可以使用它来执行各种初始化任务。

   ```java
   import org.springframework.boot.ApplicationArguments;
   import org.springframework.boot.ApplicationRunner;
   import org.springframework.stereotype.Component;
   
   @Component
   public class MyApplicationRunner implements ApplicationRunner {
       @Override
       public void run(ApplicationArguments args) throws Exception {
           // 在这里执行初始化任务
       }
   }
   ```

2. **CommandLineRunner**：这也是一个函数式接口，它定义了一个名为`run`的方法，该方法在Spring应用程序完全启动后执行。不同于`ApplicationRunner`，`CommandLineRunner`的参数是`String`数组，可以访问应用程序启动时的命令行参数。

   ```java
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.stereotype.Component;
   
   @Component
   public class MyCommandLineRunner implements CommandLineRunner {
       @Override
       public void run(String... args) throws Exception {
           // 在这里执行初始化任务，可以访问命令行参数
       }
   }
   ```

你可以根据需要选择使用`ApplicationRunner`或`CommandLineRunner`，具体取决于你的应用程序初始化需求以及你是否需要访问命令行参数。这两个接口的主要目的是在应用程序启动后执行自定义的初始化逻辑。


简化：
`ApplicationRunner` 和 `CommandLineRunner` 都是在Spring Boot应用程序启动后执行自定义初始化代码的接口，但它们的参数和用途略有不同。`ApplicationRunner` 接口的 `run` 方法的参数是 **ApplicationArguments**，用于访问应用程序启动时传递的命令行参数，而 `CommandLineRunner` 接口的 `run` 方法的参数是 **String数组**，用于访问命令行参数。因此，你可以根据需要选择一个接口来满足你的初始化需求和参数访问方式。



就是说，cli只能访问命令行传进来的参数，ali可以访问任何传递进来的参数


## page

### 理论

是的，`JpaRepository` 也包含了分页和排序的方法，因为它是 `PagingAndSortingRepository` 的子接口。所以，你可以使用 `JpaRepository` 来执行分页查询，就像使用 `PagingAndSortingRepository` 一样。

以下是一些常用的分页查询方法：

- `findAll(Pageable pageable)`：根据给定的分页信息查询所有实体。
- `findAll(Sort sort)`：根据给定的排序信息查询所有实体。
- `findAll(Pageable pageable)`：根据给定的分页和排序信息查询所有实体。
- `findAllById(Iterable<ID> ids, Pageable pageable)`：根据给定的ID列表和分页信息查询实体。
- `findAll(Specification<T> spec, Pageable pageable)`：根据给定的查询条件（`Specification`）和分页信息查询实体。

这些方法都可以在 `JpaRepository` 中找到，它们使得在 Spring Data JPA 中进行分页查询变得非常方便。

### 源码

当使用 Spring Data JPA 和 `Pageable` 进行分页查询时，通常会遵循以下步骤：

1. 创建实体类（Entity）。
2. 创建一个扩展 `JpaRepository` 的 Repository 接口，用于对实体进行数据库操作。
3. 创建一个 Service 类，用于处理业务逻辑。
4. 创建一个 Controller 类，用于处理请求并调用 Service 来获取分页数据。

以下是一个使用 `Pageable` 和 `JpaRepository` 的完整示例，假设你有一个名为 `Student` 的实体类。

首先，创建 `Student` 实体类：

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private int age;

    // 省略构造函数、getter 和 setter 方法
}
```

接下来，创建一个 `StudentRepository` 接口，继承自 `JpaRepository`：

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
}
```

然后，创建一个 `StudentService` 类，用于处理业务逻辑：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;

@Service
public class StudentService {

    private final StudentRepository studentRepository;

    @Autowired
    public StudentService(StudentRepository studentRepository) {
        this.studentRepository = studentRepository;
    }

    public Page<Student> getAllStudents(Pageable pageable) {
        return studentRepository.findAll(pageable);
    }
}
```

最后，创建一个 `StudentController` 类，用于处理请求并调用 `StudentService` 获取分页数据：

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class StudentController {

    private final StudentService studentService;

    @Autowired
    public StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    @GetMapping("/api/students")
    public Page<Student> getAllStudents(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size) {
        
        PageRequest pageRequest = PageRequest.of(page, size);
        return studentService.getAllStudents(pageRequest);
    }
}
```

在这个示例中，`StudentController` 接受两个分页参数 `page` 和 `size`，并使用 `PageRequest` 创建一个分页请求对象。然后，它调用 `studentService.getAllStudents(pageRequest)` 来获取分页的学生数据，最终将 `Page<Student>` 对象返回给客户端。

这个示例演示了如何在 Spring Boot 中使用 `Pageable` 和 `JpaRepository` 进行分页查询。

### 排序
在pageRequest中指定排序方式
```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

@Service
public class StudentService {

    // ...

    public Page<Student> getAllStudents(int page, int size, String sortBy) {
        Sort sort = Sort.by(sortBy).ascending(); // 按照指定属性升序排序
        Pageable pageable = PageRequest.of(page, size, sort);
        return studentRepository.findAll(pageable);
    }
}

```

### 简化流程
`org.springframework.data.domain
1. 说明JPARepository它**本身就支持分页和排序**，因为它是 PagingAndSortingRepository 的子接口
2. 自定义的repo implement JPA，所以可以**传入一个pageable对象给repo中的函数**
3. 在service类中使用**pageRequest** 类， 将需要的page size和page number转换成pageable对象（**pageRequest** 是**pageable**接口的具体实现 ），然后传给repo中的函数（比如findAll()），service类的函数返回一个page<student>。
4. 在controller中接受的参数里设置page size和page number，传给service类中的函数。

补充：controller可以直接返回Page<student>给前端，因为spring会自动将起转成Json。也可以显式创建一个ResponseEntity，放入Page<student>和http状态码。


