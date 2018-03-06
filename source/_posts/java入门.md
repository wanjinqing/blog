---
title: java入门
date: 2017-08-29 09:48:02
tags:
---

### java中数据类型
``` bash
8种基本数据类型
1.浮点型:float(4byte),double(8byte)
2.整型:byte(1byte),short(2byte),int(4byte),long(8byte)
3.字符型:char(2byte)
4.布尔型：boolean（1/8 byte)

```

### java中的比较运算符

* \> 、 < 、 >= 、 <= 只支持左右两边操作数是数值类型.
* == 、 != 两边的操作数既可以是数值类型，也可以是引用类型.

### java中Scanner对象

接收并保存用户输入的值

``` bash
Scanner input = new Scanner(System.in);
int score = input.nextInt();
```

### java中的Arrays类

1. `
此类中只有升序排序，而无降序排序。
Arrays.sort(list)
`

2. `
foreach遍历数组
for (int item: list) {
    System.out.println(item);
}
`

3. `
将数组转换为字符串
Arrays.toString(list);
`

4. `
比较两个数组是否相等
Arrays.equals(arry1, arry2)
`

5. `
复制
String [] arry1={"北京","上海","重庆","深圳"};
String [] arry2=Arrays.copyOf(arry1, 4);// 创建新数组
int[] copied = new int[10];
System.arraycopy(arr, 0, copied, 0, 3) // 只拷贝已经存在数组元素
`

6. `
二维数组
int[][] num = new int[3][];
int[][] num2 = new int[3][4];
`

### java中的方法

``` bash
访问修饰符 返回值类型 方法名（参数列表）{
    方法体
}
1、 访问修饰符：方法允许被访问的权限范围， 可以是 public、protected、private 甚至可以省略 ，其中 public 表示该方法可以被其他任何代码调用，其他几种修饰符的使用在后面章节中会详细讲解滴
2、 返回值类型：方法返回值的类型，如果方法不返回任何值，则返回值类型指定为 void ；如果方法具有返回值，则需要指定返回值的类型，并且在方法体中使用 return 语句返回值
3、 方法名：定义的方法的名字，必须使用合法的标识符
4、 参数列表：传递给方法的参数列表，参数可以有多个，多个参数间以逗号隔开，每个参数由参数类型和参数名组成，以空格隔开


判断方法重载的依据：
1、 必须是在同一个类中
2、 方法名相同
3、 方法参数的个数、顺序或类型不同
4、 与方法的修饰符或返回值没有关系
```

### 类

``` bash
成员变量和局部变量的区别
1.初始值不同：java会给成员变量一个初始值，不会给局部变量赋予初始值。
2.在同一个方法中，不允许有同名局部变量；在不同的方法中，可以有同名局部变量。
3.成员变量和局部变量同名时，局部变量具有更高的优先级。

构造方法
当没有自定义构造方法时，系统会自动创建无参构造方法，否则不会创建；
public 构造方法名 (参数) {
}

static
1.静态方法里面不能直接调用非静态变量（如果希望在静态方法中调用非静态变量，可以通过创建类的对象，然后通过对象来访问非静态变量）
（静态变量，静态方法，类方法）
2.在普通成员方法中，则可以直接访问同类的非静态变量和静态变量
3.静态方法中不能直接调用非静态方法，需要通过对象来访问非静态方法。
4.程序运行时"静态初始化块"最先被执行，然后执行"普通初始化块"，最后才执行"构造方法"。
由于静态初始化块只在类加载时执行一次，所以当再次创建对象时并未执行静态初始化块。
```

### java中的访问修饰符

``` bash
private： 本类
默认： 本类 同包
protected：本类 同包 子类
public：本类 同包 子类 其他
```

### extends继承

``` bash
public class Animal {
    public int age;
    public String name;
    public void eat () {
        System.out.println("动物具有吃东西的能力");
    }
}
public class Dog extends Animal {
}
public class Initail {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.age = 10;
        dog.name = "bilibili";
        dog.eat();
    }
}
1.final修饰类时，该类不能被继承
2.final修饰方法，子类不能覆盖父类的方法
3.final修饰属性，则该属性不能被修改（可在构造方法中初始化它）
```

### 多态
``` bash
引用多态
父类引用指向本类对象：
Animal an1 = new Animal();
父类引用指向子类对象：
Animal an2 = new Dog();

方法多态
创建本类对象时，调用的方法为本类方法：
an1.eat();
创建子类对象时，调用的方法为子类重写的方法或者继承的方法：(子类独有的方法不能调用)
an2.eat();

引用类型的转换
1.向上类型转换（隐式/自动类型转换）（小=>大）
2.向下类型转换（强制类型转换）（大=>小）
3.instanceof运算符，来解决引用对象的类型，避免类型的安全性问题
Dog dog = new Dog();
Animal animal = dog;（小=>大）
Dog dog2 = (Dog)animal;（大=>小）
if (animal instanceof Cat) {
    Cat cat = (Cat)animal;
}
```

### 抽象类

``` bash
1.abstract定义抽象类
2.abstract定义抽象方法只有声明，不需要实现
3.包含抽象方法的类是抽象类
4.抽象类中可以包含普通的方法，也可以没有抽象方法
5.抽象类不能直接创建，可以定义引用变量

public abstract class Telphone {
    public abstract void call();
    public abstract void message();
}
public class Cellphone extends Telphone{
    @Override
    public void call(){
        // 必须的方法
    }
    @Override
    public void message () {
        // 必须的方法
    }
}
```

