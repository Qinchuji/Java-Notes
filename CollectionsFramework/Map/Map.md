# Map(映射)

Map集合没有继承Collection接口,其提供的是key到value的映射,Map中不能包含相同的key值,一个key只能影射一个value.key值还决定了存储对象在映射中的存储位置.但不是key对象本身决定的,而是通过散列技术进行处理,可产生一个散列码的整数值,散列码通常用作一个偏移量,该偏移量对应分配给映射的内存区域的起始位置,从而确定存储对象在映射中的存储位置.Map集合包括Map接口以及Map接口所实现的类.





### public interface Map< K, V>

>java.util

An object that maps keys to values. A map cannot contain duplicate keys; each key can map to at most one value.
This interface takes the place of the Dictionary class, which was a totally abstract class rather than an interface.
The Map interface provides three collection views, which allow a map's contents to be viewed as a set of keys, collection of values, or set of key-value mappings. The order of a map is defined as the order in which the iterators on the map's collection views return their elements. Some map implementations, like the TreeMap class, make specific guarantees as to their order; others, like the HashMap class, do not.

Note: great care must be exercised if mutable objects are used as map keys. The behavior of a map is not specified if the value of an object is changed in a manner that affects equals comparisons while the object is a key in the map. A special case of this prohibition is that it is not permissible for a map to contain itself as a key. While it is permissible for a map to contain itself as a value, extreme caution is advised: the equals and hashCode methods are no longer well defined on such a map.

All general-purpose map implementation classes should provide two "standard" constructors: a void (no arguments) constructor which creates an empty map, and a constructor with a single argument of type Map, which creates a new map with the same key-value mappings as its argument. In effect, the latter constructor allows the user to copy any map, producing an equivalent map of the desired class. There is no way to enforce this recommendation (as interfaces cannot contain constructors) but all of the general-purpose map implementations in the JDK comply.

The "destructive" methods contained in this interface, that is, the methods that modify the map on which they operate, are specified to throw UnsupportedOperationException if this map does not support the operation. If this is the case, these methods may, but are not required to, throw an UnsupportedOperationException if the invocation would have no effect on the map. For example, invoking the putAll(Map) method on an unmodifiable map may, but is not required to, throw the exception if the map whose mappings are to be "superimposed" is empty.

一个key映射到value上的对象。一个映射不能包含重复的key；每个key最多只能影射一个value上。这个接口是用来取代Dictionary抽象类的。Map接口提供三个集合视图，1.key的集合 2.value的集合 3.key-value的集合。map内元素的顺序取决于Iterator的具体实现，获取集合视图其实是获取一个迭代器，实现对遍历元素细节的隐藏。TreeMap类能保证遍历元素的顺序，而HashMap就无法保证遍历元素的顺序。

注意：当使用一个可变对象作为key的时候要小心，map是根据hashCode和equals方法决定存放的位置的。一个特殊的案例是不允许一个map将自己作为一个key，但允许将自己作为一个value。

所有多种用途的map实现类应该提供两个“标准”构造器，一个无参构造器用来创建一个空map，一个只有一个参数，参数类型是map的构造器，用来创建一个新的和传入参数有一样key-value映射的map。实际上，后者允许复制任何一个map，这仅仅是一个建议，并没有强制要求，因为接口是无法包含构造器的，不过这个建议在JDK被遵守。
 
如果一个方法的操作是不被支持的，这个方法指定抛出UnsupportedOperationException异常。如果这个操作对mao是没有影响的，那么也可以不抛出UnsupportedOperationException异常。例如，在一个不能被修改的map调用putAll（Map）方法，如果该map的映射是空的，就不要求抛出UnsupportedOperationException异常。


## 接口中声明的方法
### V put(K key,V value)
>java.util.Map

Associates the specified value with the specified key in this map (optional operation). If the map previously contained a mapping for the key, the old value is replaced by the specified value. (A map m is said to contain a mapping for a key k if and only if m.containsKey(k) would return true.)

Parameters:

key - key with which the specified value is to be associated

value - value to be associated with the specified key

Returns:
the previous value associated with key, or null if there was no mapping for key. (A null return can also indicate that the map previously associated null with key, if the implementation supports null values.)


作用：将指定的value关联到指定的key并且返回到map上。


Map接口的实现类：HashMap

### public class HashMap<K, V>
>java.util

>extends java.util.AbstractMap<K, V>

>implements java.util.Map<K, V>, Cloneable, java.io.Serializable

