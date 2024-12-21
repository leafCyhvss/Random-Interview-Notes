## 杂乱
Int的i要大写
区分‘和“
var变量， val常量

会自动推导变量类型
## 数据结构

### 类型转换：
数字不能加string：
`b + "3"`  因为int的plus方法没有string类型参数

但是`"3"+b`可以，因为String的plus方法参数是Any

Any相当于java中的object，所有类的父类
Any有 hashcode tostring equals
### 基本数据类型
都是对象
小数默认double
1.0f 1.0F 1L


### 复杂数据类型

#### String
str[0]是可以的
比较字符串可以直接str.euqals或者运算符重载==


字符串拼接：
`println("Str${b}ing")`
```kotlin
val b:String= """
	asd
	asd
	asd
""".trimIndent()

// 多行字符串的编写。自动删除前面的缩进（2格）
```


### Collection
所有的get方法都重载成中括号
### Array
```kotlin
val array = arrayof<Int>(1,2,3)
val arr = arrayOf<Int>(1)
val arr3:Array<Int> = arrayOf(2)
val arr2:IntArray = IntArray(2) 
arr[0] = 1
```

### List
list不可变 没有add remove方法
mutableList可以变

### set
也有mutableSet
mutableSetOf

### map
Pair(A, B)就是kv
mutableMapOf("1" to 3)
to 就是调用pair

也可以通过[]访问元素，[]中是key

map.keys.forEach{}

### pair&triple
pair()
pair<>()
first second

triple()三元组

### Any & nothing