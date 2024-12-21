Encapsulation is used to Hide the internal details of an object Like we have sensitive data we can set the access modifier as private and define a get method access it

Abstraction: Abstraction allows developers to create simplified representations of real-world objects or concepts.

Inheritance: Inheritance allows one class (subclass or derived class) to inherit the properties and behaviors of another class (superclass or base class).

#### b. Polymorphism;

##### Overloading.

overload several method with same name takes different arguments

##### Overidding

Method overriding:

Method overriding is when a subclass provides its own implementation of a method that is already defined in its superclass

interfaces are used to define contracts that classes must adhere to, while abstract classes provide a combination of abstract and concrete methods, allowing for shared behavior and defining a common base for subclasses.

automatic It's a built in java feature to automatically free up memory no longer used by the program.

The `static` keyword is used to indicate the member belongs to a class itself instead of an instance of the class
the `final` keyword is used to indicate that a  variable, method, or class can not modified or extended,
"finally" is a block in Java used in conjunction with the try-catch block to handle exceptions and perform cleanup actions.
In summary, "final" is used to declare constants, prevent method overriding, and restrict class inheritance, while "finally" is used to guarantee execution of specific code, regardless of whether an exception is thrown or not.

hashmap O(1) put and get
The main differences between TreeMap and HashMap in Java are related to their underlying data structures, ordering of elements, and performance characteristics. 

Here's a summary of the key differences:
When a new key-value pair is added to the `HashMap`, the key's `hashCode()` method is called to compute its hash code value, which is used to determine the index of the bucket where the entry should be stored. Since there may be multiple keys with the same hash code value, the `equals()` method is used to check if the key already exists in the bucket. If a matching key is found, the corresponding value is updated; otherwise, a new entry is added to the bucket. If two or more entries are in the same bucket it  mean collision.

checked at the compile time
null pointer exception
stream
filter
map
Sorting:

Lambdas: Lambdas are anonymous functions that allow you to write inline code blocks in a more concise and expressive way. You can use lambdas with functional interfaces to implement single abstract methods.
thread pool
more flexible
`CompletableFuture` is a advanced version of  a future class.
It allow us to chain multiple dependent tasks: With the `Future` interface, you don't have an easy way to run tasks sequentially or in parallel. `thenApply()`, `thenAccept()`, `thenRun()`,  These methods allow you to run tasks sequentially or in parallel based on the completion of previous tasks, without the need for manual management.


get() will block the current thread
shared resources
wait() and notify()
join()、valatile use for atomic classes
it ensures that dependencies are fully prepared once the object is initialized. 

Allows dependencies to be declared as final, which makes them immutable,  to ensure that an object's state is consistent

 It simplifies unit testing by making it easy to provide mock or stub dependencies when creating instances of a class for testing.
它确保了一旦对象初始化，依赖项就完全准备好了。

允许将依赖项声明为final，这使得它们不可变，以确保对象的状态是一致的

它简化了单元测试，因为在创建用于测试的类的实例时，可以很容易地提供模拟或存根依赖项。

3 layers controller service dao
design api end point
http method
http method

Setup/Arrange
设置/设置
Exercise/Act
Verify/Assert
Teardown

Setup: Prepare the test environment and set up any necessary dependencies, objects, or data required for the test.
Test Execution: Call the method or functionality you want to test with specific inputs or conditions.

都发生 都不发生
**the JDBC process involves loading the driver, establishing a connection, creating and executing statements, processing results, and properly closing resources.** JDBC allows Java applications to interact with databases seamlessly and is widely used for database access in Java-based projects.
Establish a Connection:
Create a Statement or PreparedStatement:
Execute SQL Queries:
Process the Results:
Close the Resources:
API Gateway, Load Balancer, Service Discovery(Eureka), Service Registry, circuit breaker, service Monitoring, Config server

fast deployment, ease of creating new instances, and faster migrations. Ease of moving and maintaining your applications. Better security, less access needed to work with the code running inside containers, and fewer software dependencies.
fast deploy

singleton
circuit breaker
factory 
最外层的if 应该是== null  里层的也是

volatile的作用是确保全都从主线程获取

btw
volatile关键字的目的是告诉虚拟机：
每次访问变量时，总是获取主内存的最新值；
每次修改变量后，立刻回写到主内存。