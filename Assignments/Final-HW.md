## Maven
1. What is the lifecycle of maven? could you tell me the details ?
Default Lifecycle:
- Prepareresources: Copies resources
- Validate: Validates if the project is correct and all necessary information is available.
- Compile: Compiles the source code of the project.
- Test: Tests the compiled source code suitable for testing framework.
- Package: Packages the compiled code into a distributable format, such as a JAR or WAR file.
- Verify: Performs any checks to verify the integrity of the package.
- Install: Installs the package into the local Maven repository, making it available for other projects.
- Deploy: Copies the final package to a remote repository, making it accessible to other developers and projects.
   
2. what is the difference between package and install in maven lifecycle ?
   **The "package" phase generates the artifact** locally for testing and sharing, while the **"install" phase copies the artifact to the local Maven repository**, making it available as a dependency for other projects within the same development environment.

Package Phase:

- Purpose: The "package" phase is responsible for packaging the compiled code and resources into a distributable format, such as a JAR or WAR file.
- Output: It generates the artifact in the target directory of the project, usually with a predefined naming convention based on the project's configuration.

Install Phase:

- Purpose: The "install" phase is responsible for installing the packaged artifact into the local Maven repository.
- Output: It copies the artifact from the project's target directory to the local repository, along with the corresponding POM file.

## Linux
https://www.yuque.com/fairy-era/yg511q/oeybmv#4975ebe7
### 用户逻辑
- 每创建一个用户，系统就会在 `/home` 目录下创建一个和用户同名的目录作为这个用户的家目录，比如：创建一个名为 `tom` 的用户，它的家目录就是 `/home/tom`。可以使用 `~` 代表用户的家目录。如果是系统的超级管理员 `root` ，`root` 用户的家目录是 `/root` 目录
- /etc 存放系统和第三方应用程序的配置文件。/home存放普通用户家目录。/opt 存放安装第三方应用程序时使用的压缩包文件。 /root 超级管理员root用户的家目录。/usr 应用程序的默认安装目录，类似于Windows下的program files目录。/var 存放经常变化的内容，例如日志文件。
### 常用命令
```shell
mkdir
cd
ls -l(详细) -a(隐藏) -r(递归)
ls -l = ll
pwd
touch
cp 被复制的文件的路径 目标目录的路径
cp -r 被复制的目录的路径 目标目录的路径
mv 被移动的文件或目录的路径 目标目录
rm -f 强制删除 -r 递归删除 -rf

vim:搜索： / + 内容

```

![[Pasted image 20230706134813.png]]
```shell
grep 参数 查找内容 文件路径
- 参数 `-v` 表示返回不匹配的行。

- 参数 `-n` 表示显示行号。

- 查找内容可以是正则表达式。

- 作用：将文件内容中匹配的行返回。

命令 A | 命令 B
- 作用：将命令 A 的输出作为命令 B 的输入。
```
#### ps
```shell
ps [-ef]
- 参数 `-e` （entire，表示全部）指显示系统中全部的进程信息。

- 参数 -f （full-formate）表示完整格式。

- 作用：查看系统中运行的进程。


kill -9 进程id
```

## Java
### Java Core & OOP
1. Write up Example code to demonstrate the three fundamental concepts of OOP.
(reference Code Demo repo as example)

a. Encapsulation;
**keyword: access modifier, hide details of a class/function and provide interface to operate** 

b. Polymorphism;
**keyword: methods with same name have more than one underlying types ** 
i. Override, Overloading.	

c. Inheritance;
**keywords: super class; sub class; sub class can have same  properties  and similar behaviors that defined in super class but subclasses can have their own implement of the inherited behaviors**

d. abstraction: focus on **extracting features of some real-world things** and represent it as a class. and **define some common methods?**

#### 垃圾回收
It is **a process in Java Virtual Machine (JVM)** that **automatically** reclaims memory occupied by objects that are no longer in use. Garbage collector identifies and removes objects that are **no longer reachable or referenced** by any part of the program. It tracks the usage of objects and determines which objects are still needed and which can be safely removed.
Several mechanisms:
	Serial Collector, Parallel Collector.
	Concurrent Mark-Sweep (CMS) Collector:  
		The collector starts a stop-the-world pause during which it identifies the root objects and mark them as live. After, the collector resumes the threads while concurrently traversing the object graph and mark objects that are reachable. If any change happens, the collector pause all of the threads, capture the change and remark them. Finally it will conduct a concurrent sweep by sweeping the memory regions to identify and reclaim the unreachable objects.
	
Garbage-First (G1) Collector: 
		From JDK 9, G1 replaces the CMS as the default GC.
		It is designed to provide better performance, lower pause times, and smaller heap space occupation. Key words: Region-Based Memory Management, Concurrent and Incremental, Adaptive Collection Strategy, Generational Concept, Heap Compaction, Predictable Pause Times.
		About  Predictable Pause Times: It achieves this by performing garbage collection incrementally, focusing on smaller regions, and employing heuristics to determine the amount of work performed in each collection cycle. This makes it suitable for latency-sensitive applications.
		
Ask G1 details Answer:
		The G1 Collector is a region-based garbage collector that divides the heap into multiple regions and uses a combination of concurrent marking, evacuation, and compaction techniques to manage memory.  The G1 Collector performs most of its garbage collection work concurrently with the application threads. It operates in cycles, where it collects garbage from a subset of regions called "garbage-first" regions.  These regions are selected based on their occupancy and the amount of garbage they contain.  The G1 Collector dynamically adjusts the sizes of the young and old generations, adapting the length of collection cycles, and dynamically prioritizing which regions to collect. The G1 Collector dynamically adjusts its collection strategy based on factors such as available CPU resources, application throughput goals, and desired pause time targets.


#### Exception

3. What is Runtime/unchecked exception? what is Compile/Checked Exception?
	a. also reading： https://www.yuque.com/fairy-era/yg511q/gldkel#b861aad7

	Runtime or Unchecked exceptions are exceptions that are **not required to be declared in a method's signature or caught by the caller.**  Unchecked exceptions are intended to represent serious or irrecoverable errors that **should be caught and fixed during development**. Examples of unchecked exceptions include NullPointerException, ArrayIndexOutOfBoundsException, and IllegalArgumentException.
	
	Checked exceptions are those that are checked at **compile-time** (and must be handled using try-catch or declared using the throws keyword). If a method throws a checked exception, then the calling method must either catch that exception or declare that it throws that exception. Some common examples of checked exceptions include **IOException, SQLException, and ClassNotFoundException**. Compile-time exceptions are subclasses of the `Exception` class but not subclasses of `RuntimeException` or its subclasses. **FileNotFindException** 也是compile time.


4. What is the difference between throw and throws?
   `throw` is used to explicitly throw an exception within a method or block, while `throws` is used in a method declaration (method signature) to specify the exceptions that the method possibly may throw, allowing the caller to handle or propagate them.

	`throw` is followed by an instance of an exception class or a subclass of `Throwable`
```java
public void divide(int dividend, int divisor) {
    if (divisor == 0) {
        throw new ArithmeticException("Divisor cannot be zero.");
    }
    int result = dividend / divisor;
    System.out.println("Result: " + result);
}


public void readFile(String filename) throws FileNotFoundException,
												IOException {
    // Code to read the file
}

```



`throw`和`throws`是Java中用于异常处理的关键字，它们有以下区别：

1. `throw`关键字用于在代码中显式地抛出一个异常对象。它通常用于方法内部，用于指示发生了某种异常情况，并将异常传递给调用方处理。使用`throw`关键字可以抛出任何类型的异常对象。

