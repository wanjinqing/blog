---
title: java集合
date: 2017-08-29 16:13:50
tags:
---

### Collection And Map

``` bash 
Collections工具类是java集合框架中，用来操作集合对象的工具类，也是java集合框架的成员。
```

### 例子

ListTest.java

``` bash
package collection_map;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

public class ListTest {

private List<Course> coursesToSelect;
public ListTest() {
    this.coursesToSelect = new ArrayList<Course>();
}
public void testAdd(){
    Course cr1 = new Course("1", "one");
    coursesToSelect.add(cr1);
    Course cr2 = new Course("2", "two");
    coursesToSelect.add(0, cr2);		
    Course[] courses = {
            new Course("3", "three"),
            new Course("4", "four")
    };
    coursesToSelect.addAll(Arrays.asList(courses));
    Course[] courses2 = {
            new Course("5", "five"),
            new Course("6", "six")
    };
    coursesToSelect.addAll(2, Arrays.asList(courses2));
    
}
public void testGet () {
    System.out.println("for:");
    for (int i = 0; i < coursesToSelect.size(); i++) {
        Course temp = (Course)coursesToSelect.get(i);
        System.out.println(i + ":" + temp.getId() + "-" + temp.getName());
    }
}
/**
    * 迭代器
    */
public void testIterator () {
    System.out.println("\n迭代器:");
    Iterator<Course> it = coursesToSelect.iterator();
    while (it.hasNext()) {
        Course course = (Course) it.next();
        System.out.println(course.getId() + "-" + course.getName());
    }
}
public void testForEach() {
    System.out.println("\nforEach:");
    for (Object obj: coursesToSelect) {
        Course course = (Course)obj;
        System.out.println(course.getId() + "-" + course.getName());
    }
}
public void testModify() {
    coursesToSelect.set(4, new Course("7", "seven"));
}
public void testRemove() {
    Course course = coursesToSelect.get(4);
    coursesToSelect.remove(course);
    Course[] course2 = {(Course)coursesToSelect.get(0)};
    coursesToSelect.removeAll(Arrays.asList(course2));
}
public static void main(String[] args) {
    ListTest test = new ListTest();
    test.testAdd();
    test.testGet();
    test.testModify();
    test.testIterator();
    test.testRemove();
    test.testForEach();
}
}
```

Course.java

``` bash
package collection_map;

public class Course {
	private String id;
	private String name;
	public Course(String id, String name) {
		this.id = id;
		this.name = name;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
```