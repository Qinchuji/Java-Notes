# 类集框架
类集框架被设计用于适应几个目的。首先，这种框架是高性能的。对基本类集（动态数组、链接表、树和散列表）的实现是高效率的。一般很少需要人工去对这些“数据引擎”编写代码（如果有的话）。**第二点，框架必须允许不同类型的类集以相同的方式和高度互操作方式工作。**;第三点，类集必须是容易扩展和/或修改的。**为了实现这一目标，类集框架被设计成包含一组标准的接口。**;对这些接口，提供了几个标准的实现工具（例如LinkedList、HashSet和TreeSet），通常就是这样使用的。为了方便起见，创建用于各种特殊目的的实现工具。一部分工具可以使自己的类集实现更加容易。**最后，增加了允许将标准数组融合到类集框架中的机制。**
https://baike.baidu.com/item/%E7%B1%BB%E9%9B%86/4887328?fr=aladdin
>**集合框架中的接口**

>所谓框架就是一个类库的集合，集合框架就是一个用来表示和操作集合的统一的架构，包含了实现集合的接口与类。

>**集合(Collection)接口作为根接口，分别被Set和List所继承，而SortedSet实现了Set。映射（Map）被排序映射(SortedMap)所继承。**

## 1.Collection（集合）

#### Interface Collection
>java.util;
>public interface Collection<E>;
>extends Iterable;

一个集合代表了一组对象,这些对象我们称之为元素。某些集合允许重复的元素而其他是不可以的，有些是排序而有些不是。JDK不提供任何一个类直接去实现Collection接口，而是提供了更加具体的子接口Set和List接口继承Collection。这两个接口最主要用于传递集合以及操纵集合。

**如何Collections接口中定义的方法：因为JDK不提供任何一个类直接去实现Collection接口而只有Set和List继承了Collection所以只能通过子接口去实现Collection的方法。**

Collections接口中定义的某些方法：
add(E e)
返回值为boolean


## 2.List（列表）
#### Interface List<E>
>java.util;

>public interface List<E>;

>extends Collections<E>;

一个有序的集合称之为列表（或称作序列）。用户可以精确的通过下标来访问每一个插入列表中的元素，实现控制以及搜索功能。

## 2.1ArrayList（数组列表）
实现了List接口的类：ArrayList和LinkedList
#### ArrayList

>java.util;

>Class ArrayList<E>;

>Implemented Inerfaces : Collections , List;

ArrayList是List接口的可变数组的实现。实现了所有可选列表操作，并允许包括null在内的所有元素。除了实现List接口外，此类还提供一些方法来操作内部用来储存列表的数组的大小。

## 2.1.1ArrayList方法

1. 构造方法:1.public ArrayList():(无参数空列表)2.public ArrayList(int initialCapacity)(长度大小为参数)
2. public void add(int index, E element) 向列表中添加对象，将指定的元素追加到列表的末尾。
3. public E get(int index) 返回列表中特定的元素。参数为元素的索引。
4. public int size() 返回列表当中元素的个数。
5. public void clear() 将列表中所有元素都删除。
6. public boolean isEmpty() 判断列表中是否包含元素。返回值为boolean值。
7. public E remove(int index) 将列表中特定的元素删除。参数为元素的索引。
8. public int indexOf(Object o)判断特定元素在里表中的位置。
9. public Object[] toArray() 以正当的顺序(从第一个元素到最后一个元素)返回一个包含所有元素的数组(返回一个Object类型的数组)。

### ArrayList构造方法源代码
>1. public ArrayList();
>2. public ArrayList(int initialCapacity);

```java
public ArrayList()                            //1、
{
  this(10);                                   //2、
}
public ArrayList(int initialCapacity)         //3、
{
  super();
  if(initialCapacity < 0)
    throw new IllegalArgumentException("Illegal Capacity: "+initialCapacity);     //4、
  this.elementData = new Object[initialCapacity];                                 //5、
}
private transient Object[] elementData;                //6、
```
#### 说明：
1. 默认的不带参数的构造方法
2. 默认将10传进另一个重载的构造方法
3. initialCapacity(整数)参数为整数。
4. 如果传值的长度小于0，返回不合法。
5. 生成一个长度大小为参数的Object数组
6. **这是一个将ArrayList元素缓存的数组。缓存数组的长度决定ArrayList最终的容量。(源码解释)**这是一个名称为elementData的Object类型数组，所以可以存放任何对象类型。也正是因为是数组，所以集合中存放的依然是对象的引用而不是对象本身。

