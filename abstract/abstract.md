# 抽象类以及抽象方法的使用
```java
package compute;
public class ComputeArea
{
	public static void main(String[] args)
	{
		Shape shape = new Triangle(10,6);
		int area = shape.computeArea();
		System.out.println("triangle:" + area);
		shape = new Rectangle(10,10);
		area = shape.computeArea();
		System.out.println("Rectangle:" + area);
	}

}
abstract class Shape
{
	public abstract int computeArea();
}
class Triangle extends Shape
{
	int width;
	int height;
	public Triangle(int width,int height)
	{
		this.width = width;
		this.height = height;
	}
	public int computeArea()
	{
		return (width * height) / 2;
	}
}
class Rectangle extends Shape
{
	int width;
	int height;
	public Rectangle(int width,int height)
	{
		this.width = width;
		this.height = height;
	}
	public int computeArea()
	{
		return width * height;
	}
}
```
1. 使用抽象类时需要注意，抽象类无法生成实例对象。
2. 抽象方法必须要定义在抽象类中，但不能有实现。
3. 在子类继承抽象父类的情况下，那么该子类必须要实现父类中所定义的所有抽象方法；否则，该子类也要声明抽象类。
4. 不管抽象父类中声明了多少种抽象方法，子类中一定要实现出来。
5. 我的理解：抽象方法的意义是抽象类中声明了抽象方法之后可以有多种具体方法来实现声明的抽象方法
