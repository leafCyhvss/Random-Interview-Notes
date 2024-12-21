## 底层原理
Short answer(java 8): 
	Java HashMap internally uses **an array of buckets and each bucket stores linked list to store the kv pairs**. It calculates the hash code of the key to **determine the index in the array, which is called a bucket**. Collisions are handled using separate chaining, where entries with the same index are stored as a linked list(after java8) or a balanced tree. The get() and put() operations locate the bucket using the key's hash code and retrieve or store the key-value pair accordingly. Resizing is performed when the load factor is exceeded.
	**如果链表长度超过8会转成红黑树，减少到6时又转回链表**
Detailed:
	**Hashing:** When a key-value pair is inserted into the HashMap, the hash code of the key is computed using the key's **hashCode()** method. This hash code is then used to calculate the index of the bucket in the internal array.
	**Bucket Array**: The HashMap maintains an **internal array of buckets**, where each bucket can hold multiple key-value pairs. The size of the array is determined by the initial capacity specified during HashMap creation.
	**Index Calculation**: The calculated hash code is transformed using a hash function to ensure a more uniform distribution of keys across the buckets. This transformed hash code is used as the index to store the key-value pair in the appropriate bucket.
	**Collision Handling**: Since multiple keys can hash to the same index (collision), **each bucket** in the HashMap can contain **a linked list of entries**. If there is a collision, the new key-value pair is added to the linked list in the corresponding bucket. This allows multiple entries with different keys but the same index to coexist.
	**Get and Put Operations**: To retrieve a value associated with a given key, the HashMap calculates the index using the key's hash code and then searches the linked list in the corresponding bucket to find the matching key-value pair. For insertion (put) operations, the process is similar but involves either updating the value if the key already exists or adding a new key-value pair if it doesn't.
	**Resize**: As the number of entries in the HashMap increases, it periodically needs to resize its internal array to maintain **a proper load factor.** During resizing, a new larger array is created, and all the key-value pairs are rehashed and redistributed into the new array. This process helps to minimize collisions and ensure a balanced distribution of entries.

## 写自定义类作为key时
为了确保 `HashMap` 正确地工作，这个类需要满足以下条件：

1. **正确重写 `equals()` 方法**：
   当判断两个键是否相等时，`HashMap` 会调用 `equals()` 方法。如果你的自定义类没有正确重写 `equals()` 方法，那么即使两个实例在逻辑上相等，`HashMap` 也可能视其为不同的键。

2. **正确重写 `hashCode()` 方法**：
   `HashMap` 使用哈希码来决定键在内部数组中的存储位置。如果两个键相等（即 `equals()` 方法返回 `true`），它们必须具有相同的哈希码。否则，`HashMap` 可能无法找到键对应的值。


### 原则

- 重写 `hashCode()` 时，必须确保如果两个对象相等，那么它们的哈希码也必须相等。
- `equals()` 方法必须满足自反性、对称性、传递性和一致性。
- 为了保持 `hashCode()` 的一致性，一旦一个对象被用作 `HashMap` 的键，就不应该修改它的任何字段，因为这些字段可能影响到 `hashCode()` 计算。

### 为什么重写`equals`就要重写`hashcode`

如果两个对象相等（即 equals() 方法返回 true），那么它们的 hashCode() 方法必须返回相同的整数结果。
如果 equals() 表明两个对象相等，但它们的 hashCode() 方法返回不同的结果，那么在使用哈希集合时就会产生问题。

默认情况下object类的hashcode()方法返回的是对象的地址。

在理论情况下，如果 `x.equals(y)==true`，如果没有重写 `equals` 方法，那么这两个对
象的内存地址是同一个，意味着 `hashCode` 必然相等。
但是如果我们只重写了 equals 方法，就有可能导致两个对象，地址不同但是内容相同时，视为相同，但是 hashCode 不相同。一旦出现这种情况，就导致这个类无法和所有集合类一起工作。


### 怎么重写`equals`
在Java中重写`equals`方法时，通常需要遵循以下步骤来判断两个对象是否相等：

1. **检查是否为同一个对象的引用**：
   使用`==`运算符检查是否是同一个对象的引用。如果是，那么它们肯定相等。
   如果不是，还需要进一步检查。
   两个对象内存地址不同时，可能内容是相同的。

2. **检查是否是正确的类型**：
   使用`instanceof`运算符或`getClass()`方法检查传入的对象是否是正确的类型。如果不是，则直接返回`false`。

3. **类型转换**：
   将传入的对象转换（强制类型转换）到当前类的类型。

4. **比较关键字段**：
   对于对象中所有的关键字段，逐一进行比较。对于基本数据类型，使用`==`比较；对于对象类型，使用`equals`比较。在比较浮点类型的字段时，由于精度问题，通常不使用`==`运算符，而应该使用`Double.compare(double, double)`或`Float.compare(float, float)`方法，或者使用一个允许的误差范围来检查它们是否相等。

5. **返回结果**：
   如果所有的关键字段都相等，那么返回`true`；否则返回`false`。

下面是一个实现`equals`方法的示例：

```java
public class Person {
    private String name;
    private int age;

    // constructor, getters, and setters are not shown

    @Override
    public boolean equals(Object obj) {
        // Step 1: Check if it's the same reference
        if (this == obj) {
            return true;
        }
        // Step 2: Check if obj is an instance of Person
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        // Step 3: Cast obj to Person
        Person person = (Person) obj;
        // Step 4: Compare key fields
        return age == person.age && (name != null ? name.equals(person.name) : person.name == null);
        // Step 5: Return the result is implicit here
    }
}
```

在上面的代码中，`name`和`age`是用来确定`Person`对象是否相等的关键字段。对于`name`（一个`String`对象），我们使用`equals`方法比较。对于`age`（一个基本数据类型`int`），我们使用`==`比较。

### 怎么重写`hashcode()`

使用一个固定的方法，该方法在计算每一个对象的hash value时，用到相同的类的fields来计算哈希码。

```java
public class Person {
    private String name;
    private int id;

    @Override
    public boolean equals(Object o) {
        // Implementation of equals...
    }

    @Override
    public int hashCode() {
        // Good practice: compute a hash code that corresponds to the fields used in equals.
        int result = id;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        return result;
    }
}
```


在这个示例中，如果两个 Person 对象有相同的 id 和 name，那么它们被视为相等。因此，它们的哈希码也应该相同。这正是 hashCode() 方法在这里所做的——它使用相同的字段（id 和 name）来计算哈希码。
如果字段是一个数组，则需要为每个元素计算哈希码，如同单独的字段一样处理。如果数组的元素可能为null，则需要检查null。可以使用Arrays.hashCode()来帮助计算数组字段的哈希码。



## `==` 和 `equals`区别


### == 运算符
- **用于基本数据类型**：比较两个变量的值是否相等。例如，当它们是基本数据类型（如 `int`, `char`, `double` 等）时，`==` 比较的是它们的数值。
- **用于对象引用**：比较两个对象引用是否指向内存中的同一个位置。换句话说，它检查两个引用是否是同一个对象的引用。

### equals() 方法
**用于对象比较**：在 `Object` 类中定义，用于比较两个对象是否在逻辑上等价。默认情况下，`Object` 类的 `equals()` 方法和 `==` 运算符的行为相同，即**比较对象的引用**， 比较两个对象引用是否指向内存中的同一个位置。

但通常我们会在自定义类中重写 `equals()` 方法，以提供基于对象的属性比较逻辑，从而确定两个对象是否“相等”。

例如，在 `String` 类中，`equals()` 方法会比较两个字符串的内容是否相同。
