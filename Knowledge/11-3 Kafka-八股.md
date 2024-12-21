## ack
ack是防止消息丢失。 producer发消息，broker收消息。ack  = 1  /  ack = all

## kafka容错
How does Kafka achieve fault tolerance?
1. Kafka is able to handle faults because each partition has replicas across multiple brokers. (Leader-follower)
2. consumer group: 一个 consumer G了， 同group里另一个会接替它的工作
   (customer-offset)先不说

## kafka基础
1. What is broker?
	
	a broker is a server that stores and handles messages (also known as records) that are sent to topics by producers and read by consumers.
	
	**distributed Storage:** Each broker stores copies of topic partitions. Data for these partitions are spread over Kafka brokers, providing a distributed data architecture.
	
	**Scalability:** When you add more brokers to your Kafka cluster, data and load are automatically balanced across them. This allows your Kafka environment to scale horizontally to handle more data and traffic.
	
	**Fault-tolerance:** In a Kafka cluster, data is replicated across multiple brokers. This means that if a broker goes down, another broker can serve the data, providing high availability.
	
	**Leader Election**: For each partition, one of the brokers is elected as the leader, and zero or more brokers are followers. The leader handles all read and write requests for the partition, while the followers replicate the leader. If the leader fails, one of the followers will automatically become the new leader.
	
2. are you a producer or consumer in your recent project?
	both 
	qoute update get info from inventory from marketing data and return result

3. What is **topic**? could you give me one example of the topics in your recent project?
	In Kafka, a topic is a **category or a feed name to which records are published**
	
	Contracts:
	inventory updates
	
	
4. What is **partition**?
	Partition: In Kafka, a partition is a **subset of the messages** in a topic. Partitions allow topics to be parallelized by splitting the data in a particular topic across multiple brokers — each partition can be placed on a separate machine to **allow for multiple consumers to read from a topic in parallel.**
	Within a partition, messages are consumed in order
	
5. What is group consumer?
	A Consumer Group consists of one or more consumers that jointly consume data from a Kafka topic. The consumers in a group divide up the topic's partitions among themselves so that each partition is consumed by exactly one consumer in the group.
	
	The use of Consumer Groups is what allows Kafka to provide **concurrent data consumption**, and is the key element in Kafka's ability to **provide fault-tolerance and scaling of consumers**.
	 **If a single consumer fails, the partitions assigned to it will be reassigned to other consumers in the same group.**
	
	一个单独的Consumer可以被分配并读取多个partitions
	一个consumer group可以同时读取一个topic的多个partitions
	一个partition可以被多个consumer group同时读取，但在一个consumer group内，一个partition在任何时刻只能被一个consumer读取。
	
6. What is offset?
	an offset is a unique identifier of a message within a partition.It denotes the position of the record within the partition

	**Sequential**: Offsets are sequential and incremental. The first message in a partition has an offset of 0, the next message has an offset of 1, and so forth.

	**Permanent:** Once a record has been written to a partition, **it cannot be updated**. Therefore, the offset of a record never changes once it has been assigned.
	
	**Tracking Consumption**: Consumers in Kafka use offsets to track their position in each partition. The consumer can either automatically commit offsets periodically, or it can choose to manually control when offsets are committed. If a consumer acknowledges an offset, it indicates that it has successfully processed all records preceding that offset in the partition.
	special topic:__consumer_offsets
	
	**Reset:** If a consumer goes offline, it will read the offset from where it left off once it comes back online. But this depends on the 'auto.offset.reset' setting. If the latest offset that the consumer read is no longer available (for example, if the record aged out and was deleted), the consumer behavior will depend on its 'auto.offset.reset' setting, which can be set to 'earliest' or 'latest'.
	
7. Explain the concept of Leader and Follower in Kafka.
	Leader: **For each partition of a topic**, **one broker is elected as the leader**. All the writes (producer requests) and reads (consumer requests) for that partition go through the leader. When a producer sends a message to a topic, it actually sends it to the leader of the partition of the topic. Similarly, when a consumer reads a message from a topic, it actually reads it from the leader of the partition of the topic.

	Follower: The other brokers that have replicas of the leader's log are called followers. The followers passively replicate the leader's log by fetching records from the leader. They don't serve client requests, but they are ready to take over if the leader fails.
	**kafka2.4版本以上，允许从最近的follower中读取**
	
1. The leader and follower roles for each partition are elected dynamically. If
	h. If consumer has a big lagging, how to solve it?
	i. do you have any event name in your recent project?
	j. Differentiate between RabbitMQ and Kafka.



## zookeeper起什么作用

简单描述：
1 选举(select)，
2 配置broker，
3 存储topic元数据(info about partition and its replica)，
4 监控broker活跃不活跃(track status) 
5 存储配置信息（可以不说）

