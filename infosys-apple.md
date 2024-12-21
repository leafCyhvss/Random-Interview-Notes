
### 10月第一次infosys
what have you build in your project
How familiar are you with kafka
Stringbuffer vs String builder (threadsafe or not)
Can a kafka consumer consume multiple partition of a topic
If so why, if not why not?
Coding: multithreading with completableFuture
### 10月第二次infosys
我补一下你当时第二次面infosys的东西：
1 项目介绍
2 项目技术栈介绍
3 coding1 不记得了
4 coding2 第二大和第二小的数
5 junit测试两个coding中的一个
6 模拟写一个controller获取学生列表+spring data八股（jpa+分页）
### 0823


1. Introduce yourself, work experience
2. recent project, how many microservice you were working on. Introduce in details
3. cloud-native solution? How is your application connected to Kafka? - used bootstrap url to connect. Was that specity in your application or somewhere else?
4. Have you worked on splitting monolithic applications into microservice applications?
5. Let's say you have a monolithic application, a single Java application, you have multiple calls in the same application. What are some steps you would take to start if you want to split it? What are some pointers you will look at?
6. What db have you used?
7. How many calls are made to db? What's the frequency of the calls, the number of transactions? (好像在问how many database connections, 不太确定..) Have you ever worked on setting up connection pools?
   
#### 微服务内存超出：
1. Your application crashed because of outOfMemory, what steps would you take to identify this issue? follow up: If the application crashed continuously, how would you look at these things?

##### 只回答内存相关解决：
当您在AWS上部署的基于Spring Boot的微服务系统因`outOfMemory`崩溃时，可以采取以下步骤来识别和解决此问题：

1. **日志分析**：通过`Splunk`检查应用的日志，查找是否有`OutOfMemoryError`的错误信息。

2. **CloudWatch监控**：查看AWS CloudWatch的指标，特别是关于内存使用的指标，以识别内存使用情况和模式。

3. **使用内存分析工具**：如果可以获取到heap dump，使用工具如`VisualVM`或`MAT`分析dump文件，以确定哪些对象占用了大部分内存。

4. **生成heap dump**：在JVM参数中配置（如`-XX:+HeapDumpOnOutOfMemoryError`），以便在`OutOfMemoryError`发生时自动生成dump文件。

5. **Kafka的考虑**：确保Kafka消费者不会一次性加载大量的消息，可能需要调整消费者的配置或批量大小。

6. **Grafana监控**：利用Grafana查看微服务的内存使用情况和其他相关指标，帮助分析问题。

7. **代码审查**：检查是否有潜在的内存泄漏或不恰当的内存使用，特别是与Kafka消息处理相关的部分。

8. **外部资源的检查**：查看与外部数据库或其他微服务的交互，确保数据交换不会导致内存溢出。

9. **调整JVM参数**：根据AWS实例的大小，可能需要调整JVM的`-Xms`和`-Xmx`参数。

10. **自动伸缩**：考虑在AWS上使用自动伸缩功能，以便在内存使用率增加时自动增加实例数量。

11. **测试**：在AWS环境中进行压力测试或性能测试，模拟高并发场景来复现`outOfMemory`问题。

在确定问题原因并进行了针对性的优化和调整后，持续监控和测试以确保`outOfMemory`问题不再发生。

##### 总结回答：
当您的基于Spring Boot的微服务应用在AWS上因`OutOfMemory`错误崩溃时，通常应采取以下步骤来识别并处理此问题：

1. **立即回滚**：如果已经有备份或上一个稳定版本，立刻在AWS上回滚到这个版本，以恢复服务的正常运行。

2. **监控与日志收集**：
   - 使用CloudWatch查看应用的内存使用情况，特别是在崩溃前的内存消耗趋势。
   - 使用Splunk收集和查看应用的日志，寻找`OutOfMemory`之前的任何异常或错误消息。

3. **堆转储**：如果可能，获取应用在崩溃时的JVM堆转储(heap dump)。这可以帮助分析内存中的对象和它们的引用，从而确定内存泄露或其他导致内存耗尽的原因。

