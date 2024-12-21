这里记录的是
https://docs.google.com/spreadsheets/d/1tonOgspEXaelZYmGIvYLgPXWCKjSFu024rXynu7Zq5w/edit#gid=0
链接里部分问题的答案
## What is Final key word?(面经*1)
final fields means the fields can not be changed, for methods means tis methods can not be override, for class means this class can not be inheritance.
1. Final Variables: When applied to a variable, the `final` keyword ensures that its value cannot be changed once assigned. It is commonly used to define constants that should not be modified.
    
2. Final Methods: When applied to a method, the `final` keyword prevents the method from being overridden by subclasses. This is useful when you want to ensure that the behavior of a method remains unchanged throughout the inheritance hierarchy.
    
3. Final Classes: When applied to a class, the `final` keyword prevents the class from being extended. It signifies that the class cannot be subclassed and its implementation is considered complete and not intended for further modification.
## What is static field, static method and static class?
Static fields are fields that belong to a class rather than an instance and are accessed directly by the class name.
A static method is a method that belongs to a class rather than an instance and is called directly by the class name.
A static class is a class nested within another class and declared static to organize and encapsulate helper functions or utility methods that are closely related to the external class.

静态字段（Static Field）是指属于类而不是实例的字段。它们在类加载时创建，并在整个程序执行过程中保持相同的值。静态字段可以通过类名直接访问，而不需要创建类的实例。

静态方法（Static Method）是属于类而不是实例的方法。它们与静态字段类似，在类加载时创建，并通过类名直接调用，而不需要创建类的实例。静态方法通常用于提供与类相关的实用函数或共享的功能。

静态类（Static Class）是指嵌套在另一个类中且被声明为静态的类。静态类只能访问外部类的静态成员，无法访问外部类的非静态成员。静态类通常用于组织和封装与外部类紧密相关的辅助功能或实用方法。
## Final Finalized finally
final：final 是一个关键字，用于**声明变量、方法或类**，表示它们不能被修改或扩展。当应用于变量时，表示该**变量的值不可更改**。当应用于方法时，表示**该方法不能在子类中被重写**。当应用于类时，表示**该类不能被继承**。

finalize：finalize 是在 Object 类中定义的一个方法，在对象被垃圾回收器回收之前被调用。它允许对象在被销毁前执行必要的清理操作或释放资源。然而，需要注意的是，由于其不确定性和性能影响，现代的 Java 编程不推荐使用 finalize 方法。

finally：finally 是异常处理中的一个关键字，用于确保一段代码始终被执行，无论是否发生异常。finally 块在 try 块（和相关的 catch 块）之后执行，在控制流离开包围的 try-catch-finally 结构之前执行。通常用于释放资源、关闭连接或执行其他清理任务。

## What is volatile?
the volatile keyword is used to indicate that a variable is shared among multiple threads and its value can be modified by any thread. When a variable is declared as volatile, it guarantees that any read or write operation on that variable will be directly performed on the main memory, ensuring visibility to all threads.

## @Configuration
自定义配置。比如swagger。然后用@bean导入第三方依赖

## How do you deal with the response get from REST api? / How to convert JSON -> Java Object?
Jackson Object mapper.是spring boot默认的转换器，会配置好。
## How to use docker in the Spring Boot?
"To use Docker in a Spring Boot application, you can follow these steps:

1. Dockerize your Spring Boot application:
   - Create a Dockerfile in the root directory of your project.
   - Specify the base image for your application, such as `openjdk:8-jdk-alpine`.
   - Copy the JAR file of your Spring Boot application into the Docker image.
   - Set the entry point command to run the Spring Boot application.

2. Build a Docker image:
   - Open a terminal or command prompt and navigate to the directory containing the Dockerfile.
   - Run the Docker build command to build the Docker image:
     ```
     docker build -t your-image-name .
     ```

3. Run the Docker container:
   - After the Docker image is built, you can run it using the Docker run command:
     ```
     docker run -p host-port:container-port your-image-name
     ```
     Replace `host-port` with the port number on the host machine that you want to map to the container's port. Replace `container-port` with the port number on which your Spring Boot application is listening.

   - The Spring Boot application inside the Docker container will now be accessible on the specified host port.

Using Docker with Spring Boot allows you to create a self-contained and portable deployment artifact, ensuring consistency across different environments. It simplifies the deployment process and makes it easier to manage dependencies and configurations.

Note: Make sure you have Docker installed and running on your machine before following these steps."

![[Pasted image 20230822145829.png]]