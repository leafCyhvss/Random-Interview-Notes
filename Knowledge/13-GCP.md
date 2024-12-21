一个概念：**Fully Managed Service**
## MIG & computing engine

### GCP如何实现auto-scaling
Auto-scaling is possible with the Google Cloud Platform's **managed instance groups**. Managed instance groups are collections of identical instances that were created from the same master template. The easiest way to auto-scale in Avi Vantage is to scale based on how much CPU a group of virtual machine instances uses.
MIGs 是一个 GCP 计算引擎的服务，允许用户管理并自动扩展多个相同配置的虚拟机实例。(是针对computing engine来的概念)
**自动扩展和收缩**：基于一组**预定义的指标（如 CPU 利用率**），自动增加或减少实例数量。
**实现高可用性**：如果某个**实例失败，MIGs 会自动重新启动或替换该实例**，确保总是有足够数量的健康实例运行。
**进行滚动更新**：MIGs 允许用户进行滚动更新，从而在不中断服务的情况下逐步更新应用或系统。

好吧，实际上部署应用是用GKE管理，不用MIG和computing engine。GKE提供一样的功能。**两者都自带负载均衡**

### app engine 不理赖与MIG，自己scale
完全托管的**平台即服务**（PaaS）解决方案。它允许开发人员轻松部署应用程序，而不需要管理基础设施（例如服务器和网络）。
无需管理服务器：你只需关注代码。部署应用后，Google会处理托管、监控、运维和扩展。
自动扩展：App Engine根据应用的流量自动调整资源，从零流量扩展到大流量，然后在需求下降时自动缩减。



## bigtable

### 原理
1. 基础概念
Bigtable 是一个稀疏的、分布式的、持久的多维排序的映射。其基本的数据单元是一个表Table，表中的每一行由行键来唯一标识。
行被组织成列族，列族是行内的一组相关列的集合。列名是由列族名和修饰符组成的。
在表中，数据按照行键的字典顺序进行存储。
**Table (表)**: A collection where you store your data.
Example: HistoricalQuotes
**Row Key (行键)**: The unique identifier for data that is stored in a row.
Example: Cust1234_2023-09-19
**Column Family (列族)**: A logical grouping of columns under a common label. All columns within a column family are usually related in terms of access patterns.
Example: quote_details

2. 存储值类型
- 与传统的关系型数据库不同，Bigtable 列可以存储多个版本的同一数据，版本是通过时间戳来区分的。（英文下一行）
- **Bigtable columns can store multiple versions of the same data. These versions are distinguished by their timestamps.**
- **Time-Series Storage**
- Bigtable 的值是未解释的字节串，即它们都是**raw byte string**。数据以字节串（byte strings）的形式存储，不同于关系型数据库（如MySQL）那样有**it doesn't have pre-defined data types like int char varchar** 
- This offers flexibility since developers **can store virtually any type of data**, but it also means that the **applications are responsible for serializing and deserializing** the data as required."
3. 查询方法
Bigtable 支持按行键、按时间戳和按列进行筛选的数据检索。它支持单行查找、范围查找和完全扫描。
由于 Bigtable 优化了按行键的读取，行键的设计对于查询性能至关重要。
**Row Key Query** (按行键查询): Retrieve data for a specific customer on a specific date.
Example Key: Cust1234_2023-09-19
**Range Query** (范围查询): Retrieve data for a specific range of row keys.
Example Range: Cust1234_2023-08-19 to Cust1234_2023-09-19
**Column Filter** (按列筛选): Filter results based on specific columns.
Example: Filtering by quote_details:product_name column.
4. 连接方法
你可以使用 Google Cloud Bigtable 的客户端库来连接和与其交互。这些客户端库为多种语言提供，包括 Java、Go、Python、Node.js 等。
除了标准的客户端库外，Bigtable 也支持 HBase API，这意味着现有的 HBase 应用程序可以轻松迁移到 Bigtable。
**与bigtable交互不需要orm框架。**使用 BigtableDataClient 进行读取、写入等操作
BigtableDataSettings用来配置连接参数，cliet用来创建新连接。

