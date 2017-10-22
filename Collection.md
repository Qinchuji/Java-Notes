# 类集框架
类集框架被设计用于适应几个目的。首先，这种框架是高性能的。对基本类集（动态数组、链接表、树和散列表）的实现是高效率的。一般很少需要人工去对这些“数据引擎”编写代码（如果有的话）。**第二点，框架必须允许不同类型的类集以相同的方式和高度互操作方式工作。**第三点，类集必须是容易扩展和/或修改的。**为了实现这一目标，类集框架被设计成包含一组标准的接口。**对这些接口，提供了几个标准的实现工具（例如LinkedList、HashSet和TreeSet），通常就是这样使用的。为了方便起见，创建用于各种特殊目的的实现工具。一部分工具可以使自己的类集实现更加容易。**最后，增加了允许将标准数组融合到类集框架中的机制。**
https://baike.baidu.com/item/%E7%B1%BB%E9%9B%86/4887328?fr=aladdin
>**集合框架中的接口**

>所谓框架就是一个类库的集合，集合框架就是一个用来表示和操作集合的统一的架构，包含了实现集合的接口与类。

>**集合(Collection)接口作为根接口，分别被Set和List所继承，而SortedSet实现了Set。映射（Map）被排序映射(SortedMap)所继承。**

## 1.Collection（集合）

#### Interface Collection
>java.util

>public interface Collection<E>

>extends Iterable

一个集合代表了一组对象,这些对象我们称之为元素。某些集合允许重复的元素而其他是不可以的，有些是排序而有些不是。JDK不提供任何一个类直接去实现Collection接口，而是提供了更加具体的子接口Set和List接口继承Collection。这两个接口最主要用于传递集合以及操纵集合。

**如何Collections接口中定义的方法：因为JDK不提供任何一个类直接去实现Collection接口而只有Set和List继承了Collection所以只能通过子接口去实现Collection的方法。**

Collections接口中定义的某些方法：
add(E e)
返回值为boolean


## 2.List（列表）
#### Interface List<E>
>java.util

>public interface List<E>

>extends Collections<E>

一个有序的集合称之为列表（或称作序列）。用户可以精确的通过下标来访问每一个插入列表中的元素，实现控制以及搜索功能。

## 2.1ArrayList（数组列表）
实现了List接口的类：ArrayList和LinkedList
#### ArrayList

>java.util

>Class ArrayList<E>

>Implemented Inerfaces : Collections , List



### 2.1.1ArrayList的使用方法：

构造方法：ArrayList（）:构造出一个的空列表

1. add(E e) 向列表中添加对象，将指定的元素追加到列表的末尾。
2. get(int index) 返回列表中特定的元素。参数为元素的索引。
3. size() 返回列表当中元素的个数。
4. clear() 将列表中所有元素都删除。
5. isEmpty() 判断列表中是否包含元素。返回值为boolean值。
6. remove（int index) 将列表中特定的元素删除。参数为元素的索引。
7. indexOf（Object object）判断特定元素在里表中的位置。
8. toArray() 以正当的顺序(从第一个元素到最后一个元素)返回一个包含所有元素的数组(返回一个Object类型的数组)。
```java
package p;
import java.util.ArrayList;
public Class ArrayListTest1
{
  public static void main(String[] args)
  {
    ArrayList arrayList = new ArrayList();
    arrayList.add("hello");
    arrayList.add("world");
    //arrayList.add("world");                  是否允许重复元素  
    String s1 = （String）arrayList.get(0);      //1、
    String s2 = （String）arrayList.get(1);
    //String s3 = （String）arrayList.get(2);
    System.out.println(s1)；    
    System.out.println(s2)；
    //System.out.println(s4)；                 索引越界异常，编译失败
    //System.out.println(s3)；                 答案是肯定的
    System.out.println("-------------")；

    for(int i = 0;i < arrayList.size();i++)     //2、
    {
      System.out.println(arrayList.get(i));
    }
    arrayList.clear()                           //3、
    {
      System.out.println(arrayList.size());
      System.out.println(arrayList.isEmpty());  //4、
    }
  }
}
```
>输出结果为：

```java
hello
world
-------------
hello
world
true
```

1. add方法添加的是字符串对象而，get方法返回的值是Object类对象,而实际上返回的本来应该就是字符串类型，所以要进行向下类型转换使返回的值与添加的值类型一致。
2. 类似于length区别是size是个方法，但可以与length使用思路相同。
3. 调用方法清除列表。
4. 也可以使用这个方法进行判断是否为空

```java
arrayList.remove(0);
arrayList.remove(“hello”);
System.out.println"---------"
for(int i = 0;i < arrayList.size();i++)     
{
  System.out.println(arrayList.get(i));
}
```
>输出结果为：world

remove（）方法另有一个重载方法，参数是传递内容。
清除的机制是将特定的元素清除后将后面索引的元素向前移动。例如：清除了s0，s0 = s1,s1 = s2....所以此时的world变成了第一个元素。

