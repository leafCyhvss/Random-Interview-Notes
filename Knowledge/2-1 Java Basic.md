## Basic

### Integer的比较
在Java中，比较两个`Integer`对象是否相等可以通过以下几种方式：

1. **使用 `equals` 方法**：
    - `Integer` 类提供了`equals`方法来比较两个`Integer`对象的值是否相等。这是比较两个`Integer`对象的推荐方式。

    ```java
    Integer x = 100;
    Integer y = 100;

    if (x.equals(y)) {
        System.out.println("Two Integers are equal.");
    } else {
        System.out.println("Two Integers are not equal.");
    }
    ```

2. **使用 `==` 操作符**：
    - 如果两个`Integer`变量指向的是自动装箱的结果，且它们的值在Java整数缓存范围内（默认是-128到127），那么`==`将会返回`true`，因为自动装箱会使用缓存的对象。
    - 对于超出缓存范围的值，使用`==`来比较两个`Integer`对象会比较它们的引用而不是值，这可能会导致意外的结果。

    ```java
    Integer a = 1000;
    Integer b = 1000;

    if (a == b) {
        System.out.println("Two Integers are equal.");
    } else {
        System.out.println("Two Integers are not equal.");
    }
    ```

    在这个例子中，即使`a`和`b`的值都是1000，使用`==`可能会返回`false`，因为它们可能是指向不同对象的引用。

3. **使用 `intValue` 方法**：
    - 如果你想比较两个`Integer`对象的原始`int`值，可以使用`intValue`方法将`Integer`对象转换为`int`类型，然后使用`==`比较它们的原始值。

    ```java
    Integer c = new Integer(100);
    Integer d = new Integer(100);

    if (c.intValue() == d.intValue()) {
        System.out.println("Two Integers are equal.");
    } else {
        System.out.println("Two Integers are not equal.");
    }
    ```

使用`equals`方法是比较`Integer`对象的值是否相等的安全方式，因为它不受对象引用的影响。使用`==`操作符时要小心，因为它可能会因为比较对象引用而产生不正确的结果。
### 八种基本数据类型：
![[Pasted image 20230706143131.png]]
### 值传递：
Java 是值传递的。复杂对象传递的是地址的值，看起来就像传递了引用
https://www.cs.virginia.edu/~jh2jf/courses/cs2110/java-pass-by-value.html
当你传递一个基本类型的变量给方法时，实际上是将该变量的值复制一份，然后将这份复制的值传递给方法。对于对象类型的变量，实际上是将该对象的引用（内存地址）复制一份，然后将这份复制的引用传递给方法

在 Python 中，所有的变量都是对象的引用，因此参数传递也是通过引用传递（pass-by-reference）。当你将一个变量作为参数传递给函数时，函数内部对于参数的修改会影响原始变量的值。需要注意的是，在 Python 中，对于不可变对象（如整数、字符串、元组等），虽然参数传递是通过引用传递，但是由于这些对象是不可变的，因此无法直接修改它们的值。在函数内部对于不可变对象的修改实际上是创建了一个新的对象，并将引用指向新的对象，而不是直接修改原始对象。

### String

#### immutable & pool
means their values cannot be changed after they are created.
why:
1. thread Safe: the value of the string will not be changed accidentally.  And its thread-safe since it can be shared among multiple threads without the risk of concurrent modification issues.
2. Better performance: immutable string supports the concept of a string pool. identical strings are stored only once and **reused** many times, conserving memory and improve performance.  (like **cached**)    **reduce cost of creations and destruction**
3. key in hashmap. Avoid collision.
   
#### builder & buffer
StringBuffer and StringBuilder are two classes in Java used for **manipulating strings**.

**StringBuffer** is **thread-safe**, meaning it can be safely accessed and modified by multiple threads simultaneously. This is achieved by adding **synchronization locks to each method.**

**StringBuilder is not thread-safe,** so attempting to access and modify a StringBuilder object concurrently from multiple threads can lead to unpredictable results. 

**Performance:**
However, in a single-threaded environment, StringBuilder typically outperforms StringBuffer.

### Final
#### Variable

`public static final String APP_NAME=“testApp”`

Purpose: define constants **cannot be changed**

#### Method

`public final int add(int a, int b){ return a + b; }

Purpose: **prevent override**

#### Class

`final class MyClass(){}``

Purpose:

1. **prevent inheritance**, like Integer, String etc;

2. Make class immutable