5. 优点
**高度可扩展**：Bigtable 可以线性扩展到数百个 PB 的数据，而且没有降低性能。
高性能：为大数据和实时应用程序提供**低延迟**和**高吞吐量**。
**支持时间序列数据**
稳健性：Google 的基础设施提供了内置的冗余和故障转移。**由于支持data verson并且有多个复制，有很好的持久性**

**缺点：不支持原子性也就是不支持transaction**

### 为什么用Bigtable存历史报价
**Scalability**: Bigtable can scale to handle petabytes of data, accommodating vast amounts of historical quote data.
**Low-latency Read/Write:** Ensures that quote information can be written and retrieved rapidly, which is crucial for high-frequency trading or scenarios with extensive customer inquiries.
**Time-Series Storage:** **Bigtable's data versioning feature allows for easy tracking of changes and history for each quote**, with each version having its own **timestamp.**

**Robust Querying Capabilities:** Efficient querying of historical quotes based on time, customer, or other dimensions is supported.


### 如何设计bigtable的 row key

#### 原理
基于时间戳的Key：将时间戳包含在row key中可确保数据按序写入，并可以为时间范围扫描提供有效读取。但是，使用原始时间戳作为row key的前缀可能会因多次写入当前时间戳而导致热点。

反转时间戳：如果你经常读取最新的数据并希望它位于顶部，使用反转的时间戳（当前最大时间戳减去实际时间戳）可能会有帮助。

复合Key：结合多个属性创建更具描述性的row key。例如，你可能会组合产品ID和时间戳，如 product12345#20230918T103055，以便高效查找特定产品的时间范围。但是key不能太长

使用**Salting**进行分片：为了避免热点，特别是当特定范围的row key发生许多写操作时，你可以用**几个随机字节（盐）为row key添加前缀**。这可以将**写操作分散到多个节点**上。但是，这会使读取变得复杂，因为你需要扫描多个前缀。

避免单调递增的值：如前所述，使用总是递增的值（如简单的时间戳）可能会导致热点。设计row key时始终要考虑这一点。

**（所以避免hotspot方法： revered timestamp. combined key, salting)**
#### 历史报价服务

使用复合key:
使用**productID + 反转时间戳作为row key**的设计方法可以帮助你达到以下目的：

1. 实时访问：由于最新的数据（根据反转的时间戳）会被放在顶部，你可以非常快速地获取一个产品的最新报价。

2. **避免热点avoid Hotspotting**：反转时间戳可以确保写入不会集中在特定的节点上，从而分散了写入负载。
   原理
   Bigtable并不是一个单线性结构，而是分布式的。数据是根据row key的范围划分并存储在不同的节点上的。所以，即使新的写入都进入了表的开始位置，但由于Bigtable是分布式的，这些写入还是会被均匀地分布到多个节点上。

3. 产品历史查询：当你想查询一个产品的价格历史时，只需根据产品ID进行范围查询即可。


## bigtable vs datastore

firestore优点
**1全自动管理，自动scale 备份 sharding
2数据一致性强：虽然还是eventually consistent，不需要用户进行复杂的配置或额外的步骤，尽量保证数据是最新的
3不需要服务器
4支持transaction**

### Bigtable:

#### Advantages:

1. **Performance at Scale**: Bigtable offers low-latency and high-throughput, making it especially suitable for large analytical and operational workloads, including time-series data.
2. **Integration with Big Data Tools**: Bigtable integrates well with tools like Hadoop, Dataflow, and BigQuery.
3. **Open-source Interface Compatibility**: It's compatible with the HBase API, allowing easier migration for HBase workloads.
4. **Single-digit Millisecond Latencies**: It provides fast access to large amounts of data.
5. **Global Replication**: Offers replication for higher availability and geographic data placement.

#### Disadvantages:

