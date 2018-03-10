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
