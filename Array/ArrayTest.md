# 数组（Array)
**学习关于数组需要注意的地方以及灵活的使用方式**
## 一、知识点
### 一、二维数组
**二维数组是一种平面的二维结构，本质上是数组的数组。**
```java
package p;

public class ArrayTest3
{
	public static void main(String[] args)
	{
		int[][] a = new int[2][3];                        //integer本身就是个类
		System.out.println(a instanceof Object);          //数组是对象
		System.out.println(a[0] instanceof int[]);        //a[0]是个一维数组
		int m = 0;
		for(int i = 0;i < 2;i++)
		{
			for(int j = 0;j < 3;j++)
			{
				m++;
				a[i][j] = m*2;
				System.out.print(a[i][j]+" ");
			}
			System.out.println();
		}
	}
}

```
>输出结果为:

```java
true

true

2 4 6

8 10 12
```

以该方式定义了一个二行三列的二维数组并为其赋值。

同时顺便证明了数组是对象，而二维数组实际逻辑上就是以行数个数组整合而成。

**从这里可以明白当遇到需要执行数组的赋值或使用时使用for循环是较为方便的。**

### 二、不规则的二维数组
#### 1.先规定行再赋值
```java
public class ArrayTest4
{
  public static void main(String[] args)
  {
    int [][] a = new int[3][];
    a[0] = new int[2];
    a[1] = new int[3];
    a[2] = new int[1];
  }
}

```
>输出结果为编译通过。

再次理解了二维数组实际逻辑上就是以行数个数整合而成的，行与行之间并无过多联系。

**那么定义不规则的二维数组为何不能通过int [ ][ ] a = new int[ ][3];即规定了列的方式来定义。**
* 假如说这种方式是可以的，那么数组中有3列，却不知道有几行，所以要初始化每行的信息，但无法在操作上实现这样的操作。

#### 2.使用初始化列表
```java
public class ArrayTest5
{
  public static void main(String[] args)
  {
    int [ ][ ] a = new int[ ][ ]{{1,2,3},{4},{5,6,7,8}};
    for(int i = 0;i < a.length; i++)
    {
      for(int j = 0;j < a[i].length; j++)
      {
        System.out.print(a[i][j]+" ");
      }
      System.out.println();
    }
  }
}
```
>输出结果为:

```java
1 2 3

4

5 6 7 8
```
**初始化列表也可直接为不规则二维数组赋值。**
### 三、引用数据类型的数组
#### 1.
```java

package p;

public class ArrayTest
{
	public static void main(String[] args)
	{
		Person[] p1 = new Person[3];                            //1、
		p1[0] = new Person(10);                                 //2、
		p1[1] = new Person(20);
		p1[2] = new Person(30);
		for(int i = 0;i < p1.length;i++)
		{
			System.out.println(p1[i].age);                      //3、
		}
		Person[] p2 = new Person[3];
		for(int i = 0;i < p2.length;i++)
		{
			System.out.println(p2[i]);                          //4、
		}
	}
}
class Person
{
	int age;
	public Person(int age)
	{
		this.age = age;
	}
}
```
1. **引用数据类型的数组，new的实际上是引用，而绝对不是对象。**

2. 希望给数组赋值的时候就相当于再给引用逐个赋上地址并利用方法传值。
```java
System.out.println(p1[i]);
 ```
3. 输出引用数据类型的数组一定要注意，单独输出数组的时候输出的是数组元素引用所指向的地址。

4. 没有为引用数据类型的数组赋初值则每一个元素则为空，因为引用数据类型的数组指向的实际上仍是引用，而没有赋值表示引用没有指向实际地址。

#### 2.接口类的数组

```java
package p;

public class ArrayTest7
{
	public static void main(String[] args)
	{
		I[] i = new I[2];                       //1、
		I j = null；
		i[0] = new C();                         //2、
		i[0] = new C();
		int[]a =new int[] {1,2,3};              //3、
		I[] b = new I[] {new C(), new C()}；    //4、
	}
}
interface I
{

}
class C implements I
{

}

```

>输出结果为编译成功
1. 说明这条语句中只是声明了一个含有两个元素的数组类型对象，而并没有生成任何I类型的实例。接口不能创建一个接口类型的实例。这条语句的含义实际上与I j = null；（表示生成了一个指向空的引用）是一样的。**告诉了java编译器数组里面生成了一个长度为2的数组，其每个元素都是I类型的，不指向任何一个对象。**

2. I[0]是一个I类型的引用，而I类型的引用可以指向一个实现了I接口的类的实例。（多态）

3. 这是数组使用初始化列表的形式来定义的，那么是否可以使用此方法定义引用数据类型数组。4、实现了这种方式。