代码示例：
```java
public void validateAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
}
```

2. `throws`关键字用于方法声明中，表示该方法可能抛出的异常类型。它通常用于声明受查异常（checked exception），以便调用方知道在调用该方法时可能需要处理这些异常。一个方法可以声明抛出多个异常，多个异常类型之间用逗号分隔。

代码示例：
```java
public void readFile(String filename) throws IOException, FileNotFoundException {
    // 读取文件的代码
}
```

总结起来，`throw`用于在代码中显式地抛出异常对象，而`throws`用于方法声明中指明该方法可能抛出的异常类型。`throw`关键字用于抛出异常，而`throws`关键字用于声明可能抛出的异常。这两个关键字在异常处理中起着不同的作用。
#### 抛出异常后怎么办
当在代码中使用`throw`关键字抛出异常之后，程序的正常执行流程会被中断，跳转到异常处理的机制。

处理异常的方式通常有两种：

1. 捕获异常（Catch Exception）：可以使用`try-catch`块来捕获并处理异常。在`try`块中编写可能抛出异常的代码，然后在相应的`catch`块中捕获并处理异常。通过捕获异常，可以在发生异常时执行特定的处理逻辑，以便程序可以继续执行。

```java
try {
    // 可能抛出异常的代码
    throw new Exception("Something went wrong");
} catch (Exception e) {
    // 处理异常的代码
    System.out.println("Exception caught: " + e.getMessage());
}
```

2. 声明抛出异常（Declare Exception）：在方法声明中使用`throws`关键字指明该方法可能抛出的异常类型。通过在方法声明中声明抛出异常，可以将异常的处理责任交给调用方。调用方可以选择捕获异常或继续将异常向上抛出，直到有能力处理该异常的代码。

```java
public void doSomething() throws IOException {
    // 可能抛出IOException的代码
    throw new IOException("IO Exception occurred");
}
```

通过捕获异常或声明抛出异常，可以确保在出现异常时能够进行适当的处理，以避免程序的崩溃或意外行为。根据实际需求和异常类型，选择适当的处理方式以保证程序的正常执行。


5. Could you give me one example of NullPointerException?
```java
public class Customer {
    private String name;
    private Address address;
    // Constructor, getters, and setters

    public String getCustomerCity() {
	    // Potential NPE if address is null
        return address.getCity(); 
    }

	/* avoid NPE
	public String getCustomerCity() {
        return address.map(Address::getCity).orElse("Unknown City");
    }
    */
}

public class Address {
    private String city;
    // Constructor, getters, and setters
}

// Usage
Customer customer = new Customer();
customer.setName("John Doe");
// Potential NPE if address is null
String city = customer.getCustomerCity(); 
```
6. Collections：
   a. reading: https://www.yuque.com/fairy-era/yg511q/ksp07m#daf18702
	i. Not necessary to read how to implement the data structure

7. Reading: https://www.yuque.com/fairy-era/yg511q/qzv31t#347111bb
   a. ⼀些OA要⽤到IO

