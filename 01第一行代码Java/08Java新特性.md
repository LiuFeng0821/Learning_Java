# 8 Java新特性

## 8.1 可变参数
* 从JDK1.5开始，为了解决参数多个的问题，在方法定义上提供了可变参数的概念，既可以接收多个参数，也可以接收数组。
```java
[public|protected|private] [static] [final] [abstract] 返回值类型 方法名称(参数类型 ... 变量){
}
```

## 8.2 foreach循环
* foreach是一种加强型的for循环操作，主要可以用于简化数组或集合数据的输出操作。
```java
for(数据类型 变量 : 数组|集合) {
    //每一次循环会自动的将数组的内容设置给变量
}
```

## 8.3 静态导入
* 如果某一个类中定义的方法全部属于static型的方法，那么其他类要引用此类时必须先使用import导入所需要的包，再使用“类名称.方法()”进行调用。如果再调用方法时不希望出现类名称，可以使用静态导入操作完成：`import static 包.类.*;`

一个程序例子：
```java
package com.liufeng.demo;
import static com.liufeng.util.MyMath.*;
public class TestDemo {
    public static void main(String args[]) {
        System.out.println("加法操作：" + add(10,20));
        System.out.println("除法操作：" + div(10,2));
    }
}
```

## 8.4 泛型
