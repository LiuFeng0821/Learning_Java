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
3. 在**指定位置追加内容**：`public StringBuffer insert(int offset,数据类型 变量)`
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