1. **Complexity**: Requires more manual setup and configuration compared to Datastore.
2. **Cost**: Given its performance and capabilities, Bigtable might be overkill and more expensive for small applications or when you don't need its specific advantages.

### Datastore (Firestore in Datastore mode):

#### Advantages:

1. **Fully Managed**: Automatic scaling, sharding, and replication with no operational overhead.
2. **Strong Consistency**: Provides strong consistency for queries without the need for complex configurations.
3. **Integrated with GCP Services**: Tighter integration with App Engine and other GCP services.
4. **Serverless**: No need to manage server infrastructure or configurations.
5. **Transaction Support**: Supports ACID transactions.

#### Disadvantages:

1. **Not Suitable for Time-Series Data**: While it can handle time-series data, it's not optimized for it like Bigtable.
2. **Limited Querying Capabilities**: Doesn't support JOIN operations or other complex queries as relational databases do.

### Conclusion:

While both are NoSQL databases, the choice between Bigtable and Datastore depends on the specific needs of the project. Bigtable is optimized for large-scale, high-throughput applications, especially those that require integration with big data tools. On the other hand, Datastore (Firestore in Datastore mode) is ideal for web and mobile applications that require a serverless, fully managed database with strong consistency and transactional capabilities.



## GKE+cicd
### the flow of CICD pipeline:
My code PR-> triggers pipelines to compile and run test auto and sonarQube for style checking -> peer review(left comments) -> merge to dev master 
Merging to the master branch triggers another pipeline, from dev -> qa -> staging-> product. 从dev之后，就是QA接手了(qa, staging)，与我无瓜

### jenkis+GKE

1. jenkis安装 Docker Plugin, Google Cloud Storage Plugin, 和 Google Kubernetes Engine Plugin。
2. Jenkins 构建步骤:

源代码拉取: 从 Git、Bitbucket 或其他源代码管理系统中获取你的代码。
构建 Docker 镜像: 使用 Dockerfile 构建你的 Docker 镜像。
推送 Docker 镜像: 将构建的镜像推送到容器镜像仓库。常用的是 Google Container Registry (GCR)，但也可以使用 Docker Hub 或其他容器仓库。
3. 部署步骤
**拉取 Kubernetes 配置**: 如果 Kubernetes 的配置（如 Deployments、Services）存储在源代码管理系统中，需要拉取它们。
设置 kubectl 上下文: 确保 Jenkins 有权使用 kubectl 命令与 GKE 集群交互。
更新 Kubernetes 配置: 如果你的部署或服务配置需要引用特定的容器镜像版本，这一步很重要。可以使用工具如 sed 或 envsubst 替换配置文件中的镜像版本标记。
应用 Kubernetes 配置: 使用 kubectl apply 命令将更新后的 Kubernetes 配置部署到 GKE 集群。

### autopilot
**一般使用GKE autopilot来自动manage kubernetes node


**完全托管的节点**：在 Autopilot 模式下，你不再管理节点或节点池。Google Cloud 负责所有节点管理、升级和修补工作。

优化的资源使用：Autopilot 会**自动为每个 pod 分配适当的资源**，确保高效利用资源并避免过度配置。

**生产就绪的默认配置**：Autopilot 集群预配置为遵循最佳实践，提供强大的安全性、可靠性和性能。

**自动扩展**：无论是集群还是工作负载，Autopilot 都可以根据需要自动扩展。

**按照实际资源使用计费**：与标准 GKE 集群按节点计费不同，Autopilot 按照实际工作负载的 vCPU、内存和存储使用情况计费。

### k8s:
一个 Kubernetes 节点可以运行多个 Pod，而每个 Pod 可以包含一个或多个容器。

每个节点由 Kubelet（一个在节点上运行的 Kubernetes 代理）管理，用于确保容器在 Pod 中正常运行。

