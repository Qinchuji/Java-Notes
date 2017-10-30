# set(集合)
Set和List一样，也继承于Collection,是集合的一种。和List不同的是，Set内部实现是基于Map的，所以Set取值时不保证数据和存入的时候顺序一致，并且不允许空值，不允许重复值。从继承结构可以看出,Set主要有2个实现方式，一个是TreeSet，另一个是HashSet 这个Set的特点，主要由其内部的Map决定的。

## Set源代码
#### public Interface Set
>java.util;

>public interface Set<E>;

>extends Colllecton Iterable;

>part of important implementing Class:Hashset;

```
A collection that contains no duplicate elements.  More formally, set contain no pair of elements <code>e1</code> and <code>e2</code> such that <code>e1.equals(e2)</code>, and at most one null element.  As implied by its name, this interface models the mathematical <i>set</i> abstraction.
```
一个集合不能包含重复的元素。更确切地讲，set 不包含满足 e1.equals(e2) 的元素对 e1 和 e2，并且最多包含一个 null 元素。正如其名称所暗示的，此接口模仿了数学上的 set 抽象。在Java中使用Set,可以方便地将需要的类型以集合类型保存在一个变量中.主要应用在显示列表。
* 根据源代码注释介绍将通过研究add方法来了解set对于不能有重复元素这一特点的实现，其中Set接口中对add方法作了声明，最终找到了实现接口的Hashset类对Set接口中add方法的具体操作。

## Hashset源代码
 HashSet类实现Set接口，由哈希表（实际上是一个HashMap实例）支持。它不保证set 的迭代顺序；特别是它不保证该顺序恒久不变。此类允许使用null元素。
#### public class HashSet<E>;
>java.util;

>extends AbstractSet<E>;

>implements Set<E>, Cloneable, java.io.Serializable


### public boolean add(E e)方法(jdk1.8)
```java
return map.put(e, PRESENT)==null;
```

```
Removes the specified element from this set if it is present. More formally, removes an element <tt>e</tt> such that <tt>(o==null&nbsp;?&nbsp;e==null&nbsp;:&nbsp;o.equals(e))</tt>, if this set contains such an element.  Returns <tt>true</tt> if this set contained the element (or equivalently, if this set changed as a result of the call).  (This set will not contain the element once the call returns.)
```

若指定的元素本来在集合中存在则将此元素移除。更确切地讲，将集合中满足(o==null&nbsp;?&nbsp;e==null&nbsp;:&nbsp;o.equals(e))这个条件的元素“e”在集合中移除，并且返回值为“真”。(相当于，如果这个集合因为调用而改变了，那么这个集合将不会在调用结束后包含这个元素。)底层实际调用HashMap的clear方法清空了Entry中所有元素。

* 注意到判定条件中equals方法的出现。

## Class object equals();
如果两个对象具有相同的类型以及相同的属性值，则称这两个对象相等。如果两个引用对象指的是同一个对像，则称这两个变量同一。Object类中定义的equals 函数原型为：public boolean equals(Object);他是判断两个对象是否同一，并不是是否相等。

```java
public boolean equals(Object obj) {
       return (this == obj);
   }
   ```

```
public boolean equals(@Nullable Object obj)
Inferred annotations available:
@org.jetbrains.annotations.Nullable
Indicates whether some other object is "equal to" this one.
The equals method implements an equivalence relation on non-null object references:
It is reflexive: for any non-null reference value x, x.equals(x) should return true.
It is symmetric: for any non-null reference values x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
It is transitive: for any non-null reference values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.
It is consistent: for any non-null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
For any non-null reference value x, x.equals(null) should return false.
The equals method for class Object implements the most discriminating possible equivalence relation on objects; that is, for any non-null reference values x and y, this method returns true if and only if x and y refer to the same object (x == y has the value true).
Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes.
Parameters:
obj - the reference object with which to compare.
Returns:
true if this object is the same as the obj argument; false otherwise.
See Also:
hashCode(), java.util.HashMap
```
用来只是另外一个对象是否与当前调用方法对象相等。
equals方法实现了一个关于非空对象引用上相等性关系：
1. 自反性：对于任意非空引用“x”，x.equals(x) 应该返回“true”。
2. 对称性：对于任意非空引用"x"、"y",x.equals(y)应该返回“true”有且只有当y.equals(x)也返回“true”。
3. 传递性：对于任意非空引用"x"、"y"和"z"如果x.equals(y)返回“true”并且y.equals(z)返回“true”，那么x.equals(z)也返回“true”。
4. 一致性：对于任意非空引用"x"、"y"多次对x.equals(y)的调用都应一致的返回同一个结果，没有为equals方法提供任何可用信息来修改对象。
5. 对于任意非空引用"x"，x.equals(null)应该返回“false”

object类的equals方法实现了最有可能的差别比较关系，就是说对于任意非空引用"x"、"y"，这个方法当且仅当两个引用指向同一对象的时候才返回"true"。

要注意通常当重写本方法时有必要将hashCode方法一并重写,这样就能保持着对于**hashCode方法的一般性的契约**即相等的对象一定要有相等的hash codes。

## Calss object hashcode()
在使用equals方法作相同性判断时(==)，实际上需要判断的是hash code值。
```java
public native int hashCode();
```
```
public int hashCode()
Returns a hash code value for the object. This method is supported for the benefit of hash tables such as those provided by java.util.HashMap.
The general contract of hashCode is:
Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.
If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.
It is not required that if two objects are unequal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hash tables.
As much as is reasonably practical, the hashCode method defined by class Object does return distinct integers for distinct objects. (This is typically implemented by converting the internal address of the object into an integer, but this implementation technique is not required by the Java™ programming language.)
Returns:
a hash code value for this object.
See Also:
equals(Object), System.identityHashCode
```
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
