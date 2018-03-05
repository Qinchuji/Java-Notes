# 多态(Polymorphism)
多态建立在封装与继承之上的概念，子类就是父类，所以说多态实际上就是父类型的引用可以指向子类型的对象。

### 强制类型转换
一共有两种类型的强制类型转换：(a)向上类型转换：将子类型转换为父类型，对于向上类型转换，不需要显式指定。(b)向下类型转换：将父类型转换为子类型，对于向下类型转换，必须要显式指定(必须要使用强制类型转换)。条件是父类型的引用指的是子类型的对象。
```java
public class PolyTest2 {
    public static void main(String[] args) {
/*      向下类型转换
        Animal a = new Dog();
        Dog dog = (Dog)a;
        dog.sing();            运行结果：dog is singing
        */
/*
        Animal b = (Dog)dog;
        b.sing();              运行结果：dog is singing
        */
/*
        Animal c = new Cat();
        Cat cat = (Cat)c;
        cat.sing();            运行结果：cat is singing
        */
/*      Dog d = (Dog)c;        运行结果:类型转换异常，猫不能强转成狗
        d.sing();
        */
/*      Animal animal = new Cat();
        Animal animal2 = new Animal();
        animal2 = animal;
        animal2.sing();         运行结果:cat is singing
        */
/*
        Animal animal = new Cat();
        Animal animal2 = new Animal();
        animal = animal2;
        animal.sing();          运行结果:animal is singing
        */
/*
        Cat cat = new Cat();
        Animal animal = cat;
        animal.sing();          运行结果:cat is singing
        */
/*
        Animal animal = new Animal();
        Cat cat = (Cat)animal;  运行结果:类型转换异常
        */
        Cat cat = new Cat();
        Animal animal = cat;
        animal.sing();
    }
}
class Animal{
    public void sing(){
        System.out.println("animal is singing");
    }
}
class Dog extends Animal{
    public void sing(){
        System.out.println("dog is singing");
    }
}
class Cat extends Animal{
    public void sing(){
        System.out.println("cat is singing");
    }
}
```
### 强转的意义
类型转换的意义：因为对于父类有的方法，子类可以添加，所以当父类型的子类对象想要使用子类型中定义的方法时，强转是唯一的方式实现，即转成子类型的对象。

```java
public class PolyTest3 {
    public void run(Car car){
        car.run();
    }

    public static void main(String[] args) {
        PolyTest3 test = new PolyTest3();
        Car car = new BMW();
        test.run(car);
        Car car2 = new QQ();
        test.run(car2);

    }
}
class Car{
    public void run(){
        System.out.println("car is running");
    }
}
class BMW extends Car{
    public void run(){
        System.out.println("BMW is running");
    }
}
class QQ extends Car{
    public void run(){
        System.out.println("QQ is running");
    }
}
```
>输出结果为：
>BMW is running
>QQ is running

父类中的run方法体现了多态在应用中带来的好处，无论有多少种车都可以通过多态使用到此方法。

### 使用上的条件

```java
public class PolyTest {
    public static void main(String[] args) {
        Parent parent = new Parent();
        parent.sing();
        Parent p = new Child();
        p.sing();  //1、
    }
}
class Parent{
    public void sing(){     
        System.out.println("parent is singing");
    }
}
class Child extends Parent{
    public void sing(){
        System.out.println("child is singing");
    }
}

```
>输出结果为：

>parent is singing

>child is singing

1、去掉父类sing()方法运行p.sing()会发现报错，运行时父类中找不到sing()方法。因为引用p是父类型的，什么类型的就要用什么方法

Parent p = new Child();当时用多态方式调用方法时，首先检查父类中是否有sing()方法，如果没有则编译错误，因为引用的类型决定其调用什么方法。如果有，再去调用子类的sing()方法。
