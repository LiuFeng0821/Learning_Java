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

* 虽然字节流与字符流表示两种不同的数据流操作，但是两种数据流之间可以实现互相转换，可以使用InputStreamReader以及OutputStreamWriter类来实现。

![InputStreamReader和OutputStreamWriter类的继承结构以及构造方法](image/11.3InputStreamReader%E5%92%8COutputStreamWriter%E7%B1%BB%E7%9A%84%E7%BB%A7%E6%89%BF%E7%BB%93%E6%9E%84%E4%BB%A5%E5%8F%8A%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95.png)

## 11.4 案例：文件复制
实现文件复制的操作：
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
* 可以利用System类中方法列出全部的系统属性：`System.getProperties().list(System.out);`

## 11.6 内存流
假设某一种应用需要进行IO操作，但是又不希望在磁盘上产生一些文件时，就可以将内存当作一个临时文件进行操作。在Java中，针对内存操作提供了以下两组类：

![]