**ArrayList底层采用数组实现，当使用不带参数的构造方法生成ArrayList对象时，实际上会在底层生成一个长度为10的Object类型数组。**

### 相关方法的使用

>1. public void add(int index, E element);
>2. public E get(int index);
>3. public int size();
>4. public void clear();
>5. public boolean isEmpty();
>6. public E remove(int index);
>7. public int indexOf(Object o);

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
4. 也可以使用这个方法进行判断是否为空.

```java
arrayList.remove(0);
arrayList.remove(“hello”);
System.out.println"---------"
for(int i = 0;i < arrayList.size();i++)     
{
  System.out.println(arrayList.get(i));
}
```
>输出结果为：world.

remove（）方法另有一个重载方法，参数是传递内容。
清除的机制是将特定的元素清除后将后面索引的元素向前移动。例如：清除了s0，s0 = s1,s1 = s2....所以此时的world变成了第一个元素。

```java
arrayList.add("aaa");
arrayList.add("bbb");
System.out.println(arrayList.indexOf("aaa"));
```
>输出结果为:"0".

"aaa"在ArrayList集合里是第一个位置，索引为0.

#### toArray()方法
以正当的顺序(从第一个元素到最后一个元素)返回一个包含所有元素的数组(返回一个Object类型的数组)。

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

### ArrayList相关方法的根源实现
当我们使用数组的时侯，在创建数组前一定要规定好数组的大小，比如说定义了一个长度为20的数组，但是后来却发现数据是30个，那么没办法只能再重新创建一个新的长度为30的数组并将长度为20的数组的值拷贝进新创建的数组，再追加元素。但ArrayList很好用，不需要管里面是怎么装的，只需要add()就可以了，再使用get()取出就可以了。根据API提供的方法来看与数组很相似，数组可以通过索引来访问，而集合也可以通过相同的方式进行访问操作。那么ArrayList是如何实现的?

### add()方法的根源实现
**add(E e) 向列表中添加对象，将指定的元素追加到列表的末尾。**
#### add()方法源代码：
```java
public boolean add(E e)                          //1、
ensureCapacity(size + 1);                        //2、   
elementData[size++] = e;                         //3、
return true;
private int size;                                //4、
```
1. 返回布尔类型数据，添加成功返回真，失败返回假
2. ensureCapacity(确保容量)
3. 调用完ensureCapacity(size + 1);后，将e赋值给element[size]元素，然后size++，代表增加了一个元素。
3. size：ArrayList的大小(其中包含的元素个数),刚开始未给其赋值，所以值为0

#### add()方法中ensureCapacity(int minCapacity)方法源代码：
**增加这个ArrayList实例的容量，如果在必要的情况下，去保证至少能容纳由最小的Capacity数量的元素个数。**
```java
public void ensureCapacity(int minCapacity)
{
  modCount++;
  int oldCapacity = elementData.length;                  //1、
  if(minCapacity > oldCapacity)                          //2、
  {
    Object oldData[] = elementData;
    int newCapacity = (oldCapacity * 3)/2 + 1;
    if (newCapacity < minCapacity)
    newCapacity = minCapacity;
    elementData = Array.copyOf(elementData,newCapacity);
  }
}
```
1. 将ArrayList构造方法中生成的elementData的length属性值赋给了oldCapacity
2. 判断参数minCapacity（size + 1)是否大于oldCapacity(elementData.length)。若大于则生成新的对象类型数组指向了elementData数组，并生成了新的整型变量newCapacity赋值为(oldCapacity * 3)/2 + 1(例：值为10，则newCapacity值为16)，若小于则不进行任何操作，直接往下进行elementData[size++] = e；再判断newCapacity与minCapacity(size + 1)若不小于则进行copyOf方法，将原数组拷贝到新数组中并将容量变成newCapacity的容量。若小于则说明新生成的容量仍不够用户add()的元素个数，则将minCapacity的长度值赋给newCapacity，若大于，则直接进行copyOf方法。

#### size()方法源代码：
size() 返回列表当中元素的个数。
```java
public int size()
{
  return size;
}
private int size;
```
ArrayList的大小(其中包含的元素个数),刚开始未给其赋值，所以值为0

**如果增加的元素个数超过了10个，那么ArrayList底层会新生成一个数组，长度为原数组的1.5倍+1，然后将原数组的内容复制到新的数组当中，并且后续增加的内容都会放到新数组当中。当新数组无法容纳增加的元素时，重复该过程。实际上底层的实现就是一个数组，如果能装下就装下，如果装不下那么就生成新的数组，将原来的数组拷贝到新的数组里，新追加的再放到后面去。**