### Static
#### Static Variable and Static Block
**静态变量**可以在多个对象之间共享数据。所有使用该类的实例都可以访问和修改同一个静态变量的值。
only one Instance
静态变量是属于类的变量，而不是属于类的实例（对象）的变量。它们在类加载时被初始化，并且在整个程序运行期间只有一份拷贝。静态变量可以通过类名直接访问，不需要创建对象。

**静态代码块**（Static Block）： 静态代码块是在类加载时执行的一段代码块。它用关键字 `static` 声明，并且没有参数和返回值。静态代码块在类的静态变量初始化之前执行，并且只执行一次。
静态代码块的作用：
1. 初始化静态变量：静态代码块可以用来初始化静态变量，给它们赋初始值或进行一些预处理操作。
2. 执行特定操作：静态代码块可以执行一些需要在类加载时进行的特定操作，比如**读取配置文件、建立数据库连接**等。

#### Static Method
Can directly call static method using Class name

Questions:

1. Can static method access non-static variables ?
静态方法不能直接访问非静态（实例）变量，因为非静态变量是属于对象的，而静态方法是属于类的。如果静态方法需要访问非静态变量，需要通过**传递对象**或**在方法内部创建对象**来访问非静态变量。
2. Common Static Methods ?
	- Math类中的数学计算方法，如Math.abs()、Math.sqrt()等。
	- Arrays类中的数组操作方法，如Arrays.sort()、Arrays.toString()等。
	- Collections类中的集合操作方法，如Collections.sort()、Collections.binarySearch()等。
	- System类中的系统操作方法，如System.out.println()、System.currentTimeMillis()等。
1. When to use static methods ?
   当方法具有通用性，不依赖于特定对象的状态时，可以声明为静态方法。
	1. ⼯具类的⽅法⼀般都设计成static。
	2. Integer, String, Math, System etc.
5. How to call static methods?
   静态方法可以直接通过类名调用，无需创建对象。可以使用以下两种方式调用静态方法：
	- 通过类名调用：`ClassName.staticMethod(parameters)`。
	- 通过对象调用：`objectName.staticMethod(parameters)`（不推荐使用，应该直接使用类名调用）

### JVM load 顺序
静态比构造函数更早


## 静态方法汇总
在实际开发中，常见的类的静态方法和接口的静态方法有以下例子：

常见的类的静态方法：
1. `Math` 类：包含数学运算相关的静态方法，如 `Math.max()`, `Math.min()`, `Math.sqrt()` 等。
2. `Arrays` 类：包含数组操作相关的静态方法，如 `Arrays.sort()`, `Arrays.copyOf()`, `Arrays.toString()` 等。
3. `Collections` 类：包含集合操作相关的静态方法，如 `Collections.sort()`, `Collections.binarySearch()` 等。
4. `File` 类：包含文件操作相关的静态方法，如 `File.exists()`, `File.createTempFile()` 等。
5. `System` 类：包含与系统相关的静态方法，如 `System.currentTimeMillis()`, `System.getenv()` 等。

常见的接口的静态方法（Java 8及以后版本）：
1. `List` 接口：包含与列表操作相关的静态方法，如 `List.of()`, `List.copyOf()` 等。
2. `Map` 接口：包含与映射操作相关的静态方法，如 `Map.of()`, `Map.copyOf()` 等。
3. `Optional` 接口：包含与可选值操作相关的静态方法，如 `Optional.of()`, `Optional.empty()` 等。
4. `Comparator` 接口：包含与对象比较相关的静态方法，如 `Comparator.comparing()`, `Comparator.naturalOrder()` 等。

这些是一些常见的类和接口的静态方法示例，它们提供了便捷的功能和操作，可以直接通过类名或接口名调用，而不需要创建对象或实现类的实例。静态方法在实际开发中常用于提供工具方法、实用函数和辅助函数等。


## 数据结构

### treeset vs hashset
`TreeSet` 和 `HashSet` 是 Java 中两种不同的集合实现，它们有以下区别：

1. **底层数据结构：**
   - `HashSet` 使用哈希表（`HashMap`）作为其底层数据结构来存储元素，这使得它具有 **O(1) 的平均时间复杂度用于添加、删除和查找元素**。但是，元素在 `HashSet` 中的存储顺序是不确定的。
   - `TreeSet` 使用红黑树**Red-Black Tree** 或者**Balanced Binary Search Tree**作为其底层数据结构来存储元素。由于树的平衡性质，`TreeSet` 中的元素总是按照自然顺序或指定的比较器顺序进行排序，这使得它具有 **O(log n) 的时间复杂度用于添加、删除和查找元素。**

