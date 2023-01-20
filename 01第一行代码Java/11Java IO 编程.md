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