### External Libs
1. What is Guava? (https://www.tutorialspoint.com/guava/index.htm). No need to learn it. it is good enough to hear about it and know its role.
   
   Guava is an open-source Java library developed by Google. It provides a set of powerful and convenient utility classes and methods to enhance Java programming. It offers features such as collections, IO operations, concurrency utilities, string processing, and more. I have used Guava in several projects to leverage its functionality and improve code readability and efficiency.
   
Sample Detailed Answer:
    In a previous project, we were developing an e-commerce platform that required extensive validation and manipulation of user input data. We utilized Guava's Preconditions class to perform input validation with concise and readable code. This helped us ensure the integrity of the data and handle invalid inputs gracefully. Additionally, we leveraged Guava's Function and Predicate interfaces to implement complex data transformations and filtering operations on collections. The functional programming style facilitated cleaner and more maintainable code. By using Guava, we achieved improved code quality, reduced boilerplate code, and enhanced readability, resulting in faster development cycles and fewer bugs
```java
import com.google.common.base.Preconditions;
import com.google.common.base.Predicate;
import com.google.common.collect.Collections2;
import com.google.common.collect.Lists;

import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> userInput = Lists.newArrayList("John", "Doe", "", "Smith", "12345");

        // Input validation using Guava Preconditions
        for (String input : userInput) {
            Preconditions.checkNotNull(input, "Input cannot be null");
            Preconditions.checkArgument(!input.isEmpty(), "Input cannot be empty");
        }

        // Data transformation using Guava Function
        List<Integer> transformedData = Lists.transform(userInput, Integer::parseInt);

        // Filtering using Guava Predicate
        Predicate<Integer> positiveNumberPredicate = num -> num > 0;
        List<Integer> filteredData = Lists.newArrayList(Collections2.filter(transformedData, positiveNumberPredicate));

        System.out.println("Filtered data: " + filteredData);
    }
}

```
2. List and explain some methods from Apache Commons/Collections
	a. https://www.baeldung.com/apache-commons-collection-utils)
	b. https://developer.aliyun.com/article/896751
	
	Apache Commons Collections is a popular Java library that provides additional data structures, utilities, and methods to enhance the functionality of Java Collections Framework. Here are some commonly used methods from Apache Commons Collections:
	`ArrayUtils.contains(Object[], Object)`: This method checks if an array contains a specific object and returns a boolean value.
	`ListUtils.removeAll(List, Collection)`: It removes all elements from a list that are contained in the given collection.
	`MapUtils.isEmpty(Map)`: This method checks if a given map is null or empty, returning a boolean value.
	`IterableUtils.find(Iterable, Predicate)`: It searches for the first element in an iterable that matches the given predicate and returns it.
	
3. How to convert json format string to java object? and how to convert java object to json string? write some demo codes.

a. ObjectMapper from Jackson, which is default in Spring framework.
```java
public static void jacksonMapper(){
	String jsonString = "{\"name\":\"John\", \"age\":30}";
	ObjectMapper objectMapper = new ObjectMapper();
	Person person = null;
	try {
		person = objectMapper.readValue(jsonString, Person.class);
	} catch (JsonProcessingException e) {
		throw new RuntimeException(e);
	}
	System.out.println(person.getName()); // Output: John
	System.out.println(person.getAge()); // Output: 30

	// Convert Java object to JSON string
	Person personObj = new Person("Jane", 25);
	String json = null;
	try {
		json = objectMapper.writeValueAsString(personObj);
	} catch (JsonProcessingException e) {
		throw new RuntimeException(e);
	}
	System.out.println(json); // Output: {"name":"Jane","age":25}
}
```
b. Gson grom google.
```java
public static void gsonMapper(){
	String jsonString = "{\"name\":\"John\", \"age\":30}";
	Gson gson = new Gson();
	Person person = gson.fromJson(jsonString, Person.class);
	System.out.println(person.getName()); // Output: John
	System.out.println(person.getAge()); // Output: 30

	// Convert Java object to JSON string
	Person personObj = new Person("Jane", 25);
	String json = gson.toJson(personObj);
	System.out.println(json); // Output: {"name":"Jane","age":25}
}
```


### Java 8
1. List Several Java 8 new features and briefly explain them.
   **Lambda Expressions:** Lambda expressions introduce a **concise way to represent anonymous functions.** They enable functional programming in Java and make the code more readable and expressive. like allowing functions to be passed as arguments
   **Stream API:** The Stream API provides a declarative and **functional way to process collections of data**. It allows for parallel execution, provides various operations like filtering, mapping, and reducing, and promotes more efficient and readable code. It makes the calculation  of dataflow more convenient and readable.
   **Optional Class:** The Optional class is used to handle nullable values in a more explicit and safer way. It encourages developers to write more robust code by **indicating the possibility of null values and avoiding null pointer exceptions.**
2. practice stream API at least 3 times (vendor ⾯试会给个情景让写stream，⼯作上也会⼤量⽤到。)
	a. https://blog.devgenius.io/15-practical-exercises-help-you-master-javastream-api-3f9c86b1cf82




### Multiple Threading
1. What is deadlock?
    Two or more threads are blocked indefinitely, **waiting for each other to release resources**. It occurs when each thread holds a resource that is required by another thread, creating a circular dependency. As a result, the threads remain in a state of waiting, unable to proceed with their execution. Deadlocks can lead to system instability and can be challenging to detect and resolve. Preventing deadlock involves careful resource allocation and synchronization management to avoid circular dependencies.
2. How to create a new thread(Please also consider Thread Pool case)?
	
   1. extends Thread Class
   ```java
	public class MyThread extends Thread {
		@Override
		public void run() {
		System.out.println("start new thread using extends
		thread");
		}
	}
		Thread t = new MyThread(); // JVM没有创建thread
		t.start(); // 此时JVM才创建新的thread
	```
	2. implements Runnable
	   注意方法名
	```java
	public class MyRunnable implements Runnable{
		@Override
		public void run() {
			System.out.println("Start new thread using Runnable");
		}
	}
	Thread t2 = new Thread(new MyRunnable());
	```
	 3. Implements Callable
	    注意方法名字是call()
	```java
	public class MyCallable implements Callable<String> {
		@Override
		public String call() throws Exception {
			Thread.sleep(5000);
			return "Start new thread using Callable";
		}
	}
	```
	4. Thread pool: ExecutorService
	   FixedThreadPool: 线程数固定的线程池；
	   CachedThreadPool：线程数根据任务动态调整的线程池
	   SingleThreadExecutor：仅单线程执⾏的线程池。
	```java
	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	
	public class ExecutorServiceExample {
	    public static void main(String[] args) {
	        ExecutorService executor = Executors.newFixedThreadPool(2);
	
	        executor.submit(() -> {
	            // 执行的任务1 callable or runnable
	            System.out.println("Task 1");
	        });
	
	        executor.submit(() -> {
	            // 执行的任务2 callable or runnable
	            System.out.println("Task 2");
	        });
	
	        // 关闭线程池
	        executor.shutdown();
	    }
	}
	```
	5. Completable future:
	```java
	import java.util.concurrent.CompletableFuture;
	public class CompletableFutureExample {
	    public static void main(String[] args) {
	        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
	            // 异步执行的任务
	            // 返回结果"Hello"
	            return "Hello";
	        });
	        future.thenAccept(result -> {
	            // 在任务完成后执行的操作
	            System.out.println(result + " World");
	        });
	        // 等待任务完成
	        future.join();
	    }
	}
	```
1. Difference between Runnable and Callable？
   Runnable have no return.
   Callable can have return value.
   
   **Callable throws checked** exception.  
   **Runnable can throw runtime exception** using try catch.
   
1. what is the diff between t.start() and t.run()?
   t.start starts a new thread to execute the task
   t.run() execute the task in the current thread.
**怎么运行一个线程**

一： **callable 和 runnable都是functional interface.**(注意里面的唯一method). 他们通常是通过线程池**ExecutorService类对象的submit()** 去执行

二：Thread class则是在继承thread之后，重写run()方法。然后对象调用.start()

三：线程池运行thread class一般是不可行的。但是可以包装成callable运行：
线程池通常不会直接接受 Thread 类的实例作为任务进行提交。线程池的主要作用是管理线程的生命周期和执行任务。它会接受实现了 Runnable 接口或 Callable 接口的任务对象，并负责调度线程执行这些任务。

如果你有一个 Thread 类的实例，并想要在线程池中执行它，通常的做法是将其包装成一个 Runnable 或 Callable 任务，然后将这个任务提交给线程池。例如：
```java
Thread thread = new Thread(() -> {
    // 这里是线程执行的逻辑
});

ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(thread); // 错误的方式，不能直接提交 Thread 类的实例

// 正确的方式：将 Thread 包装成一个 Runnable 任务
executor.submit(() -> {
    thread.start(); // 启动 Thread 类的实例
});
```
在上面的示例中，我们首先创建了一个 Thread 类的实例 thread，然后将其包装在一个 Runnable 任务中，并将这个任务提交给线程池。这样可以确保 Thread 实例在线程池中执行。

四： future 和 completable future除了可以按上面的例子执行外，更常用的是和线程池一起用：
```java
public class OrderProcessingExample {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(3); // 创建线程池
        Inventory inventory = new Inventory(50); // 初始库存

        CompletableFuture<Void> orderProcessing = CompletableFuture.runAsync(() -> {
            Order order = new Order(1); // 创建订单
            int orderQuantity = 10;

            // 模拟订单处理
            System.out.println("Processing Order #" + order.getOrderId());

            if (inventory.reserve(orderQuantity)) {
                System.out.println("Order #" + order.getOrderId() + " is approved.");
            } else {
                System.out.println("Order #" + order.getOrderId() + " is rejected due to insufficient stock.");
            }
        }, executor);

        CompletableFuture<Void> shipping = CompletableFuture.runAsync(() -> {
            // 模拟发货
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Order is shipped.");
        }, executor);

        // 等待所有任务完成
        CompletableFuture<Void> allTasks = CompletableFuture.allOf(orderProcessing, shipping);
        // join其实可以不用，是为了防止task还没结束main就结束了
        // 但是 all of就会等待task结束。
        x.join();

        // 关闭线程池
        executor.shutdown();
    }
}
```


1. how do threads communicate with each other?
   - 共享内存和消息队列
   Multiple threads can access and modify **shared variables** or data structures. However, this approach requires proper synchronization mechanisms to ensure thread-safety and prevent data races or inconsistent states. Synchronization tools like locks, semaphores, or atomic variables can be used to coordinate access to shared resources.
   - **signal: synchronized lock, wait notify**
   - Threads **send messages** containing information or requests to other threads, which then process the messages and respond accordingly. Message passing can be implemented using various mechanisms, such as queues, channels, or explicit message passing libraries. Examples of message passing implementations in Java include the BlockingQueue interface or the java.util.concurrent package.
3. What is Atomic classes? when do we use it?
	They provide atomic operations for specific data types, ensuring thread safety without the need for explicit synchronization.AtomicInteger, AtomicBoolean, and AtomicReference, are part of the `java.util.concurrent.atomic` package.
	
	Usage:
	**Counter or Accumulator**: Atomic classes are useful when multiple threads need to increment or update a shared counter or accumulator. For example, in a multi-threaded application where multiple threads are processing tasks and updating a shared counter, using AtomicInteger ensures that the increments are atomic, preventing race conditions and guaranteeing correctness.
	**State Flag**: Atomic classes can be used to represent shared state flags that need to be modified atomically. For instance, AtomicBoolean can be used to coordinate the execution of threads based on a shared flag, ensuring that the flag is read and modified atomically by different threads.
	**Reference Updates**: Atomic classes like AtomicReference are used when updating references atomically. In scenarios where multiple threads need to update a shared reference to an object, using AtomicReference ensures that the updates are atomic and visible to other threads, preventing inconsistent or incorrect references.
1. What is the concurrent collections?
   **Thread-safe versions of standard collections** that provide support for concurrent access and modification by multiple threads. They are part of the **java.util.concurrent package** and are designed to handle concurrent operations **without requiring external synchronization**. Concurrent collections are particularly useful in concurrent programming scenarios, such as multi-threaded applications, thread pools, or when multiple **threads need to share and modify collections**
   
	Some commonly used concurrent collections in Java include:
	**ConcurrentHashMap**: A thread-safe implementation of the Map interface, allowing multiple threads to read and write concurrently without external synchronization.
	
	**ConcurrentSkipListMap** and **ConcurrentSkipListSet**: Concurrent versions of SortedMap and SortedSet interfaces that support concurrent access with sorted ordering.
	
	ConcurrentLinkedQueue and ConcurrentLinkedDeque: Thread-safe implementations of Queue and Deque interfaces that provide high-concurrency operations for adding, removing, and iterating over elements.
	
	**CopyOnWriteArrayList and CopyOnWriteArraySet**: Thread-safe implementations of List and Set interfaces that create a new copy of the underlying array on every modification, ensuring safe concurrent access during iteration.
   
2. What is keyword synchronized, how do you understand it?
   It is used to **control access to shared resources or critical sections of code**. It ensures that only **one thread at a time** can execute a synchronized block or method on a particular object.
   When a method or block is declared as synchronized, it acquires a lock on the associated object before executing(**获取的是method所在对象的锁，也就是多个syn-method不可能同时执行**). If multiple threads try to access the synchronized code, they will be blocked and wait until the lock is released.
   
1. What is future and completableFuture?
   They both represent **the future result of an asynchronous computation or task.** They provide a way to interact with the outcome of an asynchronous operation and **perform further actions when the result becomes available.**
   **Future** provides methods to check if the computation is done, retrieve the result, and cancel the computation if needed.
   **CompletableFuture** is an extension of Future. It supports **chaining** and composing multiple asynchronous operations together, allowing you to **express complex asynchronous workflows** in a more concise and readable way.
   `thenApply()`,`thenCompose()`, and `thenCombine()`.
1. leetcode for multiple threading (paste your solution code to here)
a. 1114- Print in Order
```java
class Foo {
    private volatile int flag = 1;
    private final Object fooLock = new Object();
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        synchronized(fooLock) {
            while (flag != 1) fooLock.wait();
            printFirst.run();
            flag = 2;
            fooLock.notifyAll();
        }
        // printFirst.run() outputs "first". Do not change or remove this line.
        
    }

    public void second(Runnable printSecond) throws InterruptedException {
        synchronized(fooLock) {
            while (flag != 2) fooLock.wait();
            printSecond.run();
            flag = 3;
            fooLock.notifyAll();
        }
        // printSecond.run() outputs "second". Do not change or remove this line.
        
    }

    public void third(Runnable printThird) throws InterruptedException {
        synchronized(fooLock) {
            while (flag != 3) fooLock.wait();
            printThird.run();
            flag = 3;
            fooLock.notifyAll();
        }
        // printThird.run() outputs "third". Do not change or remove this line.
        
    }
}
```
b. 1115-Print FooBar Alternately
```java
class FooBar {
    private int n;
    private volatile int flag = 0;
    private final Object lock = new Object();

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            synchronized(lock){
                if (flag == 1) lock.wait();
                if (flag == 0){
                    // printFoo.run() outputs "foo". Do not change or remove this line.
        	        printFoo.run();
                    flag = 1;
                    lock.notifyAll();
                }
                
            }
        	
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            synchronized(lock){
                if (flag == 0) lock.wait();
                if (flag == 1){
                    // printBar.run() outputs "bar". Do not change or remove this line.
        	        printBar.run();
                    flag = 0;
                    lock.notifyAll();
                }
                
            }
            
        }
    }
}
```
c. 1116-Print Zero Even Odd
```java
class ZeroEvenOdd {
    private int n;
    private final Object lock = new Object();
    private volatile int flag = 1;
    
    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for(int i = 1; i <= n; i++) {
            synchronized(lock) {
            while (flag % 2 == 0) lock.wait();
            printNumber.accept(0);
            flag++;
            lock.notifyAll();
            }
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for(int i = 2; i <= n; i+=2) {
            synchronized(lock) {
                while (flag % 4 != 0) lock.wait();
                printNumber.accept(i);
                flag++;
                lock.notifyAll();
            }
        }
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for(int i = 1; i <= n; i+=2) {
            synchronized(lock) {
                while (flag % 4 != 2) lock.wait();
                printNumber.accept(i);
                flag++;
                lock.notifyAll();
            }
        }
    }
}
```

## Database
1. what is the difference between RDBMS and NoSQL?
   **Data model:** Relational Database organizes data in redefined schemas. It uses tables with rows and columns to store and manage data, which establishes relationships between tables using primary keys and foreign keys. While NoSQL databases have a flexible structure that allows for dynamic and schema-less data storage. They store data in various formats like key-value pairs, documents, column families, or graphs.
   
   Relational databases are NOT suited for hierarchical data storage while NoSQL database are.
   
   **Scalability:** NoSQL databases are designed for horizontal scalability, allowing them to handle large amounts of data and high traffic loads by distributing data across multiple servers. RDBMS are better in vertically scalability.  NoSQL focus on the availability while RDBMS focus on data consistency.
   
   **Querying:** NoSQL databases use various query languages, such as MongoDB's query language for document databases or Cassandra Query Language (CQL) for columnar databases. RDBMS uses SQL (Structured Query Language), are best suited for complex queries
   
   **Principles:** NoSQL follows CAP(consistency, availability, partition tolerance) while RDBMS follows ACID (Atomicity, Consistency, Isolation, Durability) .
   
2. In which cases you choose NoSQL or RDBMS?

**NoSQL:** 
- Data Model: When application requires a flexible and evolving schema that can handle unstructured or semi-structured data.
- Scalability: Needs to handle massive amounts of data and high traffic loads, and strong horizontal scalability. Or data pattern may be unpredictable or rapidly changing.
- High Performance instead of consistency: when application requires fast read and write operations, and eventual consistency is acceptable.
- Rapid Development: Need to quickly prototype and iterate on the application.

**SQL:**
- Data Model: Structured data and requires a fixed and well-defined schema. Or data has complex relationships between entities
- Data Integrity and Consistency: When maintaining strict data integrity and enforcing consistency is crucial for your application.
- Complex Querying: When your application requires complex querying capabilities and extensive support for join operations.
- Mature Ecosystem: When you need a wide range of tools, frameworks, and ORMs that have extensive support for SQL and RDBMS.

1. Do you know Cassandra and MongoDB?
   Cassandra is a distributed and highly scalable NoSQL database known for its excellent write performance, fault tolerance, and ability to handle large-scale data workloads. It uses **a wide column store data model** and provides **tunable consistency levels.** On the other hand, MongoDB is a document-oriented NoSQL database that stores data in flexible, JSON-like documents. It offers rich querying capabilities, powerful aggregation framework, and supports horizontal scalability through automatic sharding. MongoDB is well-suited for applications that require flexibility in data modeling and powerful querying.
   
   **Differences:**
   **Data Model:** Cassandra follows a wide column store model, while MongoDB uses a document-oriented model.
   **Consistency:** Cassandra offers tunable consistency levels, allowing for eventual consistency, while MongoDB provides strong consistency by default.
   **Scalability:** Cassandra is designed for linear scalability and can handle massive amounts of data, while MongoDB offers horizontal scalability through sharding.
   **Querying:** MongoDB's query language provides expressive querying capabilities, including rich filtering, aggregation, and indexing options, while Cassandra has limited query flexibility.
   
2. LEFT JOIN & RIGHT JOIN
   The LEFT JOIN returns all records from the left (or "left-hand") table and the matching records from the right (or "right-hand") table. If there is no match, NULL values are returned for the columns of the right table. In the result set, the rows from the left table will be included, even if there are no matches in the right table. The matched rows from the right table will be included as well. Right JOIN is on the contrary.
   If you want all records from the left table and matching records from the right table, you would use a LEFT JOIN. If you want all records from the right table and matching records from the left table, you would use a RIGHT JOIN.
3. SQL design: given two tables, product and order, how will you design?
```sql
-- Create the Product table
CREATE TABLE Product (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(255),
    price DECIMAL(10, 2),
);

-- Create the Order table
CREATE TABLE Order (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    -- Add other attributes specific to the order entity
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id) 
);
```
4. How to improve the database performance?
   **Indexing**: Identify frequently queried columns and create indexes on them.
   **Partitioning and Sharding:** Partitioning involves splitting large tables into smaller, more manageable pieces based on specific criteria, such as ranges or hash values. Sharding distributes data across multiple servers to handle large data volumes and improve scalability. This approach allows parallel processing and reduces the load on individual database nodes.
   **Query Optimization**: Analyze and optimize the SQL queries by ensuring they are well-structured, avoiding unnecessary joins or subqueries, and utilizing appropriate WHERE clauses and JOIN conditions.
   **Caching**: Utilize caching mechanisms, such as in-memory caches like Redis or Memcached, to store frequently accessed data or query results. Caching can reduce the need for repeated database queries and improve response times.
   **Connection Pooling:** Implement connection pooling to efficiently manage database connections, reducing the overhead of establishing new connections for each request.

   有时可以使用 JOIN 来替代子查询以提高性能。对于分页查询，使用 LIMIT 和 OFFSET 可以降低查询的复杂性。
1. LeetCode (paste your solution code to here)

595. BIG COUNTRIES
```SQL
# Write your MySQL query statement below
select name, population, area
from World
where area >= 3000000 or population >= 25000000 
```
1757. Recyclable and Low Fat Products
```sql
# Write your MySQL query statement below
select product_id
from Products
where low_fats = 'Y' and recyclable = 'Y'
```
584. Find Customer Referee
```mysql
# Write your MySQL query statement below
select name
from Customer
where referee_id != '2' or referee_id is null
```
183. Customers Who Never Order
```mysql
# Write your MySQL query statement below
select name as Customers
from Customers
where id not in (
    select CustomerId
    from Orders
)
```
175. Combine Two Tables
```mysql
# Write your MySQL query statement below
select firstName, lastName, city, state 
from Person natural left join Address
```
176. Second Highest Salary
```mysql
select max(salary) as SecondHighestSalary
from Employee
where salary not in (
  select max(salary)
  from Employee
)
```
177. Nth Highest Salary
```mysql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    Set N = N-1;
  RETURN (
      # Write your MySQL query statement below.
      select distinct(salary)
      from Employee
      order by salary desc
      limit 1 offset N
  );
END
```
181. Employees Earning More Than Their M
```mysql
# Write your MySQL query statement below
select e1.name as Employee
from Employee e1 join Employee e2 on e2.id = e1.managerId
where e2.salary <= e1.salary
```
196. Delete Duplicate Emails
```mysql
# Please write a DELETE statement and DO NOT write a SELECT statement.
# Write your MySQL query statement below
delete A 
from Person A, Person B 
where A.id > B.id and A.email=B.email;
```
180. Consecutive Numbers
```mysql
# Write your MySQL query statement below
SELECT distinct(t1.num) as ConsecutiveNums 
FROM logs t1, logs t2 , logs t3
WHERE t1.id = t2.id + 1 
AND t2.id = t3.id + 1 
AND t1.num = t2.num 
AND t2.num = t3.num
```
184. Department Highest Salary
```mysql
select Department.name as Department, Employee.name as Employee, salary as Salary
from Employee join Department on Employee.departmentId = Department.id
WHERE salary = (
    SELECT MAX(salary)
    FROM Employee
    WHERE departmentId = Department.id
)
```
596. Classes More Than 5 Students
```mysql
# Write your MySQL query statement below
select class 
from Courses 
group by class 
having count(class) >= 5;
```
## Design Pattern
1. List several design pattern names. and write the code for Singleton and simple factory design pattern.
   Singleton
   factory
   prototype



## Spring
1. What is the IOC?
   **Inversion of Control**, is a design principle that promotes loose coupling and modularity. It involves **transferring the responsibility of** **managing object** dependencies from the developer to the spring framework. Spring framework manages the lifecycle of objects and their dependencies and configuration, promoting modularity and testability
2. What is Dependency Injection?
   Since IOC concept separated the creation and management of an object as well as its dependency, **the dependencies are provided (injected) to the object by the Spring framework, instead of an object creating or looking up its dependencies directly.**
3. Different ways of DI
   a. Constructor Injection: Dependencies are injected via a class constructor. The dependencies are passed as arguments to the constructor when creating an instance of the class. 
   b. Setter Injection: Dependencies are injected via setter methods. The class exposes setter methods for each dependency, and the dependencies are set using these methods after the instance is created.
   c. Field Injection: Dependencies are directly injected into the class's fields. The dependencies are assigned directly to the class's fields, typically using annotations like @Autowired.
4. Types of dependency injection
   same to 3
5. What is the Scope of a Bean?
   **The scope of a bean defines the lifecycle and visibility of an instance of that bean within an application. The scope determines how long the bean instance lives and how it is shared between different components.**
   
   
   *Basic:*
   **Singleton:** The singleton scope is the default scope in Spring. With the singleton scope, Spring creates only one instance of the bean for the entire application context. Any subsequent requests for the bean will receive the same shared instance.
   **Prototype:** With the prototype scope, a new instance of the bean is created every time it is requested from the Spring container. Each instance is independent and has its own state.
   *Web:*
   **Request:** The request scope is applicable in web applications. It creates a new instance of the bean for each HTTP request and associates that instance with that specific request. Once the request is completed, the bean instance is destroyed.
   **Session:** The session scope is also specific to web applications. It creates a new instance of the bean for each user session. The bean instance is maintained throughout the duration of the user's session and is destroyed when the session ends.
   **Application:** The application scope creates a single instance of the bean for the entire web application. It is similar to the singleton scope but is specific to web applications.
   **WebSocket:** The WebSocket scope is applicable in WebSocket-based applications. It creates a new instance of the bean for each WebSocket connection and maintains the instance throughout the lifetime of that connection.
6. Could you give me the example for singleton bean scope
```java
import org.springframework.stereotype.Component;

@Component
public class CustomerService {
    private String customerName;

    public void setCustomerName(String name) {
        this.customerName = name;
    }

    public String getCustomerName() {
        return customerName;
    }
}


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class OrderProcessor {
    private final CustomerService customerService;

    @Autowired
    public OrderProcessor(CustomerService customerService) {
        this.customerService = customerService;
    }

    public void processOrder(String customerName) {
        // Business logic to process the order
        customerService.setCustomerName(customerName);
        System.out.println("Order processed for customer: " + customerService.getCustomerName());
    }
}
```
Since the CustomerService component is annotated with @Component and no explicit scope is provided, it will be treated as a singleton bean by default. The OrderProcessor class is also a Spring component and depends on the CustomerService singleton. It is injected with an instance of CustomerService through constructor injection.

Since both CustomerService and OrderProcessor are singleton-scoped beans, they will share the same instance of CustomerService. Any changes made to the customer name in CustomerService will be reflected across all components that depend on it.

7. What are the differences between @RequestParam and @PathVariable?
   `@RequestParam` is used to extract request parameters from the **query string** or form data of an HTTP request. 请求参数**以 ? 开头（question mark）**，然后是一系列的键值对，例如：/api/products?category=electronics&page=1。
   `@PathVariable` is used to extract variable **values from the URI** (Uniform Resource Identifier) of the request. 路径变量使用**大括号 {} covered by Curly Braces ** 包裹在URL的一部分中，例如：/api/users/{userId}。
   `@RequestBody` Used to extract the request body payload.
```java
@RequestMapping("/example")
public String exampleMethod(@RequestParam("param") String paramValue) {
    // Method implementation
}

@RequestMapping("/example/{id}")
public String exampleMethod(@PathVariable("id") String idValue) {
    // Method implementation
}

@PostMapping("/api/users")
public ResponseEntity<UserDto> createUser(@RequestBody UserDto userDto) {
    // Process the userDto object received in the request body
    // ...
}

```
About Null:
**@PathVariable parameters are required** by default and must be present in the URL
**@RequestParam** parameters are **optional** by default unless explicitly marked as required:(问号的可以为空)
```java
@RequestMapping("/example")
public String exampleMethod(@RequestParam(value = "param", required = true) String paramValue) {
    // Method implementation
}
```
8. What is AOP? could you list any annotations and briefly explain it? could give me one example how do you use it?
   Aspect-Oriented Programming is used to intercept method invocations and **add additional behavior or functionality before, after, or around the target method execution**. It provides a way to achieve cross-cutting concerns such as logging, transaction management, security, caching, and error handling.
   
   Here are some commonly used annotations in Spring AOP and their brief explanations:
   1. @Aspect: This annotation is used to declare a class as an aspect. An aspect contains advice and pointcut definitions.
   2. @Before: This annotation is used to define advice that should be executed before a method invocation.
   3. @AfterReturning: This annotation is used to define advice that should be executed after a method successfully returns a result.
   4. @AfterThrowing: This annotation is used to define advice that should be executed after a method throws an exception.
   5. @After: This annotation is used to define advice that should be executed regardless of the method outcome (success or exception).
   6. @Around: This annotation is used to define advice that surrounds a method invocation. It has the most control over the target method execution and can modify the method behavior.
```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void beforeMethodExecution() {
        System.out.println("Executing the method...");
    }
}
```

  The @Before annotation is used to define advice that should be executed before the execution of methods in the com.example.service package (and its sub-packages). It prints a log message indicating that the method is being executed.

  By applying this aspect, the defined advice will be executed before the methods in the specified package are invoked. The aspect can be applied to multiple classes and methods, allowing for modular and reusable implementation of cross-cutting concerns.
  
  
9. What is spring batch?
	It is a good tool to build robust and scalable **batch processing** applications. It simplifies the development of batch jobs by providing reusable components common batch processing tasks, such as r**eading input data, processing records, and writing output data**. Using spring batch including Job Configuration, job launcher, Item reader, Item Processors and item writer. It also provides Job Scheduling and Reporting functions.
1. What is cron/spring task?
	Cron is a time-based job scheduling syntax. Spring task is a feature provided by the Spring Framework for scheduling tasks within the application's context. It allows developers to define and execute scheduled tasks using a more flexible and expressive syntax like cron. For example fixed-rate, fixed-delay, and cron-based scheduling.
```java
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class MyTask {

    @Scheduled(fixedRate = 5000) // Runs every 5 seconds
    public void runTask() {
        // Task logic
    }
}
```
11. **how to monitor you application? (spring actuator)**
	 We can utilize Spring Boot Actuator, which provides production-ready features for monitoring and managing your application.
	 Once Actuator is configured, you can access the monitoring endpoints via HTTP requests.cSpring Actuator **provides a variety of endpoints** that offer valuable information about your application's health, metrics, environment, and more. Some commonly used endpoints include:
	 
	*/health:* Provides information about the application's health status.
	*/info:* Displays custom information about your application.
	*/metrics:* Shows various metrics such as CPU usage, memory usage, and request counts.
	*/env:* Presents details about the application's environment variables.
	*/logfile:* Provides access to the application's log file.
	*/trace:* Shows recent HTTP requests made to the application.
### spring validation
1. **Spring validation?**
    Spring Validation is a feature to **validate user input or data** in a Spring-based application. It helps ensure that the data **meets** the **specified** criteria or **constraints** before processing it further. Spring Validation is typically used in web applications to validate form inputs, REST API requests, or any other user-provided data.
    **Some commonly used validation annotations include @NotNull, @NotBlank, @Size, @Pattern, and @Email, among others.**
    Spring Validation integrates with Spring's data binding mechanism, allowing automatic validation of objects bound to form inputs or request payloads. It can be used with various data binding approaches, such as Spring MVC's @ModelAttribute or @RequestBody, or even manual binding with Validator methods.
```java
public class User {
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    private String name;

    // Getter and setter methods
}
@RestController
public class UserController {
    @PostMapping("/users")
    public String createUser(@Valid @RequestBody User user, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            // Handle validation errors
        }
        
        // Process valid user object
        return "User created successfully";
    }
}
```
13. how to handle exception in spring?
    customized exception class extends RuntimeException
    @ExceptionHandler(Cus-Exception)
	    Method Level
	    used to handle the specific exceptions and sending the custom responses to the client
	@ControllerAdvice
		Class Level
		used to handle the exceptions globally
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(Exception ex) {
        // Create and return an appropriate error response
    }
}
```
14. What is Dispatcher Servlet/Front Controller? please describe the flow.
    A key component of the Spring MVC (Model-View-Controller) framework. It acts as a **central entry point** for incoming requests and handles the overall request processing flow in a Spring MVC application.
    ![[Pasted image 20230714174937.png]]
    ![[Pasted image 20230714175043.png]]
	1. Tomcat 收到Http Request，将request交给DispatcherServlet来处理
	2. DispatcherServlet 拿着该req去call HandlerMapper,
	3. HanlerMapper将会找到对应的Controller以及对应的method，并返回给
	dispactherServlet
	4. dispatcherServlet将call该Controller对应的method，此时会触发call service,
	repository and database.
	5. 然后结果(ModelAndView)再通过Controller返回。⼀般是返回view name, ⼀个字
	符串，⽐如list-customers. ⽽数据是要set到Model 中。
	6. dispatcherServlet拿着view name去call view Resolver,
	7. View Resolver会帮助我们找到view template, ⽐如`list-customers.jsp`
	8. 此时我们有了view template, 也有了数据model，则可以call View engine去帮助我们把数据放到view template⾥，然后转换成纯粹的HTML
	9. 该HTML就是前端显⽰的内容，最终返回给browser。
15. Difference between @Component and @Bean
    @Component Annotation:
	- The @Component annotation is a generic stereotype annotation used to mark a class as a Spring-managed component. It indicates that the class is eligible for **auto-detection and auto-configuration as a bean** within the Spring application context.
	- The @Component annotation can be **applied to any class**, including plain Java classes, Spring MVC controllers, service classes, or data access classes.
	- When a class is annotated with @Component, Spring automatically **creates a bean instance of that class during application context initialization**.
	@Bean Annotation:
	- The @Bean annotation is used to explicitly **declare a method** as a producer of a bean within a Spring configuration **class, typically annotated with @Configuration.**
	- The @Bean annotation is applied to **methods that** **return instances of beans** that need to be managed by the Spring container.(比如构造函数)
	- When a method is annotated with @Bean, Spring calls that method to obtain the bean instance and manages its lifecycle, including dependency injection, initialization, and destruction.
	```java
