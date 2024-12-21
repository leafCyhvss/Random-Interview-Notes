
**JDBC --> JDBCTemplate --> Session --> ⽤EntityManager⾃⼰写Dao --> JPA Interface method ->@Query**

想改子数据库，同时影响母数据库的情况下，怎么做？Lazy Fetching；
改母数据库，影响子数据库，用cascade

## Transaction
### 定义
事务（Transaction）是一个数据库概念，用于保证一组数据库操作**要么全部成功执行，要么全部不执行**。事务必须满足**ACID**属性，即原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

原子性：原子性意味着事务中的所有操作要么全部成功执行，要么全部不执行。如果事务中的任何操作失败，整个事务都会回滚到开始状态。

一致性：一致性保证了事务在执行前后，数据库从一个一致性状态转移到另一个一致性状态。也就是说，事务的执行不能破坏数据库的完整性约束。

隔离性：隔离性保证了并发执行的事务互不干扰。每个事务都在一个隔离的环境中执行，好像没有其他并发事务一样。

持久性：持久性意味着一旦事务提交，其所做的修改就是永久性的。即使在提交后发生系统崩溃，修改也不会丢失。

### 实现
三种方式: 
1. session
2. @transactional（best)
3. entity manager手动管理


在Hibernate中，Session提供了一个开始和提交事务的方法，这就是你看到的Session Transaction。在你的示例中：

```java
transaction = session.beginTransaction(); // 开始一个新的事务
Post savedPost = (Post) session.merge(post); // 将post对象与Session合并，即保存或更新post对象
transaction.commit(); // 提交事务，将改动持久化到数据库
transaction.rollback(); // 在出现错误时回滚事务
```

Session-Transaction可以进行显式的事务管理，但是你需要手动处理事务的开始、提交和回滚。这种方式在处理复杂的事务逻辑或需要微调事务配置的情况下是有用的。

另一方面，Spring提供了声明式的事务管理，这就是你看到的`@Transactional`。使用`@Transactional`，你不需要手动开始和提交事务，Spring会自动为你处理。
**你只需要在配置类（比如Spring Boot的主配置类或者其他的配置类）上添加`@EnableTransactionManagement`，
然后在需要事务管理的方法上添加`@Transactional`。**
如果该方法在执行过程中出现任何异常，Spring会自动回滚事务；如果方法成功执行，Spring会自动提交事务。

```java
@EnableTransactionManagement // 启用Spring的声明式事务管理
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@Service
public class SomeService {
    @Transactional // 该方法需要事务管理
    public void someMethod() {
        // 一些数据库操作...
    }
}
```

对于大多数应用，`@Transactional`提供的声明式事务管理足够使用，它使代码更简洁，更易于维护。

**EntityManager是session的上位替代**

至于EntityManager，它是JPA规范的一部分，Hibernate提供了对其的实现。在Spring Data JPA中，你通常不需要直接使用EntityManager，因为Spring Data JPA提供了更高级别的抽象（如Repository）来简化数据访问。然而，在某些需要更细粒度控制的情况下，你可能需要使用EntityManager，比如执行复杂的JPQL查询或者进行显式的事务管理等。

### 关于entitymanager和session
对于大多数开发者来说，选择`EntityManager`通常是更好的选择，主要有以下几个原因：

1. **规范兼容性**：`EntityManager`是JPA（Java Persistence API）规范的一部分，而JPA是Java EE标准的一部分。选择使用JPA和`EntityManager`意味着你的代码将更加符合规范，这让你的应用更容易在不同的JPA实现（如Hibernate、EclipseLink、OpenJPA等）之间移植。

2. **Spring Data JPA的支持**：如果你使用的是Spring Data JPA，那么Spring已经为你自动配置了`EntityManager`，你可以很容易地在你的Repository或Service中注入并使用它。而且，Spring Data JPA的Repository接口已经为你提供了大量的CRUD操作和查询方法，这使得你很少需要直接使用`EntityManager`或Hibernate的`Session`。

3. **更高级的特性**：`EntityManager`接口提供了一些Hibernate的`Session`接口没有的高级特性，比如实体的生命周期管理、更灵活的查询API等。

