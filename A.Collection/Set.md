# set(集合)
Set和List一样，也继承于Collection,是集合的一种。和List不同的是，Set内部实现是基于Map的，所以Set取值时不保证数据和存入的时候顺序一致，并且不允许空值，不允许重复值。从继承结构可以看出,Set主要有2个实现方式，一个是TreeSet，另一个是HashSet.将通过在集合中添加元素以及从集合中取出元素来研究相关类以及方法。

## Set源代码
#### public Interface Set
>java.util;

>public interface Set<E>;

>extends Colllecton Iterable;

>part of important implementing Class:Hashset;

源码注释：
A collection that contains no duplicate elements.More formally, sets contain no pair of elements e1 and e2 such that e1.equals(e2), and at most one null element.As implied by its name, this interface models the mathematical set abstraction.

一个集合不能包含重复的元素。更确切地讲，set 不包含满足 e1.equals(e2) 的元素对 e1 和 e2，并且最多包含一个 null 元素。正如其名称所暗示的，此接口模仿了数学上的 set 抽象。在Java中使用Set,可以方便地将需要的类型以集合类型保存在一个变量中.主要应用在显示列表。
* 根据源代码注释介绍将通过研究add方法来了解set对于不能有重复元素这一特点的实现，其中Set接口中对add方法作了声明，找到实现接口的Hashset类对Set接口中add方法的具体操作。

## Hashset源代码
 HashSet类实现Set接口，由哈希表（实际上是一个HashMap实例）支持。它不保证set 的迭代顺序；特别是它不保证该顺序恒久不变。此类允许使用null元素。
#### public class HashSet<E>;
>java.util;

>extends AbstractSet<E>;

>implements Set<E>, Cloneable, java.io.Serializable


## public boolean add(E e)方法
```java
return map.put(e, PRESENT)==null;
```

源码注释：
Adds the specified element to this set if it is not already present.More formally, adds the specified element e to this set if this set contains no element e2 such that (e==null ? e2==null : e.equals(e2)).If this set already contains the element, the call leaves the set
unchanged and returns false.



若指定元素与集合中元素不重复则添加进集合中。更确切地讲，将指定元素添加进集合中，如果这个集合中的元素不满足(o==null&nbsp;?&nbsp;e==null&nbsp;:&nbsp;o.equals(e))这个条件，如果这个集合中已经包含了这个元素，那么返回值将不对集合做任何改变并且返回值为“假”。

* 注意到判定条件中equals方法的出现。

## Object类方法

### Class object equals();
如果两个对象具有相同的类型以及相同的属性值，则称这两个对象相等。如果两个引用对象指的是同一个对像，则称这两个变量同一。Object类中定义的equals 函数原型为：public boolean equals(Object);他是判断两个对象是否同一，并不是是否相等。

```java
public boolean equals(Object obj) {
       return (this == obj);
   }
   ```

源码注释：
public boolean equals(@Nullable Object obj) Inferred annotations available: @org.jetbrains.annotations.Nullable Indicates whether some other object is "equal to" this one.The equals method implements an equivalence relation on non-null object references:

It is reflexive: for any non-null reference value x, x.equals(x) should return true.

It is symmetric: for any non-null reference values x and y, x.equals(y)should return true if and only if y.equals(x) returns true.

It is transitive: for any non-null reference values x, y, and z,if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.

It is consistent: for any non-null reference values x and y,multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.

For any non-null reference value x, x.equals(null) should return
false.

The equals method for class Object implements the most discriminating possible equivalence relation on objects; that is, for any non-null reference values x and y, this method returns true if and only if x and y refer to the same object (x == y has the value true).Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes.

Parameters:
obj - the reference object with which to compare.

Returns:
true if this object is the same as the obj argument; false otherwise.

See Also:
hashCode(), java.util.HashMap

用来只是另外一个对象是否与当前调用方法对象相等。
equals方法实现了一个关于非空对象引用上相等性关系：
1. 自反性：对于任意非空引用“x”，x.equals(x) 应该返回“true”。
2. 对称性：对于任意非空引用"x"、"y",x.equals(y)应该返回“true”有且只有当y.equals(x)也返回“true”。
3. 传递性：对于任意非空引用"x"、"y"和"z"如果x.equals(y)返回“true”并且y.equals(z)返回“true”，那么x.equals(z)也返回“true”。
4. 一致性：对于任意非空引用"x"、"y"多次对x.equals(y)的调用都应一致的返回同一个结果，没有为equals方法提供任何可用信息来修改对象。
5. 对于任意非空引用"x"，x.equals(null)应该返回“false”

object类的equals方法实现了最有可能的差别比较关系，就是说对于任意非空引用"x"、"y"，这个方法当且仅当两个引用指向同一对象的时候才返回"true"。

