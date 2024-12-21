BufferedReader + FileReader
```java
try (BufferedReader br = new BufferedReader(new FileReader("path/to/your/file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

```



### try可以没有括号：
您提到的 `try()` 结构是 Java 7 引入的 `try-with-resources` 语句，它的设计目的是为了自动管理资源，比如文件、网络连接或数据库连接等。在 `try-with-resources` 语句中，你可以声明一个或多个资源，在代码块执行完毕时，这些资源将自动关闭。

**何时在 `try()` 中的 `()` 里写**:
1. 当你创建一个对象，**该对象实现了 AutoCloseable 或 Closeable 接口时**，你应该在括号内声明它，这样，**无论代码块如何结束（正常结束或因为异常），该资源都会被自动关闭**。
2. 当你需要确保资源在使用完毕后被正确关闭，且不需要在后续代码中访问这些资源时。

**示例**:
```java
try (BufferedReader br = new BufferedReader(new FileReader("path/to/your/file.txt"))) {
    // Use the resource
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
// br is out of scope here and has been closed
```

**何时在 `try` 的 `{}` 代码块里写**:
1. 当你需要在 `try` 代码块之外访问一个资源时。
2. 当你正在使用不实现 `AutoCloseable` 或 `Closeable` 接口的对象时。

**示例**:
```java
BufferedReader br = null;
try {
    br = new BufferedReader(new FileReader("path/to/your/file.txt"));
    // Use the resource
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (br != null) {
        try {
            br.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

使用 `try-with-resources` 可以简化代码并减少资源泄漏的可能性。当可能时，建议优先使用 `try-with-resources`。



## 遍历map
```java
import java.util.*;
class HelloWorld {
    public static void main(String []args) {
        System.out.println("Hello, World!");
        
        int[] arr = new int[]{1,2,3,4,5,1,3,4,6};
        Map<Integer, Integer> map = new HashMap<>();
        for(int num:arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        for(Map.Entry entry: map.entrySet()) {
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
        for(Map.Entry<Integer, Integer> entry: map.entrySet()) {
            System.out.print(entry.getKey() + ":" + entry.getValue() + " ");
        }
        System.out.println(map.toString());
    }
}
```