### 四、数组中的值传递
**讨论如何实现将整型变量通过方法进行值的互换问题。**
```java
public class Swap
{
	public static void swap(int a, int b)
	{
		int temp = a;
		a = b;
		b = temp;
	}
	public static void main(String[] args)
	{
		int x = 3;
		int y = 4;
		Swap.swap (x,y);
		System.out.println(x);
		System.out.println(y);
	}
}
```
>输出结果为：“3，4”

说明swap方法并没有起作用，原因是原生数据类型在方法中只传递参数本身，所以即使在swap方法中实际的进行了参数的互换，在main方法中的两个值也并不会有什么影响。（详见ParamTest.md)

**那么这是否表示整型的变量就根本无法在方法中实现出参数呼唤的功能呢？**

答案当然是否定的。
```java
package p;

public class ArrayTest5
{
	public static void swap(char[]ch , char c)
	{
		ch[0] = 'B';
		c = 'D';
	}

public static void main(String[] args)
{
	char[] ch = {'A', 'C'};
	swap(ch,ch[1]);
	for(int i = 0;i < ch.length;i++)
	{
		System.out.println(ch[i]);
	}
}
}
```

>输出结果为：“B，C”

这一结果说明在swap方法中传入的两个参数只有第一个值实现了功能。这给我们了一点提示，像上面的例子一样，既然原生数据类型无法在方法中进行，而字符型数组却可以（其原因是因为数组变量本身是一个引用即方法中发生的是引用数据类型的参数传递）。

**那么如果想要实现整型变量在方法中实现功能是否也可以使用数组的方式进行？**

```java
package p;

public class ArrayTest6
{
	public static void swap(int[] i)
	{
		int temp = i[0];
		i[0] = i[1];
		i[1] = temp;
	}
	public static void main(String[] args)
	{
		int[] i = {1, 2};
		swap(i);
		System.out.println(i[0]);
		System.out.println(i[1]);
	}
}
```

>输出结果为：“2，1”

说明通过这个方式就可以实现整型变量在方法中值的互换。（C中可用指针实现，而java是不可以的）



## 二、灵活的使用方式
### 一、Arrays.equals方法
**自己构建一个方法用来实现判断两个数组是否相同**
```java
package p;
//import java.util.Arrays;                               //2、

public class ArrayTest8
{
	public static boolean isEquals(int[]a, int[]b)
	{
		if(a == null || b == null)                           //1、
		{
			return false;
		}
		if(a.length != b.length)
		{
			return false;
		}
		for(int i = 0;i < a.length; i++)
		{
			if(a[i] != b[i])
			{
				return false;
			}
		}
		return true;
	}
	public static void main(String[] args)
	{
		int[] a = {1,2,3};
		int[] b = {1,2,3};
		int[] c = null;
		int[] d = {1,2,3,4};
		System.out.println(isEquals(a,b));
		System.out.println(isEquals(a,c));
		System.out.println(isEquals(b,d));
//		System.out.println(Arrays.equals(a,b));
	}
}
```
>输出结果为
“true
false
false”
1、设立此判断是因为传递进入方法的参数是引用，而如果是引用则值可能为空。

**若想判断不同数据类型数组，JDK为我们提供了一个通用的方法，专门为数组的比较，排序查询等相关辅助功能。这个类叫做Class Arrays,这个类是定义在java.util包下的。**

2、因为Class Arrays是定义在java.util包下的，所以想使用方法则需导入java.lang包。
### 二、arraycopy方法
**将源数组拷贝进另一个目标数组的方法**
```java
package p;

public class ArrayTest9
{
	public static void main(String[] args)
	{
		int[] a = new int[] {1,2,3,4};
		int[] b = new int[4];
		System.arraycopy(a, 0, b, 0, 4);
		for(int i = 0; i < b.length;i++)
		{
			System.out.println(b[i] + " ");
		}
	}
}
```
>输出结果为：“1，2，3，4”

Class System是定义在java.lang包下的所以无需导入包。arraycopy方法就是定义在此类中的方法。

public static void arraycopy(Object src,int srcPos,Object dest,int destPos,int length)
#### 方法参数
src-源数组

srcPos-源数组中的起始位置

dest-目标数组

destPos-拷贝至目标数组的起始位置

length-拷贝数组的个数

* **运用此方法直接套用即可**

### 三、有条件赋值
**写一个程序想实现生成10个Student类型对象，并将基数对象赋值zhangsan偶数对象赋值lisi。**
```java
package p;

public class ArrayTest2
{
	public static void main(String[] args)
	{
		Student[] s = new Student[10];
		for(int i = 0;i < s.length;i++)
		{
			s[i] = new Student();
			s[i].name = i % 2 == 0 ? "zhangsan" : "lisi";     //1、
		}
		for(int i = 0;i < s.length;i++)
		{
			System.out.println(s[i].name);
		}
	}
}
class Student
{
	String name;
}
```
>输出结果为:

```java
zhangsan
lisi
zhangsan
lisi
zhangsan
lisi
zhangsan
lisi
zhangsan
lisi
```