尽管如此，如果你需要使用一些Hibernate特有的特性，或者你正在维护一个已经使用了Hibernate `Session`的旧项目，那么直接使用`Session`仍然是一个可行的选择。在这种情况下，你需要知道的是，`EntityManager`实际上是对`Session`的一种包装，你可以通过`EntityManager`获取到底层的`Session`对象。例如：

```java
Session session = entityManager.unwrap(Session.class);
```

总的来说，对于新的开发项目，建议优先选择`EntityManager`，它更符合JPA规范，且能更好地与Spring Data JPA集成。而对于需要使用Hibernate特有特性或已经使用了`Session`的旧项目，直接使用`Session`仍然是一个合理的选择。

#### entitymanager的代码实例
以下是如何在Spring Data JPA中使用`EntityManager`的一个基本示例。在这个示例中，我们将使用`EntityManager`来查询所有的User实体。

首先，假设你有一个名为`User`的实体类：

```java
import javax.persistence.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "name")
    private String name;

    // getters and setters...
}
```

然后，你可以在你的服务类中注入`EntityManager`，并使用它来查询所有的User实体：

```java
import javax.persistence.*;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    private final EntityManager entityManager;

    public UserService(EntityManager entityManager) {
        this.entityManager = entityManager;
    }

    public List<User> getAllUsers() {
        CriteriaQuery<User> query = entityManager.getCriteriaBuilder().createQuery(User.class);
        query.select(query.from(User.class));
        return entityManager.createQuery(query).getResultList();
    }
}
```

在上面的`getAllUsers()`方法中，我们使用了`EntityManager`的`getCriteriaBuilder()`方法创建了一个`CriteriaQuery`，然后设置了查询的结果类型和来源，最后使用`EntityManager`的`createQuery()`方法执行了这个查询，并获取到结果。

需要注意的是，虽然在这个示例中我们直接在服务类中使用了`EntityManager`，但在实际的开发中，你可能更多地会使用Spring Data JPA的Repository接口，这个接口已经为你提供了大量的CRUD操作和查询方法，使得你很少需要直接使用`EntityManager`。



## JPA ORM和访问
是的，你的理解是正确的。`@Repository` 和 `@Entity` 在Spring框架和JPA中扮演着不同的角色：

1. `@Entity`：这是JPA的注解，用于指示持久化引擎，该类是一个实体类，它将会映射到数据库的一个表。在该类中，可以通过各种其他的JPA注解（如 `@Table`，`@Column`，`@Id` 等）来进一步定义这个映射。每个实体类的实例都对应数据库表中的一行记录。

2. `@Repository`：这是Spring的注解，用于定义数据访问层的组件。Spring在运行时会自动创建`@Repository`注解的类的实例，并注入到需要使用它的其他组件中。在 `@Repository` 注解的接口中，你可以定义各种用于访问数据库的方法，例如，保存、删除、查询等操作。

总的来说，`@Entity` 主要跟数据库表的映射有关，而 `@Repository` 主要跟数据的操作有关。你在实体类中定义的映射告诉JPA如何将数据库记录转化为Java对象，而你在仓库接口中定义的方法则告诉Spring如何进行数据库操作。

### repository
在Spring Data JPA中，**`Repository`是用于操作数据库的接口。Spring Data JPA为你提供了一些基础接口，如 `Repository`、`CrudRepository`、`PagingAndSortingRepository`、和 `JpaRepository`。
可以使用@query显式写query(写的是JPQL)
可以用naming convention

- `Repository` 是一个标记接口，它本身不包含任何方法。你可以将自定义的repository接口定义为`Repository`接口的子接口，然后为这个接口添加自定义的数据访问方法。

- `CrudRepository` 接口继承自 `Repository` 并添加了一些用于创建、读取、更新和删除操作的方法。这些方法可以让你操作实体类对象，而不用写任何SQL或JPQL代码。

- `PagingAndSortingRepository` 接口继承自 `CrudRepository` 并添加了一些用于分页和排序的方法。

- `JpaRepository` 接口继承自 `PagingAndSortingRepository` 并添加了一些JPA相关的方法，如 `flush`、`saveAndFlush`、和 `deleteInBatch` 等。