Pod 是 Kubernetes 的最小部署单元。一个 Pod 可以包含一个或多个容器。
在 Pod 内的所有容器共享相同的网络命名空间，意味着它们可以通过 localhost 访问彼此，并共享同一个 IP 地址和端口空间。
通常情况下，具有紧密耦合的、需要紧密通信的容器会放在同一个 Pod 中。而不同的服务或应用通常会被放在不同的 Pod 中。

## 在gcp微服务看log

在Spring Boot微服务中，使用`SLF4J`和`Log4j2`进行日志记录是非常常见的。与此同时，`AOP`（Aspect-Oriented Programming）确实是一个强大的技术，可以用于跟踪和记录方法的执行，特别是对于诸如服务调用、数据库交互等常见操作。

以下是如何结合这些技术进行日志记录的简要步骤：

 1. 配置SLF4J和Log4j2

然后，为`Log4j2`创建一个配置文件（例如`log4j2.xml`）。

 2. 使用AOP记录日志

你可以使用Spring的`@Aspect`注解创建一个切面来记录日志。例如：

```java
@Aspect
@Component
public class LoggingAspect {

    private static final Logger logger = LoggerFactory.getLogger(LoggingAspect.class);

    @Around("execution(* com.yourpackage..*.*(..))") // 匹配你的包中的所有方法
    public Object logMethodExecution(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object result = joinPoint.proceed();

        long elapsedTime = System.currentTimeMillis() - start;
        logger.info("Method {} executed in {} ms", joinPoint.getSignature(), elapsedTime);

        return result;
    }
}
```

确保你的Spring Boot应用已经启用了AOP，通过在`pom.xml`中添加相应的依赖：


 3. 将日志发送到Splunk（可以不需要用splunk，不说）

你可以配置`Log4j2`将日志直接发送到Splunk。Splunk通常提供一个HTTP Event Collector（HEC）端点，你可以使用Log4j2的HTTP Appender将日志事件发送到该端点。

 4. 在GCP上监控日志

当你的应用部署在GCP（Google Cloud Platform）上时，可以利用Google Cloud的**Operations Suite**（以前称为`Stackdriver`）来收集、查看和分析日志。

确保你的GCP项目已经启用了Operations API，并为VM或Kubernetes引擎配置了适当的IAM权限，以便可以从应用容器中拉取日志。

同时用的情况：使用Stackdriver进行基本的监控和日志管理，而使用Splunk进行高级分析或特定的安全用例

## stackdriver
Stackdriver 是 Google Cloud Platform 的原始名称，用于其运维管理工具套件。后来，它被重命名为 "Google Cloud Operations Suite"。在这个套件中，主要有以下几个核心组件：

**Cloud Monitoring**：用于实时性能监控、设置警报和创建定制的仪表板。
**Cloud Logging**：收集、存储、查询和可视化日志数据。
Cloud Trace：用于分析应用程序的延迟数据。
Cloud Debugger：在不停止应用程序或降低其性能的情况下进行实时调试。
Cloud Profiler：自动化的应用程序性能分析。
当我们谈论在 GCP 上进行日志管理时，我们确实是指使用 Cloud Logging（前身为 Stackdriver Logging）来处理这些日志。这个服务可以与 GKE、Compute Engine、Cloud Functions、App Engine 和其他 GCP 服务紧密集成，以自动捕获和存储日志。同时，你还可以创建日志基于指标的警报，并与 Cloud Monitoring 集成来进行更深入的分析和可视化

## load balancing

### 一笔带过
1. **HTTP(S) 负载均衡器**：
   - 是一个全局的负载均衡器，用于将 HTTP 和 HTTPS 流量路由到最近的全局后端。
   - 适用于 Content Delivery Network (CDN) 集成。
   - 提供 SSL/TLS 终端和 HTTP/HTTPS 负载均衡。

2. **TCP/SSL 代理负载均衡器**：
   - 为非 HTTP/HTTPS 的 TCP 流量提供全局负载均衡。
   - SSL 代理负载均衡器提供 SSL/TLS 终端。