@Configuration
public class MyConfiguration {

    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
	```
16. Difference between @Component, @Service, @Repository and @RestController?

	  1.  `@Component` is often used for utility classes, helper classes, or other general-purpose classes.
	   @Service, @Repository and @RestController are specializations of @Component.
	   these help in distinguishing service/Dao/ controller classes from other types of components, improving code organization and clarity. 
	   
	   2. **@RestController** combines **@Controller** and **@ResponseBody** annotations, simplifying the creation of RESTful controllers by eliminating the need for explicit @ResponseBody annotations.
	   
 当使用@Controller注解时，该类被标记为一个控制器，并且控制器的方法通常负责处理用户请求并返回适当的视图。这意味着控制器方法将处理请求并生成用于呈现给用户的HTML页面或其他视图。
		 与此不同，使用@RestController注解时，类被标记为一个RESTful风格的控制器。控制器的方法处理用户请求并返回数据，而不是视图。
		 如果要让@controller类的函数返回一个restful数据，就要在函数前返回值类型加@ResponseBody
		   **@RequestMapping** 还是要写的
		   注意 **@ResponseBody对应的class是 ResponseEntity**
   ```java
@RestController
@RequestMapping("/api/v1/posts")
public class PostController {
	@Autowired
	private PostService postService;
	@PostMapping()
	public ResponseEntity<PostDto> createPost(@RequestBody PostDto postDto) {
		PostDto postResponse = postService.createPost(postDto);
		return new ResponseEntity<>(postResponse,HttpStatus.CREATED);
	 }
}


@Controller
@RequestMapping("/api/v1/posts")
public class PostController {
	@Autowired
	private PostService postService;
	@PostMapping()
	public @ResponseBody ResponseEntity<PostDto> createPost(@RequestBody PostDto postDto) {
		PostDto postResponse = postService.createPost(postDto);
		return new ResponseEntity<>(postResponse, HttpStatus.CREATED);
	}
}
```
	1. What is @ComponentScan? `` sildes: 43iocbean`
   Define where the spring need to scan the bean definitions and generate the beans. typically annotated with @Configuration. It instructs Spring to scan the specified packages and automatically detect and register beans as Spring-managed components in the application context.
   @ComponentScan instructs Spring to scan the specified packages and its sub-packages to discover classes annotated with stereotypes such as @Component, @Service, @Repository, etc.
   **In summary, @ComponentScan is used to enable component scanning and specify the packages to scan for Spring components.** It is typically used at the **configuration class level.** **@Component**, on the other hand, is a stereotype annotation used to **mark a class as a Spring-managed component**. It is applied to individual classes to indicate that they should be instantiated and managed as beans by the Spring container.
```java
	@Configuration
	@ComponentScan(basePackages = "com.example.myapp")
	public class AppConfig {
	    // Configuration class code
	}
```
1. Authentication vs. Authorization
	In authentication process, the identity of users **are checked for providing the access to the system**. While in authorization process, person’s or **user’s authorities are checked for accessing the resources.**
	
