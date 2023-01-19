# 10 Java常用类库

## 10.1 StringBuffer类
* StringBuffer类方便用户对字符串内容进行修改。
* **StringBuffer类**中**使用append()方法进行字符串的连接**：`public StringBuffer append(数据类型 变量)`
* 注意：String类可以直接**赋值实例化**，但是**StringBuffer类不行**。
* **String类**和**StringBuffer类**都属于**CharSequence接口的子类**。

String类对象和StringBuffer类对象的相互转换：
* **String类对象转化为StringBuffer类对象**：
1. 利用**StringBuffer**的**构造方法**：`public StringBuffer(String str)`
2. 利用**StringBuffer**中的**append()方法**：`public StringBuffer(String str)`
* **StringBuffer类对象转化为String类对象**：
1. 利用**StringBuffer**中的**toString()方法**：`public String toString()`
2. 利用**String**的**构造方法**：`public String(StringBuffer buffer)`

StringBuffer类中常用的操作方法：
1. **数据追加**：`public StringBuffer append(数据类型 变量)`
2. **字符串反转**：`public StringBuffer reverse()`
3. **在指定位置追加内容**：`public StringBuffer insert(int offset,数据类型 变量)`
4. **删除指定索引范围的内容**：`public StringBuffer delete(int start,int end)`

* Java中除了StringBuffer类还有StringBulider类，这两个类可以说基本上是一模一样，只不过**StringBuffer类中的方法**全部使用“**synchronized**”定义，属于**同步操作**，**线程安全**，而**StringBulider类中的方法**都属于**异步方法**，属于**非线程安全**的操作。

## 10.2 Runtime类
* 在**每一个JVM进程**中，都存在一个**运行时的操作类对象**，这个对象所属的类型就是**Runtime类**。

**Runtime类**中的常用方法：
1. **取得Runtime类实例化对象**（**只有这个是静态方法**）：`public static Runtime getRuntime()`
2. 返回**最大可用内存**大小：`public long maxMemory()`
3. 返回**所有可用内存**大小：`public long totalMemory()`
4. 返回**所有空余内存**大小：`public long freeMemory()`
5. 执行**垃圾回收**操作：`public void gc()`
6. **创建新的进程**：`public Process exec(String command) throws IOException`

* **GC**(Garbage Collector)**垃圾收集器**，指的是**释放无用的内存空间**，GC会由**系统不定期进行自动回收**，或者**调用Runtime类中的gc()方法手工回收**。

## 10.3 System类
**System类**中的常用方法：
1. **数组复制**操作：`public static void arraycopy(Object src,int srcPos,Object dest,int destPos,int length)`
2. **取得当前的日期时间**，以**long型数据返回**（单位为**毫秒**）：`public static long currentTimeMillis()`
3. **垃圾收集**：`public static void gc()`

* **Object类**中提供了一个**对象回收**的方法：`protected void finalize() throws Throwable`

## 10.4 对象克隆
* 在**Object类**中存在一个用于**对象克隆**的方法：`public Object clone throws CloneNotSupportedException`

注意：**由于clone()方法使用了protected访问权限**，所以如果想要实现克隆操作，需要**子类来覆写clone()方法**，才可以完成正常克隆操作，另外该对象一定要实现**Cloneable接口**，Cloneable接口没有任何方法，属于**标识接口**，**用于表示一种能力**。

一个对象克隆的例子，**注意Object子类一定要覆写clone()方法！**
```java
class Book implements Cloneable{
	private String title;
	private double price;
	public Book(String title,double price){
		this.title = title;
		this.price = price;
	}
	public Object clone() throws CloneNotSupportedException{
		return super.clone();
	}
	public String toString(){
		return "图书名称：" + this.title + "\t图书价格：" + this.price;
	}
}
public class TestDemo{
	public static void main(String args[]) throws Exception{
		Book bookA = new Book("Java开发",79.8);
		Book bookB = (Book)bookA.clone();
		System.out.println(bookB);
	}
}
```

## 10.5 数字操作类

* **java.lang.Math**类就是一个专门进行数学计算的操作类，它提供了一系列数学计算方法，在Math类里面提供的**一切方法都是static型方法**，可以**直接由类名称调用**。

* Math类中提供了一个四舍五入的操作方法：`public static long round(double a)`

**java.util.Random**类是一个**专门负责产生随机数的操作类**，此类常用的方法有：
1. **创建一个新的Random实例**：`public Random()`
2. 创建一个新的Random实例并设置一个种子数：`public Random(long seed)`
3. **产生一个小于指定边界的随机整数**：`public int nextInt(int bound)`