3. **内部 TCP/UDP 负载均衡器**：
   - 是一个区域性的负载均衡器，用于在同一 GCP 区域的实例之间平衡内部 TCP/UDP 流量。

4. **网络 TCP/UDP 负载均衡器**：
   - 为 TCP 和 UDP 流量提供全局负载均衡。
   - 用于路由流量到多个区域的后端。

5. **内部 HTTP(S) 负载均衡器**：
   - 是一个区域性的负载均衡器，用于在同一 GCP 区域的实例之间平衡内部 HTTP/HTTPS 流量。

这些负载均衡器提供了自动缩放、健康检查、多区域负载均衡和其他高级功能。
### GKE

如果你使用 Google Kubernetes Engine (GKE)，Kubernetes 也提供了其自己的负载均衡机制。例如，当你在 **GKE 中创建一个 Kubernetes 服务类型为 LoadBalancer 时，GCP 会自动为这个服务分配一个外部 IP 地址，并设置合适的负载均衡规则**。

## cloud sql
当然可以！`Cloud SQL` 是 Google Cloud Platform (GCP) 提供的一种完全托管的关系型数据库服务。它旨在简化数据库的设置、维护、管理、备份和扩展。以下是关于 Cloud SQL 的主要概述：

1. **支持的数据库**：Cloud SQL 支持几种关系型数据库管理系统（RDBMS），包括 MySQL、PostgreSQL 和 Microsoft SQL Server。

2. **完全托管的服务**：这意味着 Google 负责日常的运维任务，如**数据库备份、软件打补丁和更新、故障恢复**等。

3. **性能和高可用性**：
   - Cloud SQL 提供了**自动备份、数据库复制和高可用性**的设置。
   - 它支持跨区域的复制，确保在一个区域出现故障时可以无缝切换到另一个区域。

4. **扩展性**：
   - Cloud SQL 支持垂直和水平扩展。
   - 用户可以轻松地调整实例的大小，或者设置只读的副本来分摊读取流量。

5. **安全性**：
   - 数据在传输中默认使用 SSL 加密。
   - Google 提供了数据加密的选项。
   - Cloud SQL 被集成到 Google's VPC，可以设置私有 IP 连接。
   - 也支持 Cloud IAM，使得访问管理更为精细。

7. **数据迁移**：Google 提供了各种工具和服务，使得从本地、其他云或其他 GCP 服务到 Cloud SQL 的数据迁移变得更为容易。
## functions 触发
在 Google Cloud 上，Cloud Functions 可以通过多种事件触发。以下是一些常见的触发方式：

1. **HTTP 触发器**：Cloud Functions 可以像普通的 RESTful API 一样被 HTTP 请求触发。

2. **Google Cloud Storage**：Cloud Functions 可以配置为响应 Google Cloud Storage 事件，例如当一个新文件被上传到存储桶或一个现有文件被删除时。

3. **Google Cloud Pub/Sub**：当新消息发布到 Pub/Sub 主题时，Cloud Functions 可以被触发。

4. **Firebase 相关的触发器**：
   - **Realtime Database**：函数可以在数据被写入、更新或删除时触发。
   - **Firestore**：函数可以在数据被写入、更新、删除或查询时触发。
   - **Firebase Authentication**：例如，当用户创建或删除帐户时触发函数。
   - **Firebase Analytics**：基于特定的分析事件触发。

5. **Google Cloud Scheduler**：可以使用 Cloud Scheduler 来定时触发 Cloud Functions，就像 cron 作业一样。

6. **直接调用**：使用 Google Cloud SDK (`gcloud`) 或 Cloud Functions API，你可以直接调用 Cloud Function。


例如，对于 HTTP 触发器，当你部署一个函数并指定它是一个 HTTP 函数时，你会得到一个 URL，你可以直接发送请求到这个 URL 来触发函数。
对于其他事件驱动的触发器，如 Cloud Storage 或 Pub/Sub，你需要在部署函数时指定要监听的特定资源和事件类型。