	AUTHENTICATION is done before the authorization process.
	
	AUTHENTICATION - verified.
	AUTHORIZATION - validated.
	
	Authentication determines whether the person is user or not. 
	While Authorization determines What permission do user have?
19. What is JWT? (header.payload.signature) and what kind of tool/dependency in java can generate and parse JWT?
    JWT (JSON Web Token) is an open standard for securely transmitting information between parties as a JSON object.A JWT consists of three parts: header, payload, and signature.
    
    **Header:** Contains metadata about the type of token and the algorithm used for signature verification.
    Example: {"alg": "HS256", "typ": "JWT"}
    **Payload:** Contains the claims or statements about the entity (user) and additional data. Example: {"sub": "1234567890", "name": "John Doe", "exp": 1628195452}
    **Signature:** A cryptographic/encrypted signature created by combining the encoded header, payload, and a secret key. It ensures the integrity and authenticity of the token.
    Example: HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secretKey)
    
	 `jjwt` (Java JWT):
	 `jjwt` is a Java library for creating and parsing JWTs.
	 It provides a simple and straightforward API to generate and validate JWTs.
	 You can add the following dependency to your Maven project:
	```xml
	<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.2</version>
	</dependency>
	```
    
1. What will happen after click the URL?
    - **URL Parsing and Protocol Resolution:** For example, if the URL starts with "https://", the browser knows that it needs to establish a secure HTTP connection using SSL/TLS.
    - **DNS Resolution**: translate the domain name to the corresponding IP address.
    - **Establishing a Connection:** a series of handshakes between your computer and the server to establish a reliable connection for data transmission.
    - **Sending an HTTP Request:** including HTTP method (e.g., GET, POST), headers, and the path to the requested resource specified in the URL.
    - **Server Processing:**
    - **Server Generating a Response:**
    - **Data Transmission:** The server sends the HTTP response back to your computer over the established TCP connection. The response is broken into packets and transmitted through various network devices, including routers and switches, to reach your computer.
    - **Rendering the Response:** Once your computer receives the response, your web browser interprets the response based on the received headers and renders the content. It may involve parsing HTML, executing JavaScript, rendering CSS, and displaying the resulting web page to you.
