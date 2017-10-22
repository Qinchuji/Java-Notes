# 单例模式（Singleton）

## 一、赋值并在方法中返回
```java
package p;

public class SingletonTest
{
  public static void main(String[] args)
  {
    Singleton singleton = Singleton.getInstance();
    Singleton singleton2 = Singleton.getInstance();
    System.out.println(singleton == singleton2);       //10、
  }
}
class Singleton
{
  public static Singleton getInstance()                //6、
  {
//  return new Singleton()                                4、
    return singleton;                                    //8、
  }
  private static Singleton singleton = new Singleton();  //8、9、
  private Singleton()                                  //3、私有构造方法，防止被实例化
  {

  }
}
```
>**输出结果为“true”**

>**表示singleton，和singleton2引用指向同一个对象。**

### 思路历程
**1. 要求实现一个类*只能*生成唯一一个实例的功能即在调用类的时候不管怎样都只能生成一个实例。**

**2. 明确new关键字在生成对象的时候完成三件事：**
   * 为对象开辟内存空间
   * **调用类的构造方法**
   * 将生成的对象的地址返回

**3. 只要生成对象就会调用构造函数，为防止反复被调用生成多个实例，所以使用private访问修饰符，如此一来私有的构造方法就只能在类内使用，防止实例化。**

**4. 因为定义了私有的构造方法所以表示只能在类的内部生成实例，因此想在main方法中直接new一个对象是不可取的。所以既然只能在类的内部生成对象，想要被外界调用就只能将生成的对象作为一个外部可调用方法的返回值。**

**5. 很明显即使方法中实现了将生成对象作为返回值的功能，main函数中仍无法调用该方法。（有对象才能调用方法）。**

**6. static关键字的引入。static修饰方法：static修饰的方法叫做静态方法。对于静态方法来说，可以直接调用方法来访问。**

**7. 这样一来就能够调用生成对象的方法了，可是这并不符合单例的特点。因为到这的调整只是可以来访问方法生成对象，却不能规定只能生成唯一一个对象。（调用一次便可生成一个）。**

**8.为了使每次访问方法时能够实现每访问一次都只生成唯一一个对象，可以使方法中生成对象的功能调整为调用类内生成完毕的对象的功能，并且在类内直接生成的对象。如此一来，不管调用多少次方法返回的都是同一个对象的引用。**

**9. 静态方法只能访问静态成员变量，所以要声明生成的对象为静态。**

**10. 判断是否指向同一地址**

## 二、不赋值判断是否为空
### 一实现
```java
package p;

public class SingletonTest
{
  public static void main(String[] args)
  {
    Singleton singleton = Singleton.getInstance();
    Singleton singleton2 = Singleton.getInstance();
    System.out.println(singleton == singleton2);
  }
}
class Singleton
{
  private static Singleton singleton;
  private Singleton()
  {

  }
  private static Singleton getInstance()
  {
    if(singleton == null)
    {
      singleton = new Singleton();
    }
    return singleton;
  }
}
```
>**输出结果为：“true”**
1. **引用值变量默认值为空，表示该引用未指向任何一个对象**
2. **未学多线程时并看不出太大区别，多线程环境下有不为单例的情况**

## 二实现
>http://blog.csdn.net/zhangerqing/article/details/8194653

**在Java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。这样的模式有几个好处：**
1. 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。
2. 省去了new操作符，降低了系统内存的使用频率，减轻GC压力。
3. 有些类如交易所的核心交易引擎，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。


* 关于多线程可能会遇到的问题：需要使用内部类来解决


```java
package p;

public class Singletontest2
{
    private static Singletontest2 instance = null;

    private Singletontest2()
    {

    }

   /*public static SingleTon getInstance()
     * {
        if (instance == null)
        {
            instance = new SingleTon();
        }
        return instance;
    }*/
  private static class SingletonFactory {
      private static Singletontest2 instance = new Singletontest2();
  }

  /* 获取实例 */
  public static Singletontest2 getInstance() {
      if (instance == null) {
          instance = SingletonFactory.instance;
      }
      return instance;
  }

    public Object readResolve(){
        return instance;
    }


}
```
