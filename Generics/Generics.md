# 泛型(Generics)
编译时类型的安全和运行时更小地抛出ClassCastExceptions的可能。
## 
类别定义时的逻辑完全一样，只是里面成员变量的类型不同

如果需要多个类似的类，需要定义多个文件，不同的只是变量的类别，而逻辑是完全一样的

泛型：变量的类型参数化

泛型的类型参数使用大写形式，且比较短，一般一个字母，这是很常见的。在java库中，使用变量E表示集合的元素类型。

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
new对象的时候可以不使用泛型但取出的时候就需要使用强制类型转换了。
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

### 泛型数组
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
### 泛型数组
>模仿Collection源码
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

泛型的泛型


用泛型使用集合