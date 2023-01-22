# 11 Java IO编程

## 11.1 文件操作类：File
在java.io包中，如果要**进行文件的自身操作**，可以依靠**java.io.File类**完成，常用操作方法如下：
1. 传递完整文件操作路径：`public File(String pathname)`
2. 设置父路径与子文件路径：`public File(String parent,String child)`
3. 创建新文件：`public boolean createNewFile() throws IOexception`
4. 判断给定路径是否存在：`public boolean exists()`
5. 删除指定路径的文件：`public boolean delete()`
6. 取得当前路径的父路径：`public File getParentFile()`
7. 创建多级目录：`public boolean mkdirs()`
8. 取得文件大小：`public long length()`
9. 判断给定路径是否为文件：`public boolean isFile()`
10. 判断给定路径是否是目录：`public boolean isDirectory()`
11. 取得文件的最后一次修改时间：`public long lastModified()`
12. 列出指定目录的全部内容：`public String[] list()`
13. 列出所有的路径以及File类对象包装：`public File[] listFiles()`

关于File类有关的操作：
```java
import java.io.File;
import java.util.Date;
import java.math.BigDecimal;
import java.text.SimpleDateFormat;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		File file1 = new File("D:" + File.separator + "demo" + File.separator + "hello" + File.separator + "test.txt");
		if (!file1.getParentFile().exists()){
			file1.getParentFile().mkdirs();
		}
		file1.createNewFile();
		File file2 = new File("D:" + File.separator + "my.jpg");
		if (file2.exists()){
			System.out.println("是否是文件：" + file2.isFile());
			System.out.println("是否是目录：" + file2.isDirectory());
			System.out.println("文件大小：" + new BigDecimal((double)file2.length()/1024/1024).divide(new BigDecimal(1),2,BigDecimal.ROUND_HALF_UP) + "M");
			System.out.println("最后修改时间：" + new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date(file2.lastModified())));
		}
	}
}
```

## 11.2 字节流与字符流
**java.io.OutputStream**是一个专门**实现字节输出流的操作类**，常用方法如下：
1. 关闭字节输出流（**必须执行**）：`public void close() throws IOException`
2. 强制刷新：`public void flush() throws IOException`
3. 输出单个字节：`public abstract void write(int b) throws IOException`
4. 输出全部字节数组数据：`public void write(byte[] b) throws IOException`
5. 输出部分字节数组数据：`public void write(byte[] b,int off,int len) throws IOException`

**OutputStream**本身是一个**抽象类**，如果要**进行文件操作**，可以**使用FileOutputStream子类**操作：
1. 将内容输出到指定路径，如果文件已经存在，则使用新的内容覆盖旧的内容：`public FileOutputStream(File file) throws FileNotFoundException`
2. 将内容输出到指定路径，如果文件已经存在，则追加到新的内容到文件中：`public FileOutputStream(File file,boolean append) throws FileNotFoundException`

文件内容的输出：
```java
import java.io.OutputStream;
import java.io.FileOutputStream;
import java.io.File;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		File file = new File("D:" + File.separator + "demo" + File.separator + "mldn.txt");
		OutputStream output = new FileOutputStream(file);
		String str = "刘丰好帅";
		output.write(str.getBytes());
		output.close();
	}
}
```

**java.io.inputStream**是一个专门**实现字节数据读取的操作类**，常用方法如下：
1. 关闭字节输入流：`public void close() throws IOException`
2. 读取单个字节：`public abstract int read() throws IOException`
3. 将数据读取到字节数组中，同时返回读取长度：`public int read(byte[] b) throws IOExcepiton`
4. 将数据读取到部分字节数组中，同时返回读取的数据长度：`public int read(byte[] b,int off,int len) throws IOException`

**InputStream**本身是一个**抽象类**，如果要**进行文件读取**，可以**使用FileInputStream子类**操作：
1. 设置要读取文件数据的路径：`public FileInputStream(File file) throws FileNotFoundException`

