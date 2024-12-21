## Topics, Partitions and Offsets
### Topic: 
a particular stream of data within kafka cluster identified by name.
不能query，没有constraints，也不检查格式
![[Pasted image 20230618170212.png]] 
### Partitions and offsets:
topic分成许多分区，每个分区中的message都是有顺序的。offset 就是 message id, 每个分区各自独立
immutable： 不能删改

![[Pasted image 20230618112057.png]]

==offset 不会被复用，删除了的不会被复用==

消息的顺序只在每个分区内有序。跨区不有序。
一个topic可以有很个partition.
![[Pasted image 20230618170318.png]]

## Producers and Message Keys

### Producers

producer提前决定那个分区写入数据

kafka scale的原因：有很多分区
![[Pasted image 20230618170528.png]]

根据key来hash，相同key的送到同一个分区

![[Pasted image 20230618170833.png]]

![[Pasted image 20230618171654.png]] 

### Serializer
集成在producer之中，负责Message serialization.
![[Pasted image 20230618172146.png]]

### key hashing
 ![[Pasted image 20230618172308.png]]
## Consumers and Deserialization

![[Pasted image 20230618172450.png]]

从一个分区中读的message是有序的，但是跨分区就不知道了
![[Pasted image 20230618172810.png]]

可以知道，consumer只能固定解码一种格式。所以topic中的数据类型不可以改变。 
## Consumer groups and Consumer offset
![[Pasted image 20230618173326.png]]

消费者与分区是==一对多==的关系！！一个分区只能被一个消费者使用。一个消费者可以读多个分区
班级对应学生是一对多
![[Pasted image 20230618174549.png]]
数量超过分区时，能有不活跃的。
### group
![[Pasted image 20230618175508.png]]

### consumer offset
崩溃恢复

![[Pasted image 20230618175922.png]]

semantics 语义
交付语义。交付的方式？
idempotent:幂等的，但是这里指对系统没有额外影响
至少一次
最多一次
就一次
![[Pasted image 20230619123739.png]]
## Broker and Topics
![[Pasted image 20230718161028.png]]
![[Pasted image 20230718161208.png]]
![[Pasted image 20230718161323.png]]
## Topic Replication


是topic层面的复制不是part也不是broker


![[Pasted image 20230718161537.png]]
![[Pasted image 20230718161613.png]]
![[Pasted image 20230718161713.png]]
![[Pasted image 20230718161833.png]]
![[Pasted image 20230718161943.png]]

## Producer Ack and Topic Durability
![[Pasted image 20230718162301.png]]
![[Pasted image 20230718162355.png]]
## Zookeeper
![[Pasted image 20230718163150.png]]
zookeeper不存consumer offset等信息
Kafka Consumer Offsets are stored in kafka instead of zookeeper. this is the case since Kafka 0.9, in the topic __consumer_offsets
![[Pasted image 20230718163224.png]]
![[Pasted image 20230718163454.png]]
## Kakfa KRaft
![[Pasted image 20230718163622.png]]
![[Pasted image 20230718163646.png]]
![[Pasted image 20230718163703.png]]
## Roundup

partition编号是broker决定的
![[Pasted image 20230718164116.png]]