#### add(int index, E element)
**在特定位置插入元素**
#### add(int index, E element)方法源代码：

```java
public void add(int index, E element)
{
  if(index >size ||index < 0)
  throw new IndexOutOfBoundsException("Index:"+index+",Size:"+size);
  ensureCapacity(size+1);
  System.arraycopy(elementData, index, elementData, index + 1,size - index);
  elementData[index] = element;
  size++;
}
```
主要使用arraycopy方法进行元素的后移，将特定元素插入即可。代价相当高，需要改变后续元素的所有位置。

#### get()方法的根源实现
**get(int index) 返回列表中特定的元素。参数为元素的索引。**
##### get()方法源代码：
```java
public E get(int index)
{
  RangeCheck(index)
  return (E) elementData[index];
}
```
1. RangeCheck:范围检查，检查元素检索是否在列表范围之内。
```java
private void RangeCheck(int index)
{
  inf (index >= size)
  throw new IndexOutOfBoundsException
  ("Index:"+index+",Size:" +size);
}
```

#### remove（int index)方法的根源实现
**remove（int index) 将列表中特定的元素删除并返回该被删元素。参数为元素的索引。**
#### remove（int index)源代码：

```java
public E remove(int index)
{
  RangeCheck(index);                                       //1、
  modCount++;
  E oldValue = (E)elementData[index];                      //2、
  int numMoved = size - index - 1;
  if (numMoved > 0)
  {
    System.arraycopy(elementData,index+1,elementData,index,numMoved);  //3、
    elementData[--size] = null;
    return oldvalue;
  }
}

```
1. RangeCheck:范围检查，检查元素检索是否在列表范围之内。
2. 取值，找到elementData[index](特定元素)赋给oldValue
3. 数组拷贝：这里的操作实际上是对元素进行了移位，并将最后一个位置的元素赋值为空。(数组长度是没有变的)
**对于ArrayList元素的删除操作，需要将被删除元素的后续元素向前移动，代价比较高。**


### 特别注意：

### 1.调用方法的参数
**从ArrayList集合方法中可以发现，凡是除了访问特定元素的方法，调用方法的参数都应该是对象，那么8种原生数据都不是对象就不能调用这些方法了么?ArrayList集合中不允许有原生数据类型的元素了么?**

#### 包装类（Wrapper Class）
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
      int value = ((Integer)list.get(i).intvalue);   //2、3、4、
    }
    System.out.println(summ);
  }
}
```
>输出结果：18

1. 首先将原生数据类型变为对象调用add()方法。
2.  list.get(i)：取i，调用get()方法取值。
3. （Integer）list.get(i)：返回值为对象，需要向下类型转换变为整型值。
4. （Integer）list.get(i).intvalue:将最后得到的整型值调用包装类方法得到int值。

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

#### toString()方法
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
重写了Point类的toString方法，输出方式改变。



## 2.2LinkedList（链表）
实现了List接口的类：ArrayList和LinkedList
#### LinkedList

>java.util;

>Class LinkedList<E>;

>Implemented Inerfaces : Collections , List;

LinkedList是动态数组的另一种实现，底层以双向循环链表为实现基础，它的优势在于可以快速的删除和添加元素，不需要像ArrayList那样移动大量元素，但对于查找元素需要遍历链表中的元素进行匹配。所以LinkedList适用于频繁删除和添加元素，较少有使用查找元素的应用场景。

## 2.2.1LinkedList的基本结构
之前提到双向循环链表为LinkedList的实现基础，在LinkedList中，我们将“链”的基本单位称之为“节点”，每个节点都是同样的结构。节点与节点之间相连，构成了我们的LinkedList的基本数据结构，也是LinkedList的核心。
* JDK1.6与1.7之间是有区别的，详见：http://m.blog.csdn.net/zw0283/article/details/51132161

## Node（节点）
LinkedList内部使用Node<E>来封装双向循环链表节点，节点中一般包括三个内容：前一个节点的引用，真正的数据以及后一个节点的引用。
### Node源代码
Node是LinkedList的内部私有类，它的组成很简单，只有一个构造方法。
```java
private static class Node<E> {
       E item;
       Node<E> next;
       Node<E> prev;

       Node(Node<E> prev, E element, Node<E> next) {
           this.item = element;
           this.next = next;
           this.prev = prev;
       }
   }
