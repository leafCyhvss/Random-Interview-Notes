

## java basic

什么是immutable, 为什么重要
String string builder string buffer

oop 4原则


static 和 private method可以被override吗？

抽象和interface的区别，什么时候用

lambda 表达式一般和什么interface一起用？ annotation  necessary?

functional interface可以inherit其他interface吗？  没有说 abstract 

讲讲hashmap原理 / treemap / linkedhashmap

equals方法和hashcode()可以被override吗？他们是什么作用？

## 多线程


你在工作中怎么使用多线程 ？（应该会答场景）

how to create thread

为什么用 thread pool =>没有提节约成本

why do we use completable future?

how to get result of a cf

Concurrent HashMap 和 用synchronized锁定的map性能区别

不用解释why thread-safe


## stream
```
[1,5,2,3,8,2,4,6,3,6,6]


1234568
2 3 6
```
stream: 统计次数
去重
重复的元素



## Spring

为什么用spring而不是其他框架搭建后端系统？个人觉得不要先说spring boot

两个bean  class名字相同怎么办？怎么注入 =》 少了一个@primary

RestController & controller

put post的区别


在你的项目中使用了什么数据库？怎么连多个数据库

type of data不准确  high volume 没讲清楚 available?


假设你有json数据，可以存在rdbms吗?

假设我们需要快速写入一些数据，什么数据库 => mysql 不对


讲讲 hibernate 的 cache



## Microservice

communicate

put post 区别  **？** 

have you ever developed producer or consumer? Is your service a producer or a consumer

zookeeper是干啥的   **not use?**

if you have multiple microservices and they depend on each other, what would happen is a service is down? is there any mechanism or design pattern that works in such situation?
**是没听清楚吗**

so apart from that design pattern, what would you do if your service application is down? How to solve the problem?  **跟service 发现无关**  也不是 leader - follower

<讲一下反问面试官要问啥>


how you is your application deployed on to cloud? become microservice?
cicd是ljq的题：

what is the benefit of using a container





问题 0721
讲项目的时候轻重问题，不用讲那么多不那么重要的细节和技术栈

api endpoints: 没有准备
thread safe 解释
string buffer

functional interface

讲hashmap说什么example
不用先讲collision

concurrent hashmap
synchronized keyword

多线程示例