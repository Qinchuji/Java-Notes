# HashSet底层实现

## HashSet类的声明
```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    static final long serialVersionUID = -5024744406713321676L;

    private transient HashMap<E,Object> map;

    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();
    .
    .
    .
```
## HashSet的构造方法
```java
   /**
    * Constructs a new, empty set; the backing <tt>HashMap</tt> instance has
    * default initial capacity (16) and load factor (0.75).
    */
        public HashSet() {
            map = new HashMap<>();
        }
```
**构造一个新的，空的集合**

可以看到构造方法中只生成了一个新的HashMap对象，说明HashSet底层是使用HashMap实现的。
```
private transient HashMap<E,Object> map;
```
属性中可以发现是接收了两个HashMap参数的对象，但既然是两个参数，那是否是和HashSet有矛盾。
## add方法
```java
/**
     * Adds the specified element to this set if it is not already present.
     * More formally, adds the specified element <tt>e</tt> to this set if
     * this set contains no element <tt>e2</tt> such that
     * <tt>(e==null&nbsp;?&nbsp;e2==null&nbsp;:&nbsp;e.equals(e2))</tt>.
     * If this set already contains the element, the call leaves the set
     * unchanged and returns <tt>false</tt>.
     *
     * @param e element to be added to this set
     * @return <tt>true</tt> if this set did not already contain the specified
     * element
     */
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
```
如果集合中不存在，则将指定的元素添加到该集合中。更正式地，将指定的元素e添加到这个集合中如果这个集合中没有e2这样的元素符合(e==null ? e2==null : e.equals(e2))这样的条件。如果该集合已经包含元素，则调用将保持不变，并返回false。

解释中只提到了对e的处理，发现另一个参数是个常量。
```java
// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
```
在底层映射中与对象关联的虚拟值。

属性中发现PRESENT常量，当使用add方法将对象添加到Set当中时，实际上是将该对象作为底层所维护的Map对象的key，而value则都是同一个Object对象即PRESENT常量。

有了这一基本信息，观察HashSet的方法发现基本都是通过HashMap来进行的。

# HashMap底层实现
## HashMap类一些属性

```java
/**
 * The load factor used when none specified in constructor.
 */
static final float DEFAULT_LOAD_FACTOR = 0.75f;
```
```java
/**
    * The default initial capacity - MUST be a power of two.
    */
   static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
```
**默认初始化容量——一定要是2的幂。**

注：<< 是位运算符，表示1的二进制往左移两位所得十进制为其结果。所以1 << 4即：16。

## HashMap的构造方法
HashMap拥有四个构造方法，实现的功能都是构造了一个带初始容量和负载因子的空HashMap。
```
public HashMap()
public HashMap(int initialCapacity)
public HashMap(int initialCapacity, float loadFactor)
public HashMap(Map<? extends K, ? extends V> m)
```
其它构造方法最终调用的都是第三个构造方法源码。
```java
/**
     * Constructs an empty <tt>HashMap</tt> with the specified initial
     * capacity and load factor.
     *
     * @param  initialCapacity the initial capacity
     * @param  loadFactor      the load factor
     * @throws IllegalArgumentException if the initial capacity is negative
     *         or the load factor is nonpositive
     */
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }
```



static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}



static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }

    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }

    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
