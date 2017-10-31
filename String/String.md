
# String类
>public final class String

>java.lang.String

1. **字符串是个final类，就是说字符串是不能继承的。**
2. **在java中所有的字面值，都是String类的实例。**

## 字符串赋值与池的关系
### String Pool（字符串池）
**string维护一个字符串池的原因：**
1. 因为string是一个常量在创建完毕以后是不能改的。
2. 因为在项目中使用的很多，有必要维护一个字符串池，只要是池里面有的值就不再需要重新创建，否则字符串就会创建很多很多，因为实际开发中字符串一般会作为一个临时变量，用完过后就会丢弃，而如果维护了一个字符串池，临时变量无用了也要在字符串池栈之中创建，等到需要的时候，直接返回地址即可，提升了效率。


* **new出来的对象都是存放在堆中的，而String Pool的值都是存放在栈中的。**

### String s = “aaa”（采用字面值的方式赋值）

1. **如果采用字面值的方式赋值，首先查找String Pool栈中是否存在“aaa”这个对象，如果不存在，则在String Pool栈中创建一个“aaa”对象，然后将String Pool栈中的这个“aaa”对象的地址返回来，赋给引用变量s，这样s会指向String Pool栈中的这个“aaa”字符串对象。**

2. **如果存在，则不创建任何对象，直接将String Pool栈中的这个“aaa”对象地址返回回来。**

```java
//池中创建了对象
String str3 = "bbb";
//池中没有创建对象只是返回了地址                   
String str4 = "bbb";                   
System.out.println(str3 == str4);
```
>**输出结果为：“true”**

这说明str3与str4指向同一个对象（==判断地址）

在给str3赋值的时候，String Pool中并没有内容为“bbb”的字符串对象，所以它就在池中创建了一个“bbb”对象，并且将创建的"bbb"地址返回给了str3引用。当执行下一行代码的时候，还是要检查String Pool中有没有“bbb”对象，如果有就不再重新创建对象而是将原有的“bbb”对象的地址返回给了str4引用。所以实际上指向的是同一个对象。

* **返回的地址是栈中的地址。**

### String s = new String（“aaa”）(new一个字符串对象)

1. **如果采用new的方式创建一个字符串对象，首先也会在String Pool中查询是否存在“aaa”字符串对象，如果不存在则首先在String Pool栈中创建一个“aaa”对象，然后将堆中在创建一个“aaa”对象，然后将这个“aaa”对象的地址返回回来，赋给了s引用，使s引用指向了堆中创建的这个“aaa”字符串对象。**

2. **如果存在就不会在String Pool创建这个“aaa”对象，而是直接在堆中创建，然后将堆中的这个“aaa”对象的地址返回来，赋给了s引用，使s引用指向了堆中创建的这个“aaa”字符串对象。**

```java
 //池中创建了对象，堆中也创建了对象
String str1 = new String("aaa");
//在堆中创建新对象，返回地址           
String str2 = new String("aaa");            
System.out.println(str1 == str2);
```
>**输出结果为：“false”**

这说明str1与str2不指向同一对象。

首先执行第一行语句，在String Pool栈中创建一个“aaa”对象，然后再在堆中创建一个新对象。执行第二行代码的时候就不再String Pool栈中创建“aaa”对象了，只是将堆中再创建一个新对象。比较时str1与str2虽然字面值相同，但比较的是对象，这分别是两个不同的对象，所以值为假。

* **返回的地址是堆中的地址**

#### 所以不管String Pool里面有没有这个对象，堆中是一定会创建新的对象的并且会将此对象地址返回给引用。

## 字符串的拼接
**字符串是常量，在它们被创建之后是不能被改变的。字符串支持加法操作，这样实际上就是生成了新的对象，而不是往原有的字符串中追加内容。**

```java
String s1 = "hello";
String s2 = "world";
String s3 = "helloworld"
String s4 = s1 + s2；
System.out.println(s4);
```
>**输出结果为：“helloworld”**

* **注意：System.out.println("test"+(s3 == s4))；输出结果为“false”说明字符内容相同但并不是同一地址，造成这一结果的原因是因为(特别记住结果待更新)**

## intern拘留池
>**public String intern()**

**一个字符串池最初是空的，它是由String这个类独自去维护的，当调用intern方法的时候，如果这个池里面包含的某一个字符串与调用方法的字符串对象使equals（Object）方法返回值为真的时候，然后便返回来自字符串池中的字符串值。否则这个字符串对象，就会被添加在池里面并且返回对此字符串的引用。**

**intern方法可用于两个引用之间：只有当s.equals(t)返回值为真，s.intern() == t.intern()返回值才可为真。**

因为equals在字符串类中比较的是内容是否相同，而只要相同，s和t若在堆中则通过intern方法将在字符串池中创建新的对象并且返回地址，而当调用t.intern（）时将直接返回字符串中在生成s时的地址返回给引用，结果便为真。

```java
String s3 = "Android" + "创新实验室";
String s4 = new String("Android创新实验室").intern();
System.out.println(s3 == s4);
```
>**输出结果为：“true”**

实际上就是在池中寻找调用intern方法的字符串是否存在，若存在就返回栈中的地址，不存在就创建一个并返回。
