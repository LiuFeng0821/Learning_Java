# 11 Java IO编程

## 11.1 文件操作类：File
在java.io包中，如果要进行文件的自身操作，可以依靠java.io.File类完成，常用操作方法如下：
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