```
##### 说明：
构造方法的参数顺序是：前继节点的引用，数据，后继节点的引用。

1.6中有了这一信息，可以将LinkedList头节点定义：
```java
private transient Entry<E> header = new Entry<E>(null,null,null)
```
1.7中将这一方式删除，因为定义LinkedList不再是环形结构而是直链式结构，代表头节点的prev为null，尾节点的next为null。不再是循环结构。

## 2.2.2LinkedList方法
1. 构造方法： 1.public LinkedList()；(无参构造)2.public LinkedList(Collection<? extends E> c)；(传入Collection对象)
2. public void addFirst(E e);链表的头部添加一个节点。**private void linkFirst(E e)关键代码**
3. public void addLast(E e);链表的尾部添加一个节点。**private void linkLast(E e)关键代码**
4. public void add(int index, E element);链表中的其中一个节点后面添加一个节点。**private void linkBefore(E e, Node<E> succ)关键代码**
5. public E getFirst()；返回链表第一个节点的数据。
6. public E getLast()；返回链表最后一个节点的数据。
7. public E get(int index)；返回链表中符合索引值节点的数据。**Node<E> node(int index)关键代码**
8. public E removeFirst();将链表中第一个节点移除。**private E unlinkFirst(Node<E> f)关键代码**
9. public E removeLast();将链表中最后一个节点移除。**private E unlinkLast(Node<E> l)关键代码**

## LinkedList构造方法源代码

>1. public LinkedList();
>2. public LinkedList(Collection<? extends E> c)；
>3. public boolean addAll(Collection<? extends E> c);
>4. public boolean addAll(int index, Collection<? extends E> c);

LinkedList包含3个全局参数：1.size存放当前链表有多少个节点。2.first为指向链表的第一个节点的引用。3.last为指向链表的最后一个节点的引用。
```java
public LinkedList() {               //1、
}
public LinkedList(Collection<? extends E> c) {          //2、
    this();
    addAll(c);
}


public boolean addAll(Collection<? extends E> c) {      //3、
    return addAll(size, c);
}
public boolean addAll(int index, Collection<? extends E> c) {    //4、
    checkPositionIndex(index);            //5、

    Object[] a = c.toArray();             //6、
    int numNew = a.length;
    if (numNew == 0)                      //7、
        return false;

    Node<E> pred, succ;                   //8、
    if (index == size) {                  //9、
        succ = null;
        pred = last;
    } else {                              //10、
        succ = node(index);
        pred = succ.prev;
    }

    for (Object o : a) {                  //11、
        @SuppressWarnings("unchecked") E e = (E) o;
        Node<E> newNode = new Node<>(pred, e, null);  //12、
        if (pred == null)                 //13、
            first = newNode;
        else
            pred.next = newNode;
        pred = newNode;
    }

    if (succ == null) {                   //14、
        last = pred;
    } else {                              //15、
        pred.next = succ;
        succ.prev = pred;
    }

    size += numNew;                       //16、
    modCount++;
    return true;
}
```
1. 构造函数中的无参构造函数，表示构造了一个空链表。
2. 构造函数中的有参构造函数，接收一个Collection参数c。this()调用了无参构造方法首先构造了一个空链表，之后以c为参数调用addAll()方法。构造了一个包含了集合中指定元素的链表。
3. 将参数(size，c)返回给addAll(index，c)方法。
4. 将c转换为链表，添加到指定的位置。index参数指定collection中插入的第一个元素的位置。
5. 检查传入的索引值是否在合理范围之内。
6. 将给定的Collection对象转换为Object数组。
7. 数组长度为0返回false，表示数组为空，没有可插入元素。
8. 定义了两个节点：pred,succ;pred(predecessor)：要添加位置的前节点。succ:要添加位置的后一节点。
9. **构造方法(原链表为空)：因为3.所以执行这条语句，这是用来判断是否要添加到末尾的，添加位置的前一个元素则为last，后一个元素为空。但是原链表为空所以即使执行这条语句也不会有什么改变。**
10. 表示插入链表对于原非空链表有特殊要求，即添加到指定位置，将index所对应的节点赋给下一个节点，并获取前一个节点。
11. 这是一个for-each遍历，表示循环取出"a"Object类数组里的每一个值赋值给"o"每取一次推行一遍。
12. 将集合中的元素封装成节点，并与当前的链表建立连接。
13. **构造方法(原链表为空)：因为原来链表为空，所以符合这个判断，第一次循环使第一个元素成为了first，后来pred指向了first使后面判断为false，更新pred下次循环使用。**
14. 判断是否是添加到末尾，集合转换成链表的最后一个节点就是末尾节点，所以要更新末尾节点的对象。将当前链表最后一个节点赋值给last。
15. 原链表非空时，将断开的部分连上。
16. 记录当前结点个数。


##### 说明：
LinkedList提供了两个构造方法。无参构造方法构造空链表，有参构造传入Collection对象，将对象转为数组，并按遍历顺序将数组首尾相连，全局变量first和last分别指向这个链表的第一个和最后一个。

### LinkedList相关方法的根源实现
#### addFirst/addLast分析
```java
public void addFirst(E e) {
        linkFirst(e);
    }