**Zookeeper 在 Kafka 中的作用:**

Apache Zookeeper 在 Kafka 中起到了关键的协调和管理作用。以下是 Zookeeper 在 Kafka 中的主要功能和作用:

- **元数据存储**: Kafka 使用 Zookeeper 来存储关于分区和副本的元数据。

- **领导选举**: 当分区的领导者失败时，Zookeeper 负责重新选举新的领导者。

- **集群监控**: Zookeeper 跟踪哪些 broker 是活跃的，以及它们的元数据信息。

- **消费者群组协调**: 在早期版本的 Kafka 中，Zookeeper 用于跟踪消费者的偏移量。但在后续版本中，这个功能已经迁移到 Kafka 本身。

- **配置管理**: 集群级别的配置信息存储在 Zookeeper 中。

注意: Kafka 正在逐步减少对 Zookeeper 的依赖，并计划在未来版本中完全去除这一依赖。这意味着在未来的 Kafka 版本中，上述部分功能可能会由 Kafka 自身处理，而不再依赖 Zookeeper。

## kafka性能优化
围绕：压缩消息，批处理大小，分区多少，网络环境，硬件升级说

**1. 如何提高 Kafka 的吞吐量:**

- **更多的分区**: 为每个主题添加更多的分区可以提高并发度。
  
- **硬件升级**: 使用更高性能的硬件，尤其是 SSD，可以显著提高吞吐量。
  
- **批处理**: 增加生产者和消费者的批处理大小可以提高吞吐量。
  
- **压缩**: 使用消息压缩（如 snappy 或 lz4）可以减少网络和存储的负载，从而提高吞吐量。
  
- **调优 JVM**: 通过调优 Kafka broker 的 JVM 设置，如更大的内存和适当的垃圾回收策略，可以提高性能。
  
- **网络调优**: 确保网络带宽足够，并减少不必要的网络延迟。

**2. 如何降低 Kafka 的延迟:**

- **减少批处理大小**: 虽然增加批处理大小可以提高吞吐量，但它可能增加延迟。为了降低延迟，你可以考虑减少批处理大小。
  
- **调优确认设置**: 使用 "acks=1" 或 "acks=0" 可以减少延迟，但可能牺牲了一定的数据持久性。
  
- **压缩**: 使用压缩可以减少传输和存储时间，从而降低延迟。
  
- **减少网络延迟**: 确保 Kafka broker 和生产者/消费者之间的网络延迟尽可能小。
  
- **异步发送**: 生产者可以异步发送消息，这样它们不必等待确认，从而降低延迟。




## dead-letter queue
Kafka 有一种模式叫做“死信队列”（Dead Letter Queue，简称 DLQ），但它不是 Kafka 本身的核心特性，而是在 Kafka Streams 和 Kafka Connect 中提供的。此外，一些 Kafka 客户端库，如 Confluent 的 Kafka 客户端，也提供了对死信队列的支持。

死信队列（DLQ）的主要思想是，当在处理消息时遇到某种不可恢复的错误或异常，而不是简单地丢弃这些消息或持续重试，你可以将它们重定向到一个特定的 Kafka 主题，称为死信主题。后续，你可以对死信主题中的消息进行审查、警报、修复和重新处理。

例如，在 Kafka Streams 应用中，处理函数可以选择在遇到无法处理的记录时将其发送到死信队列。Kafka Connect 同样支持 DLQ；当连接器或转换器遇到错误时，它可以将出错的消息发送到死信队列。

如果你正在使用 Kafka 生产者和消费者客户端库，并希望实现类似的行为，你可能需要自己实现逻辑：捕获处理消息时的异常，并将问题消息发布到指定的死信主题。

总的来说，虽然 Kafka 本身并没有内置死信队列的概念，但在其生态系统和一些客户端库中确实提供了这种功能。


## consumer group 有什么用
Kafka 的消费者组（Consumer Group）是一个重要概念，并在消息消费的扩展性和可靠性方面发挥着核心作用。以下是消费者组在 Kafka 中的几个关键点和作用：

### 1. **负载均衡**
- **分配分区**：消费者组内的各个消费者会在 Kafka 主题的不同分区上进行消息消费，实现负载均衡。
- **动态调整**：如果组内增加或减少消费者，分区分配会自动重新调整。

### 2. **并行处理**
- **分区作为并行单元**：Kafka 主题分区和消费者组之间的映射允许并行处理消息。
- **横向扩展**：可以通过增加消费者数量来线性地提高处理能力。

### 3. **容错**
- **消费者失败**：如果一个消费者失败，其分配的分区会被其他消费者接管。
- - **恢复状态**：允许消费者在故障后从上次处理的位置继续消费。
- **无消息丢失**：确保每条消息至少被消费组内的一个消费者处理。