这些接口的方法足够大多数简单的CRUD操作，但如果你需要更复杂的查询，可以在你的repository接口中定义自定义方法，Spring Data JPA会根据方法名自动实现查询。例如：

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByEmail(String email);
    List<User> findByAgeGreaterThan(int age);
}
```

在上面的代码中，`findByEmail` 方法会返回所有电子邮件地址匹配指定值的用户，`findByAgeGreaterThan` 方法会返回所有年龄大于指定值的用户。

此外，你还可以使用 `@Query` 注解来定义自定义的JPQL查询，例如：

```java
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.email = ?1")
    List<User> findByEmail(String email);
}
```

在上面的代码中，`@Query` 注解告诉Spring Data JPA使用提供的JPQL查询，而不是根据方法名自动生成查询。

这些特性可以大大简化数据访问代码，让你更专注于业务逻辑。

### java对象和表的映射
在JPA中（包括其实现如Hibernate或Spring Data JPA），有许多注解可以帮助你将Java对象映射到数据库表。以下是一些最常用的注解：

1. `@Entity`: 这个注解表示该类是一个实体类，它将会映射到数据库的一个表。

2. `@Table`: 这个注解用于指定实体类映射到的表的名称。如果不使用这个注解，或者这个注解没有指定表名，那么默认的表名就是实体类的名称。

3. `@Column`: 这个注解用于指定实体类的属性如何映射到数据库表的列。如果不使用这个注解，那么默认的列名就是属性名。

4. `@Id`: 这个注解表示该属性是实体类的主键。

5. `@GeneratedValue`: 这个注解用于指定主键的生成策略，如自增、序列等。

以下是一个例子：

```java
@Entity
@Table(name = "users") // 映射到数据库的 "users" 表
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 主键生成策略为自增
    private Long id;

    @Column(name = "username", nullable = false, length = 50) // 映射到列名为 "username" 的列，该列不允许为空，长度为 50
    private String username;

    @Column(name = "email", unique = true, length = 200) // 映射到列名为 "email" 的列，该列值唯一，长度为 200
    private String email;
    
    // 其他属性...
}
```

以上代码定义了一个名为`User`的实体类，它映射到数据库的`users`表。类的`id`、`username`和`email`属性分别映射到表的对应列。使用这些注解，JPA知道如何将Java对象保存到数据库，以及如何从数据库中查询Java对象。


## JDBC
**JDBC process involves loading the driver, establishing a connection, creating and executing statements, processing results, and properly closing resources.** 

JDBC allows Java applications to interact with databases seamlessly and is widely used for database access in Java-based projects.

1. Load a Driver
2. Establish a Connection:
3. Create a Statement or PreparedStatement:
4. Execute SQL Queries:
5. Process the Results:
6. Close the Resources:


## hibernate

### caching
![[Pasted image 20230924111607.png]]
![[Pasted image 20230924111725.png]]
![[Pasted image 20230924111742.png]]



## 一对多关系
`FetchType.LAZY` 是 JPA (Java Persistence API) 中的一种设置，用于定义关联关系的加载策略。当关联关系设置为 `FetchType.LAZY` 时，它表示在访问关联属性时不会立即从数据库中加载关联的数据，而是在首次访问关联属性时才会进行加载。这有助于提高性能，因为它避免了在查询主实体时立即加载所有关联的数据。

例如，如果有一个 `User` 实体和一个 `Order` 实体，`User` 拥有多个 `Order`，可以将关联关系定义为：

```java
@Entity
public class User {
    // ...
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders;
    
    // ...
}
```

在上面的示例中，`orders` 属性被定义为 `FetchType.LAZY`，这意味着当你获取一个 `User` 实体时，它的 `orders` 属性不会立即加载，只有在你首次访问 `orders` 属性时才会从数据库中加载相关的订单数据。

你也可以使用 `FetchType.EAGER`，它表示在加载主实体时立即加载关联数据。但要小心，使用 `FetchType.EAGER` 可能导致性能问题，因为它可能会导致一次性加载大量数据。

至于是否可以只返回表的部分数据，这取决于你的查询和映射配置。通常情况下，JPA 默认情况下会根据你的实体关系加载所有相关的数据，但你可以使用投影（Projection）或查询特定的字段来选择要返回的数据。你可以使用 JPQL（Java Persistence Query Language）或 Criteria API 来编写自定义查询，以获取所需的数据。另外，Spring Data JPA 还提供了一些自定义查询方法的功能，可以帮助你轻松地筛选和选择要返回的数据。