```java
arrayList.add("aaa");
arrayList.add("bbb");
System.out.println(arrayList.indexOf("aaa"));
```
>输出结果为:"0"

"aaa"在ArrayList集合里是第一个位置，索引为0.

##### toArray()方法
* 以正当的顺序(从第一个元素到最后一个元素)返回一个包含所有元素的数组(返回一个Object类型的数组)。

```java
package p;
import java.util.ArrayList;
public Class ArrayListTest4
{
  public static void main(String[] args)
  {
    ArrayList arrayList = new ArrayList();
    list.add(new Integer(1));
    list.add(new Integer(2));
    list.add(new Integer(3));
    list.add(new Integer(4));
    list.add(new Integer(5));
    list.add(new Integer(6));
    Integer[] in = (Integer[])list.toArrary();          //错误
    for(int i = 0; i < in.length; i++)  
    {
      System.out.println(in[i].intValue());
    }
  }
}
```
>输出结果为：编译成功，java.lang.ClassCastException(类成员例外)错误。

**错误原因：Integer[ ] in = (Integer[ ])list.toArrary();解释一、Integer数组本身是对象，继承了Object但他并没有继承Object数组，所以不能实行强转。解释二、Object[]中可以存在多种不同类型的对象，但若将它们全部强转为Integer类型的就错了，本例中全部是integer，但并不是在Object[]中不存在其他类型的数据，所以这是错误的操作。想实现将Object[]强转为integer[]这种做法是不合法的，但将Object类型转换为integer类型是可以的，所以想要实现这一目的就要将元素逐个进行转换，而不是直接去使用数组。**

**改正：**
```java
Object[] in = list.toArrary();             //1、
for(int i = 0; i < in.length; i++)  
{
  System.out.println(((Integer)in[i]).intValue());    //2、
}
```

1. 返回一个Object类型的数组。
2. 将元素分别进行转换并调用方法输出。

注意：因为已经将集合变成了数组，所以调用“长度”概念时就不再使用size()而使用的是length。

### 特别注意：

#### 1.调用方法的参数
**从ArrayList集合方法中可以发现，凡是除了访问特定元素的方法，调用方法的参数都应该是对象，那么8种原生数据都不是对象就不能调用这些方法了么?ArrayList集合中不允许有原生数据类型的元素了么?**

##### 包装类（Wrapper Class）
* 针对原生数据类型的包装，所有的包装类（8个）都位于java.lang包下，java中的8个包装类分别是：byte,short,Integer,long,float,double,character,boolean 他们的使用方式是一样的，可以实现原生数据类型与包装类型的双向转换（将原生数据类型变成对象）

```java
list.add(new Integer(10));                   //add（）方法
Integer in = (Integer)list.get(0);           //get（）方法
```
所以若想使用原生数据类型调用ArrayList集合的方法可以使用包装类的方法将原生数据类型变成对象。

```java
package p;
import java.util.ArrayList;
public Class ArrayListTest3
{
  public static void main(String[] args)
  {
    ArrayList arrayList = new ArrayList();
    list.add(new Integer(3));                        //1、
    list.add(new Integer(4));
    list.add(new Integer(5));
    list.add(new Integer(6));
    int sum = 0;
    for(int i = 0; i < list.size(); i++)
    {
      int value = ((Integer)list.get(i).intvalue);   //2、
    }
    System.out.println(summ);
  }
}
```
>输出结果：18

1.首先将原生数据类型变为对象调用add()方法。

2.1 list.get(i)：取i，调用get()方法取值。

2.2（Integer）list.get(i)：返回值为对象，需要向下类型转换变为整型值。

2.3（Integer）list.get(i).intvalue:将最后得到的整型值调用包装类方法得到int值。

#### 2.向ArrayList集合中添加的对象类型需要与返回的对象类型一致。(add() , get())
```java
ArrayList list = new ArrayList();
list.add("hello")
list.add(new Integer(2));
String str = (String)list.get(0);
Integer in = (Integer)list.get(1);
System.out.println(str);
System.out.println(in.intvalue());
```

>输出结果：
```java
hello
2
```

ArrayList集合本身接受的是Object类型，所以除了原生数据类型以外都可以往里面添加（都是对象），但是取出来的时候一定要与添加值的类型相符（强转实现这一条件）。如果不符合会怎么样?

```java
String str = (String)list.get(0);
String in = (String)list.get(1);
```

>输出结果：编译成功，输出显示java.lang.ClassCastException(类型转换异常)

编译成功是因为java编译器只知道用户是想将Object类强转为String，这是合法的，但只有运行的时候才能知道这是一个字符串还是一个整型变量。

类型转换异常：Integer被强行转换为字符串属于错误操作。

#### 用户定义类

