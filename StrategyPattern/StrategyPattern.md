# 策略模式(Strategy Pattern)
策略模式中体现了两个非常基本的面向对象设计的原则

* 封装变化的概念。

* 编程中使用接口，而不是对接口的实现。

所以策略模式是个面向接口的编程。


### 策略模式的定义

* 定义一组算法，将每个算法都封装起来，并且使他们之间可以互换。

* 策略模式使这些算法在客户端调用它们的时候能够股不影响的变化。




### 策略模式的意义

* 策略模式使开发人员能够开发出由许多可替换的部分组成的软件，并且各个部分之间是弱连接的关系。

* 弱连接的特性使软件具有更强的可扩展性，易于维护；更重要的是，它大大提高了软件的可重用性。


### 策略模式的组成
* 抽象策略角色；策略类，通常由一个接口或者抽象类实现。

* 具体策略角色：包装了相关的算法和行为。

* 环境角色：持有一个策略类的引用，最终给客户端调用的.

### 按照学生对象的成绩进行排序

>Set中可见

```java
import java.util.Comparator;
import java.util.TreeSet;
import java.util.Iterator;
public class ComparatorTest2 {
    public static void main(String[] args){
        TreeSet set = new TreeSet(new PersonComparator());
        Person p1 = new Person(10);
        Person p2 = new Person(20);
        Person p3 = new Person(30);
        Person p4 = new Person(40);
        set.add(p1);
        set.add(p2);
        set.add(p3);
        set.add(p4);
        for(Iterator i = set.iterator();i.hasNext();){
            Person p = (Person)i.next();
            System.out.println(p.score);
        }
    }
}
class Person{
    int score;
    public Person(int score){
        this.score = score;
    }
    public String toString(){
        return String.valueOf(this.score);
    }
}
class PersonComparator implements Comparator {
    @Override
    public int compare(Object arg0, Object arg1) {
        Person p1 = (Person) arg0;
        Person p2 = (Person) arg1;
        return p1.score - p2.score;
    }
}
```
>输出结果:

```java
10
20
30
40
```
其中Comparator接口起到的就是一个抽象策略角色，它仅仅定义了一个compare()方法。而这个方法起到了主要的算法和行为，并且由PersonComparator类所封装并且实现Comparator接口，所以称此类为具体策略角色。这个策略类最终是通过将类对象作为比较器返回给构造方法作为参数而被使用，而set便是那个持有的引用，所以环境角色是set。



### 策略模式的实现

* 策略模式的用意是针对一组算法，将每一个算法封装到具有共同接口的独立的类中，**从而使得他们可以互相替换。**

* 策略模式使得算法可以在不影响到客户端的情况下发生变化。**使用策略模式可以把行为和环境分割开来。**

* 环境类负责维持和查询行为类，各种算法则在具体策略中提供。**由于算法和环境独立开来，算法的修改都不会影响环境和客户端。**

之所以维持抽象的而不维持具体的是因为维持了具体的就只能传一个类的实现，而如果维护的是抽象的那么根据多态传多少种策略都可以。(父类引用指向子类对象)



### 策略模式的编写步骤

1. 对策略对象定义一个公共接口。

2. 编写策略类，该类实现了上面的公共接口。

3. 在使用策略对象的类中保存一个对策略对象的引用。

4. 在使用策略对象的类中，实现对策略对象的set和get方法(注入)或者使用构造方法完成赋值。

### 实现对数值的运算

1.对策略对象定义一个公共接口。
```java
package Test;

public interface Strategy {
    public int Calculate(int a, int b);

}

```

2.编写策略类，该类实现了上面的公共接口。
```java
package Test;

public class AddStrategy implements Strategy{
    @Override
    public int Calculate(int a, int b){
        return a + b;
    }
}

```
以及：
```java
package Test;

public class SubstractStrategy implements Strategy{
    @Override
    public int Calculate(int a, int b) {
        return a - b;
    }
}

```
3.在使用策略对象的类中保存一个对策略对象的引用。

4.在使用策略对象的类中，实现对策略对象的set和get方法(注入)或者使用构造方法完成赋值运用这种做法可以方便的取不同的策略类，而不用重新new环境变量，甚至可以像如下获得更简便的操作。

* 当有成员变量想要使用set和get方法的时候可以使用ALT + INS 键完成两个方法的定义.

* 当我们想要看一个方法的具体实现类而不是接口的时候，我们可以使用CTRL + N快捷键的方式进行查找。在源代码中也可以使用，

```java
package Test;

public class Environment {
    private Strategy strategy;
    public Environment(strategy){
      this.strategy = strategy;
    }
    /*public Environment(){

     */}
    public void setStrategy(Strategy strategy){
        this.strategy = strategy;
    }

    /*public Strategy getStrategy() {
        return this.strategy;
    }*/
    public int Calculate(int a, int b){
        return strategy.Calculate(a, b);
    }
}

```
客户端：
```java
package Test;

public class Client {
    public static void main(String[] args){
        AddStrategy addStrategy = new AddStrategy();
        Environment environment = new Environment(addStrategy);
        //Environment environment = new Environment();
        //environment.setStrategy(addStrategy);
        System.out.println(environment.Calculate(3,4));
        SubstractStrategy substractStrategy = new SubstractStrategy();
        environment.setStrategy(substractStrategy);
        System.out.println(environment.Calculate(3,4));
    }
}

```

>输出结果：

```java
7
-1
```

### 策略模式的缺点

* 客户端必须要知道所有的策略类，并自行决定使用哪一个策略类。

* 造成了很多的策略类，维护的代价很高。

>解决方案
* 采用工厂方法(也属一种设计模式)