## dataflow
问到就说自己没用过。
但是这道这玩意是基于**apache beam**的完全托管的流和批处理服务
是一个好用的批处理工具，可以进行窗口化处理**Aggregating data based on a time window.**
例如，我们可以创建5分钟的窗口，以计算每5分钟内最受欢迎的产品。
处理过程中持久化的磁盘存储，确保在出现故障时数据不会丢失。

## bigquery

### 基本原理
可以通过predefined view来加速

store in column
capacitor storage format _>Separation of Storage and Compute_ Column Grouping：相邻的分在同一组

自动 scaling sharding

支持
**分区表 (Partitioned Tables)**: 允许按日期或其他属性范围划分数据。
   **聚簇表 (Clustered Tables)**: 按某列或某些列的值排序数据，实现相似值的物理近似存储。


1. **列式存储 (Columnar Storage)**:
   - 数据按列存储，适用于分析型数据库。
   - **高效读取 (Efficient Reads)**: 只需读取查询所涉及的列，从而提高查询速度。
   - **更好的压缩 (Better Compression)**: 列中的值往往有高度的重复性，从而可以实现更好的压缩。

2. **Capacitor 存储格式 (Capacitor Storage Format)**:
   - BigQuery 使用的专有存储格式。
   - **独立的存储和计算 (Separation of Storage and Compute)**: 存储和计算是分离的，可独立扩展。
   - 记录的列式分组 (**Record-level Column Grouping)**: 改进列式存储的策略，通过将相邻记录分组在一起。
   - **逐块处理 (Block Processing)**: 数据被切分为小块，每块独立编码和压缩。

3. **数据压缩 (Data Compression)**:
   - 列数据可以高效压缩，降低存储成本，提高查询速度。

4. **数据分片 (Data Sharding)**:
   - BigQuery 将数据分片为多个工作节点并行处理。

5. **分区和聚簇 (Partitioning and Clustering)**:
   - **分区表 (Partitioned Tables)**: 允许按日期或其他属性范围划分数据。
   - **聚簇表 (Clustered Tables)**: 按某列或某些列的值排序数据，实现相似值的物理近似存储。

这些术语为 BigQuery 提供了在处理大数据分析查询时的高效性和快速性的基础。



## 优化bigquery 和 bigtable
优化 BigQuery 和 Bigtable 的性能是一项重要的任务，尤其当面对大规模数据和复杂查询时。这两者的性能优化策略略有不同，由于它们的使用场景和技术实现差异。下面我将列出针对两者的性能优化建议：

### BigQuery:

1. **结构化查询**: 设计高效的 SQL 查询，尽量避免 `SELECT *` 和避免不必要的计算。

2. **使用分区和聚簇**: 分区表按日期或其他字段进行分区，聚簇表按某个列进行排序，使相关数据物理上相邻。

3. **管理查询并发**: 控制同时运行的查询数，避免资源竞争。

4. **避免重复数据**: 尽量避免数据重复或存储过多不必要的数据。

5. **缓存**: BigQuery 自动缓存查询结果，利用这一特性可以减少重复查询的成本。

6. **选择合适的数据存储格式**: 如 Parquet 或 ORC 格式，这些格式可以提高压缩效果和查询性能。

7. **索引和物化视图**: 使用索引加速查询，物化视图将经常查询的结果集存储为一个新的表。

### Bigtable:

1. **设计适当的 Row Key**: 避免热点问题，如使用反转时间戳或随机前缀。

2. **预先拆分表**: 如果预知写入的数据量很大，可以预先定义拆分，从而均匀分布数据。

3. **均匀的写入和读取**: 避免突发的大量写入或读取。

4. **使用过滤器**: 进行读取操作时，使用过滤器只读取必要的数据。

5. **限制单次 RPC 的大小**: 大的 RPC 可能导致超时或延迟，建议将其分解。

6. **垃圾回收策略**: 设定合理的版本保留策略，定期删除旧的、不再需要的数据版本。

7. **压缩数据**: 使用 Bigtable 的内建压缩功能。