### 4. **读取顺序保证**
- **分区内顺序**：每个消费者读取一个或多个分区的消息，并保证在一个分区内部，消息的处理顺序与其被写入的顺序一致。



## mirror maker是什么
Kafka 提供了一个叫做 "MirrorMaker" 的工具，用来在不同的 Kafka 集群之间复制数据。MirrorMaker 可以将一个 Kafka 集群的数据镜像到另一个 Kafka 集群。它通常用于以下几种场景：

- **灾难恢复**：在主要集群出现问题时，可以切换到镜像集群来继续服务。
- （先不要提）**数据聚合**：将多个小集群的数据聚合到一个大集群中进行集中分析。
- **地理复制**：在不同地理位置的数据中心之间复制数据，以减少数据访问的延迟。


下面的不用管：
  
### 如何工作？
MirrorMaker 使用了 Kafka 的生产者和消费者 API。基本工作流程如下：
1. **消费源集群的数据**：MirrorMaker 包含一个或多个消费者来订阅源 Kafka 集群中的一个或多个主题。
2. **生产数据到目标集群**：MirrorMaker 同时包含一个生产者，这个生产者将源集群消费到的数据发送到目标 Kafka 集群。
   
### 配置
MirrorMaker 的配置涵盖了消费者和生产者的配置，还包括一些特定的设置项。主要配置包括：
- **消费者配置**：定义如何连接到源集群，以及其他消费者设置（例如消费组 ID）。
- **生产者配置**：定义如何连接到目标集群，以及其他生产者设置（例如 ACKs 设置）。
- **主题白名单/黑名单**：定义哪些主题需要被复制。
   
### 注意事项
虽然 MirrorMaker 提供了一种简单直接的方式来在 Kafka 集群之间复制数据，但在大规模或高吞吐的环境中，它也可能遇到一些挑战，例如：
- **同步问题**：确保源集群和目标集群之间的数据同步的准确性和及时性可能是个挑战。
- **配置复杂性**：在处理更复杂的用例时，可能需要更多的配置和管理工作。
   
在一些情况下，社区和一些组织可能会选择使用其他的工具或服务来进行 Kafka 数据的复制，例如 Confluent 的 Replicator。


## kafka宕机时的数据恢复和broker恢复

### kafka宕机时正在处理的数据怎么办？
#### 简要
首先回答： kafka的**所有消息都是保存在disk中的**，数据并不会直接消失。broker恢复后就可以继续处理
主要回答：
1. 正在消费的消息和在队列中还没有被消费的消息：可以通过让consumer从上次提交的Offset重新开始读取
2. producer正在提交的消息：通过producer的重试机制重写写入数据。或者重试多次失败时，写入dead letter queue等待未来处理。

#### 详细
当Kafka宕机时，对于处理中的数据、未处理的数据以及新进入的数据，如何处理是一个关键问题。以下是建议的处理方式：

1. **已经在处理中的数据**：
   - **Consumer的位移管理**：在Kafka中，consumer的进度是通过offset来跟踪的。当一个message被消费者处理完毕后，消费者可以选择是否提交这个offset。如果在处理message的过程中Kafka宕机或其他任何问题出现，消费者在下次启动时可以从上次提交的offset开始重新处理数据。
   - **幂等性**：为了确保数据不会被处理两次，消费者的处理逻辑应该是幂等的。

2. **未处理的数据**：
   - 由于Kafka的持久化特性，即使broker宕机，数据仍然保存在磁盘上。当broker恢复后，数据会再次变得可用，消费者可以继续从它停止的地方开始消费。
   - 如果设置了足够的replication factor，即使某些broker宕机，数据的其他replica仍然可以被消费。

3. **新进入的数据**：
   - **Producer重试**：Kafka的producer客户端通常有重试机制。如果一个message发送失败（例如因为broker宕机），producer可以选择重试发送。需要确保producer的重试策略和设置是适当的，以避免数据丢失或重复。
   - **Dead-letter Queue**：如果producer重试失败多次，可以选择将消息发送到一个dead-letter queue，这样可以稍后进行手动干预或分析。
   - **外部存储**：在某些极端情况下，例如Kafka集群长时间不可用，可以考虑将数据暂时保存到外部存储，如数据库或对象存储，等Kafka恢复后再进行同步。

4. **监控和警报**：
   - 建立有效的监控和警报机制，以便在宕机或任何其他故障时快速响应。

5. **备份和恢复策略**：
   - 考虑定期备份Kafka的数据。虽然Kafka的数据是持久化的，但备份可以作为额外的安全网，以防发生数据丢失或损坏。

总之，正确配置和使用Kafka的特性，如offset管理、producer重试和replication，可以确保即使在宕机或其他故障情况下，数据仍然是完整、一致和可用的。