2. **元素的顺序：**
   - `HashSet` 不保证元素的存储顺序，因此元素在集合中的顺序可能是随机的。
   - `TreeSet` 会根据元素的自然排序或指定的比较器对元素进行排序，因此元素在集合中以有序的方式存储。

3. **性能：**
   - `HashSet` 通常比 `TreeSet` 具有更好的性能，特别是在插入和查找操作方面。如果您只关心快速的插入、删除和查找，`HashSet` 可能是更好的选择。
   - `TreeSet` 在需要元素有序存储或基于自定义排序逻辑时非常有用，但由于它的排序开销，它可能在某些操作上比 `HashSet` 慢一些。

4. **自定义排序：**
   - 您可以在 `TreeSet` 上使用自定义比较器来定义元素的排序方式，这使得您可以根据自己的需求对元素进行排序。
   - `HashSet` 不提供自定义排序选项，它总是按照哈希码来管理元素。

5. **唯一性：**
   - `HashSet` 用于存储唯一元素，不允许重复。如果尝试添加重复元素，它会被忽略。
   - `TreeSet` 同样用于存储唯一元素，不允许重复。

根据您的需求，您可以选择使用 `HashSet` 或 `TreeSet`。如果您需要快速的插入、删除和查找，并且不关心元素的顺序，`HashSet` 可能更适合。如果您需要元素有序存储或具有自定义排序需求，`TreeSet` 是一个更好的选择。

### Treeset 不是基于treemap
`TreeSet` 和 `TreeMap` 的底层数据结构都是红黑树，但它们在如何使用这个数据结构上有所不同。

`TreeSet` 是一个有序集合，只存储元素（键），而 `TreeMap` 是一个有序映射，存储键值对。虽然它们的底层原理相似，但它们的用途和行为略有不同。

## ArrayList VS linkedlist

<!DOCTYPE html>
<html>
<head>
    <style>
        table {
            border-collapse: collapse;
            width: 50%;
            margin: 20px auto;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>

<h2>ArrayList vs. LinkedList</h2>

<table>
    <tr>
        <th>特性</th>
        <th>ArrayList</th>
        <th>LinkedList</th>
    </tr>
    <tr>
        <td>底层数据结构</td>
        <td>an underlying dynamic array</td>
        <td>双向链表</td>
    </tr>
    <tr>
        <td>随机访问性能</td>
        <td>快（O(1)）</td>
        <td>慢（O(n)）</td>
    </tr>
    <tr>
        <td>插入和删除性能</td>
        <td>相对较低</td>
        <td>较高</td>
    </tr>
    <tr>
        <td>容量扩展</td>
        <td>容量固定，<br>if array is full  <br>resize the array to a larger size. （copy to new)</td>
        <td>without resizing since it can allocate memory for each new element individually.</td>
    </tr>
</table>

</body>
</html>


## heap VS stack ： memory

 the heap is where **objects** are stored, 较长生命周期
 
 堆内存的管理由 Java 虚拟机（JVM）负责，它会根据需要自动分配和释放对象的内存。 memory management for objects is handled **automatically by the JVM.**


The stack is a region of memory that is used for the **execution of threads.**
stores **local variables**, **method call information** (including parameters and return addresses), and other related data for each method or function call.


## heap data structure
1 - heap is Complete Binary Tree
		is fully filled except for the last level, which is filled from left to righ
2- 分成最大最小堆：max heap, the value of each **parent** node is greater than or equal to the values of its child nodes.

3-使用场景Heaps are commonly used in priority queues and sorting algorithms. Due to their properties, heaps efficiently allow finding the maximum or minimum element in a dataset.


## comparable comparator
1. comparable规定的是类内部的排序方法。一个类需要实现comparable接口，重写compateTo方法来自定义这个类的排序方式。（比如调用collections.sort时自动用到）
2. comparator是在类外的排序方式。需要创建一个独立的`Comparator`接口的实例。然后在排序时指定该比较器。


## Java内存区域
我明白你的疑惑。在 Java 中，内存分为不同的区域，每个区域用于存储不同类型的数据，并且它们有不同的生命周期。下面是 Java 虚拟机（JVM）内存的主要区域以及它们的作用：

