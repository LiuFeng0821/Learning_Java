# 1 Java简介

## 1.1 Java简介
* 所有的Java程序的后缀都是.java，该类文件需要先经过**编译**，编译之后会形成一个.class的文件(**字节码文件**)，**解释**程序通过**Java虚拟机**来完成(JVM)，在Java中所有的程序都是在JVM上运行的，Java实现**可移植性**靠的就是**JVM**。

## 1.2 JDK的安装与配置
要进行Java的程序开发，就需要Java开发工具包(JDK)的支持，可以在Oracle官网下载并安装。
* Java开发工具包(**JDK**)：包含     **开发工具和编译器**。
* Java运行环境(**JRE**)：包括**Java虚拟机、Java核心类库和支持文件**。
JDK里面的"**javac**.exe"和"**java**.exe"这两个命令要学会，命令所在路径默认为JDK的安装路径，所以我们在**Windows环境下**需要将该路径**配置到PATH环境属性中**。

## 1.3 第一个Java程序：永远的“Hello World！”
* 第一个Java程序：Hello.java
```java
public class Hello{
    public static void main(String args[]){
        System.out.println("Hello World!");
    }
}
```
当一个.java程序编写完成后，可以按照以下步骤执行：
1. **编译程序**：通过命令行进入程序所在路径，输入`javac *.java`，会**生成.class文件**(字节码文件)。
2. **解释程序**：将**生成的.class文件**在**JVM上执行**，输入`java *`。

## 1.4 第一个程序解释
* **类**是Java中的**基本组成元素**，所有的Java程序一定要被类管理，类的简单格式如下：
```java
[public] class 类名称 {}
```
在类前面可以有选择性的决定是否添加public，**类的定义有两种格式**：
1. **public class**定义：**类名称必须与文件名称保持一致**，否则将无法编译，**一个.java文件中只能有一个public class**。
2. **class**定义：**类名称可以和文件名称不一致**，会生成对应名称的.class文件，如果一个Java程序中包含多个class定义，则编译通过后会生成多个.class文件。
* **类名称**命名规范：**每一个单词的首字母大写**。
* **主方法：main()**
```java
public static void main(String args[]){
    编写程序代码;
}
```
系统输出分为换行和不换行：
1. 输出后加换行：`System.out.println();`
2. 输出后不加换行：`System.out.print();`

## 1.5 CLASSPATH
如果要执行某个Java程序，就一定要进入到.class文件中执行`java.exe`命令，但如果想要在不同目录下也可以执行指定目录下的.class文件，则可以通过配置CLASSPATH来解决。
* 配置CLASSPATH
```java
SET CLASSPATH = 路径
```
* CLASSPATH和JVM的关系：**CLASSPATH主要指的就是类的运行路径**，实际上用户**在执行`java`命令时**，对于本地的操作系统就相当于**启动了一个JVM**，**JVM在运行时**是**通过CLASSPATH来定位并加载所需要的类**，**默认情况**下**CLASSPATH指向**的就是**当前目录**。
```java
SET CLASSPATH = .
```