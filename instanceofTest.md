# instanceof运算符简要说明
>**instanceof是java的一个二元操作符（运算符），它的作用是判断其左边对象是否为其右边类的实例，返回boolean类型的数据。可以用来判断继承中的子类的实例是否为父类的实现。**
#### 参数
result：布尔类型。

>object：必选项，任意对象表达式。

>class：必选项，任意已定义的对象类。

>#### 语法
boolean result = object instanceof class
#### 说明
如果object是class的一个实例，则instanceof运算符返回为true，如果object不是指定类的一个实例，或者object是null，则返回值为false

```java
package p;

public class instanceofTest
{
	public static void main(String[] args)
	{
		A b = new B();
		System.out.println(b instanceof A);     //根据继承；子类就是父类     因此b也可以看作是A的实例
	}
}
class A
{

}
class B extends A
{

}
```
>**输出结果为“true”
