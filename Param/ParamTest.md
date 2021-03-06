# 参数传递（Param Test)
java中参数传递的原理将以代码形式进行理解性说明
## 1.非原生数据类型
```java
public class ParamTest
{
  public void changePoint(Point point)
  {
    point.x = 3;
    point.y = 4;
  }
  public static void main(String[] args)
  {
    Point point = new Point();
    pt.changePoint(point);
    System.out.println(point x);
    System.out.println(point y)
  }
}
class Point
{
  int x;
  int y;
}
```
>输出结果为：“3，4”

首先执行main方法中的代码，生成了一个Point对象并且使point引用指向它，然后调用了changePoint方法传递参数(point)输出结果x，y；从结果中可以看出changePoint方法被成功调用，那么这种方法参数为非原生数据类型的参数传递是怎样实现的？

说明：Point对象生成的时候默认值为x = 0，y = 0；当调用changPoint方法的时候point作为实参传递给了方法的形参，使形参的point也指向了实参point所指向的对象，而成功调用方法时是形参point执行的结果。当输出的时候虽然调用的是main方法中的point引用，然而对象的值却是因为changePoint方法中的形参所改变的。
**对象只有一个，但是Point的引用有两个，通过一个引用改变了对象的值，自然会反映到另外一个引用上。**

**若将changePoint方法做如下调整，那么输出结果会是什么情况?**
```java
public void changePoint(Point point)
{
  point = new Point();
  point.x = 3;
  point.y = 4;
}
```
>输出结果为："0, 0"

说明：与原来的changePoint方法唯一的不同之处是在方法中有生成了一个对象，这样的做法使得形参中的point指向的是方法体中新生成的对象，而不再是实参传递的对象，所以方法中改变的并不是main方法中的point引用所指向的对象，而是将方法中的对象做了改变。输出的是main方法中point所指向的对象。

* **对于非原生数据类型来说java传递的仍是值，其中这个值代表的是所定义的引用指向的地址。千万要注意java中没有传引用。**

### 1.2方法中的参数关系
```java
package ParamTest;

public class ParamTest3
{

    public void change(Person person,Person person2)    
    {                                                  
        person.name = "lisi";
        person2.name = "wangwu";
    }
    public static void main(String[] args)
    {
        Person person = new Person();
        person.name = "zhangsan";
        Person person2 = person;                
        ParamTest3 test = new ParamTest3();
        test.change(person,person2);            
        System.out.println(person.name);        
    }
}
class Person
{
    String name;
}
```
>输出结果为："wangwu"

说明：Person person2 = person；实际上是将person2引用也指向了与person相同的对象，而因为person和person2都指向同一个对象，所以change()方法对person.name和person2.name的变更实际上是对同一个对象的变更，所以方法中的最后一个参数变更才是对象的最终值。

* 主要想说明当多引用指向一对象的时候，方法最终对对象的改变体现在最后一个引用

## 2.原生数据类型

```java
package ParamTest;

public class ParamTest
{
    public static void main(String[] args)
    {
        C c = new C ();
        int x = 1;
        c.changeInt(x);
        System.out.println(x);       //运行结果为1，说明changeInt并没有实现
    }

}
class C
{
    public void changeInt(int a)
    {
        a++;
    }
}
//对于原生数据类型来说，方法的参数传递实际上只是传值
```
>输出结果为“1”

说明：changeInt()方法并没有返回a，对于原生数据类型来说，方法的参数传递实际上只是传值，main方法中实参传递为1使得changeInt()方法中a = 1作为形参执行方法，但是没有返回给参数a。



### 总结
对于java当中的参数传递来说，分为两种数据类型即原生数据类型和非原生数据类型，很难以理解提出非原生数据类型的时候难道说的不就是引用么，其实不然，这个“非原生数据类型”具体其实指的是对象的地址信息。java的参数传递只会传值而不会传引用，而为什么却能够达到传引用的效果？因为当对象的地址作为参数调用方法的时候，地址信息的改变是最本质的变化，其后只要是调用对应的引用返回的实际上都是它所指向的对象的信息。看似会比较迷惑，毕竟是使用引用来作为参数调用的方法，但是真正发生改变的是地址信息即“非原生数据类型”。原生数据类型则不然，当其作为参数调用方法的时候，方法的确是会执行，但是main方法中的值仍不会发生变化，传递的只是字面值而已。
**在java中进行方法的参数传递的时候，无论传递的是原生数据类型还是引用类型，参数传递方式统一是传值(pass by value)。java中没有传引用(pass by reference)的概念。**