```java
package p;
public class ArrayListTest5
{
  public static void main(String[] args)
  {
    ArrayList list = new ArrayList();
    list.add(new point(2,3));
    list.add(new point(2,2));
    list.add(new point(2,4));
    for(int i = 0; i < list.size(); i++)
    {
      System.out.println(list.get(i));
    }
    //System.out.println(list);           //1、
  }
}
class point
{
  int x;
  int y;
  point(int x,int y)
  {
    this.x = x;
    this.y = y;
  }
  /*public String toString()                  //2、
  {
    return "x=" + this.x +", y=" + this.y;
  }*/
}

```
>输出结果：
```java
p.Point@c17164
p.Point@lfb8ee3
p.Point@61de33
```

##### toString()方法
当打印引用时，实际上会打印出引用所指对象的toString()方法的返回值，因为每一个类都会直接或间接地继承自Object类而Object类中定义了toString(),因此每个类都有toString()方法。Object类的toString()方法会打印出类的名字和@符号和地址信息。

list.get(i)虽然返回值类型是Object，但真实类型为Point，实际上会调用的是Point类型的toString()方法，因为没有重写toString()方法，所以会继承Object类的toString()方法即打印出类的名字和@符号和地址信息。

1. 此行输出结果为：[p.Point@c17164,p.Point@lfb8ee3,p.Point@61de33]后续说明原因。
2. 输出结果为
```java
x=2, y=3
x=2, y=4
x=2, y=2
[x=2,y=3,x=2, y=4,x=2, y=2]
```
重写了Point类的toString方法，输出方式改变


### 2.1.2ArrayList以及ArrayList方法的根源实现
当我们使用数组的时侯，在创建数组前一定要规定好数组的大小，比如说定义了一个长度为20的数组，但是后来却发现数据是30个，那么没办法只能再重新创建一个新的长度为30的数组并将长度为20的数组的值拷贝进新创建的数组，再追加元素。但ArrayList很好用，不需要管里面是怎么装的，只需要add()就可以了，再使用get()取出就可以了。根据API提供的方法来看与数组很相似，数组可以通过索引来访问，而集合也可以通过相同的方式进行访问操作。那么ArrayList是如何实现的?
###### ArrayList构造方法源代码：
```java
public ArrayList()                                                  //1、
{
  this(10);                                                         //2、
}
public ArrayList(int initialCapacity)                               //3、
{
  super();
  if(initialCapacity < 0)
    throw new IllegalArgumentException("Illegal Capacity: "+initialCapacity);       //4、
  this.elementData = new Object[initialCapacity];                                   //5、
}
private transient Object[] elementData;                             //6、
```
##### 说明：
1. 默认的不带参数的构造方法
2. 默认将10传进另一个重载的构造方法
3. initialCapacity(整数)参数为整数。
4. 如果传值的长度小于0，返回不合法。
5. 生成一个长度大小为参数的Object数组
6. **这是一个将ArrayList元素缓存的数组。缓存数组的长度决定ArrayList最终的容量。(源码解释)**这是一个名称为elementData的Object类型数组，所以可以存放任何对象类型。也正是因为是数组，所以集合中存放的依然是对象的引用而不是对象本身。

**ArrayList底层采用数组实现，当使用不带参数的构造方法生成ArrayList对象时，实际上会在底层生成一个长度为10的Object类型数组。**

#### 2.1.2.1add()方法的根源实现
add(E e) 向列表中添加对象，将指定的元素追加到列表的末尾。
###### add()方法源代码：
```java
public boolean add(E e)                          //1、
ensureCapacity(size + 1);                        //2、   
elementData[size++] = e;
return true;
private int size;                                //3、
```
1. 返回布尔类型数据，添加成功返回真，失败返回假
2. ensureCapacity(确保容量)
3. ArrayList的大小(其中包含的元素个数),刚开始未给其赋值，所以值为0

###### 2.ensureCapacity(int minCapacity)方法源代码：
**增加这个ArrayList实例的容量，如果在必要的情况下，去保证至少能容纳由最小的Capacity数量的元素个数。**
```java
public void ensureCapacity(int minCapacity)
{
  modCount++;
  int oldCapacity = elementData.length;                             //1、
  if(minCapacity > oldCapacity)                                     //2、
  {
    Object oldData[] = elementData;
    int newCapacity = (oldCapacity * 3)/2 + 1;
    if (newCapacity < minCapacity)
    newCapacity = minCapacity;
    elementData = Array.copyof(elementData,newCapacity);
  }
}

```
1. elementData为数组，使用length属性
2. 判断是否大于数组的容量，若大于这个数组，则生成新的数组
elementData[size++] = e: 调用完ensureCapacity(size + 1);后，将e赋值给element[size]元素，然后size++，代表增加了一个元素。



>###### 3.size()方法源代码：
size() 返回列表当中元素的个数。
```java
public int size()
{
  return size;
}
private int size;
```
ArrayList的大小(其中包含的元素个数),刚开始未给其赋值，所以值为0
