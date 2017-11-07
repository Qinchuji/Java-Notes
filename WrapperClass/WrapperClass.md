# 包装类(WrapperClass)
Java当中有8种原生数据类型并不是对象，所以当用户想要进行值的传递时就会有限制，为了能够更加符合面向对象的特点，Java为我们提供了8种原生数据类型的包装类，所谓的包装就是将一个原生数据类型的值包装进一个对象中，那么对象中就承载了这个具体的值，而用户就可以使用对象的操作方式操作这个原生数据的包装类。

## Integer源代码注释
>java.lang

>public final class Integer

>extends Number

>implements Comparable<Integer>

The Integer class wraps a value of the primitive type int in an object. An object of type Integer contains a single field whose type is int.
In addition, this class provides several methods for converting an int to a String and a String to an int, as well as other constants and methods useful when dealing with an int.


这个整型类在一个对象中包装了一个int类型的原生数据，Integer对象包含了一个单独的整型值。
附加：这个类为例如将整型转换为字符串以及将字符串转换为整型提供了多种方法，也适用于当常量和方法在对待整型的时候。
#### 说明
针对原生数据类型的包装，所有的包装类(8个)都位于Java.lang包下，Java中的8个包装类分别是：Byte,Short,Integer,Long,Float,Double,Character,Boolean它们的使用方式是一样的可以实现原生数据类型与包装类型的双向转换(原生数据类型变成对象)例如：Integer可以将一个原生数据的整数类型值包装到一个对象中，这个类别为Integer的对象包含了一个整型成员变量。


### public Integer(int value)
```
public static final Class<Integer>  TYPE = (Class<Integer>) Class.getPrimitiveClass("int");
```
>参数：int value:Integer对象所代表的值。

>The {@code Class} instance representing the primitive type

构造一个代表了指定的整数类型数据的对象。

```java
Integer a = new Integer(1);
System.out.println(a instanceof Object);
```
>输出结果为：true

说明此时a是一个对象，而同样使用int a = 0替换结果无法编译。
