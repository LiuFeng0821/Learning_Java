# 12 Java网络编程

## 12.1 网络编程
**网络编程**实质意义在于**数据的交互**，交互过程中一般就分为**服务器端**与**客户端**，分为以下两种形式：
1. **C/S结构（Client/Server）**：此类模式开发要编写两套程序，一套是**客户端代码**，一套是**服务器端代码**。这种结构**开发以及维护成本较高**，但是由于**使用自己的连接端口与交换协议**，所以**安全性比较高**，**C/S结构分为两种**：**TCP（传输控制协议）**以及**UDP（数据报协议）**。
2. **B/S结构（Browser/Server）**：不再单独开发客户端代码，**只开发一套服务器端程序**，**客户端将利用浏览器进行访问**，一般**安全性不高**，因为使用的是**公共的HTTP协议**以及**公共的80端口**。

## 12.2 开发第一个网络程序
**java.net包**中提供了**网络编程有关的工具开发类**，有两个主要的核心操作类：
* **ServerSocket类**：是一个**封装支持TCP协议**的操作类，主要工作在**服务器端**，用于**接收客户端请求**。
* **Socket类**：也是一个**封装了TCP协议**的操作类，每一个Socket对象都表示一个**客户端**。

**ServerSokcet类常用方法**：
1. 开辟一个指定的端口监听，一般使用5000以上的端口：`public ServerSocket(int port) throws IOException`
2. 服务器端接收客户端请求，通过Socket返回：`public Socket accept() throws IOException`
3. 关闭服务器端：`public void close() throws IOException`

**Socket类常用方法**：
1. 指定要连接的主机（IP地址）和端口：`public Socket(String host,int port) throws UnknownHostException,IOException`
2. 取得指定客户端的输出对象，使用PrintStream操作：`public OutputStream getOutputStream() throws IOException`
3. 从指定的客户端读取数据，使用Scanner操作：`public InputStream getInputStream() throws IOException`

## 12.3 网络开发的经典模型————Echo程序
**定义服务器端**：
```java
import java.io.PrintStream;
import java.net.ServerSocket;
import java.net.Socket;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		ServerSocket server = new ServerSocket(9999);
		System.out.println("等待客户端连接······");
		Socket client = server.accept();
		PrintStream out = new PrintStream(client.getOutputStream());
		out.println("Hello World!");
		out.close();
		client.close();
		server.close();
	}
}
```

**编写客户端**：
```java
import java.util.Scanner;
import java.net.Socket;

public class TestDemo{
	public static void main(String args[]) throws Exception{
		Socket client = new Socket("localhost",9999);
		Scanner scan = new Scanner(client.getInputStream());
		scan.useDelimiter("\n");
		if (scan.hasNext()){
			System.out.println(scan.next());
		}
		scan.close();
		client.close();
	}
}
```