1、核心为该循环内生成对象并判断基数偶数赋值。
一定要明确引用类型数组中的值是引用，想要进行访问一定要再对数组生成对象。
### 四、冒泡排序
**冒泡排序的作用是将一个数组以一定的规律进行排序，一般是进行大小排序。**
```java
package p;

public class ArrayTest10
{
	public static void bubbleSort(int[] array)
	{
		for(int i = 0;i < array.length - 1; i++)
		{
			for(int j = 0 ;j <array.length - i - 1; j++)
			{
				if(array[j] >array[j +1])
				{
					int temp = array[j];
					array[j] = array[j + 1];
					array[j + 1] = temp;
				}
			}
			System.out.println("第" +(i + 1) + "趟排序");
			for(int k = 0;k < array.length; k++)
			{
				System.out.print(array[k] + " ");
			}
			System.out.println();
		}
	}
	public static void main(String[] args)
	{
		int[] array = {4, 7, 8, 9, 3, 2};
		bubbleSort(array);
	}
}
```
>输出结果为：

```java
第1趟排序
4 7 8 3 2 9
第2趟排序
4 7 3 2 8 9
第3趟排序
4 3 2 7 8 9
第4趟排序
3 2 4 7 8 9
第5趟排序
2 3 4 7 8 9
```
本例中使用的是向上冒泡法。

**冒泡排序的主要思想是分若干次将数组中最大或最小的值向上或向下进行逐步移动，所以方法中应该有两个嵌套形式的for循环结构来完成。**

**最里层的if判断语句是用来比较两个相邻数值的大小并且将两数当中的较大者换至数组中元素位置大的一边。（这样相当于就是将数组当中最大的数摘到末尾位置）**

**第二层for循环语句是用来规定从数组的第一个元素位置开始进行if判断，并且避开了之前将较大者排在末端之后再次执行if判断。**

**第一层for循环的作用是限定了这套循环要执行的次数。**

### 五、二分查找（Binary Search）

**首先引用普通的查找方式，查找方式的作用是将数组中的一个值找到并且明确其在数组中的位置。**

```java
package p;

public class ArrayTest11
{
	public static int search(int[] array, int value)
	{
		for(int i = 0; i < array.length; i++)
		{
			if(value == array[i])
			{
				return i + 1;
			}
		}
		return -1;
	}

public static void main(String[] args)
{
	int[] a = new int[] {1, 5, 6, 7, 10, 3, 9};
	int value = 9;
	int index = search(a, value);
	System.out.println(index);
}
}
```
>输出结果为“7”

主要思想为将想要查找的值在数组中逐个进行比较，若值相等则返回其位置。

很好理解但效率是最低的，因为需要一个个进行比较。

#### 二分查找
**在数组是有序的前提下，将数组一分为二，并取中间元素与待查找数值进行比较。若中间元素大于待查找数据则将小于该元素的数组再次往复进行二分操作，若中间元素小于待查找数据则将大于该元素的数组再次往复进行二分操作。**

```java
package p;

public class ArrayTest12
{
public static int binarySearch(int[] array, int value)
{
	int low = 0;                            //第一个元素的位置
	int high = array.length - 1;            //最后一个元素
	int middle = 0;                         //暂赋中间值
	while(low <= high)                      //5、
	{
		middle = (low + high) /2;             //1、

		for(int i = 0;i < array.length; i++)
		{
			System.out.print(array[i]);
			if(i == middle)
			{
				System.out.print("#");
			}
			System.out.print(" ");
		}
		System.out.println();
		if(array[middle] == value)            //2、
		{
			return middle;
		}
		if(value < array[middle])             //3、
		{
			high = middle - 1;
		}
		if(value > array[middle])             //4、
		{
			low = middle + 1;
		}
	}
	return -1;                               //表示没有找到
}
public static void main(String[] args)
{
	int[] b = new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9};
	int index = binarySearch(b,8);
	System.out.println(index);
}
}
```
>输出结果为：
```java
1 2 3 4 5# 6 7 8 9
1 2 3 4 5 6 7# 8 9
1 2 3 4 5 6 7 8# 9
7

```

**井号标记出了每次二分取的中间元素的位置。**

1、第一步首先要确定数组的中间元素的位置。

**分别有三种情况，待查找数据为中间值、待查找值小于中间值、待查找值大于中间值分别进行对号入座。**

2、若待查找数据为中间值则返回中间值。

3、若待查找值小于中间值则另设数组最大值为middle-1，含义为将包括middle位置元素在内大于middle值的所有值都忽略。

4、若待查找值大于中间值则另设数组最小值为middle+1，含义为将包括middle位置元素在内小于middle值的所有值都忽略。

5、只要最小值小于最大值则循环往复的进行。若即使退出循环也没有返回值则表示待查找值不在此数组中。
