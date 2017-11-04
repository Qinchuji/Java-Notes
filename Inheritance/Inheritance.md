# 继承（Inheritance）
java是单继承的，意味着一个类只能从另一个类继承(被继承的类叫做父类【基类.base class】继承的类叫做子类)，java中的继承使用extends关键字。
```java
public class Child extends Parent
{
  public Child()
  {
    System.out.println("child");
    public static void main(String[] args)
    {
      Child child = new Child();
    }
  }
  class Parent
  {
    public Parent()
    {
      System.out.println("parent");
    }
  }
}
```
>输出结果:

```java
parent
child
```
#### 说明：
执行main方法，生成child对象时首先开辟内存空间，然后通过调用Child类的不带参数的构造方法初始化对象，最后将地址返回给引用。当通过构造方法对对象进行初始化的时候，发现Child类是继承自Parent类的，那么就会去寻找**父类的不带参数的构造方法**。即先去执行父类的构造方法然后执行子类的构造方法。

当生成子类对象的时候，java默认首先调用父类的不带参数的构造方法，然后才执行该构造方法，生成父类对象，接下来再取调用子类的构造方法，生成子类对象。【要想生成子类对象，就要生成父类对象】。

* 那么生成子类对象时却想使用父类除了不带参数的构造方法以外的构造方法怎么办?

## super()关键字的第一种用法
若不想调用父类不带参数的构造方法或者想要调用父类参数为其他类型的构造方法则使用super()

**super()表示对父类对象的引用。**

```java
public class Child extends Parent
{
  public Child()
  {
     //显示的调用父类特定的带整型参数的构造方法。
    super(1);
    System.out.println("child");
    public static void main(String[] args)
    {
      Child child = new Child();
    }
  }
  class Parent
  {
    public Parent()
    {
      System.out.println("parent1");
    }
    public Parent(int i)
    {
      System.out.println("parent2");
    }  
  }
}
```
>输出结果：

```java
parent2
child
```
super()括号中的参数与父类中用户想要调用的构造方法参数需一致。

如果子类使用super()显示的调用父类特定的某个参数的构造方法，那么在执行的时候就会寻找与super()所对应的构造方法而不会再去用默认的方式寻找父类的不带参数的构造方法。

**注意：若需要使用super()关键字，那么必须在第一行中出现。**

### 关于super()关键字必须在构造方法中作为第一条语句出现的原因：
子类与父类存在继承关系，而继承就意味着得到了父类中所有的成员变量和方法，所以为了保证父类在被继承的时侯得到正确的初始化引入了super()关键字。在子类的构造方法中，如果没有显示使用super()并且第一行没有使用this()调用子类的构造函数，那么编译器也会自动的在第一行补齐super()来调用父类的默认构造函数，如果父类没有默认的构造方法就会报错。

* super()在第一行的原因是子类有可能访问了父类对象, 比如在构造函数中使用父类对象的成员函数和变量, 在成员初始化使用了父类, 在代码块中使用了父类等, 所以为保证在子类可以访问父类对象之前要完成对父类对象的初始化。
* this()在第一行的原因就是: 为保证父类对象初始化的唯一性. 我们假设一种情况, 类B是类A的子类, 如果this()可以在构造函数的任意行使用, 那么会出现什么情况呢? 首先程序运行到构造函数B()的第一行, 发现没有调用this()和super(), 就自动在第一行补齐了super() , 完成了对父类对象的初始化, 然后返回子类的构造函数继续执行, 当运行到构造函数B()的"this(2) ;"时, 调用B类对象的B(int) 构造函数, 在B(int)中, 还会对父类对象再次初始化! 这就造成了对资源的浪费, 当然也有可能造成某些意想不到的结果, 不管怎样, 总之是不合理的, 所以this() 不能出现在除第一行以外的其他行!




## 关于父类的三点：
(a)父类有的，子类也有。

(b)父类没有的，子类可以增加。

(c)父类有的，子类可以改变。

### 关于继承的注意事项
(a)构造方法不能被继承。

(b)方法和属性可以被继承。

(c)子类的构造方法隐式的调用父类不带参数的构造方法.

(d)当父类没有不带参数的构造方法时，子类需要super()显式的调用父类的构造方法，super指的是对父类的引用。




## 重写(Override)
```java
class Animal
{
  public void run()
  {
    System.out.println("animal is running");
  }
}
class Dog extends Animal
{

}
```
此时生成dog对象并调用dog.run输出结果为animal is running。这说明子类继承了父类的方法。下面将Dog类做更改。
```java
class Dog extens Animal
{
  public void run()
  {
    System.out.println("dog is running");
  }
}
```
此时再次调用dog.run输出结果为dog is running。

**方法重写：又叫作覆写。子类与父类的方法返回类型一样、方法名称一样，参数一样，这样我们说子类与父类的方法构成了重写关系。**

注：方法重写与方法重载之间的关系：重载发生在同一个类内部的两个或多个方法。重写发生在父类与子类之间。

* 重载：平行关系。

* 重写：上下关系。

## super()关键字的第二种用法
当子类的方法将父类的方法进行了重写，但是用户仍想调用父类的方法时，可以用super关键字。
```java
lass Animal
{
  public void run()
  {
    System.out.println("animal is running");
  }
}
class Dog extens Animal
{
  public void run()
  {
    super.run();
    System.out.println("dog is running");
  }
}
```
当两个方法形成重写关系时，可以在子类方法中通过super.run形式调用父类的run()方法，其中super.run()不必放在第一行语句，因为此时父类对象已经构造完毕，先调用父类的run()方法还是先调用子类的run()方法是根据程序的逻辑决定的。