文件内容的读取：
```java
import java.io.InputStream;
import java.io.FileInputStream;
import java.io.File;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		File file = new File("D:" + File.separator + "demo" + File.separator + "mldn.txt");
		if (file.exists()){
			InputStream input = new FileInputStream(file);
			byte data[] = new byte[1024];
			int len = input.read(data);
            input.close();
			System.out.println(new String(data,0,len));
		}
	}
}
```

**java.io.Writer类**可以**实现字符数组（包含字符串）的输出**，常用方法如下：
1. 关闭字节输出流：`public void close() throws IOException`
2. 强制刷新：`public void flush() throws IOException`
3. 追加数据：`public Writer append(CharSequence csq) throws IOException`
4. 输出字符串数据：`public void write(String str) throws IOException`
5. 输出字符数组数据：`public void write(char[] cbuf) throws IOException`

**java.io.FileWriter类**实现**Writer类对象的实例化**操作，常用方法如下：
1. 设置输出文件：`public FileWriter(File file) throws IOException`
2. 设置输出文件以及是否文件追加：`public FileWriter(File file,boolean append) throws IOException`

**使用Writer类实现内容的输出**：
```java 
import java.io.File;
import java.io.Writer;
import java.io.FileWriter;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		File file = new File("D:" + File.separator + "demo" + File.separator + "mldn.txt");
		Writer writer = new FileWriter(file);
		if (!file.getParentFile().exists()){
			file.getParentFile().mkdirs();
		}
		String str = "朱国针好美";
		writer.write(str);
		writer.close();
	}
}
```

**java.io.Reader类**可以**实现字符数据（包含字符串）的输入**操作，常用方法如下：
1. 关闭字节输入流：`public void close() throws IOException`
2. 读取单个数据：`public int read() throws IOException`
3. 读取单个字符：`public int read() throws IOException`
4. 读取数据到字符数组中，返回数组长度：`public int read(char[] cbuf) throws IOException`
5. 跳过字节长度：`public long Skip(long n) throws IOException`

**Reader类**是一个**抽象类**，要实现**文件数据的字符流读取**，可以利用**FileReader子类**为Reader类对象**实例化**，有关方法如下：
1. 定义要读取的文件路径：`public FileReader(File file) throws FileNotFoundException`

**使用Reader类实现数据的读取**：
```java
import java.io.File;
import java.io.Reader;
import java.io.FileReader;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		File file = new File("D:" + File.separator + "demo" + File.separator + "mldn.txt");
		if (file.exists()){
			Reader reader = new FileReader(file);
			char data[] = new char[1024];
			int len = reader.read(data);
			reader.close();
			System.out.println(new String(data,0,len));
		}
	}
}
```

* **字节流直接与终端文件进行数据交互**，而**字符流需要将数据经过缓冲区处理才与终端文件进行数据交互**。在使用**OutputStream输出数据**即使**最后没有关闭输出流**，**内容可以正常输出**，但是如果使用**Writer输出数据**，如果最后**不关闭输出流**，则在**缓冲中的数据不会被强制性清空**，**不会输出数据**，如果有特殊情况不能关闭字符输出流，**可以使用flush()方法强制清空缓冲区**。

* 虽然**字节流与字符流**表示两种不同的数据流操作，但是**两种数据流之间可以实现互相转换**，可以使用**InputStreamReader**以及**OutputStreamWriter**类来实现。

![InputStreamReader和OutputStreamWriter类的继承结构以及构造方法](image/11.3InputStreamReader%E5%92%8COutputStreamWriter%E7%B1%BB%E7%9A%84%E7%BB%A7%E6%89%BF%E7%BB%93%E6%9E%84%E4%BB%A5%E5%8F%8A%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95.png)