### kafka宕机了怎么处理（恢复broker）

首先：kafka是大**型分布式集群**，每次Kafka宕机**并不是所有的brokers都不可用**。

然后，检查日志：使用Kafka自带的管理工具（如 kafka-topics.sh）验证服务状态；或者**连接到zookeeper**，通过日志检查问题，同时**看其他broker的状态，找到是哪个broker寄了**

在安全地解决任何明显的问题后（比如网络问题，硬件故障），尝试重启Kafka broker。注意观察broker启动期间的日志输出，以获取任何有关问题的提示。

data integrity后，验证topic的数据完整性**data integrity**。在Kafka中，由于其分布式和复制的特性，数据通常会被备份在多个replica中。确保所有的replicas都是健康的。

如果你有备份策略，考虑使用备份来恢复丢失的数据。
如果部分brokers仍然不健康，可能需要从**健康的brokers迁移partitions**。


### 消息始终无法被消费成功：

处理方法简化：
1. 查看日志找到原因： 
2. 可以在隔离环境中进行message reply来重现问题 **reproduce the issue**，找原因
如果是消息格式问题就调整producer/consumer的逻辑
3. 实在不行就放进dead letter queue
4. 考虑硬件性能问题，给broker的server 提高内存， more powerful cpu



如果在 Kafka 中某些消息始终无法被成功消费，这可能是由以下原因造成的：

1. **消息格式或内容问题**：消费者应用在尝试解析或处理消息时可能会出现异常。
   
2. **消费者的处理逻辑问题**：消费者的业务逻辑在处理某些特定消息时可能会出现错误或异常。

3. **消费者的资源问题**：消费者可能遇到内存溢出、数据库连接失败或其他资源限制问题。

4. **网络问题**：消费者与 Kafka 之间的网络问题可能导致消息延迟或传输失败。

5. **消费者的配置问题**：可能存在关于消费者如何连接到 Kafka、如何处理消息或其他相关设置的配置问题。

处理方法如下：

1. **日志和监控**：首先查看消费者和 Kafka broker 的日志，以找出可能的错误或警告信息。

2. **调试和重放消息**：
   - 如果可能，尝试在一个隔离的环境中重放问题消息，以便更深入地了解问题。
   - Kafka 提供了重放消息的能力，允许从特定的 offset 开始重新消费。

3. **错误处理策略**：
   - 考虑实现一个死信队列 (Dead Letter Queue, DLQ) 策略，将不能处理的消息转移到一个特定的 topic。
   - 对于那些可恢复的错误，考虑使用重试策略。

4. **消息校验**：
   - 在消费消息之前，进行消息内容的校验。如果消息格式不正确或包含无效数据，则可以直接丢弃或移至死信队列。

5. **资源管理**：
   - 确保消费者有足够的资源来处理消息，例如内存、CPU 和网络。
   - 如果资源受限，考虑优化消费者的代码或增加资源。

6. **消费者代码和配置的审查**：
   - 审查消费者的代码和配置，确保没有明显的错误。
   - 考虑进行性能测试和负载测试，以确定消费者在高负载下的行为。

7. **更新和升级**：
   - 如果是由于已知的 bug 导致的问题，考虑更新或升级 Kafka 客户端库。

8. **监控和报警**：
   - 使用工具进行持续监控，如 Prometheus、Grafana 等。
   - 设置报警，确保在消息处理出现问题时能够及时得到通知。

9. **团队协作**：与生产消息的团队合作，确保消息格式和内容的正确性。

## poll pull

Kafka 消费者是使用 "poll" 操作来获取消息的，而不是 "pull" 操作。Kafka 消费者通过轮询（polling）的方式从 Kafka 服务器获取消息，而不是主动向 Kafka 服务器请求消息。

这是 Kafka 消费者的工作原理：
1. 消费者订阅一个或多个主题（topics）。
2. 消费者定期调用 "poll" 操作来检查是否有新的消息可用。"poll" 操作是一个非阻塞的操作，可以设置一个超时时间，以确定是否有消息可用。
3. 如果有新的消息可用，消费者会将这些消息从 Kafka 服务器拉取到本地，并将其处理。
4. 处理完消息后，消费者可以提交偏移量（offset）以记录已处理的消息位置，以便在重启后继续消费。

这种轮询的方式允许 Kafka 消费者按照自己的速度拉取消息，而不需要一直等待新消息的到达。这也使得 Kafka 消费者可以有效地处理大量的消息，并具有灵活性和可伸缩性。

总结：Kafka 消费者使用 "poll" 操作来主动拉取消息，而不是 "pull" 操作。这种方式使得消费者可以控制消息的获取速度，适应不同的消费情况。