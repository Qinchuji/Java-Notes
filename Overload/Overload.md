# 重载（OverLode）
## 构造方法重载（construstor overload)
构造方法重载只许看参数即可
1. 参数类型不同
2. 参数个数不同

```java
public ConstructorOverload()
{
  System.out.println("test");
}
public ConstructorOverload(int i)
{
  System.outl.println("++i");
}
```
>如ConstructorOverload c = new ConstructorOverload()生成对象，调用的一定是不带参数的构造方法

### this()关键字
如果想在一个构造方法中调用另一个构造方法，那么可以使用this()的方式调用。

将无参构造方法做稍加更改
```java
public ConstructorOverload()
{
  this(3);
  System.out.println("test");
}
public static void main(String[] args)
{
  ConstructorOverload c = new ConstructorOverload();
}
```
>输出结果为：

```java
4
test
```
this()表示对当前对象的引用。this()括号中的参数表示目标构造方法的参数。

**注意：this()必须要作为构造方法的第一条语句，换句话说，this()之前不能有任何可执行的代码。**

## 方法重载(OverLode)
表示两个或多个方法名字相同，但方法参数不同.
1. 参数类型不同
2. 参数类型不同

```java
public int add (int a, int b)
{
  return a + b;
}
public int add (int a, int b, int c)
{
  return a + b + c;
}
```
表示这两个方法互为重载关系。
```java
public int method(int a)
{
  System.out.println(a);
  return 1;
}
public boolean method(int a)
{
  System.out.println(a);
  return false;
}
```
结果是编译不通过的，返回值无论是相同或者不同，对重载都不会有影响。因为当用户使用参数调用方法的时候，如果参数是相同，那么并没有明确要调用哪个互为重载关系的方法。

注意：方法的返回值对重载没有任何影响。(是否重载仅由参数决定)
