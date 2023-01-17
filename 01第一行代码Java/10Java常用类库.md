# 10 Java常用类库

## 10.1 StringBuffer类
* StringBuffer类方便用户对字符串内容进行修改。
* **StringBuffer类**中**使用append()方法进行字符串的连接**：`public StringBuffer append(数据类型 变量)`
* 注意：String类可以直接**赋值实例化**，但是**StringBuffer类不行**。
* **String类**和**StringBuffer类**都属于**CharSequence接口的子类**。

String类对象和StringBuffer类对象的相互转换：
* String类对象转化为StringBuffer类对象：
1. 利用StringBuffer的构造方法：`public StringBuffer(String str)`
2. 利用StringBuffer中的append()方法：`public StringBuffer(String str)`
* StringBuffer类对象转化为String类对象：
1. 利用StringBuffer中的toString()方法：`public String toString()`
2. 利用String的构造方法：`public String(StringBuffer buffer)`

