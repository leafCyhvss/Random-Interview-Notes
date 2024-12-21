
## 后端-安卓的定义：
The backend refers to the **server side** of an application, responsible for **data processing, business logic, and database interactions**

ensure the application's functional logic and data storage security.


Android 是Both 前后端：
It is mainly about **user interfaces, user experiences, and client-side logic**
Android developers often need to interact with the backend, such as **fetching** data from a server via APIs or **sending** data to the server

both backend and Android development share common ground in areas such as architecture design, data processing, and API integration


Although my primary experience is in backend development, I understand the importance of **collaboration between frontend and backend teams**. I am aware that both backend and Android development share common ground in areas such as architecture design, data processing, and API integration. Android developers need to fetch data from the backend and ensure secure data transmission. I believe my backend experience will help me better understand and optimize the communication between client and server, making the application more efficient and reliable. I am eager to take on new challenges and expand my skill set




## 反问的问题：

I know this position is very competitive. But if I were accepted, could you please give me an idea of what my typical day would look like?

What are the key areas or skills that you would expect me to focus on if I join the team？


What’s the team’s biggest challenge so far ? 
What’s my role/project going to be if I join the team ? 



## B+树为什么比B树更适合当作mysql索引

B+树是一种**平衡树**数据结构，常用于数据库和文件系统中。它是一种**多路搜索树**，其每个节点包含多个子节点，并且**所有叶子节点都在同一层**。B+树的每个节点包含指向子节点的指针和关键字，不同于B树，**B+树的所有数据都存储在叶子节点中，内部节点只存储索引。**


**中文回答：**

B+树和B树都是平衡树，常用于数据库和文件系统中，但B+树在许多方面比B树更适合用作数据库索引。以下是两者的主要区别和B+树的优势：

1. **所有数据都在叶子节点**：
   - **B树**：数据存储在所有节点（包括内部节点和叶子节点）。
   - **B+树**：数据仅存储在叶子节点，内部节点仅存储索引。

   **优势**：B+树的内部节点仅存储索引，叶子节点可以存储更多的数据。这使得B+树的内部节点更小，树的高度更低，从而减少了查找过程中所需的磁盘I/O次数。

2. **顺序访问效率**：
   - **B树**：顺序访问需要通过树的结构逐个节点查找，效率较低。
   - **B+树**：叶子节点形成一个有序链表，顺序访问和范围查询非常高效。

   **优势**：B+树的叶子节点通过链表连接，允许快速顺序访问和范围查询，这在数据库中非常常见。

3. **树的高度和磁盘I/O**：
   - **B树**：由于数据和索引混合存储，树的高度相对较高，增加了磁盘I/O次数。
   - **B+树**：内部节点存储更多的索引，减少了树的高度，从而减少了磁盘I/O次数。

   **优势**：B+树更低的高度意味着在数据库查找操作中更少的磁盘I/O，这提高了性能。

4. **拓展性更好，节点分裂和合并**：
   - **B树**：由于数据和索引混合存储，节点的分裂和合并相对复杂。
   - **B+树**：仅需在叶子节点进行数据操作，分裂和合并操作更简单。

   **优势**：B+树的节点分裂和合并操作更简单，更适合动态数据增长的处理。

**英文回答:**

Both B+ trees and B trees are balanced tree structures commonly used in databases and file systems, but B+ trees offer several advantages over B trees for database indexing:

1. **Data Stored Only in Leaf Nodes**:
   - **B Tree**: Data is stored in all nodes (including internal and leaf nodes).
   - **B+ Tree**: Data is stored only in leaf nodes, with internal nodes storing only indices.

   **Advantage**: In B+ trees, internal nodes only store indices, allowing them to be smaller and enabling the tree to have a lower height. This reduces the number of disk I/O operations needed during searches.

2. **Efficient Sequential Access**:
   - **B Tree**: Sequential access requires traversing the tree structure node by node, which is less efficient.
   - **B+ Tree**: Leaf nodes form a sorted linked list, making sequential access and range queries very efficient.

   **Advantage**: The linked leaf nodes in B+ trees allow for quick sequential access and range queries, which are common in databases.

3. **Tree Height and Disk I/O**:
   - **B Tree**: Mixing data and indices in nodes results in a higher tree height, increasing disk I/O operations.
   - **B+ Tree**: Internal nodes store more indices, reducing the tree height and thus decreasing disk I/O operations.

   **Advantage**: The lower height of B+ trees means fewer disk I/O operations during database lookups, improving performance.

4. **Node Splitting and Merging**:
   - **B Tree**: Node splitting and merging are more complex due to the mixing of data and indices.
   - **B+ Tree**: Data operations are confined to leaf nodes, making splitting and merging simpler.

   **Advantage**: B+ trees have simpler node splitting and merging operations, making them better suited for handling dynamic data growth.

These advantages make B+ trees more efficient and better suited for database indexing compared to B trees, especially in environments where range queries and sequential access are common.


## 自我介绍


My name is Junhui Yang, and I am currently a software engineer working for Walmart global tech in the Dallas office. 

I hold a Master's degree in computer science from University of Southern California, which is based in Los Angeles, and previously I got my bachelor's degree in Internet of Things engineering, which is a specialization branch in computer science. And so I got my bachelor's degree from Nankai University in Tianjin, in China. 

As for the technical part, I am a skilled Java and Python programmer. I have a lot of backend development experiences, especially on developing with frameworks like Spring, Spring Boot as well as working on microservice architecture. I am familiar with relational databases like  MySQL and nonsql databases like MongoDB, Azure cosmos. 

I also accumulated a lot of experiences and skills in AWS and Microsoft Azure, and other tools like kafka, redis, splunk and others.

I think my skills and my professionalism can bring a positive contribution to your team.



## 项目介绍

I am working for the CPC team intl in wm glb tech, which is mainly for Canada/MX/UK market. Our project involves two main applications: the Cart and Checkout, both of them are built up with Spring Boot and Microservice systems, Azure cosmos db, kafka. These applications facilitate the entire process from customer scanning an item in store to placing an order, sending a purchase contract and finalizing payment, making both online and in-store shopping experience seamless and efficient.

My work here mainly includes several aspects: First I designed and developed a new promotion engine in the Cart app using spring boot, which applies the discount from coupon to each eligible item in the buyer's cart. It can handle different promotion events in Canada and MX market cooperating with store config service and coupon service.  And I also implemented the integration of associate discount service for the mx market especially. For the checkout application side, I developed a promotion handler that gets data from the OE engine, processes data into correct format and forwards data to kafka and cosmos db.

And some other more daily things: test / code review / documentations






## Chanllenge

The biggest challenge was to manage high-volume, real-time data exchange among microservices in our Order and Contract system. Given the huge data volume, we first decided to use Kafka message queue to handle the communication messages among the service applications. It a event-driven tool and it can handle a lot of data but it still cannot satisfy our requirement on low latency at the first time.

To address this issue, I optimized the Kafka settings for batch size and commit latency, and adopted a more efficient data serialization format instead of using Json we used Protobuf. Furthermore, I implemented database sharding to distribute data across multiple instances as well as adding more service instances, reducing pressure on a single server. And make messages being produced more quickly. Lastly, we conducted performance monitoring using google cloud monitoring and Grafana, which helped us identify and solve bottlenecks in time.