Hash table based implementation of the Map interface. This implementation provides all of the optional map operations, and permits null values and the null key. (The HashMap class is roughly equivalent to Hashtable, except that it is unsynchronized and permits nulls.) This class makes no guarantees as to the order of the map; in particular, it does not guarantee that the order will remain constant over time.
This implementation provides constant-time performance for the basic operations (get and put), assuming the hash function disperses the elements properly among the buckets. Iteration over collection views requires time proportional to the "capacity" of the HashMap instance (the number of buckets) plus its size (the number of key-value mappings). Thus, it's very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.
An instance of HashMap has two parameters that affect its performance: initial capacity and load factor. The capacity is the number of buckets in the hash table, and the initial capacity is simply the capacity at the time the hash table is created. The load factor is a measure of how full the hash table is allowed to get before its capacity is automatically increased. When the number of entries in the hash table exceeds the product of the load factor and the current capacity, the hash table is rehashed (that is, internal data structures are rebuilt) so that the hash table has approximately twice the number of buckets.
As a general rule, the default load factor (.75) offers a good tradeoff between time and space costs. Higher values decrease the space overhead but increase the lookup cost (reflected in most of the operations of the HashMap class, including get and put). The expected number of entries in the map and its load factor should be taken into account when setting its initial capacity, so as to minimize the number of rehash operations. If the initial capacity is greater than the maximum number of entries divided by the load factor, no rehash operations will ever occur.
If many mappings are to be stored in a HashMap instance, creating it with a sufficiently large capacity will allow the mappings to be stored more efficiently than letting it perform automatic rehashing as needed to grow the table. Note that using many keys with the same hashCode() is a sure way to slow down performance of any hash table. To ameliorate impact, when keys are Comparable, this class may use comparison order among keys to help break ties.
Note that this implementation is not synchronized. If multiple threads access a hash map concurrently, and at least one of the threads modifies the map structurally, it must be synchronized externally. (A structural modification is any operation that adds or deletes one or more mappings; merely changing the value associated with a key that an instance already contains is not a structural modification.) This is typically accomplished by synchronizing on some object that naturally encapsulates the map. If no such object exists, the map should be "wrapped" using the Collections.synchronizedMap method. This is best done at creation time, to prevent accidental unsynchronized access to the map:
     Map m = Collections.synchronizedMap(new HashMap(...));

```java
public class MapTest {
    public static void main(String[] args){
        HashMap map = new HashMap();
        map.put("a","zhangsan");
        map.put("b","lisi");
        map.put("c","wangwu");
        map.put("a","zhaoliu");
        System.out.println(map);
    }
}

```
>输出结果为：

```java
{a=zhaoliu, b=lisi, c=wangwu}
```
发现之前的a的value：zhang'san被之后所赋的value：zhao'liu替代。在Map中放置信息的时候，如果两个key是相同的，那么后放置的value将会替换调之前这个key的value。
### public V get(Object key)
>java.util.HashMap

Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key.
More formally, if this map contains a mapping from a key k to a value v such that (key==null ? k==null : key.equals(k)), then this method returns v; otherwise it returns null. (There can be at most one such mapping.)
A return value of null does not necessarily indicate that the map contains no mapping for the key; it's also possible that the map explicitly maps the key to null. The containsKey operation may be used to distinguish these two cases.

作用：将指定的key所映射的value返回。

```java
String value = (String)map.get("b");
System.out.println(value);
String value2 = (String)map.get("d");
System.out.println(value2);
```
仍要进行强转才能使输出结果为String类。null也可以进行强转操作。





* 在不知道key的情况下，想要将Map中的value全部返回。
## 遍历map的第一种方式

### public Set< K> keySet()
>java.util.HashMap