* 如果有两个非常大的数字要进行操作，其值已经超过了double型数据的取值范围，那么可以使用**大数字操作类**：**java.math.BigInteger**和**java.math.BigDecimal**，这**两个类都属于Number的子类**。

**大整数操作类BigInteger**中的基本操作方法：
1. **实例化BigInteger对象**：`public BigInteger(String val)`
2. **加法**操作：`public BigInteger add(BigInteger val)`
3. **减法**操作：`public BigInteger subtract(BigInteger val)`
4. **乘法**操作：`public BigInteger multiply(BigInteger val)`
5. **除法**操作（**不保留余数**）：`public BigInteger divide(BigInteger val)`
6. **除法**操作（**保留余数**），数组第一个元素是商，第二个元素是余数：`public BigInteger[] divideAndRemainder(BigInteger val)`

**大小数操作类BigDecimal**中与四舍五入相关的操作方法：
1. 遇到0.5时**向上进位**：`public static final int ROUND_HALF_UP`
2. 遇到0.5时**向下进位**：`public static final int ROUND_HALF_DOWN`
3. 传递一个double型数据：`public BigDecimal(double val)`
4. 传递一个String型数据：`public BigDecimal(String val)`
5. **除法**操作，**第一项为除数**，**第二项为保留小数位数**，**第三项为进位模式**：`public BigDecimal divide(BigDecimal divisor,int scale,int roundingMode)`

## 10.6 日期处理类
**java.util.Date类**主要方法有如下几个：
1. **实例化Date对象**：`public Date()`
2. 将数字变为**Date类对象**，**long为日期时间数据**：`public Date(long data)`
3. **将当前日期变为long型**：`public long getTime()`

如果要对显示的**日期时间进行格式转换**，可以通过**java.text.SimpleDateFormat类**完成，有关方法如下：
1. **传入日期时间标记实例化对象**：`public SimpleDateFormat(String pattern)`
2. 将**日期格式化为字符串数据**：`public final String format(Date date)`
3. 将**字符串格式化为日期数据**：`public Date parse(String source) throws ParseException`

一些**常用的日期时间标记**：
* **年：yyyy**
* **月：MM**
* **日：dd**
* **时：HH**
* **分：mm**
* **秒：ss**
* **毫秒：SSS**

一个关于日期格式转换的例子：
```java
import java.util.Date;
import java.text.SimpleDateFormat;
public class TestDemo{
	public static void main(String args[]) throws Exception{
		Date date = new Date();
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println(sdf.format(date));
		System.out.println(sdf.parse("1999-07-11 13:14:15"));
	}
}
```

**java.util.Calendar抽象类**可以分别**取得日期时间数字**，进行各种日期时间的计算操作，有关常量及方法如下：
1. **取得年**：`public static final int YEAR`
2. **取得月（索引从0开始）**：`public static final int MONTH`
3. **取得日**：`public static final int DAY_OF_MONTH`
4. **取得小时**：`public static final int HOUR_OF_DAY`
5. **取得分**：`public static final int MINUTE`
6. **取得秒**：`public static final int SECOND`
7. **取得毫秒**：`public static final int MILLISECOND`
8. **根据默认的时区实例化对象**：`public static Calendar getInstance()`
9. **判断一个日期是否在指定日期之后**：`public boolean after(Object when)`
10. **判断一个日期是否在指定日期之前**：`public boolean before(Object when)`
11. **返回给定日历字段的值**：`public int get(int field)`

## 10.7 比较器
**java.util.Arrays类**定义了与所有**数组有关的基本操作**：二分查找，相等判断，数组填充，有关的方法如下：
1. **判断两个数组是否相等**：`public static boolean equals(int[] a,int[] a2)`
2. **将指定内容填充到数组中（数组中元素全部变成指定内容）**：`public static void fill(int[] a,int val)`
3. **数组排序**：`public static void sort(Object[] a)`
4. **对排序后的数组进行检索（数组必须排序后才能使用该方法）**：`public static int binarySearch(int[] a,int key)`
5. **输出数组信息**：`public static String toString(Object[] o)`

**java.lang.Comparable接口**定义如下：
```java
public interface Comparable<T>{
    public int compareTo(T o);
}
```

* 在**Comparable接口**中只定义了一个**compareTo()方法**，此方法**只返回一个int型数据**，用户**覆写时**只需要**返回三个结果**：1（大于）、0（等于）、-1（小于）。
```java
import java.util.Arrays;
class Book implements Comparable<Book>{
	private String title;
	private double price;
	public Book(String title,double price){
		this.title = title;
		this.price = price;
	}
	public String toString(){
		return "图书名称：" + this.title + "\t图书价格：" + this.price + "\t";
	}
	@Override
	public int compareTo(Book o){
		if (this.price > o.price){
			return 1;
		} else if (this.price == o.price){
			return 0;
		} else {
			return -1;
		}
	}
}
public class TestDemo{
	public static void main(String args[]){
		Book books[] = new Book[]{
			new Book("Javak开发",79.8),
			new Book("JAVA WEB开发",69.8),
			new Book("Oracle开发",99.8),
			new Book("Android开发",89.8)
		};
		Arrays.sort(books);
		System.out.println(Arrays.toString(books));
	}
}
```

