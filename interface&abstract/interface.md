# 接口
抽象类是从多个类中抽象出来的模板，如果将这种抽象进行得更彻底，则可以提炼出一种更加特殊的“抽象类”————接口，接口里不能包含普通方法，接口里的所有方法都是抽象方法。java8对接口进行了改进，允许在接口中定义默认方法，默认方法可以提供方法实现。**类是一种具体实现体。而接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。可见，接口是从多个相似类中抽象出来的规范，接口不提供任何实现。接口体现的是规范和实现分离的而设计哲学。**
## 接口的用法
1. 可以将接口看作是特殊的抽象类(抽象类中可以有具体方法，也可以有抽象方法，而接口中只能有抽象方法，不能有具体方法)。
2. 类可以实现接口。是先用关键字implements表示，代表了某个类实现了某个接口。
3. 一个类实现了某个接口，那么该类必须要实现接口中声明的所有方法。如果该类是个抽象类，那么就无需实现接口中的方法了。
4. Java是单继承的，也就是说某个类只能有唯一一个父类。一个类可以实现多个接口，多个接口之间使用逗号分隔。

```java
public interface Myinterface {
    public void output();

    public static void main(String[] args) {
        MyClass a = new MyClass();
        a.output();
        a.output2();
    }
}
interface  MyInterface2{
    public void output2();
}
class MyParent{
    public void output3(){
        System.out.println("output3");
    }
}
class MyClass extends MyParent implements Myinterface,MyInterface2{
    public void output(){
        System.out.println("output");
    }
    public void output2(){
        System.out.println("output2");
    }
}
```
>输出结果：

```java
output
output2

```

## 多态的使用

接口类型的引用指向了实现该接口的类的实例也称之为多态。关于接口与实现接口的类之间的强制类型转换方式与父类和子类之间的强制类型转换方式完全一样。

```java
public class InterfaceTest {
    public static void main(String[] args) {
//        向上类型转换
        BB bb = new BB();
        AA aa = bb;
//        向下类型转换
        AA cc = new BB();
        BB dd = (BB)cc;
        bb.output();
    }
}
interface AA{
    public void output();
}
class BB implements AA{
    public void output(){
        System.out.println("BB");
    }
}
```