2. how to call api in java code?
    There are multiple ways to make API calls in Java, depending on the libraries and frameworks you choose. Here are three common approaches with simple examples:
	1. Using HttpURLConnection (Java built-in):
	    - `HttpURLConnection` is a built-in Java class for making HTTP requests.
	   - Example:
	```java
	import java.io.BufferedReader;
	import java.io.InputStreamReader;
	import java.net.HttpURLConnection;
	import java.net.URL;
	
	public class APICaller {
	    public static void main(String[] args) {
	        try {
	            URL url = new URL("https://api.example.com/endpoint");
	            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
	            conn.setRequestMethod("GET");
	
	            BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
	            String line;
	            StringBuilder responseContent = new StringBuilder();
	
	            while ((line = reader.readLine()) != null) {
	                responseContent.append(line);
	            }
	
	            reader.close();
	            conn.disconnect();
	
	            System.out.println(responseContent.toString());
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	    }
	}
	```
	
	2. Using Apache HttpClient:
	   - Apache HttpClient is a widely used library for making HTTP requests in Java.
	   - Example (using Apache HttpClient 4.x):
	
	```java
	import org.apache.http.HttpResponse;
	import org.apache.http.client.HttpClient;
	import org.apache.http.client.methods.HttpGet;
	import org.apache.http.impl.client.HttpClients;
	import org.apache.http.util.EntityUtils;
	
	public class APICaller {
	    public static void main(String[] args) {
	        try {
	            HttpClient httpClient = HttpClients.createDefault();
	            HttpGet request = new HttpGet("https://api.example.com/endpoint");
	            HttpResponse response = httpClient.execute(request);
	            String responseContent = EntityUtils.toString(response.getEntity());
	
	            System.out.println(responseContent);
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	    }
	}
	```
	3. Using OkHttp:
	   - OkHttp is a popular open-source HTTP client library for Java.
	   - Example:
	```java
	import okhttp3.OkHttpClient;
	import okhttp3.Request;
	import okhttp3.Response;
	
	public class APICaller {
	    public static void main(String[] args) {
	        try {
	            OkHttpClient client = new OkHttpClient();
	            Request request = new Request.Builder()
	                    .url("https://api.example.com/endpoint")
	                    .build();
	
	            Response response = client.newCall(request).execute();
	            String responseContent = response.body().string();
	
	            System.out.println(responseContent);
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	    }
	}
	```