如果在类的设计之初没有实现Comparable接口，而且不想修改类的原始定义，则可以使用**java.util.Comparator接口（挽救比较器）**：
```java
public interface Comparator<T>{
    public int compare(T o1,T o2);
    public boolean equals(Object obj);
}
```

*如果利用**Comparator接口实现对象数组的排序操作**，需要**更换java.util.Arrays类中的排序方法**,使用新的排序方法：`public static <T> void sort(T[] a,Comparator<? super T> c)`

## 10.8 正则表达式
正则表达式（Regular Expression）规则：
1. 单个字符（数量1）
* 单个字符：`任意一个字符`
* 转义字符“\”：`\\`
* 制表符：`\t`
* 换行符：`\n`
* 点：`\.`
* 斜杠：`\\\`
2. 字符集（数量1）
* a,b,c中任意一个字符：`[abc]`
* 不是a，b，c中任意一个字符：`[^abc]`
* 任意一个小写字母：`[a-z]`
* 任意一个字母（不区分大小写）：`[a-zA-Z]`
* 任意一个数字：`[0-9]`
* 任意一个字母，数字，下划线：`[a-zA-Z_0-9]`
* 任意一个字符：`.`
3. 边界匹配（不要在Java中使用）
* 正则开始：`^`
* 正则结束：`$`
4. 数量表达式
* 0个或多个：`*`
* 1个或多个：`+`
* 0个或1个：`?`
* 刚好出现n次：`{n}`
* 大于等于n次：`{n,}`
* 大于等于n次且小于等于m次：`{n,m}`
5. 逻辑运算
* 设为一组：`()`
* 或者（满足其一即可）：`|`

**String类中方法参数为“regex”的都可以使用正则表达式**：
* 正则验证，使用指定字符串判断是否符合正则表达式结构：`public boolean matches(String regex)`
* 将满足正则标记的内容全部替换为新的内容：`public String replaceAll(String regex,String replacement)`
* 将满足正则标记的首个内容替换为新的内容：`public String replaceFirst(String regex,String replacement)`
* 按照指定的正则标记进行字符串的全部拆分：`public String[] split(String regex)`
* 按照指定的正则标记进行字符串的部分拆分：`public String[] split(String regex,int limit)`

**java.util.regex包**中提供了**Pattern**与**Matcher**类可以实现正则表达式的操作。

**java.util.regex.Pattern类**主要功能是进行**数据拆分**以及为**Matcher类对象实例化**，常用方法如下：
1. 编译正则表达式：`public static Pattern compile(String regex)`
2. 数据全部拆分：`public String[] split(CharSequence input)`
3. 数据部分拆分：`public String[] split(CharSequence input,int limit)`
4. 取得Matcher类对象：`public String[] Matcher(CharSequence input)`

**java.util.regex.Matcher类**主要功能是进行**数据的验证与替换**，常用方法如下：
1. 正则匹配：`public boolean matches()`
2. 全部替换：`public String replaceAll(String replacement)`
3. 替换首个：`public String replaceFirst(String replacement)`

## 10.9 反射机制
* 如果要通过实例化对象找到对象所在的类信息，可以通过Object类中的getClass()方法：`public final Class<?> getClass()`

**java.lang.Class类是反射操作的源头**，所有的反射操作都需要通过此类开始，此类有三种实例化方式：
1. **调用Object类中的getClass()方法**，调用此方法必须要有实例化对象。
2. **使用“类.class”取得**，这种情况可以不需要通过指定类的实例化对象取得。
3. **调用Class类中提供的方法**：`public static Class<?> forName(String className) throws ClassNotFoundException`

Class类中提供了如下的常用方法：
1. 通过字符串设置的类名称实例化Class对象：`public static Class<?> forName(String className) throws ClassNotFoundException`
2. 取得类实现的所有接口：`public Class<?> [] getInterfaces()`
3. 取得反射操作类的全名：`public String getName()`
4. 取得反射操作类名，不包括包名称：`public String getSimpleName()`
5. 取得反射操作类所在的包：`public Package getPackage()`
6. 取得反射操作类的父类：`public Class<? super T> getSuperclass()`
7. 反射操作的类是否是枚举：`public boolean isEnum()`
8. 反射操作的类是否是接口：`public boolean isInterface()`
9. 反射操作的类是否是数组：`public boolean isArray()`
10. 反射实例化对象：`public T newInstance() throws InstantiationException,IllegalAccessException`

**Class类中的newInstance()方法**可以利用反射实现**对象的实例化操作**,但是要求**类中必须有无参构造方法**：
```java
package com.lf.cn;
class Book{
	public Book(){
		System.out.println("Book类的无参构造方法！");
	}
	@Override
	public String toString(){
		return "《Java实战经典》";
	}
}
public class TestDemo{
	public static void main(String args[]) throws Exception{
		Class <?> cls = Class.forName("com.lf.cn.Book");
		Object obj = cls.newInstance();
		Book bk = (Book) obj;
		System.out.println(bk);
	}
}
```

* 使用**反射机制**实例化对象可以更好的**解耦合操作**，也可以**增加代码的复用性**，可以通过反射机制**配合一些配置文件**来**动态定义项目中所需要的类**。

使用Class类中的newInstance()方法可以实现反射实例化对象的操作但是有无参构造的限制，当要利用**有参构造方法**时，就要使用**java.lang.reflect.Constructor类**。

**Class类中取得类中的构造方法（返回Constructor对象）**的操作：
1. 取得全部构造方法：`public Constructor<?> [] getConstructors() throws SecurityException`
2. 取得指定参数类型的构造方法：`public Constructor<T> getConstructor(Class<?> ... parameterTypes) throws NoSuchMethodException,SecurityException`

在**java.lang.reflect.Comstructor类**中常用的方法如下：
1. 返回构造方法上所有抛出异常的类型：`public Class<?> [] getExceptionType()`
2. 取得构造方法的修饰符：`public int getModifiers()`
3. 取得构造方法的名字：`public String getName()`
4. 取得构造方法中的参数个数：`public int getParameterCount()`
5. 取得构造方法中的参数类型：`public Class<?> [] getParemeterTypes()`
6. 调用指定参数的构造实例化对象：`public T newInstance(Object ... initargs) throws InstantiationException,IllegalAccessException,IllegalArgumentException,InvocationTargetException`

**利用Constructor类明确调用类中的有参构造**：
```java
package com.lf.cn;