## 11.4 案例：文件复制
**实现文件复制的操作**：
```java
import java.io.File;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		if (args.length != 2){
			System.out.println("命令执行错误！");
			System.exit(1);
		}
		long start = System.currentTimeMillis();
		File inFile = new File(args[0]);
		if (!inFile.exists()){
			System.out.println("源文件不存在，请确认执行路径。");
			System.exit(1);
		}
		File outFile = new File(args[1]);
		if (!outFile.getParentFile().exists()){
			outFile.getParentFile().mkdirs();
		}
		InputStream input = new FileInputStream(inFile);
		OutputStream output = new FileOutputStream(outFile);
		int temp = 0;
		byte data[] = new byte[1024];
		while ((temp = input.read(data)) != -1){
			output.write(data,0,temp);
		}
		input.close();
		output.close();
		long end = System.currentTimeMillis();
		System.out.println("复制所花费时间：" + (end - start));
	}
}
```

## 11.5 字符编码
* 可以**利用System类中方法列出全部的系统属性**：`System.getProperties().list(System.out);`

## 11.6 内存流
假设某一种应用**需要进行IO操作**，但是又**不希望在磁盘上产生一些文件**时，就可以**将内存当作一个临时文件进行操作**。在Java中，针对内存操作提供了以下两组类：
* **字节内存流**：**ByteArrayInputStream（内存字节输入流）**和**ByteArrayOutputStream（内存字节输出流）**
* **字符内存流**：**CharArrayReader（内存字符输入流）**和**CharArrayWriter（内存字符输出流）**

![内存流继承关系](image/11.6%E5%86%85%E5%AD%98%E6%B5%81%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB.png)

利用内存流实现大小写的转换：
```java
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.OutputStream;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		String str = "Hello World!";
		byte data[] = str.getBytes();
		InputStream input = new ByteArrayInputStream(data);
		OutputStream output = new ByteArrayOutputStream();
		int temp = 0;
		while ((temp = input.read()) != -1){
			output.write(Character.toUpperCase(temp));
		}
		System.out.println(output);
		input.close();
		output.close();
	}
}
```

## 11.7 打印流
为了**更方便地实现输出操作**，**java.io包**提供了两个**数据打印流的操作类**：**PrintStream（打印字节流）**、**PrintWriter（打印字符流）**。

**PrintStream类的常用操作方法**：
1. 通过已有的OutputStream确定输出目标：`public PrintStream(OutputStream out)`
2. **输出各种常见数据类型**：`public void print(数据类型 参数名称)`
3. **输出各种常见数据类型**，并追加一个换行：`public void println(数据类型 参数名称)`

使用PrintStream类实现输出：
```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.PrintStream;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		PrintStream pu = new PrintStream(new FileOutputStream(new File("D:" + File.separator + "yootk.txt")));
		pu.println("Name:");
		pu.println("Liu Feng");
		pu.println(1 + 1);
		pu.println(1.1 + 1.1);
		pu.close();
	}
}
```

## 11.8 System类对IO的支持
**System类中与IO有关的三个对象常量**：
1. 显示器上错误输出：`public static final PrintStream err`
2. 显示器上信息输出：`public static final PrintStream out`
3. 键盘数据输入：`public static final InputStream in`

* `System.out.println()`代码从本质上来说就是**调用了System类中的out常量**，由于此**常量类型为PrintStream**，所以可以**继续调用PrintStream类中的print()和println()方法**，由此可见，**Java的任何输出操作实际上都是IO操作**。
* **System.err**是输出**不希望用户看见的异常**，而**System.out**是输出**希望用户看到的信息**。

**实现键盘的数据输入**：
```java
import java.io.InputStream;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		InputStream input = System.in;
		byte data[] = new byte[1024];
		System.out.println("请输入数据：");
		int len = input.read(data);
		System.out.println("输入数据为：" + new String(data,0,len));
	}
}
```

## 11.9 字符缓冲流：BufferedReader
为了可以**完整地进行数据的输入操作**，最好的做法就是**采用缓冲区的方式对输入的数据进行暂存**，而后程序可以利用输入流一次性读取内容，如果要使用缓冲区进行数据操作，java.io包中提供了以下两种操作类：
* **字符缓冲区流**：BufferedReader、BufferedWriter
* **字节缓冲区流**：BufferedInputStream、BufferedOutputStream

**BufferedReader类是Reader类的子类**，常用方法如下：
1. 设置字符输入流（**构造方法接收Reader类对象**）：`public BufferedReader(Reader in)`
2. **读取一行数据，默认以“\n”为分隔符**：`public String readLine() throws IOException`