1. **程序计数器（Program Counter Register）**：程序计数器是一个较小的内存区域，它保存了当前线程执行的字节码指令地址。在多线程环境下，每个线程都有一个独立的程序计数器。

2. **Java 虚拟机栈（JVM Stack）**：每个线程都有自己的栈，用于存储方法调用的局部变量、操作数栈、方法返回地址以及异常处理信息。栈帧在方法调用和返回时被入栈和出栈。

3. **本地方法栈（Native Method Stack）**：类似于 Java 虚拟机栈，但用于执行本地（Native）方法。

4. **Java 堆内存（Java Heap）**：Java 堆是用于存储对象实例的内存区域。这是 Java 程序中最大的内存区域，包括新生代和老年代等部分。其中的对象实例由垃圾回收器管理。

5. **方法区（Method Area）**：方法区用于存储类的元信息、静态变量、常量池、类的字节码等数据。字符串常量池也位于方法区内。注意，方法区在 Java 8 及之前的版本中是独立的，但在 Java 8 后，它的部分功能被移到了元空间（Metaspace）。

6. **元空间（Metaspace）**：元空间是 Java 8 后引入的替代方法区的概念，它用于存储类的元信息、静态变量等。与方法区不同，元空间不具有固定的最大容量，而是根据需要进行扩展。元空间的内存通常由操作系统管理。

7. **运行时常量池（Runtime Constant Pool）**：运行时常量池是方法区的一部分，用于存储编译期生成的字面常量和符号引用。这些常量包括字符串常量、类和接口的全限定名、字段和方法的名称和描述符等。

8. **直接内存（Direct Memory）**：直接内存不是 JVM 内存区域的一部分，但它通常由 JVM 管理。它用于执行 I/O 操作，通常通过 Java NIO 进行使用。

需要注意的是，每个内存区域都有不同的作用，生命周期，以及在运行时的管理方式。了解这些内存区域有助于更好地理解 Java 程序的内存管理和性能调优。



从 Java 8 开始，字符串常量池（String Pool）的存储位置从方法区（Method Area）迁移到了元空间（Metaspace）。元空间（Metaspace）的主要优势是它不受固定容量的限制，可以根据需要动态扩展，而且不容易导致 OutOfMemoryError 错误，因为它的内存通常由操作系统管理，不再受限于 Java 堆的大小。



## minheap 排序 和 sort的优缺点

优先队列（通常实现为最小堆）和`Collections.sort`（通常实现为归并排序或快速排序的变体）确实都有 `O(n log n)` 的时间复杂度，但它们在使用场景和性能上有一些关键区别。

### 使用最小堆排序

1. **时间复杂度**：构建堆的时间复杂度为 `O(n)`，每次删除最小元素的操作是 `O(log n)`。因此，如果对所有元素进行排序，则总体时间复杂度为 `O(n log n)`。

2. **空间复杂度**：最小堆通常可以原地在数组上进行，所以空间复杂度为 `O(1)`（不包括输入数据的存储空间）。

3. **适用场景**：最小堆在需要频繁地插入和删除元素的场景下特别有用，例如实时数据处理、Dijkstra算法等。

4. **优点**：
   - 可以在任何时刻获取到目前为止的最小（或最大）元素。
   - 对于部分排序（如找出最小的k个元素）非常有效率。

5. **缺点**：
   - 不能直接访问非堆顶元素。
   - 堆排序通常比快速排序、归并排序要慢。

### 使用 Collections.sort

1. **时间复杂度**：`Collections.sort` 通常是 `O(n log n)`，这是快速排序或归并排序的时间复杂度。

2. **空间复杂度**：对于归并排序，空间复杂度通常是 `O(n)`，因为需要额外空间来合并子序列。

3. **适用场景**：当需要对整个集合进行完全排序时，尤其是在不需要频繁插入和删除操作的场景下。

4. **优点**：
   - 非常适合完全排序，特别是对大型数据集。
   - 对于随机访问数据结构（如数组、ArrayList）效率非常高。
   - 实现通常针对实际性能进行了优化。

5. **缺点**：
   - 对于小型数据集或只需要部分排序的情况，可能不如其他方法（如堆排序或插入排序）高效。

### 总结

虽然两者在最坏情况下的时间复杂度都是 `O(n log n)`，但实际上，它们适用于不同的场景。堆排序（最小堆）非常适合于动态数据集或只需要部分排序的场景，而 `Collections.sort` 更适用于一次性的完全排序。在实际应用中，选择哪种方法取决于具体的问题场景和数据特性。