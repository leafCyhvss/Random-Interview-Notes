## OOP
### Class and Objects

Question: what is class ? what is object ?

An object is an instance of a module, and a class is its definition. (e.g. "Human" class --> "Bob" object)

docker image and docker container

### Class Components
Fields
Methods
Constructor:
- 如果我们没有写，JVM会⾃动帮我们提供该无参constructor
-  如果我们**写了有参constructor, JVM 就不会帮我们提供default constructor**, 如果我们需要，则需要⾃⼰写。
### Encapsulation(封装)
**keyword: access modifier, hide and provide interface

Encapsulation hide details of a class/function like the fields of the class and the implementation of the method and provide interface to access.

#### Access Modifiers

**Default** declarations are visible only **within the package** (package private)

**Private** declarations are visible **within the class** only

**Protected** declarations are visible **within the package or all subclasses**

**Public** declarations are visible **everywhere**

### Inheritance
**keywords:   allow sub class to extend super class.**
sub class can have same  properties  and similar behaviors that defined in super class 
but subclasses can have their own implement of the inherited behaviors

**enable code reuse and provide structure and orgnazition of the classes.**

In Java, Multiple and Hybrid inheritance are applicable using interface only.

#### inherit ways
Single Inheritance
Multi-level Inheritance （1生1生1）
Hierarchical Inheritance （1生2）
Multiple Inheritance （2生1）
Hybrid Inheritance （混合以上类型的继承）

Advantages of Inheritance
- Re-usability/Avoiding Duplication of Code
- Extensibility
  add a new method to super class, then all sub-classes have this new method
  
### Polymorphism(多态)
**allows one interface to be implemented by multiple data types or a method to have multiple implementations.**

methods with same name have more than one underlying types 
 
Static Polymorphism - Overload (same class) - compile time
Dynamic Polymorphism - Override (child class) - run time

![[Pasted image 20230706150622.png]]
Overloading: diff num of arguments, diff type arguments, same method name

Question: Same arguments, same method name, diff return type. is it overloading? is it allowed in java? **不可以**
Question: what is static polymorphism? **overload** 
				what is dynamic polymorphism? **override**

### Abstraction
Abstraction allows developers to **create simplified representations** of real-world objects or concepts and it **hides complexity, presenting only the essential features** of an object.
### Abstract Classes and Methods

#### 抽象方法
Rule to be followed:
- No Method body {...}
- can declared inside an abstract class and interface
	- non-abstract classes cannot have abstract methods
- cannot be declared private
	- it has to be implemented in some other class
- 继承时候会被强制写Override该⽅法
#### 抽象类
An abstract class can **have everything else as same as a normal Java class** has i.e. constructor, static variables and methods.
- Child classes must implement all the abstract methods declared in the parent abstract class.
- You can NOT do Person person = new Person()
- Non-abstract/normal methods can be implemented in an abstract class

### 接口
[[20-Java&OOP.pdf]] 第19页
java9开始接口中才可以有private 变量和方法
![[Pasted image 20230706151329.png]]

##### Important Questions

Question: Why Java add default method in Java 8 ? 
在Java 8中引入默认方法的目的是为了在接口中提供方法的默认实现，以便在向已有接口添加新方法时，可以保持向后兼容性。
默认方法允许在接口中定义方法的实现，而不需要实现类立即实现该方法。
这使得在现有的接口中添加新方法成为可能，而不会破坏实现该接口的现有类。
默认方法还为接口提供了一种途径来实现多重继承的特性，使接口可以具有默认行为。
它还使得在函数式接口中添加新方法成为可能，从而支持函数式编程的特性

This feature enables the evolution of interfaces by adding new methods without affecting the existing implementations

Question: Interface 可以多继承，那么两个interface的同样的method的情况下该怎么办
```java
class MyClass implements InterfaceA, InterfaceB {
	@Override
    public void doSomething() {
    InterfaceA.super.doSomething(); // 调用 InterfaceA 的默认方法实现
    InterfaceB.super.doSomething(); // 调用 InterfaceB 的默认方法实现
    System.out.println("Custom implementation"); // 提供自定义实现
    }
}
/*
类需要提供自己的实现：类必须提供自己的方法实现，覆盖接口中的方法，并指定具体的逻辑。

明确指定接口方法：在类中实现时，需要明确指定是要实现哪个接口的方法。可以使用"接口名.super"的语法来调用指定接口的默认方法。

解决命名冲突：如果两个接口中定义的方法有相同的名称但不同的默认实现，类必须提供自己的实现来解决冲突。这可以通过在类中重写方法并提供自定义实现来实现。
*/
```

Question: Static Methods in interfaces ? code
提供公共工具方法：接口的静态方法可以提供一些公共的工具方法，供其他类直接调用，无需创建实例。

提供辅助方法：接口的静态方法可以提供一些辅助方法，用于在接口内部实现其他方法的逻辑。

模块化组织：静态方法可以帮助在接口内部组织和封装逻辑，避免将方法散落在接口的各个实现类中。
### Abstract class vs. Interface
回答要点5大点：
1. 多继承问题
2. 只有public和都可以有
3. 变量只能有static 和 final， 抽象类无限制
4. 没有构造函数， 抽象类可以有
5. 在 Java 8 之后，接口可以包含默认方法和静态方法。在 Java 9 之后，接口还可以包含私有方法。

![[Pasted image 20230721130952.png]]

[[20-Java&OOP.pdf]] 22页


## Design pattern

### singleton 代码实现
Singleton模式的目标是确保一个类在应用程序中仅有一个实例，并提供一个全局访问点。

**Eager loading** 是指在程序启动时就创建唯一的单例实例，无论是否真正使用到这个单例。这种方式优点是简单，缺点是如果这个单例很少使用或者创建实例的开销很大，那么可能会造成资源的浪费。

Java代码示例（饿汉式）：
```java
public class Singleton {
    // 在类加载时就完成了初始化，静态属性只会在第一次加载类的时候初始化
    private static Singleton instance = new Singleton();

    // 私有构造函数
    private Singleton() {}

    // 提供全局访问点
    public static Singleton getInstance() {
        return instance;
    }
}
```

**Lazy loading** 是指在第一次需要单例的时候才创建它。这种方式优点是如果这个单例很少使用，可以节省资源，但缺点是需要处理并发问题，确保在多线程环境下仍然只有一个实例。

**错误**Java代码示例（懒汉式，线程安全）：
```java
public class Singleton {
    private static Singleton instance;//错误1：没写volatile

    private Singleton() {}
		//错误2：同步浪费性能
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
以上代码在`getInstance()`方法上添加了`synchronized`关键字，保证了在多线程环境下，只会创建一个实例。**但缺点是每次访问都需要同步，会影响性能。**


**正确**：使用双重检查锁定（Double-Checked Locking）模式来实现线程安全的懒汉式单例：

**首先第一次检查是在同步块外部，如果实例为空，则进入同步块；如果实例已经被初始化，那么就直接返回实例，这样就避免了每次访问时都需要获取锁，提高了效率**。
**然后在同步块内部，再次检查实例是否为空。这是因为可能会有多个线程一起进入同步块等待锁，假设线程A获取到了锁并初始化了实例，然后`线程B也进入了同步块，如果没有这一次检查，线程B也会创建一个新的实例，那么就不再是单例模式了。**

```java
public class Singleton {
    private volatile static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
这种方式只在第一次初始化时进行同步，避免了每次访问时的同步开销，同时保证了线程安全。

### singleton 实际开发例子
