问题要等几秒回答，需要拖时间让面试官尽量少问问题


**recent exp**

**In project have you used webflux?**

**advantage of using webflux**
non blocking


**why high throughput is the ad of webflux**
event loop and call back?

**coding 题**
given array of numbers
for each number, multiply the same number 
sum all the values
produce the sum and also print out the sum
also print the array
before summary, run in parallel


using webflux(reactive programming)


解释什么是 schedule.parallel
如果之前的code不用schedule.parallel，会发生什么？还是并行计算吗？


**difference between map and flat map**

**讲讲项目中的microservices**

问了什么AWS服务被用到了，尤其是部署相关的，EKS

当你创建微服务时，用到docker image了吗，怎么创建docker image

<怎么写docker file>

kafka 

how consumer works in kafka? poll or push

10个consumer   读取一个topic： 分成10个part 然后读

如果想让10个同时读一个partition就把10个consumer分散到不同groups

consumer group 是什么


nosql db : MongoDB

which LTE of java?

假设你有个lambda。你尝试访问lambda之外的变量可以吗？可以改变该变量吗？理由是什么，为什么java不让改变lambda中的value.  Enclosing?


什么是序列化和反序列化

怎么深拷贝一个Object
deep clone   什么是clone-able interface
   1 - 创建一个新object
   2 - 序列化反序列化去深拷贝


Relational database:
会问 Oracle.

## Spring boot: 

Bean scope: 
怎么创建prototype bean以及什么senario使用prototype.

## 分布式事务
### 场景一
订单成功，邮件应该发送。但是如果订单失败了，邮件不应该发送。

分布式事务，比如saga模式，核心就是**event driven communication**来触发下一个**本地事务**

1. **事件驱动通信**：当订单状态发生变化时（例如，订单成功或失败），订单服务应该生成一个事件。这个事件可以通过消息队列（如RabbitMQ, Kafka等）发送，这样做可以解耦订单服务和邮件服务。

2. **邮件服务监听事件**：邮件服务应该监听这些事件。当它接收到一个表示订单成功的事件时，它会触发发送邮件的流程。如果事件表示订单失败，则不执行任何操作。

3. **事务一致性**：你需要确保**订单状态的更改和事件的发布在一个事务中完成**。这意味着，如果订单状态更新失败，事件不会被发布；反之亦然。这可以通过本地事务确保，或者使用更复杂的分布式事务解决方案（如SAGA模式）。

4. **容错和重试机制**：邮件服务在处理接收到的事件时应该具备容错能力。比如，如果邮件发送失败，应该有重试机制。

5. **可观察性和监控**：为了确保系统的健壮性，邮件服务和订单服务都应该有日志记录、监控和警报机制，以便于问题的快速发现和解决。


### 场景二
如果订单成功了但是email失败了，不应该回滚，依然应该commit，怎么办？（因为订单成功了用户支付成功了，总不能退钱给用户让她重来吧？）


define roll back condition.



gradle maven

using maven

反问



mono / flux?