要注意通常当重写本方法时有必要将hashCode方法一并重写,这样就能保持着对于**hashCode方法的一般性的契约**即相等的对象一定要有相等的hash codes。

### Calss object hashcode()
在使用equals方法作相同性判断时(==)，实际上需要判断的是hash code值。
```java
public native int hashCode();
```
源码注释：
Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by java.util.HashMap.The general contract of hashCode is:Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no  information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.If two objects are equal according to the equals(Object) method,then calling the hashCode method on each of the two objects must produce the same integer result.It is not required that if two objects are unequal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce distinct integer results. However,the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hashtables.As much as is reasonably practical, the hashCode method defined by class Object does return distinct integers for distinct objects.(This is typically implemented by converting the internal address of the object into an integer, but this implementation technique is not required by the Java™ programming language.)

Returns:
a hash code value for this object.

See Also:
equals(Object), System.identityHashCode

返回一个对象的hash code值。此方法是为了能够更好的实现Hashtable类就像那些Hashtable所提供的。

**hashCode的一般性契约：**
1. 无论何时如果在一次Java应用的执行过程当中调用了多次同一个对象的hashCode方法，那么hashCode方法必须一致性地返回同一个整型值，没有提供任何一个方法用来修改对象的equals比较。从执行一次应用到执行下一次相同应用的时候这个整数值不需要保持一致。
2. 如果根据equals方法两个对象相等的话，那么两个对象各调用hashCode方法返回的整数值必须也是相同的。
3. 当两个对象通过调用equals方法返回值为“false”的话，并不需要满足hashCode方法所返回的两个整型值也不同。然而程序员应该注意对于提升hash tables的性能，对两个不相等的对象生成不同的整型值是有必要的。

根据实际的情况来说，对于不同的对象，hashCode方法一般会返回不同的整型值。(通常是用来将对象内部的地址转换为整数值的操作，但是这一技术在Java编程语言当中并不是强制的。)
## equals()和hashCode()方法的使用
equals()和hashCode()方法是用来在同一类中做比较用的，尤其是在容器里如set存放同一类对象时用来判断放入的对象是否重复。
### 判断元素重复的过程
HashSet集合中不能有重复的元素，当一个元素添加到了集合中的时候即当使用hashSet时，hashCode()方法就会得到调用，判断已经存储在集合中的对象的hash code值是否与增加的对象的hash code一致；HashSet判断元素是否重复的依据是这样的：
1. 判断已经存储在集合中的对象的hash code值是否与增加的对象的hash code一致；如果不一致，则认为两个对象也不一致。如果一致执行equals方法。
2. 判断两个对象调用equals方法返回值是否为真。如果不相等，认为两个对象也不相等，如果相等，认为两个对象相等(equals()是判断两个对象是否相等的关键)

为什么光用第一条是不行的？因为，hashCode()相等时，equals()方法也可能不等，所以必须用第二条准则进行限制，才能保证加入的为非重复元素。那么为什么一定要有hashCode方法的判断，因为如果每增加一个元素就一次调用一次equals方法，那么当元素很多时，后添加到集合中的元素比较的次数就非常多了。于是，Java采用了哈希表的原理。哈希算法也称为散列算法，是将数据依特定算法直接指定到一个地址上。这样一来，当集合要添加新的元素时，先调用这个元素的hashCode方法，就一下子能定位到它应该放置的物理位置上。 如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了， 就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。

### 为什么重写了equals方法就一定要一并重写hashCode方法？（反之亦然）
注意查看hashCode一般性契约的的二条
>如果根据equals方法两个对象相等的话，那么两个对象各调用hashCode方法返回的整数值必须也是相同的。

假设两个对象，重写了其equals方法，其相等条件是内容相等，就返回"true"。如果不重写hashCode方法，其返回的依然是两个对象的内存地址值，必然不相等。这就出现了equals方法相等，但是hashCode不相等的情况。这不符合hashCode的规则。在HashSet中明确提到集合是不能重复的，而当重写了equals方法而进行内容的比较时会将地址的比较功能舍弃，那么如果不重写hashCode方法的话，也许会将内容相同的两个元素因为地址的不同当成两个非重复元素添加在Set当中，这种错误是一定要考虑到的。

## public Iterator<E> interator
iterator用到了一种设计模式(第二种)，称为迭代模式。
```java
return map.keySet().iterator();
```
源码注释：
Returns an iterator over the elements in this set. The elements are returned in no particular order.

返回一个针对集合的迭代器，这些元素的返回没有特殊顺序。
* 既然返回值是一Inerator接口的实例，那就顺藤摸瓜找一下是如何实现的。

### public Interface interator<E>
源码注释：
An iterator over a collection. Iterator takes the place of Enumeration in the Java Collections Framework. Iterators differ from enumerations in two ways:Iterators allow the caller to remove elements from the underlying collection during the iteration with well-defined semantics.Method names have been improved.

