---
title: java注解
date: 2017-08-30 19:59:17
tags:
---

### JDK自带注解（编译时注解）
1. @Override 重写父类的方法
2. @Deprecated 注解该方法已经过时
3. @Suppvisewarnings 当使用到@Deprecated注解的方法时，可忽略警告。

### 常见的第三方注解

Spring

``` bash
@Autowired（运行时注解）
@Service
@Repository
```

Mybatis

``` bash
@InsertProvider
@UpdateProvider
@Options
```

### 注解的分类

1. 按照运行机制分为

```bash
源码注解：注解只在源码中存在，编译成.class文件就不存在了。
编译时注解：注解在源码和.class文件中都存在。
运行时注解：在运行阶段还起作用，甚至会影响运行逻辑的注解。
```

2. 按照来源

``` bash
来自JDK的注解
来自第三方的注解
我们自己定义的注解
```

### 自定义注解

``` bash
//元注解
@Target({ElementType.METHOD,ElementType.TYPE})// CONSTRUCTOR：构造方法声明, FIELD：字段声明, LOCAL_VARIABLE：局部变量声明, METHOD：方法声明, PACKAGE：包声明, PARAMETER：参数声明, TYPE：类接口
@Retention(RetentionPolicy.RUNTTIME)// SOURCE, CLASS, RUNTIME
@Inherited // 允许子类继承
@Documented // 生成javadoc时会包含注解
// 使用@interface关键字定义注解
public @interface Description{
    // 成员类型是受限的，合法的类型包括原始类型，以及String,Class,Annotation,Enumeration
    // 如果注解只有一个成员，则成员名必须取名为value(),在使用时可以忽略成员名和赋值号（=）。
    String desc();// 成员以无参无异常方式声明
    String author();
    int age() default 18;// 可以用default为成员指定一个默认值
}

String value();
```

### 使用自定义注解

``` bash
@<注解名\>(<成员名1>=<成员值1>,<成员名1>=<成员值1>,...)
@Description(desc="I am eyeColor", author="wang", age=25)
public String eyeColor{
    return "black";
}

@Description("name")
```

### 解析注解

概念：通过反射获取类、函数或成员上的运行时注解信息，从而实现动态控制程序运行的逻辑。

Description.java
``` bash
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Description {
	
    String desc();

    String author();

    int age() default 18;
}
```

Person.java

``` bash
@Description(author = "Person", desc = " Class")
public class Person {
	
    @Description(author = "Person", desc = " name method")
    public String name(){return "";}

    public int age(){return 0;}

    @Deprecated
    public void sing(){}
}
```

Child.java

``` bash
//@Description(author = "Child", desc = " class")
public class Child extends Person{
    @Override
    @Description(author = "Child", desc = " name method")
    public String name() {
        // TODO Auto-generated method stub
        return null;
    }

    @Override
    @Description(author = "Child", desc = " age method")
    public int age() {
        // TODO Auto-generated method stub
        return 0;
    }

    @Override
    public void sing() {
        // TODO Auto-generated method stub
        
    }
	
}
```

Parse.java

``` bash
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;

public class Parse {
public static void main (String[] args){
    // 使用类加载器加载类
    try {
        Class<?> class1 = Class.forName("com.test.Child");
        System.out.println(class1);
        // 1.找到类上面的注解(判断该类上是否存在这个注解)
        boolean isExist = class1.isAnnotationPresent(Description.class);
        if (isExist) {
            // 拿到注解实例
            Description d = (Description)class1.getAnnotation(Description.class);
            System.out.println("类上面的注解:"+d.author()+d.desc());
        }
        // 2.找到方法上面的注解
        Method[] methods = class1.getMethods();
        for (Method method: methods) {
            // 方式一
            boolean isMExist = method.isAnnotationPresent(Description.class);
            if (isMExist) {
                // 拿到注解实例
                Description d = (Description)method.getAnnotation(Description.class);
                System.out.println("方式一："+d.author()+ d.desc());
            }
            // 方式二
            Annotation[] annotations = method.getAnnotations();
            for (Annotation annotation: annotations) {
                if (annotation instanceof Description) {
                    Description description = (Description)annotation;
                    System.out.println("方式二："+description.author()+ description.desc());
                }
            }
        }
    } catch (ClassNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
}
}
```


