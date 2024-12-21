
## Stream
### Create stream object
1. 从集合（Collection）创建Stream
⼏乎所有的集合类（如List和Set）都可以通过调⽤ stream() ⽅法创建⼀
个Stream对象。这是创建Stream的最常见⽅式。
```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Stream<String> stream = list.stream();
```

2. 使⽤ Stream.of() ⽅法
使⽤ Stream.of() ⽅法可以直接从多个元素创建⼀个Stream对象。
```java
Stream<String> stream = Stream.of("apple", "banana", "cherry");
String[] array = {"apple", "banana", "cherry"};
```
3. 从数组创建Stream
通过使⽤ Arrays.stream() ⽅法，可以从数组创建⼀个Stream对象。`
```java
Stream<String> stream = Arrays.stream(array);
```
1. 使⽤ Stream.iterate() ⽅法
a. Stream.iterate() ⽅法允许创建⼀个⽆限的、有规律的序列。通常与 limit() ⽅法⼀起使⽤，以限制⽣成的元素数量。
```java
Stream<Integer> stream = Stream.iterate(0, n -> n + 2).limit(5); // Generates 0, 2, 4, 6, 8
```
5. 使⽤ Stream.generate() ⽅法
a. Stream.generate() ⽅法允许创建⼀个⽆限的、⽆规律的序列。通常与 limit() ⽅法⼀起使⽤，以限制⽣成的元素数量。
```java
Stream<Double> stream = Stream.generate(Math::random).limit(5);
```



## Optional
java 8 17页
#### optional 静态方法（创建optional）：
Optional.empty()
Optional.of(T value) **如过传进来是null会报npe**
Optional.ofNullable(T value)
```java
String name = "Alice";
Optional<String> nameOptional = Optional.of(name);
String name2 = null;
Optional<String> nameOptional = Optional.of(name2);
// 会抛出NullPointerException
```
#### optional object的方法：
get()
orElse(T other)
orElseGet(`Supplier<? extends T> other)`
orElseThrow()
```java
Optional<String> optionalName = Optional.of("Alice");
String name = optionalName.get();

Optional<User> optionalUser = Optional.ofNullable(userMap.get(2));
// Using isPresent() to check if the Optional contains a value
if (optionalUser.isPresent()) {
	User user = optionalUser.get();
	// Perform some business logic with the user object
	System.out.println("Performing business logic with user: " + user.getName());
 }
```
isPresent()
a. 该⽅法⽤于检查Optional对象是否包含值。如果包含值，返回true；如 果Optional对象为空（不包含值），返回false。
2. ifPresent(Consumer`<? super T> action)`
a. 该⽅法⽤于在Optional对象包含值时执⾏指定的操作。如果Optional对象
包含值，将该值传递给作为参数的Consumer函数；如果Optional对象为
空（不包含值），不执⾏任何操作。

## Lambda & functional interface
### Functional Interface

Functional Interface（函数式接口）是指只**包含一个抽象方法的接口**。

由于java8的新特性，接口可以添加默认方法和静态方法，从而使得接口可以增加新的方法，而不破坏现有的代码。但是FC只能有一个抽象方法
不强制要使用 @FunctionalInterface 注解

### Lambda 表达式

Lambda表达式是**anonymous function**，可以用于创建函数式接口的实例。Lambda表达式提供了一种更简洁、更灵活的方式来**implement**函数式接口中的**抽象方法**。它可以将一个函数作为参数传递，或者将一个函数作为返回值返回。

Lambda表达式与函数式接口之间的关系：
- Lambda表达式可以用于实现函数式接口中的抽象方法，省去了创建匿名类的繁琐步骤。
- Lambda表达式通过推断上下文来确定函数式接口的类型，无需显式地指定接口类型。

```java
@FunctionalInterface
interface MyFunction {
    void doSomething();
}

public class Main {
    public static void main(String[] args) {
        // 使用Lambda表达式创建函数式接口的实例
        MyFunction func = () -> {
            System.out.println("Doing something");
        };

        // 调用函数式接口的抽象方法
        func.doSomething(); // 输出：Doing something
    }
}
```


## Method Reference
一种**简洁的方法来引用一个已经存在的方法**。方法引用可以被看作是一个Lambda表达式的简写形式，它用来直接调用目标方法.

是的，Lambda 表达式可以访问它外部的变量，这些变量被称为Lambda 表达式的"捕获变量"或"外部变量"。Lambda 表达式可以访问以下类型的外部变量：

1. **final 变量或 effectively final 变量：** 如果在 Lambda 表达式内部引用一个外部变量，那么这个变量必须是 final 或 effectively final。`final` 变量是显式声明为 `final` 的，而 `effectively final` 变量是指在 Lambda 表达式内部没有修改的变量。Lambda 表达式可以读取这些变量的值，但不能修改它们。

```java
final int x = 10;
Consumer<Integer> lambda = (y) -> System.out.println(x + y);
```

2. **实例变量和静态变量：** Lambda 表达式可以访问外部类的实例变量和静态变量。它们不需要被标记为 `final` 或 `effectively final`。

```java
public class Example {
    private int x = 10;
    private static int y = 20;

    public void lambdaExample() {
        Consumer<Integer> lambda1 = (z) -> System.out.println(x + z);
        Consumer<Integer> lambda2 = (z) -> System.out.println(y + z);
    }
}
```

Lambda 表达式捕获外部变量的值，因此在 Lambda 内部使用这些变量时，它们的值是不可变的。这有助于确保线程安全性和一致性。Lambda 表达式允许在匿名函数中访问外部作用域的数据，使得代码更加简洁和灵活。