一个针对集合的迭代器。......

可以知道返回值一定是实现一个类的实例，并且是一个interator类型的，但是具体是哪个子类的实例，这样的现象就是通过接口以及多态屏蔽掉了子类的差异性。

#### interator接口的方法
##### boolean hasNext();
源码注释：
Returns true if the iteration has more elements.
(In other words, returns true if next would return an element rather than throwing an exception.)

如果迭代器还有元素则返回“真”，（换句话说，如果下一个元素返回的是值而不是异常则返回值为“真”。
##### E next();

Returns the next element in the iteration.

返回迭代器当中的下一个元素。
### 迭代器的原理以及使用
#### 迭代器的工作原理
当用户调用了hashSet的iterator方法，那么将返回一个Interator对象即针对集合的迭代器。最初在生成后迭代器指向了第一个元素的前面，接下来通过方法的调用，进行相对应的操作以及往下一个元素移动的循环操作，直到hasNext()方法返回“假”。


#### 使用迭代函数的过程
在通过迭代函数访问类集之前，必须得到一个迭代函数。每一个Collection类都提供一个iterator()函数，该函数返回一个对类集头的迭代函数。通过使用这个迭代函数对象，可以访问类集中的每一个元素，一次一个元素。通常，使用迭代函数循环通过类集的内容，步骤如下：
1. 通过调用类集的iterator()方法获得对该类集头的迭代函数(获得迭代器)。
2. 建立一个调用hasNext()方法的循环，只要hasNext()返回true，就进行循环迭代。
3. 在循环内部，通过调用next()方法来得到每一个元素。

例：
```java
import java.util.HashSet;
public class InerarotTest
{
  public static void main(String[] args)
  {
    HashSet set = new HashSet();
    set.add("a");
    set.add("b");
    set.add("c");
    set.add("d");
    set.add("e");
    Iterator ite = set.iterator();
    while(ite.hasnext())
    {
      String value = (String)ite.next();
      System.out.println(value)
    }
  }
}
```
>输出结果为：
```java
d
e
b
c
a
```

注意：next()方法返回的是Object类的对象，而add方法添加的是字符串对象，实际上返回的本来应该就是字符串类型，所以要进行向下类型转换使返回的值与添加的值类型一致。

## SortedSet(排序集合)
public interface SortedSet<E>

>java.util

>extends Set<E>

>See Also:Set, TreeSet, SortedMap, Collection, Comparable, Comparator, ClassCastException

部分源码注释:

A Set that further provides a total ordering on its elements.The elements are ordered using their natural ordering, or by a Comparator typically provided at sorted set creation time.

一个对元素进一步提供了排序的集合。元素要么以自然顺序进行排序，要么以Comparator所提供的特定排序规则进行排序。

### Class TreeSet
>public class TreeSet<E>

>java.util

>extends java.util.AbstractSet<E>

实现了接口SortedSet所以类的特点参照SortedSet接口继承了Set类，所以也继承了Set的所有方法。
## 自然顺序排列
例：
```java
import java.util.TreeSet;
public class TreeSetTest
{
  public static void main(String[] args)
  {
    TreeSet set = new TreeSet();
    set.add("C");
    set.add("B");
    set.add("D");
    set.add("A");
    System.out.println(set);
  }
}

```
>输出结果

```java
A
B
C
D
```
结果按照字母的升序排列(自然顺序排序)

## 特定排序规则排列
例：按照学生对象的成绩进行排序
```java
imort java.util.TreeSet;
public class TreeSetTest2
{
  public static void main(String[] args)
  {
    TreeSet set = new TreeSet();
    Person p1 = new Person(10);
    Person p1 = new Person(20);
    Person p1 = new Person(30);
    Person p1 = new Person(40);
    set.add(p1);
    set.add(p2);
    set.add(p3);
    set.add(p4);
    System.out.println(set);
  }
}
class Person
{
  int score;
  public Person(int score)
  {
    this.score = score;
  }
  public String toString()
  {
    return String.valueof(this.score)
  }
}
```
>输出结果：java.lang.ClassCastException:Person cannot be cast to java.lang.Comparable

结果中显示set.add(p2);语句错误，类型转换异常，person不能被转换成java.lang.Comparable类型。

* 其中提到了Comparable类，在文档中找到该类了解一下为什么会有异常。

#### public interface Comparable<T>
>java.lang

>See Also:Comparator

源码注释：
This interface imposes a total ordering on the objects of each class that implements it.This ordering is referred to as the class's natural ordering,and the class's compareTo method is referred to as its natural comparison method.

这是个会使每一个实现了这个接口的类的对象完全强迫性的遵循规律的接口。这个规律是以类的自然规则遵循的，并且类的compareTo方法也是根据其自然比较方法。