private void linkFirst(E e) {
        final Node<E> f = first;                           //1、
        final Node<E> newNode = new Node<>(null, e, f);    //2、
        first = newNode;
        if (f == null)                //3、
            last = newNode;
        else
            f.prev = newNode;         //4、
        size++;
        modCount++;
    }
```
1. 获取当前链表的第一个节点。
2. 创建新的节点，新节点的后继指向原来的头节点，即将原头节点向后移一位，新节点代替头节点的位置。从定义方式也可以看出这是当作头节点去定义的(头节点前驱必然为null)。
3. 此时的"f"为第二个节点，所以判断若为真则表示其原链表为空，即新节点既是头节点又是尾节点。
4. 若判断为假，则表示原链表不为空，所以f的前继便为新节点即头节点。

##### 说明：

addLast方法在实现上是与addFirst一致的。

LinkFirst是将现在的链表的头部添加一个节点。了解了基本的数据结构其实是很好理解的，add系列的方法基本是大同小异的，都是创建新的节点，并且改变了之前节点的指向关系。

#### get分析
**getFirst/getLast分析**
```java
public E getFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return f.item;
    }

public E getLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return l.item;
        }
```
##### 说明：
分别取头节点和尾节点，并作判断是否为空，若不为空则返回内容。

**get()分析**
```java
public E get(int index) {
        checkElementIndex(index);
        return node(index).item;
    }

Node<E> node(int index) {
   // assert isElementIndex(index);

    if (index < (size >> 1)) {                   //1、
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)   //2、
            x = x.prev;
        return x;
    }
}
```
1. size >> 1 右移一位相当于size / 2。
2. size - 1 下表从零开始则最后一个元素的索引为总数减一。

##### 说明：
实际上node(int index)做的是判断给定的索引值，若索引值大于整个链表长度的一半，则从后往前查找，若索引值小于整个链表长度的一半，则从前往后查找。这样使得不管链表长度有多大，搜索的时候最多只搜索链表长度的一半就可以找到，大大提升了效率，其原理相当于小规模的二分查找，即使提升了查找效率，却仍比不上ArrayList查找。

#### removeFirst分析
```java
public E removeFirst() {
       final Node<E> f = first;
       if (f == null)
           throw new NoSuchElementException();
       return unlinkFirst(f);
   }

private E unlinkFirst(Node<E> f) {
       // assert f == first && f != null;
       final E element = f.item;              //1、
       final Node<E> next = f.next;           //2、
       f.item = null;                         //3、
       f.next = null; // help GC
       first = next;
       if (next == null)                      //4、
           last = null;
       else
           next.prev = null;
       size--;
       modCount++;
       return element;
    }
```
1. 将头节点的数据赋给了新定义的element作为返回值返回。
2. 定义f.next节点为next节点。
3. 将头节点删除，并将next节点变为头节点。
4. 判断next节点是否为空，若为空则代表原链表只有一个节点即“f”，若非空则将头节点的前继节点的引用置为空。

##### 说明：
对于removeLast方法与之相似，对于LinkedList的其他方法，大致上都是包装了以上几个方法，没有什么其他的大变动。具体思想是数据结构，而这其实只是用java语言实现了而已。


## 最后有关于ArrayList 和LinkedList的比较分析
1. ArrayList底层采用数组实现，LinkedList底层采用双向链表实现。
2. 当执行插入或者删除操作的时候LinkedList效率更高。因为对于LinkedList来说，插入删除操作只需动用引用即可，元素无需移动，只改变了引用，而对于ArrayList来说元素的移动幅度很大，代价很高。
3. 当执行搜索操作时，ArrayList效率更高因为ArrayList为线性存储，首元素的地址一旦确立，并且知道了每一项的存储内存就能很快的找到指定元素，而LinkedList则需要一项一项进行查找无法跳跃。