4. **本地复现**：在本地环境中使用与生产环境相同的配置和数据尝试复现问题，这样可以在一个安全的环境中更详细地调试问题。

5. **压力测试**：使用工具如JMeter或Gatling对本地或测试环境进行压力测试，观察在大负载下应用的内存使用情况。

6. **代码审查**：查看最近的代码更改，尤其是与数据处理、缓存或其他可能消耗大量内存的功能相关的部分。

7. **调优与配置**：根据需要调整JVM参数，如堆大小、垃圾回收策略等。

**Follow up**:
如果应用连续崩溃，那么：

1. **临时解决方案**：考虑临时增加资源（如RAM）或使用自动扩展功能（如果配置在AWS上），以应对峰值负载，同时给予团队更多时间来识别问题。

2. **持续监控**：使用Grafana设置实时监控和报警，这样在内存达到危险水平之前就能得到通知。

3. **服务隔离**：如果你的微服务架构中有多个服务，考虑临时关闭或限制可能导致问题的服务，从而隔离问题。

4. **分析历史数据**：分析过去的崩溃模式，例如时间、频率和持续时间，这可能会提供一些线索。

确保在问题解决后继续密切监控，验证所做的更改确实解决了问题，并确保系统的长期稳定性。
   
   
9. Responsible for Kafka? work experience on setting?
10. Have you written any dockerfile? example
11. What service have you used in AWS?
12. What system connected with the backend?(组里的项目)
13. How do you take care of the security of the cloud?
14. Used docker to containerize your application?
15. If you design APIs, what framework would you follow?
16. Are you responsible for deploying on EKS?
17. Did you build the deployment file?
18. Have you written to fetch any secrets from the secret manager? Can you walk through this process, how to define that?
19. Apart from cloudwatch what other tools do you use to monitor?
20. How to integrate dynatrace into your application? What are the kpi you are observing?


## 去年


### 10/20
infosys/first round
apple
webex
Tianyi

Manager: hari sharan
 SpringBoot experience. What annotation has you used? Please explain
 How to inject one bean when there are multiple beans(parent-children like circle rectangle
Multi Thread: how to make a variable thread safe. (static, volatile, private public) @qualifier
follow up:  is count and globalcount thread safe? why
how to use syncronization in method? lock, atomicInteger
how to create thread
what is future/completable future. how to create future tusk
lambda expression
stack queue? why are they important for java?(memory management)
How is set implemented in java? hashcode() Hashset


怎么链接项目到mongodb

### 11/2
Manager: hari sharan
logic to implemting hashmap from scratch
Java bean factory usage
Java interface usage
factory design pattern usage
and other design patterns
coding: leetcode 277, 101
### 11/22
What’s your spring experience 
Write a singleton and explain it
Write a example for function overloading
Write a example for function overriding
Sql related - nested query?

### 1/24

Which class contains method: Cloneable or Object?
Is ++ operator thread-safe in Java?
Is it possible to cast an int value into a byte variable? What would happen if the value of int is larger than byte?
What is difference between eager and lazy loading?
What is shallow cloning and deep cloning?
coding：
How much time (in seconds) does it take for a flight attendant to serve coffee?
There are n passengers on a plane.
There is only one passenger sitting per row.
There are no empty rows. 
The flight attendant carries a coffee pot that can serve 
‘passengers. 
The flight attendant starts serving passengers from the front of the plane with a full pot of coffee. 
It takes 1 second for the flight attendant to move from one row to another.
It takes 3 seconds for the flight attendant  to fill a cup of coffee.
Refilling the coffee pot takes 30 seconds regardless of how much coffee is added.
After serving all passengers, the flight attendant needs to return to the front of the plane.

Write an algorithm that finds the amount of time needed to serve all passengers if only the passengers in the rows defined in the table [i] (below) ordered coffee.
There are n number of rows. 
          int n = 30; // 1 - 31
          int i[] = {3, 4, 5, 7, 10, 13, 18, 20, 24, 25}; // a set of rows where seated passengers want to order coffee
