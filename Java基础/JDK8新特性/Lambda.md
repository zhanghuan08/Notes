# Lambada表达式

## 一、Lambda表达式简介

### 1.1 什么是`Lambda`表达式？

>`Lambda` 是 Java8添加的一个新特性，就是一个匿名函数

### 1.2 为什么要使用`Lambda`？

>使用`Lambda`可以对一个接口非常简洁的实现。

### 1.3 Lambda 对接口的要求

>接口中定义的必须要实现的抽象方法只能是一个。



**@FunctionalInterface** ：用来修饰函数式接口，接口中的抽象方法只能有一个。



接口实现的方式：

1、使用接口实现类

2、使用匿名内部类

3、使用Lambda表达式来实现接口

```java
public class Demo {
    public static void main(String[] args) {

        //1.使用接口实现类
        MyComparator comparator = new MyComparator();

        //2.使用匿名内部类
        Comparator comparator1 = new Comparator() {
            @Override
            public int compare(int a, int b) {
                return a-b;
            }
        };

        //3.使用Lambda表达式来实现接口
        Comparator comparator2 = (a,b)-> a-b;

    }
}



class MyComparator implements Comparator {

    @Override
    public int compare(int a, int b) {
        return a - b;
    }
}

@FunctionalInterface
interface Comparator {
    int compare(int a, int b);
}
```

