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
        System.out.println("加法操作：" + add(10,20));   //此处没有出现类名称
        System.out.println("除法操作：" + div(10,2));
    }
}
```

## 8.4 泛型
定义多个泛型标记：
```java
class Point<P,R> {
    public R fun(P p) {
        return null;
    }
}
```

使用泛型定义的类时也要指明泛型对应的数据类型，另外**泛型使用的类型只能是类**，不能是基本类型（**基本类型使用包装类**），只能是引用类型，如果**不设置泛型类型**，则**默认使用Object类**。
```java
Point<Integer,String> p = new Point<Integer,String>();
```

使用通配符"?"解决参数传递问题：
```java
class Message<T> {
    private T msg;
    public void setMsg(T msg) {
        this.msg = msg;
    }
    public T getMsg() {
        return this.msg;
    }
}
public class TestDemo {
    public static void main(String args[]) {
        Message<Integer> m1 = new Message<Integer>();
        Message<String> m2 = new Message<String>();
        m1.setMsg(100);
        m2.setMsg("LiuFeng");
        fun(m1);
        fun(m2);
    }
    public static void fun(Message<?> temp) {   //此处使用了通配符解决了参数传递的问题
        System.out.println(temp.getMsg());
    }
}
```

在"?"通配符基础上有两个子的通配符：
* **设置泛型上限**，可以在**声明和方法参数**上使用：`? extends 类`
* **设置泛型下限**，**方法参数**上使用：`? super 类`

泛型可以定义在类中，也可以定义在**接口**上，对于使用泛型的接口子类而言，有两种实现方式：
```java
Interface IMessage<T> {    //定义了一个泛型接口
    public void print(T t);
}
class MessageImpl<S> implements IMessage<S> {   //定义了一个泛型接口子类，接口子类继续设置了泛型
    public void print(S t) {
        System.out.println(t);
    }
}
public class TestDemo {
    public static void main(String args[]) {
        IMessage<String> msg = new IMessage<String>();   //在实例化接口子类时设置泛型类型
        msg.print("LiuFeng");
    }
}
```

```java
Interface IMessage<T t> {   //定义了一个泛型接口
    public void print(<T t>);
}
class MessageImpl<String> implements IMessage<String> {   //在子类实现接口时明确设置了泛型类型
    public void print(String t) {
        System.out.println(t);
    }
}
public class TestDemo {
    public static void main(String args[]) {
        IMessage<String> msg = new MessageImpl<String>();
        msg.print("LiuFeng");
    }
}
```

* 泛型不仅可以定义在类上，还可以在**方法**上定义（这个方法不一定要在泛型类中）。
```java
public class TestDemo {
    public static void main(String args[]) {
        String str = fun("LiuFeng");
        System.out.println(str.length());
    }
    public static<T> T fun(T t) {
        return t;
    }
}
```

## 8.5 枚举
* **枚举**主要用于**定义一组可以使用的类对象**，这样在使用时只能通过**固定的几个对象来进行类的操作**，从JDK1.5开始，专门提供了一个新的关键字：**enum**。

Java中**使用enum定义的枚举类**就相当于**默认继承java.lang.Enum类**，此类定义如下：
```java
public abstract class Enum<E extends Enum<E>> 
extends Object
implements Comparable<E>,Serializable
```

Enum类中定义的方法：
1. 传递枚举对象的名字和序号：`protected Enum(String name,int ordinal)`
2. 取得当前枚举对象的序号：`public final int ordinal()`
3. 取得当前枚举对象的名字：`public final String name()`

```java
enum Color {
    RED,GREEN,BLUE;
}
public class TestDemo {
    public static void main(String args[]) {
        Color red = Color.RED;
        System.out.println("枚举对象序号：" + red.ordinal());
        System.out.println("枚举对象名称：" + red.name());
    }
}
```

注意：
* 枚举中定义的**构造方法不能使用public声明**，如果**没有无参构造**，要**手工调用构造传递函数**。
* **枚举对象**必须要放在**首行**，随后才可以定义属性、构造、普通方法等结构。
```java
enum Sex {
    MALE("男"),FEMALE("女");
    private String title;
    private Sex(String title) {
        this.title = title;
    }
    public String toString() {
        return this.title;
    }
}
```

请解释一下**enum**和**Enum**的关系：
**enum**时JDK1.5之后定义的新**关键字**，主要**用于定义枚举类型**，在Java中每一个使用**enum定义的枚举类型**实际上都表示一个类**默认继承了Enum类**。

## 8.6 Annotation
在Java SE为了方便用户编写代码，提供了三种最常用的基础注解定义：
1. **准确的覆写：@Override**
2. **声明过期操作：@Deprecated**
3. **压制警告：@SuppressWarnings**

## 8.7 接口定义加强
从**JDK1.8**开始，可以在**接口**中定义**普通方法（使用default声明）**与**静态方法（使用static声明）**。

## 8.8 Lambda表达式
Lambda表达式有如下几种形式：
1. 方法主体为一个表达式：`(params) -> expression;`
2. 方法主体为一行执行代码：`(params) -> statements;`
3. 方法主体需要编写多行代码：`(params) -> {statements}`

例子：
```java
interface IMessage {
    public int add(int x,int y);
}
public class TestDemo {
    public static void main(String args[]) {
        fun((s1,s2) -> {
            return s1 + s2;
        });
    }
    public void fun(IMessage msg) {
        System.out.println(msg.add(10,20));
    }
}
```

## 8.9 方法引用
对于方法引用，Java 8一共定义了以下四种操作形式：
* 引用静态方法：`类名称 :: static 方法名称;`
* 引用某个对象的方法：`实例化对象 :: 普通方法;`
* 引用特定类型的方法：`特定类 :: 普通方法;`
* 引用构造方法：`类名称 :: new`

## 8.10 内建函数式接口
在JDK1.8之后提供了一个新的开发包：**java.util.Function**，在这个包中提供了**4个核心的函数式接口**。
1. **功能型接口（Function)：接收一个参数，并且返回一个处理结果。**
```java
@FunctionalInterface
public interface Function<T,R> {
    public R apply(T t);
}
```

2. **消费型接口（Consumer）：接收数据，不返回处理结果。**

```java
@FunctionalInterface
public interface Consumer<T> {
    public void accept(T t);
}
```
3. **供给型接口（Supplier）：不接受参数，可以返回结果。**

```java
public interface Supplier<T> {
    public T get();
}
```
4. **断言型接口（Predicate）：进行判断操作使用。**

```java
@FunctionalInterface
public interface Predicate<T> {
    public boolean test(T t);
}
```