Returns a Set view of the keys contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa. If the map is modified while an iteration over the set is in progress (except through the iterator's own remove operation), the results of the iteration are undefined. The set supports element removal, which removes the corresponding mapping from the map, via the Iterator.remove, Set.remove, removeAll, retainAll, and clear operations. It does not support the add or addAll operations.

Returns:a set view of the keys contained in this map.

返回一个包含在map当中的Key的set视图，这个set是由map所维护的，因此对于这个map的改变将会反映到set上，反之亦然。

返回值：map中包含的key的set视图。

### public Collection< V> values()
>java.util.HashMap

Returns a Collection view of the values contained in this map. The collection is backed by the map, so changes to the map are reflected in the collection, and vice-versa. If the map is modified while an iteration over the collection is in progress (except through the iterator's own remove operation), the results of the iteration are undefined. The collection supports element removal, which removes the corresponding mapping from the map, via the Iterator.remove, Collection.remove, removeAll, retainAll and clear operations. It does not support the add or addAll operations.

返回一个包含在map当中的values的collection视图，这个collection是由map所维护的，因此对于这个map的改变将会反映到collection上，反之亦然。


**此处发现两个方法的返回类型是不一样的，其中keySet返回的是Set类型，而values返回的是Collection类型。这种现象出现的原因是set是不允许有重复元素的，而collection是允许有重复元素的，刚好对应于key和value，根据源码注解我们知道Map中key是只允许有一个且不同于其它而单独存在的，所以是与Set规则相符的，而value值却没有限制，collection类可意容纳重复的元素。**





可以做到遍历map
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Set;

public class Little5 {
    public static void main(String[] args){
        HashMap map = new HashMap();
        map.put("a","aa");
        map.put("b","bb");
        map.put("c","cc");
        map.put("d","dd");
        map.put("e","ee");
        Set set = map.keySet();
        for(Iterator iter = set.iterator();iter.hasNext();){
            //String Key = (String)iter.next();
            Object Key = iter.next();
            String value = (String)map.get(Key);
            System.out.println(Key + "=" + value);
        }
    }
}
```
> 输出结果为：
```java
a:aa
b:bb
c:cc
d:dd
e:ee
```

#### 使用Hashmap实现通过输入多个单词统计每个单词出现的次数

```java
import java.util.*;
public class MapTest2 {
    public static void main(String[] args) {
        String str;
        ArrayList arrayList = new ArrayList();
        Scanner scanner = new Scanner(System.in);
        HashMap map = new HashMap();
        while (!(str = scanner.next()).equals("end")){
            arrayList.add(str);
        }
        for (int i = 0; i < arrayList.size(); i++) {
            if (map.get(arrayList.get(i)) == null){
                map.put(arrayList.get(i),new Integer(1));
            }else {
                //map.put(arrayList.get(i),((Integer)(map.get(arrayList.get(i)))).intValue() + 1);
                Integer in = (Integer)map.get(arrayList.get(i));
                in = new Integer(in.intValue() + 1);
                map.put(arrayList.get(i),in);
            }
        }
        Set set = map.keySet();
        for (Iterator iterator = set.iterator();iterator.hasNext();){
            String key = (String)iterator.next();
            Integer value = (Integer)map.get(key);
            System.out.println(key +":"+ value);
        }
    }
}

```
> 输出结果为：
```
hello
world
hello
welcome
world
welcome
hello
end
world:2
hello:3
welcome:2
```

先调用keyset方法获取key的Set，接下来遍历Set,通过get方法获取每一个key的value,得到key和value的一对信息。

## 遍历map的第二种方式

### interface Map.Entry< K, V>
>java.util

A map entry (key-value pair). The Map.entrySet method returns a collection-view of the map, whose elements are of this class. The only way to obtain a reference to a map entry is from the iterator of this collection-view. These Map.Entry objects are valid only for the duration of the iteration; more formally, the behavior of a map entry is undefined if the backing map has been modified after the entry was returned by the iterator, except through the setValue operation on the map entry.

一个映射对key-value的entry。Map.entrySet方法返回一个元素是本类map的Collection视图，唯一一个可以将一个引用包含进map entry的方式是通过这个集合视图的迭代器。...

>Entry指的是一个内部类，即表示为在一个类的内部又定义了一个类。

如：
```java
public class A{
    public class B{

    }
}
//实例化A
A a = new A();
//实例化B
A.B b = new A.B();
```
则将类B可以表示为public A.B

* 详见Entry专栏

#### K getKey()
>java.util.Map.Entry

Returns the key corresponding to this entry.

Returns:
the key corresponding to this entry

* 将entry对应的key值返回。
#### V getValue()

>java.util.Map.Entry

Returns the value corresponding to this entry. If the mapping has been removed from the backing map (by the iterator's remove operation), the results of this call are undefined.

Returns:
the value corresponding to this entry

* 将entry对应的value值返回

### Set< Map.Entry< K, V>> entrySet()
>java.util.Map

### public Set< Map.Entry< K, V>> entrySet()
>java.util

Returns a Set view of the mappings contained in this map. The set is backed by the map, so changes to the map are reflected in the set, and vice-versa. If the map is modified while an iteration over the set is in progress (except through the iterator's own remove operation, or through the setValue operation on a map entry returned by the iterator) the results of the iteration are undefined. The set supports element removal, which removes the corresponding mapping from the map, via the Iterator.remove, Set.remove, removeAll, retainAll and clear operations. It does not support the add or addAll operations.

返回一个包含在map当中**映射**的set视图,这个set是由map所维护的，因此对于这个map的改变将会反映到set上，反之亦然。



```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class Little6 {
    public static void main(String[] args){
        HashMap map = new HashMap();
        map.put("a","aa");
        map.put("b","bb");
        map.put("c","cc");
        map.put("d","dd");
        map.put("e","ee");
        Set set = map.entrySet();
        for(Iterator iter = set.iterator();iter.hasNext();){
  //此时返回的是一个包含着map的set而接下来要使用Map.Entry类中的方法，所以要向下类型中转换为Map.Entry类
            Map.Entry entry = (Map.Entry)iter.next();  
            String key = (String)entry.getKey();
            String value = (String)entry.getValue();
            System.out.println(key + ":" + value);
        }
    }
}

```
>输出结果：

```java
a:aa
b:bb
c:cc
d:dd
e:ee
```
##### 总结：
比较两种遍历方式：第一种方式是首先拿到一个key的set(集合)，然后遍历这个key的set(集合)，分别得到key和value。第二种是首先拿到一个map的set(集合)遍历时通过得到Map.Entry对象，而其本身就已经包含了key与value的映射关系，并且是封装在Map.Entry类中的，所以再通过调用Map.Entry中的方法即可分别调用key-value。其实本质上是一样的.