8. **监控和警报**: 使用 Cloud Monitoring 监控 Bigtable 的性能指标，及时发现并解决问题。

不同的应用和使用场景可能需要不同的优化策略，所以建议定期评估性能，并根据具体情况调整优化策略。
## Pubsub
### 比较kafka
同样是异步的，event-driven

一旦消息被发布到 Pub/Sub，它就是不可变的，不能被修改。

相比之下，Google Cloud Pub/Sub 的设计更注重完全托管的服务和简化操作：

Pub/Sub 在内部可能使用了类似于副本或冗余的策略来确保数据的可靠性和持久性，但它并**没有**公开类似于 Kafka 中的 leader-follower 机制。
Pub/Sub 将这些细节**抽象化**，使用户能够更加集中于发布和订阅消息，而不用担心底层的数据持久性和可用性问题。
Google Cloud 在其多个数据中心内部为 Pub/Sub 数据提供**冗余**，确保数据的持久性和可用性。



Offset 和 Consumer Offset:

不同于 Kafka 的 offset 概念，Pub/Sub 使用了一个称为“**acknowledgment deadline**”的机制。当消费者从订阅中拉取消息时，它接收到一个特定的 **ack ID，消费者需要在指定的时间内确认该消息已被处理。如果在这个期限内没有确认，Pub/Sub 会重新发送该消息。**
Pub/Sub 本身没有类似于 Kafka 中 consumer offset 的概念，因为它不保留消息的长时间存储（除非配置了死信队列）。一旦消息被确认，它就从 Pub/Sub 的系统中删除。


### 数据模型：
**Topics:** A named resource to which messages can be sent by publishers.

**Messages:** The data that travels between publishers and subscribers. **Each message contains** **data and attributes**. The **data is a base64-encoded string**, and **attributes** provide additional optional information in the form of **key-value pairs**.

**Subscriptions**: Associated with a single topic, a subscription captures the stream of messages sent to that topic. Subscribers "pull" or are "pushed" messages from their subscriptions.

### 消息持久化：
**Retention**: By default, Pub/Sub retains **unacknowledged** messages in a subscription's backlog for seven days.


### 数据是怎么被订阅的：
Consumption Model:
**Pull:** Subscribers can actively request or "pull" messages from a subscription. This method provides more control to the subscriber regarding the rate of message processing. Messages remain in the queue until pulled and acknowledged by a subscriber.
工作负载有明显的**高峰期**，从需要message的应用来拉取模式允许您根据需要增加拉取频率。可以完成flow control

**Push:** Pub/Sub can be configured to automatically "push" messages to subscriber endpoints over HTTP/HTTPS. In this case, the endpoint (usually a web service) processes the message and returns an acknowledgment.
**实时处理：**如果您希望消息在到达时立即被处理，推送模式更为合适。

**Flow Control:** Subscribers can leverage flow control settings to specify the maximum number of unacknowledged messages or maximum size of unacknowledged messages they wish to pull. This allows for effectively managing the rate of message processing.

**Dead-letter Topics:** If a subscriber **fails to acknowledge a message within a certain period** or after a certain number of retries, the message can **be forwarded to a dead-letter topic**, ensuring problematic messages **don't stall** the processing pipeline.


## cloud storage
### 描述Google Cloud Storage的存储类别。
答：GCS提供四种存储类别:

Standard: 用于经常访问的数据。
Nearline: 用于月访问频率较低的数据。
Coldline: 用于年访问频率较低的数据。
Archive: 用于长期存储和归档的数据。

**Bucket是GCS中用于存储对象数据的基本容器。**
可以使用Identity and Access Management (IAM)和访问控制列表（ACL）来控制对Buckets和对象的访问。
**Object Lifecycle Policies**来自动地转移数据到其他存储类别或删除**过时的数据**。

GCS保证即时一致性(strong consistency)。这意味着一旦写入或删除操作完成，后续的读取操作会立即看到这一变化。