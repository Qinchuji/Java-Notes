# equals方法
>**该方法定义在Object类中，因此java中的每一个类都具有该方法。而在Object类子类之中的一些类会根据自己类的特性适当的将Object类中的equals进行重写，而这里我将重点学习String类中equals方法的重写以及在String类中的应用。**

## Object类中的equals方法
```java
public boolean equals(Object obj)                     //Object类中的equals方法（含义只有比较值是否相等）
	{
		return(this == obj)                               //equals方法中的参数则为外来参数；表示obj与this指针所指Object实例发生equals方法
	}
```
**这是位于java.lang包中的Object类源代码。**

**对于Object类的equals方法来说，它是判断调用equals方法的引用与传进来的引用是否一致，即这两个引用是否指向同一对象即等价于==。**

```java
...
Object object = new Object();
Object object2 = new Object();
System.out.println(object.equals(object2));
System.out.println(object.equals(object));
```
>输出结果为“false”"true"

```java
package p;

public class EqualsTest
{
	public static void main(String[] args)
	{
		Student s1 = new Student("zhangsan");
		Student s2 = new Student("zhangsan");
		System.out.println(s1 == s2);
		System.out.println(s1.equals(s2));
	}
}
class Student
{
	String name;
	public Student(String name)
	{
		this.name = name;
	}
}
```
>输出结果为“false”“false”

**输出值为false是因为s1调用的equals方法仍是Object类中的equals方法；看似字符串的equals方法的使用其实不然。**

* **此代码本来预想实现的功能为直接比较两Student对象只要名字一样就表示为同一个人。这一功能以接下来的思想作为铺垫将会得到实现。**

## String类重写的equals方法

```java
package p;

public class EqualsTest2
{
	public static void main(String[] args)
	{
		String str = new String("aa");
		String str2 = new String("aa");                     //生成两个对象，若是object类中的equals方法，这一定不会是true
		System.out.println(str.equals(str2));               //说明equals方法的作用发生了改变
		String str3 = "aa";
		String str4 = "aa";
		System.out.println(str3.equals(str4));
```

>输出结果为“true”“true”

1. **相较于Object类中的equals方法，这种输出结果说明equals方法的作用发生了改变。**

2. **对于String类的equals方法来说，他是判断当前字符串与传进来的字符串的内容是否相等。**

3. **当使用String对象的相等性判断，最好使用equals方法而不要使用==。**

#### String类重写equals方法源代码
```java
public boolean equals(Object anObject)
	{
		if(this == anObject)                            //（1）
		{
			return true;                                  //返回值为真则表示自己在跟自己比
		}
		if(anObject instanceof String)                  //（2）
		{
			String anotherString = (String)anObject;      //（3）
			int n == count;
			if (n == anotherString.count)                 //（4）
			{
				char v1[] = value;
				char v2[] = anotherString.value;
				int i = offset;
				int j = anotherString.offset;
				while (n-- !=0 )                            //(5)
				{
					if(v1[i++] != v2[j++])
						return false;
				}
				return true;
			}
			return false;
		}
	}
```
**这是位于java.lang包中String类重写了Object类中equals方法的源代码。**

* **关于（3）处为何要执行向下类型转换：想要重写父类里的equals方法，势必要接受一个Object参数，而上条语句已经判断出来此参数为字符串为了使用字符串中的方法所以一定要进行向下类型转换。**

#### 方法执行逻辑

* （1）首先判断传递的参数是否和调用该方法的对象相等，如果相等则表示是自己在和自己比

* （2）判断anObject是否为字符串类的实例,使用到instanceof关键字。

* （3）向下类型转换使传进来的Object参数转换为String类，为了方便下文比较字符。

* （4）比较字符串大小，若值为真则继续进行，为假则字符串内容不相等。

* **（5）字符串中字符逐个进行比较直到没有字符可比为止**

### 实现引用通过访问equals方法来达到比较内容的目的

```java
package p;

public class EqualsTest
{
	public static void main(String[] args)
	{
		Student s1 = new Student("zhangsan");
		Student s2 = new Student("zhangsan");
		System.out.println(s1 == s2);
		System.out.println(s1.equals(s2));
		//System.out.println(s1.name.equals(s2.name));      //调用了String类中重写的equals方法，此为理想输出
	}
}
class Student
{
	String name;
	public Student(String name)
	{
		this.name = name;
	}
	public boolean equals(Object anObject)                 //经过加了这一方法。Student类将equals方法重写，使其equals方法在调用时，不同于原有的Object类中的equals方法。
	{
		if(this == anObject)
		{
			return true;
		}
		if(anObject instanceof Student)
		{
			Student student = (Student)anObject;
			if(student.name.equals(this.name))
			{
				return true;
			}
		}
		return false;
	}
}

```
>**照猫画虎的重写了Object类中equals方法，并用到了Sting类中所重写的equals方法，实现了使方法比较两引用的内容，而不再是比较两引用的地址，如此一来达到了最初的目的。其实只是想更透彻的感受一下String类对equals方法的重写，想要达到预想结果其实并不用这么麻烦。System.out.println(s1.name.equals(s2.name))；即可。**