### 接口
``` bash
接口可以理解为一种特殊的类，由全局常量和公共的抽象方法所组成。

接口是用来被继承，被实现的，修饰符一般建议用public。
注意：（不能使用private，protected）。
[修饰符] interface 接口名 [extends 父接口1, 父接口2]{
    零个到多个常量定义
    零个到多个抽象方法的定义
}
```

### java异常

``` bash
1.Throwable(
    Error:(虚拟机错误VirtualMachineError,线程死锁ThreadDeath)
    Exception:RuntimeException(非检查异常):(
        NullPointerException空指针异常
        ArrayIndexOutOfBoundsException数组下标越界异常
        ClassCastException类型转换异常
        ArithmeticException算术异常
        ...
    )
    检查异常:(
        IOException文件异常
        SQLException（SQL异常）
    )
)

2.try{}catch(){}finally(){} : => 子 => 父

3.异常链
public class ChainTest {
    public static void main(String[] args) {
        ChainTest chainTest = new ChainTest();
        try {
            chainTest.test2();
        }catch (Exception e) {
            e.printStackTrace();
        }
    }
    public void test1() throws DrunkException{
        throw new DrunkException("喝酒别开车");
    }
    public void test2(){
        try {
            test1();
        } catch (DrunkException e) {
            // RuntimeException newExc = new RuntimeException(e);
            RuntimeException newExc = new RuntimeException("司机一滴酒，亲人两行泪");
            newExc.initCause(e);
            throw newExc;
        }
    }
}
```

### java中的字符串

``` bash
String s1 = "imooc";
String s2 = "imooc";
System.out.println(s1.equals(s2));// true
System.out.println(s1 == s2);// true
s1 = s1 + "45";
s2 = s2 + "45";
System.out.println(s1.equals(s2));// true
System.out.println(s1 == s2);// false
String s3 = new String("aa");
String s4 = new String("aa");
System.out.println(s3.equals(s4));// true
System.out.println(s3 == s4);// false

1）对于==，如果作用于基本数据类型的变量，则直接比较其存储的 “值”是否相等；
如果作用于引用类型的变量，则比较的是所指向的对象的地址

2）对于equals方法，注意：equals方法不能作用于基本数据类型的变量如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址；
诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容。

常用方法：
int length();
int indexOf(char);
int indexOf(String);
int lastIndexOf(char);
int lastIndexOf(String);
String substring(beginIndex);
String substring(beginIndex, endIndex);
String trim();
boolean equals(Object);
String toLowerCase();
String toUpperCase();
char charAt(index);
String[] split(regex, limit);
byte[] getBytes();

总结：
1.如果要操作少量的数据用 = String
2.单线程操作字符串缓冲区下操作大量数据 = StringBuilder
3.多线程操作字符串缓冲区下操作大量数据 = StringBuffer
```

### java中基本类型和字符串之间的转换
``` bash
基本类型 => 字符串
Integer.toString(int);
String.valueOf(int);

字符串 => 基本类型
Integer.parseInt(str);
Integer.valueOf(str);
```

### java 日期

``` bash
Date date = new Date();
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String result = simpleDateFormat.format(date);
Date newDate = simpleDateFormat.parse(result);

Calendar calendar = Calendar.getInstance();// 创建Calendar对象
int year = calendar.get(Calendar.YEAR);
int month = calendar.get(Calendar.MONTH) + 1;
int day = calendar.get(Calendar.DAY_OF_MONTH);
int hour = calendar.get(Calendar.HOUR_OF_DAY);
int minute = calendar.get(Calendar.MINUTE);
int second = calendar.get(Calendar.SECOND);
System.out.println(year + "-" + month + "-" + day + " " +hour + ":" +minute + ":" + second);

Date date = calendar.getTime();// 将Calendar转化为Date对象
Long time = ccalendar.getTimeInMillis();// 获取当前毫秒数
```

### java Math

``` bash
long round();// 返回四舍五入后的整数
double floor();// 返回小于参数的最大整数
double ceil();// 返回大于参数的最小整数
double random();// 返回[0,1)之间的随机浮点数
```

### java 集合框架

``` bash
集合的概念：
java中的集合类是一种工具类，就像是容器，存储任意数量的具有共同属性的对象。

集合的作用：
1.在类的内部，对数据进行组织
2.简单而快速的搜索大数量的条数；
3.有的集合接口，提供了一系列排列有序的元素，并且可以在序列中间快速的插入或者删除有关的元素。
4.有的集合接口，提供了映射关系，可以通过关键字（key）去快速查找到对应的唯一对象，而这个关键字可以是任意类型。

Collection接口:
// 有序可重复
1.List序列：ArrayList
2.Queue队列:LinkedList
// 无序不可重复
3.Set集:HashSet

Map接口:
HashMap哈希表

Collections工具类

Comparable接口

Comparator接口
```

### Comparable & Comparator

``` bash
1.Comparable：默认比较规则（可比较的）
* 实现该接口表示：这个类的实例可以比较大小，可以进行自然排序。
* 定义了默认的比较规则。
* 其实现类需实现compareTo()方法。
* comparaTo()方法返回正数表示大，负数表示小，0表示相等。

Comparator：临时比较规则（比较工具接口）
* 用于定义临时比较规则，而不是默认比较规则
* 其实现类需要实现compare()方法
* Comparable和Comparator都是Java集合框架的成员
```