此接口强行对实现它的每个类的对象进行整体排序。此排序被称为该类的自然排序 ，类的 compareTo 方法被称为它的自然比较方法 。
* 这个接口只有一个方法即注释中反复提到的compareTo

**public int compareTo(T o);**

Compares this object with the specified object for order.

按照内部机制比较调用方法的对象与特殊对象。

* 因为是调用了add()方法的语句出现了错误，所以关注一下TreeSet的add()方法。

### public boolean add(E e)
>java.util.TreeSet

>Overrides:add in class AbstractCollection

源码注释：
Adds the specified element to this set if it is not already present.More formally, adds the specified element e to this set if the set contains no element e2 such that (e==null ? e2==null : e.equals(e2)).If this set already contains the element, the call leaves the set unchanged and returns false.

源码注释发现重写的add方法没什么不同。但从中发现了错误中出现的ClassCastException关键字。

Throws:
ClassCastException - if the specified object cannot be compared with the elements currently in this set

抛掷一个类型转换异常-如果指定的对象不能与集合中当前的元素进行比较。

将这个类型转换异常与compareTo方法进行联系发现例子中出现的错误是因为TreeSet是带排序的，在p2放进去之前编译器需要得知按照什么方式进行排序，但由于是自己定义的Person类，所以不存在所谓的自然排序顺序，最终返回的是"抛掷一个类型转换异常-如果指定的对象不能与集合中当前的元素进行比较。"的异常。所以如果用户想使用TreeSet类并且往里面放置对象就只能自己明确的定义内部机制的排序方式，前提是放置的对象本身没有一个自然排序顺序，不然会引起混乱。

* 那么如何定义用户类内部机制的排序方式？

### TreeSet(Comparator<? super E> comparator)方法(构造方法之一)
```java
public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }
```
源码注释：
Constructs a new, empty tree set, sorted according to the specified comparator.  All elements inserted into the set must be <i>mutually comparable</i> by the specified comparator: {@code comparator.compare(e1,e2)} must not throw a {@code ClassCastException} for any elements{@code e1} and {@code e2} in the set.  If the user attempts to add an element to the set that violates this constraint, the {@code add} call will throw a {@code ClassCastException}.@param comparator the comparator that will be used to order this set If {@code null}, the {@linkplain Comparable natural ordering} of the elements will be used.

根据指定的comparator进行排序，构造一个新的空的tree set。所有输入集合的元素一定要通过指定的comparator进行互相排序，如此comparator.compare(e1,e2)将一定不会向集合中任何e1，e2元素抛掷类型转换异常错误。反而如果用户想要将违反这个约定的元素加入集合中，那么add方法将会抛掷类型转换异常错误抛掷类型转换异常错误。

参数：Comparator比较器将被用作指定集合的排列顺序。如果值为空，那么自然排序顺序将被使用

* 注释中comparator.compare(e1,e2)调用了方法compare

### public interface Comparator<T>
源码注释：
A comparison function, which imposes a total ordering on some collection of objects.Comparators can be passed to a sort method (such as Collections.sort or Arrays.sort) to allow precise control over the sort order. Comparators can also be used to control the order of certain data structures (such as sorted sets or sorted maps), or to provide an ordering for collections of objects that don't have a natural ordering.

一个给对对象集合强加总体排列顺序的比较功能。比较器可以传递给排序方法(比如说Collections.sort或者Arrays.sort)用来允许对顺序排列的精确控制。比较器也可以用来控制某些数据结构的排列顺序(比如说排列集合或者排列映射)，亦或者为没有自然排列规则的对象集合提供一个排列顺序。

### int compare(T o1,T o2)
>java.util.Comparator

>int compare(T o1,T o2)

源码注释：
Compares its two arguments for order. Returns a negative integer,zero, or a positive integer as the first argument is less than,equal to, or greater than the second.In the foregoing description, the notation sgn(expression)designates the mathematical signum function, which is defined to return one of -1, 0, or 1 according to whether the value of expression is negative, zero or positive.

Parameters:
o1 - the first object to be compared.
o2 - the second object to be compared.

Returns:
a negative integer, zero, or a positive integer as the first argument
is less than, equal to, or greater than the second.

Throws:
NullPointerException - if an argument is null and this comparator does
not permit null arguments

ClassCastException - if the arguments' types prevent them from being
compared by this comparator.



按顺序比较两个参数。当第一个参数小于，等于或大于第二个参数时分别返回负数，0或者正数。从前面的说明，符号表达指定了通过定义参数是否小于，等于或大于第二个参数时返回一个-1，0或者+1的数学正负号函数。

参数：o1-被比较的第一个值。o2-被比较的第二个值。

返回值：当第一个参数小于，等于或大于第二个参数时分别返回负数，0或者正数。

NullPointerException-参数为空，这个比较器并不许可空参数。

ClassCastException-如果参数类型妨碍了它们被比较器比较。
