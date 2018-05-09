# 泛型(Generics)
java 泛型是java SE 1.5的新特性，泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。
>泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。

>泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。

泛型：变量的类型参数化


## 泛型的使用
泛型的类型参数使用大写形式，且比较短，一般一个字母，这是很常见的。在java库中，使用变量E表示集合的元素类型。

类别定义时的逻辑完全一样，只是里面成员变量的类型不同
```java
public class GenericTest<T> {
    private T foo;

    public T getFoo() {
        return foo;
    }

    public void setFoo(T foo) {
        this.foo = foo;
    }

    public static void main(String[] args) {
        GenericTest<Boolean> foo1 = new GenericTest<Boolean>();
        GenericTest<Integer> foo2 = new GenericTest<Integer>();

        foo1.setFoo(new Boolean(false));
        foo2.setFoo(new Integer(23));

        Boolean b = foo1.getFoo();
        Integer i = foo2.getFoo();

        System.out.println(b);
        System.out.println(i);

        //foo1 = foo2; 类型不兼容

        GenericTest foo3 = new GenericTest();
        foo3.setFoo("hello");
        String str = (String)foo3.getFoo();
        System.out.println(str);
    }
}
```
>输出结果：
```
false
23
hello
```
发现new对象的时候也可以不使用泛型但是取出就需要使用强制类型转换。
### 两个类型参数的泛型
```java
public class GenericTest<T1 , T2> {
    private T1 foo1;

    private T2 foo2;

    public T1 getFoo1() {
        return foo1;
    }

    public void setFoo1(T1 foo1) {
        this.foo1 = foo1;
    }

    public void setFoo2(T2 foo2) {
        this.foo2 = foo2;
    }

    public T2 getFoo2() {
        return foo2;
    }

    public static void main(String[] args) {
        GenericTest<Integer, Boolean> foo = new GenericTest<Integer, Boolean>();

        foo.setFoo1(new Integer(-20));
        foo.setFoo2(new Boolean(false));

        Integer in = foo.getFoo1();
        Boolean bo = foo.getFoo2();

        System.out.println(in);
        System.out.println(bo);
    }

}
```
>输出结果：
```java
-20
false
```
在泛型中可以声明多个类型参数。为了指定两个或更多个类型参数，只需要使用逗号分隔参数列表即可。

### 字符串数组
```java
public class GenericTest<T> {
    private T[] fooArray;

    public T[] getFooArray() {
        return fooArray;
    }

    public void setFooArray(T[] fooArray) {
        this.fooArray = fooArray;
    }

    public static void main(String[] args) {
        GenericTest<String> foo = new GenericTest<String>();
        String[] str1 = {"hello","world","welcome"};
        String[] str2 = null;
        foo.setFooArray(str1);
        str2 = foo.getFooArray();

        for (int i = 0; i < str2.length; i++) {

            System.out.println(str2[i]);
        }
    }
}
```
>输出结果：
```java
hello
world
welcome
```

### 模仿Collection源码
```java
public class SimpleCollection<T> {
    private T[] objArray;

    private int index = 0;


    //private transient Object[] elementData;
    public SimpleCollection(){
        objArray = (T[]) new Object[10];
    }

    public SimpleCollection(int capacity){
        objArray = (T[])new Object[capacity];
    }

    public void add(T t){
        objArray[index++] = t;
    }

    public int getLength(){
        return this.index;
    }

    /*public E get(int index){
        RangeCheck(index);
        return (E) elementData[index];
    }*/
    public T get(int i){
        return objArray[i];
    }

    public static void main(String[] args) {
        SimpleCollection<Integer> c = new SimpleCollection<Integer>();
        for (int i = 0; i < 10; i++) {
         c.add(new Integer(i));
        }
        for (int i = 0; i < 10; i++) {
            Integer in = c.get(i);
            System.out.print(in + " ");
        }
    }
}
```
>输出结果：
```
0 1 2 3 4 5 6 7 8 9 
```
源码中定义的是一个Object类型数组并在get方法中进行泛型类别的强行类型转换，而这里在刚开始就定义的是一个进行了泛型类别的强制类型转换的Object类型数组。