22. I want to get the customer's history orders between 01/02/2023 and 02/07/2023, and also want to get the customer's payments in this account. Please design the API url.
`GET /api/customers/{customerId}/orders?startDate=2023-02-01&endDate=2023-07-02`
`GET /api/customers/{customerId}/payments

`POST /api/customers/{customerId}/orders` with RequestBody
```json
{
	 "startDate": "2023-02-01",
	 "endDate": "2023-07-02"
}
```

### Spring Code
Develop the API to CRUD the customer's history orders.
	a. Need to write the full codes including Controller, Service & ServiceImpl, Repository, Entity
	b. You need to design the payloads(request, and response body).
	c. Use the correct status code for each API
	d. Use ResponseEntity
	e. Use Validation for request body
	f. Write unit tests.
	g. Exception handling.
	h. use log

## Test
1. Functional test & Integration test & Unit test & Regression Test & Performance/Load test
   **Functional testing** focuses on testing the software's functionality against the specified requirements or user stories.
   **Integration testing** involves testing the interaction between multiple components or modules to ensure they work together correctly.
   **Unit testing** is focused on testing individual units (methods, functions, or classes) of code in isolation.
   **Regression testing** ensures that previously developed and tested software still functions correctly after modifications or updates.
  **Performance/load testing** is conducted to assess the system's behavior and performance under specific conditions, such as high user loads or large data volumes.
  
2. What's the test framework in your team?
	Mockito and JUnit
	JUnit主要用于定义和运行测试用例，而Mockito通过提供创建模拟对象和控制依赖项行为的能力来补充JUnit。
3. Talk about Mockito, PowerMockito and give an example
   
   **Mockito:**
    It allows you to create mock objects, set expectations on their behavior, and verify interactions with those objects during testing. Mockito focuses on simplicity and ease of use.
    We want to test a class UserProcessor that depends on UserService. However, UserService may modify the database and it is not wanted. So we can mock the UserService to isolate UserProcessor for unit testing
	```java
	import static org.mockito.Mockito.*;
	import org.junit.Test;
	
	public class UserProcessorTest {
	  
	  @Test
	  public void testUserProcessor() {
	    // Create a mock instance of the UserService interface
	    UserService userServiceMock = mock(UserService.class);
	    
	    // Set the behavior of the mock object
	    when(userServiceMock.getUserById(123)).thenReturn(new User("John Doe"));
	    
	    // Create an instance of the UserProcessor class, injecting the mocked UserService
	    UserProcessor userProcessor = new UserProcessor(userServiceMock);
	    
	    // Perform your test using the UserProcessor instance
	    String result = userProcessor.processUser(123);
	    
	    // Verify interactions with the mock object
	    verify(userServiceMock).getUserById(123);
	    
	    // Perform assertions on the result
	    assertEquals("Hello, John Doe!", result);
	  }
	}
	```
 **PowerMockito** 
	It is an extension of Mockito that provides additional capabilities to mock static methods, final classes, private methods, constructors, and more. It is useful when dealing with legacy code or situations where Mockito's basic mocking capabilities are insufficient.
	
	mock private method: `verifyPrivate(utilityClassMock).invoke("doSomething");`
```java
import static org.mockito.Mockito.*;
import static org.powermock.api.mockito.PowerMockito.*;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.powermock.core.classloader.annotations.PrepareForTest;
import org.powermock.modules.junit4.PowerMockRunner;

