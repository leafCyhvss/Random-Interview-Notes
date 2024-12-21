## Exception 执行顺序

例子： 

```java
    public static void main(String[] args) {
        try {
            //String s1 = null; // 或者赋予一个具体的值
            String s1;
            System.out.println(s1.substring(2, 4));
        } catch (Exception ex) {
            System.out.println("In Exception");
        } catch (NullPointerException ne) {
            System.out.println("In NullPointerException");
        } finally {
            System.out.println("In finally");
        }
    }
```

这段代码中有两个checked exception：
1. 在Java中，局部变量会在使用之前被初始化，但是如果局部变量在 try 块内部被声明，编译器不会报错，即使没有显式初始化。编译器只会在无法确定变量是否已经初始化的情况下报告错误。
   `java: variable s1 might not have been initialized`
- [x] 你需要在声明 s1 时进行初始化，或者将其初始化放在 try 块内部，以确保它在使用之前具有一个值。
3. 在Java中使用多个catch块来捕获异常时，您必须从最特定的异常类型开始，然后才是其超类（更一般的异常类型）。由于NullPointerException是Exception的子类，因此它永远不会被执行。当异常被捕获时，它会匹配第一个合适的catch块，并执行该块中的代码。
   在您的示例中，任何异常（包括NullPointerException）都会被第一个catch块捕获，因此In Exception会被打印出来，而第二个catch块永远不会被执行。
   要解决这个问题，您应该首先捕获NullPointerException，然后再捕获Exception。
`java: exception java.lang.NullPointerException has already been caught`

## error vs exception

Java 中的异常（Exception）和错误（Error）是两种不同的异常情况，它们之间的区别在于它们的类型和产生的原因：

1. **异常（Exception）**：
   - 异常是指程序在正常运行过程中遇到的**非预期**情况。
   - 异常通常由程序员编写的代码引发，用于**处理可预见的问题**，例如除零错误、空指针异常等。
   - 异常是可以被捕获（通过 try-catch 块捕获）和处理的，程序可以继续执行。
   - 在 Java 中，异常通常继承自 `java.lang.Exception` 类。

2. **错误（Error）**：
   - 错误是指程序在运行过程中遇到的严重问题，**通常无法被程序本身处理。**
   - 错误通常由虚拟机（JVM）或底层系统引发，例如内存溢出错误（OutOfMemoryError）、栈溢出错误（StackOverflowError）等。
   - 错误是不应该被程序捕获和处理的，因为它们表示了系统级别的问题，无法恢复。程序通常会终止运行。
   - 在 Java 中，错误通常继承自 `java.lang.Error` 类。

总结起来，异常是程序可以预测和处理的非正常情况，而错误是严重的问题，通常表示系统或环境出现了不可恢复的错误。在编写 Java 代码时，通常只捕获和处理异常，而对错误则无法进行处理。因此，程序员通常不需要担心如何处理错误，而应该专注于如何处理异常以确保程序的稳定性和可维护性。


## How to handle exception

## Exception handling in spring


## Avoid NPE

1. manually check using if obj == null
2. optional class. `Optional<string> x; x.orElse("default")
3. try catch handle NPE
4. for-each loop ` for (String item:myList){}`
   When iterating over collections, prefer the enhanced for loop (for-each) as it automatically handles null values. 
   