### 泛型的泛型
```java
public class WrapperFoo<T> {
    private GenericFoo<T> foo;

    public GenericFoo<T> getFoo() {
        return foo;
    }

    public void setFoo(GenericFoo<T> foo) {
        this.foo = foo;
    }

    public static void main(String[] args) {
        GenericFoo<Integer> foo = new GenericFoo<Integer>();
        foo.setFoo(new Integer(-10));

        WrapperFoo<Integer> wrapperFoo = new WrapperFoo<Integer>();
        wrapperFoo.setFoo(foo);

        GenericFoo<Integer> g = wrapperFoo.getFoo();
        System.out.println(g.getFoo());
    }
}
class GenericFoo<T>{
    private T foo;

    public T getFoo() {
        return foo;
    }

    public void setFoo(T foo) {
        this.foo = foo;
    }
}
```
>输出结果：
```
-10
```
## 用泛型使用集合
#### ArrayList
```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ArrayListTest2 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();

        list.add("a");
        list.add("b");
        list.add("c");

        for (int i = 0; i < list.size(); i++) {
            String value = list.get(i);
            System.out.print(value);
        }

        //迭代器
        for (Iterator<String> iterator = list.iterator();iterator.hasNext();) {
            String value = iterator.next();
            System.out.print(value);
        }
    }
}

```

#### 泛型存放自定义对象(HashSet)
```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class SetTest {
    public static void main(String[] args) {
        Set<String> set = new HashSet<String>();

        set.add("aa");
        set.add("bb");
        set.add("cc");

        for (Iterator<String> iterator = set.iterator();iterator.hasNext();){
            String value = iterator.next();
            System.out.println(value);
        }
        System.out.println("------------------");

        Set<People> set1 = new HashSet<People>();
        set1.add(new People("Zhangsan",20,"Beijing"));
        set1.add(new People("Lisi",20,"Shanghai"));
        set1.add(new People("Wangwu",30,"Wuhan"));

        for (Iterator<People> iterator = set1.iterator();iterator.hasNext();){

            People people = iterator.next();

            String name = people.getName();
            int age = people.getAge();
            String address = people.getAddress();

            System.out.println(name + ' ' + age + ' ' + address);
        }
    }
}
class People{
    private String name;

    private int age;

    private String address;

    public People(String name,int age,String address){
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof People)) return false;

        People people = (People) o;

        if (age != people.age) return false;
        if (!name.equals(people.name)) return false;
        return address.equals(people.address);
    }

    @Override
    public int hashCode() {
        int result = name.hashCode();
        result = 31 * result + age;
        result = 31 * result + address.hashCode();
        return result;
    }
}
```
### HashMap
```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class MapTest {
    public static void main(String[] args) {
        Map<String,String> map = new HashMap<String, String>();

        map.put("a" ,"aa");
        map.put("b" ,"bb");
        map.put("c" ,"cc");

        Set<String> set = map.keySet();

        for(Iterator<String> iterator = set.iterator();iterator.hasNext();){
            String key = iterator.next();
            String value = map.get(key);
            System.out.println(key + ":" + value);
        }

        System.out.println("----------------");

        //调用一个entrySet方法获得一个Map.Entry对象，Map.Entry对象里面封装了key和value，说明有两个泛型

        //entrySet方法会返回一个set对象，set里面是一个个Map.Entry对象，Map.Entry对象里面封装了key和value，都是String类型的
        Set<Map.Entry<String,String>> set2 = map.entrySet();
        for (Iterator<Map.Entry<String,String >> iterator = set2.iterator();iterator.hasNext();){
            Map.Entry<String,String> entry = iterator.next();

            String key = entry.getKey();
            String value = entry.getValue();

            System.out.println(key + ":" + value);
        }
    }
}

```

## 泛型的优势
1. 类型安全。 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。

2. 消除强制类型转换。 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。

这可以消除代码中的强制类型转换，同时获得一个附加的类型检查层，该检查层可以防止有人将错误类型的键或值保存在集合中。这就是泛型所做的工作。**编译时类型的安全和运行时更小地抛出ClassCastExceptions的可能。**