**键盘输入的标准格式**：
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		BufferedReader buf = new BufferedReader(new InputStreamReader(System.in));
		System.out.print("请输入数据：");
		String str = buf.readLine();
		System.out.println("输入的数据：" + str);
	}
}
```

## 11.10 扫描流：Scanner
**java.util.Scanner类**可以方便地**实现数据的输入操作**，有关操作方法如下：
1. **接收InputStream输入流对象**：`public Scanner(InputStream source)`
2. 判断是否有数据输入（**支持正则验证**）：`public boolean hasNext()`
3. 取出输入数据，以String形式返回：`public String next()`
4. 判断是否有指定类型数据存在：`public boolean haxNextXxx()`
5. 取出指定数据类型的数据：`public 数据类型 nextXxx()`
6. 设置读取的分隔符：`public Scanner useDelimiter(String pattern)`

**利用Scanner实现键盘数据输入**：
```java
import java.util.Scanner;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		Scanner scanner = new Scanner(System.in);
		System.out.println("请输入内容:");
		if (scanner.hasNext()){
			System.out.println("输入内容为：" + scanner.next());
		}
		scanner.close();
	}
}
```

**利用Scanner实现文件内容的读取**：
```java
import java.util.Scanner;
import java.io.File;
import java.io.FileInputStream;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		Scanner scanner = new Scanner(new FileInputStream(new File("D:" + File.separator + "yootk.txt")));
		scanner.useDelimiter("\n");
		while (scanner.hasNext()){
			System.out.println(scanner.next());
		}
		scanner.close();
	}
}
```

## 11.11 对象序列化
* **Java中允许用户在程序运行过程中进行对象的创建**，但是这些**创建的对象都只保存在内存中**，这些**对象的生命周期都不会超过JVM进程**。在很多时候**可能需要JVM进程结束后对象依然可以被保存下来**，或者**在不同的JVM之间进行传输**，在这种情况下，就可以**采用对象序列化的方式进行处理**。
* **对象序列化**的本质就是**将内存中保存的对象数据转换为二进制数据流**进行传输操作，但并不是所有类都可以直接进行序列化操作，**要被序列化的对象所在的类要实现java.io.Serializable接口**，这是一个没有任何操作方法的**标识接口**。

**实现了Serializable接口并不意味着可以实现序列化操作**，在对象序列化与反序列化的操作中，**还需要两个类的支持**：
1. **序列化操作类**：`java.io.ObjectOutputStream`，将对象序列化为指定格式的二进制数据。
2. **反序列化操作类**：`java.io.ObjectInputStream`，将序列化的二进制对象信息转换回对象内容。

**java.io.ObjectOutputStream类**中常用方法：
1. 指定对象序列化的输出流：`public ObjectOutputStream(OutputStream out) throws IOException`
2. 序列化对象：`public final void writeObject(Object obj) throws IOException`

**java.io.ObjectInputStream类**中常用方法：
1. 指定对象反序列化的输入流：`public ObjectInputStream(InputStream in) throws IOException`
2. 从输入流中读取对象：`public final Object readObject() throws IOException,ClassNotFoundException`

实现序列化与反序列化对象操作：
```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.FileInputStream;
import java.io.ObjectOutputStream;
import java.io.ObjectInputStream;
import java.io.Serializable;

class Book implements Serializable{
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
		ser();
		dser();
	}
	public static void ser() throws Exception{
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("D:" + File.separator + "book.ser")));
		oos.writeObject(new Book("Java开发",69.8));
		oos.close();
	}
	public static void dser() throws Exception{
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream(new File("D:" + File.separator + "book.ser")));
		Object obj = ois.readObject();
		Book book = (Book) obj;
		System.out.println(book);
		ois.close();
	}
}
```

* Java中对象最有意义的内容就是对象的属性信息，所以在**默认情况下进行对象的序列化操作会将对象的所有属性信息全部序列化**，如果**某些属性在序列化的过程中不需要被保存**，可以使用**transient关键字**定义。