import java.lang.reflect.Constructor;

class Book{
	private String title;
	private double price;
	public Book(String title,double price){
		this.title = title;
		this.price = price;
	}
	public String toString(){
		return "图书名称：" + this.title + "\t图书价格：" + this.price;
	}
}
public class TestDemo{
	public static void main(String args[]) throws Exception{
		Class<?> cls = Class.forName("com.lf.cn.Book");
		Constructor <?> c = cls.getConstructor(String.class,double.class);
		Object obj = c.newInstance("Java开发",79.8);
		System.out.println(obj);
	}
}
```

反射调用方法可以先**利用Class类取得操作类的方法对象**，常用方法如下：
1. 取得类中的全部方法：`public Method[] getMethods() throws SecurityException`
2. 取得类中指定方法名称与参数类型的方法：`public Method getMethod(String name,Class<?> ... paremeterTypes) throws NoSuchMethodException,SecurityException`

在反射操作中，**每一个方法**都通过**java.lang.reflect.Method类表示**，Method类中的常用方法如下：
1. 取得方法的修饰符：`public int getModifiers()`
2. 取得方法的返回值类型：`public Class<?> getReturnType()`
3. 取得方法中定义的参数数量：`public int getParameterCount()`
4. 取得方法中定义的所有参数类型：`public Class<?> [] getParameterTypes()`
5. 反射调用方法并传递执行方法所需要的参数数据：`public Object invoke(Object obj,Object ... args) throws IllegalAccessException,IllegalArgumentException,InvocationTargetException`
6. 取得方法抛出的异常类型：`public Class<?> [] getExceptionTypes()`

**利用Method类明确调用类中的方法**：
```java
package com.lf.cn;

import java.lang.reflect.*;

class Book{
	private String title;
	private double price;
	public Book(String title,double price){
		this.title = title;
		this.price = price;
	}
	public void setTitle(String title){
		this.title = title;
	}
	public String toString(){
		return "图书名称：" + this.title + "\t图书价格：" + this.price;
	}
}
public class TestDemo{
	public static void main(String args[]) throws Exception{
		Class<?> cls = Class.forName("com.lf.cn.Book");
		Constructor<?> con = cls.getConstructor(String.class,double.class);
		Object obj = con.newInstance("Java开发",79.8);
		System.out.println(obj);
		Method setTitle = cls.getMethod("setTitle",String.class);
		setTitle.invoke(obj,"Oracle开发");
		System.out.println(obj);
	}
}
```

反射调用成员可以先利用Class类取得操作类的成员，常用方法如下：