@RunWith(PowerMockRunner.class)
@PrepareForTest(UtilityClass.class)
public class UtilityClassTest {
  
  @Test
  public void testUtilityClass() throws Exception {
    // Mock the private method of the UtilityClass
    UtilityClass utilityClassMock = mock(UtilityClass.class);
    when(utilityClassMock, "doSomething").thenReturn("mocked result");
    
    // Replace the real UtilityClass with the mock instance
    mockStatic(UtilityClass.class);
    when(UtilityClass.getInstance()).thenReturn(utilityClassMock);
    
    // Perform your test
    String result = UtilityClass.getInstance().publicMethod();
    
    // Verify interactions
    verifyPrivate(utilityClassMock).invoke("doSomething");
    
    // Perform assertions on the result
    assertEquals("mocked result", result);
  }
}
```
   
   补充材料：
   实际上，使用 Mockito 只能模拟接口和非静态方法。Mockito 无法直接模拟静态方法、私有方法和构造函数。这是因为 Mockito 是基于动态代理和反射的原理进行模拟的，而静态方法、私有方法和构造函数无法通过动态代理直接进行拦截和替换。
   然而，你可以结合 PowerMockito 来模拟静态方法、私有方法和构造函数。PowerMockito 是 Mockito 的扩展，提供了对静态方法、私有方法和构造函数的模拟能力
   
1.  What is SonarQube?
   SonarQube is an open-source platform designed for continuous inspection of code quality. It provides static code analysis, code coverage, and software metrics to help developers identify and fix issues early in the development process. SonarQube provides an intuitive web interface.

## Microservice
1. Microservice architecture in your recent project?
2. Microservice components
   Spring cloud config server
   API Gateway
   Eureka
   Ribbon (load balancer)
   Hystrix/Resilience4j (circuit breaker)
   kafka (message queue)
   Docker
   K8s (for scale)
   
1. How to do server discovery?
  微服务在启动时，会将自己注册到 Eureka 服务器上，同时微服务也会从 Eureka 服务器中查询其他服务的位置信息。然后，微服务直接调用其他服务，通常会配合 Ribbon 进行客户端负载均衡。所以，尽管 Eureka 是一个服务器，但是这种方式被称为客户端发现，是因为大部分的决策权（发现服务、负载均衡等）都在客户端。
  
1. What is API gateway and what we can do in API Gateway?
   An API Gateway is a server that acts as an API front-end, receives API requests, and forwards them to the appropriate microservice. The API Gateway will often handle a request by invoking multiple microservices and aggregating the results. It can translate between web protocols such as HTTP and WebSocket and web-unfriendly protocols that are used internally.

	1. Functions:
	   Routing:
	   Composition/Aggregation: Combine data from multiple services and return it as a unified response.
	   Authentication and Authorization:
	   Fault Isolation and Circuit Breaker
	   Monitoring and Logging: 
	   caching
	   Protocol Transformation
   
2. how to communicate each other between services?
   Rest template and MQ
3. How do you use RestTemplate? write the code example to call an API
4. How to scale up your microservice?
   1. Horizontal Scaling (Scale Out 添加更多instances
   2. Stateless Services: Making your microservices stateless, i.e., not storing any client-specific data between requests, can greatly simplify horizontal scaling.
   3. Data Store Scaling: The backend data store often becomes a bottleneck when scaling microservices. Techniques such as **sharding** (splitting a database into smaller parts), **replication** (duplicating the database), and using a **cache** can help scale the data layer.
   4. **Message Queue Scaling:** If your microservices communicate through a message queue, the queue itself might need to be scaled. This can be done by **partitioning (dividing the queue into multiple parts)** or replicating the queue.
   5. **Service Mesh:** A service mesh can be used to control how requests are routed between microservices, allowing you to easily scale up or down as needed. It also provides additional features that are useful when scaling, such as load balancing, circuit breaking, and retries.
   6. **Performance Tuning:** Optimizing your code and application configurations can often lead to significant performance gains. For example, you might be able to increase throughput by improving your database queries or using a more efficient serialization format.



