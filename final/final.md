# final关键字
>**final关键字：final可以修饰类，方法，属性。**

>**final修饰类：当一个类被final所修饰时，表示该类是一个终态类，即不能被继承。**

>**final修饰方法：当一个方法被final所修饰时，表示该方法是一个终态方法即不能被改写。**

>**final修饰原生数据类型：当final修饰了一个原生数据类型时，表示该原生数据类型的值不能发生变化。**

#### 重点学习final修饰属性的特殊性以及使用陷阱。
## final修饰引用数据类型

```java
package p;

public class Finaltest
{
	public static void main(String[] args)
	{
		People people = new People();
    //people.address = new address                  编译不成功 违背了1、将引用所指向的地址改变
		people.address.name = "Shanghai";               //2、
		System.out.println(people.address.name);        //final引用的地址不能发生变化
	}
}
class People
{
	final Address address = new Address();
}
class Address
{
	String name = "Beijing";
}
```
>**输出结果为“Shanghai”**

1. class people中final的是引用类型的数据，表示的是该引用只能指向刚开始所指向的地址，并且地址不可发生变化

2. 表示将引用所指向的地址中的属性改变。这是可行的，因为只有address引用是final的，而地址中的值是可以进行改变的。

3. 当final修饰一个引用类型时，表示该引用类型不能再指向其他对象了，但该引用所指向的对象的内容是可以发生变化的。

## final类型成员变量的赋值
**对于final类型的成员变量，一般来说有两种赋初值方式。**

（a）在声明final类型的成员变量时就赋上初值。

（b）在声明final类型的成员变量时不赋初值。
### 声明时赋初值
**是否可以直接声明final类型的变量a**
```java
public class Finaltest
{
  final int a;
}
```
>**输出结果为：编译不成功。可能尚未初始化变量。**

```java
public class Finaltest
{
  final int a = 0;
}
```
**对于一般整型成员变量来说，如果没有赋初值的话，初值等于零。但是对于final类型的变量来说的话就必须显示的赋初值。因为如果一般的成员变量没有赋初值的话之后的代码中也可以根据需要赋值，而对于final修饰的属性来说就没有机会了，所以例如此例中的a，即使想赋初值为整型默认初值，也要显示的进行一次赋值操作。**
* **当声明了final类型的成员变量时就赋了初值，其他地方就不要再去重新赋值了（不然成了改写）**

### 声明时不赋初值
```java
package p;

public class Finaltest2
{
	final int a;
	public Finaltest2()
	{
		a = 0;
	}                      //final的赋值一定要确定
	public Finaltest2(int a)
	{
		this.a = 0;          //在声明final类型的成员变量时不赋初值，但在类的所有构造方法中都为其赋上初值
	}
}

```
**一个程序中构造方法是一定会执行的，所以对于开始声明final类型时未赋初值的变量来说，在后续的程序结构中唯一除了声明时赋值以外的机会就只有在构造函数中赋值了。**

**在声明final类型的成员变量时不赋初值，但在类中声明的所有构造方法中都为其赋上初值。**

* **仍要显示的赋初值。**
