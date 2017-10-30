# Constant(常量)
1. 对于Java中的常量的命名规则：所有单词的字母都是大写，如果有多个单词，那么是用下划线链接即可。比如说：
public static final int AGE_OF_PERSON = 20;

2. **在java中声明final常量时通常都会加上static关键字，这样对象的每一个实例都会访问唯一一份常量值。**
static与final关键字经常在一起使用的原因：如果想要只使用final定义一个常量的话，那么每一个对象都有这个常量并且都不能改变（n个对象，生成了n个常量),而加了static的话，不管生成多少个对象，常量只有一个也是不能改变的并且是共享值（n个对象，只生成一个常量）。

### 常量的应用场合

例如访问权限
```java
public class Authorization
{
public static final int MANAGER = 1;
public static final int DEPARTMENT = 2;
public static final int EMPLOYEE = 3;
}
```
设置对不同身份的人员的访问权限。
```java
public class Test
{
  public boolean canAccess(int accees)
  {
    if(access == Authorization.MANAGER)
    {
      return true;
    }
    if(access == Authorization.DEPARTMENT)
    {
      return false;
    }
    if(access == Authorization.EMPLOYEE)
    {
      return false;
    }
    return false;
  }
}
```
>通过相应的身份以通过访问权限。
