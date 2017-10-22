# static关键字
**static可以用于修饰属性，也可以用于修饰方法还可以用来修饰类**
## 一、static修饰属性
```java
package place;

public class Statictest4
{
	public static void main(String[] args)
	{
		Mystatic statictest = new Mystatic();
		Mystatic statictest2 = new Mystatic();
		Mystatic.a = 10;
		System.out.println(statictest2.a);
	}

}
class Mystatic
{
	static int a;
}
```
>**输出结果为“10”**
* **无论一个类生成了多少个对象，所有这些对象共同使用唯一一份静态的成员变量，一个对象对该静态成员变量进行了修改，其他对象的改静态成员变量的值也会随之发生变化。**

#### 类名.成员变量(推荐使用)
```java
Mystatic mystatic = new Mystatic();
Mystatic.a = 10;
System.out.println(mystatic.a)
```
* **假如没有static关键字的时候，想要访问成员变量，只能通过生成一个对象来访问。但当该类为静态类时便可通过类名来直接调用，因为静态类中的成员变量不属于任何一个对象，而是被所有对象共同拥有的一份。**

*简单理解:为什么一个静态的成员变量就可以直接通过类名来访问，而对于一个非静态成员变量就必须生成一个对象来访问？*

**因为对于非静态成员变量来说，每个对象里都有一份。假如说直接使用了类名访问成员变量，那么访问的值是具体哪一个对象的并不能确定。而静态成员变量被访问的时候，因为所有对象都拥有一份，所以一旦访问，访问的也都是同一个。**

## 二、static修饰方法
**static修饰的方法叫做静态方法。对于静态方法来说，仍推荐类名.方法名的方式来访问。**

**接下来主要学习static在继承中的特点**
```java
package place;

public class Statictest
{
	public static void main(String[] args)
	{
		People2 p = new People2();
		p.address();     //static的隐藏；people2的调用
		People q = new People2();
		q.address();      //static的隐藏;people的调用
	}

}
class People
{
	public static void address()
	{
		System.out.println("Beijing");
	}
}
class People2 extends People
{
	public static void address()
	{
		System.out.println("Shanghai");
	}
}
```
>**输出结果为“Shanghai”“Beijing”**
* **静态方法只能继承，不能重写。相对于重写的操作称为隐藏。当调用互为隐藏关系的方法时，调用的为父类还是子类的隐藏方法取决于实例的类型。**

## 三、静态方法中成员变量的访问
**静态方法中访问成员变量的规矩**
```java
package place;

public class Statictest3
{
	public static void main(String[] args)
	{
		W.change();
		System.out.println(W.a);
	}
}
class W
{
	static int a = 10;                    
	public static void change()           //静态方法中只能访问静态变量，非静态方法静态和非静态变量都能访问   
	{
		a ++;                               //4、this.a++
	}
}

```
>**输出结果为11**
1. 不加static是编译不成功的。  其原因是：如果该成员变量是非静态成员变量的话当你通过类名去调用change方法的时候你不知道改变的是哪个对象的a。
2. 不能在静态方法中访问非静态成员变量；可以在静态方法中访问静态的成员变量；可以在非静态方法中访问静态的成员变量.
3. 静态的只能访问静态的，非静态的可以访问一切。
4. 不能在静态方法中使用this关键字。因为this表示对当前对象的引用，而静态方法可以直接通过类名访问，无需生成对象。那么this指的是哪一个对象就无从得知了。（注释4、)

## 四、静态代码块（static block）
**静态代码块的作用也是完成一些初始化工作。**
```java
package place;

public class Statictest2
{
	public static void main(String[] args)
	{
	  /*new P();
        new P();*/             //输出结果是一次static block两次p constructor;原因是当class文件加载到java虚拟机上面的时候，静态代码块就已经在执行了而构造函数是先生成对象再调用的
	    new S();
	    new S();                    //类似
	}
}

class P
{
	static
	{
		System.out.println("p static block");  //静态代码块（放在类里面，与方法和属性平级）
	}
	public P()
	{
		System.out.println("P constructor"); //加了这一构造方法输出发现静态代码块优先于构造方法输出
	}
}
class Q extends P
{
	static
	{
		System.out.println("Q static block");
	}
	public Q()
	{
		System.out.println("q constructor");
	}
}
class S extends Q
{
	static
	{
		System.out.println("S static block");
	}
	public S()
	{
		System.out.println("S constructor");
	}
}
```
>**输出结果为**
```java
p static block
Q static block
S static block
P constructor
q constructor
S constructor
P constructor
q constructor
S constructor
```
1. 首先执行静态代码块,然后执行构造方法。静态代码块在类被加载的时候执行，而构造方法是在生成对象的时候执行的。即要想调用某个类来生成对象，首先需要将类加载到java虚拟机（JVM）上，然后由JVM加载这个类生成对象。
2. 类的静态代码块只会执行一次，并且是在类被加载的时候执行的。因为每个类只会被加载一次，所以静态代码块也就只会被执行一次；而构造方法则不然，每次生成一个对象的时候都会调用类的构造方法，所以new一次就会调用一次构造方法。
3. 如果继承体系中既有构造方法，又有静态代码块，那么首先执行最顶层的类的静态代码块，一直执行到最底层类的静态代码块，然后再去执行最顶层类的构造方法，一直执行到最底层类的构造方法.
4. **每个类的静态代码块只会执行一次**
