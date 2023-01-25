# 13 Java类集框架

## 13.1 类集框架简介

## 13.2 单对象保存父接口：Collection
**java.util.Collection**是进行**单对象保存的最大父接口**，即每次利用**Collection接口**都**只能保存一个对象信息**，单对象保存父接口定义如下：
`public interface Collection<E> extends Iterable<E>`

**在Collection接口中定义了9个常用操作方法**：
1. 向集合里面保存数据：`public boolean add(E e)`
2. 追加一个集合：`public boolean addAll(Collection<? extends E> c)`
3. 清空集合，根元素为null：`public void clear()`
4. 判断是否包含指定的内容，需要equals()支持：`public boolean contains(Object o)`
5. 判断是否是空集合（不是null）：`public boolean isEmpty()`
6. 删除对象，需要equals()支持：`public boolean remove(Object o)`
7. 取得集合中保存的元素个数：`public int size()`
8. 将集合变为对象数组保存：`public Object[] toArray()`
9. 为Iterator接口实例化（Iterable接口定义）：`public Iterator<E> iterator()`

* Collection接口虽然是单对象集合的最大父接口，但是**Collection接口**本身存在一个问题，它**无法区分保存的数据是否重复**。所以在实际的开发中，往往使用它的两个子接口：**List子接口（数据允许重复）**和**Set子接口（数据不允许重复）**。

![Collection及其子接口继承关系](image/13.2Collection%E5%8F%8A%E5%85%B6%E5%AD%90%E6%8E%A5%E5%8F%A3%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB.png)

## 13.3 List子接口
**List子接口**最大的功能就是里面所**保存的数据可以存在重复内容**，List子接口扩充的方法如下：
1. 取得索引编号的内容：`public E get(int index)`
2. 修改指定索引编号的内容：`public E set(int index,E element)`
3. 为ListIterator接口实例化：`public ListIterator<E> listIterator()`

* 在使用**List接口**时可以利用**ArrayList**或**Vector**两个子类来进行**接口对象的实例化**。

**使用ArrayList**：
```java
import java.util.List;
import java.util.ArrayList;

public class TestDemo{
	public static void main(String args[]){
		List<String> all = new ArrayList<String>();
		System.out.println("长度：" + all.size() + "\t是否为空：" + all.isEmpty());
		all.add("Hello");
		all.add("LiuFeng");
		all.add("World!");
		System.out.println("长度：" + all.size() + "\t是否为空：" + all.isEmpty());
		for (int x = 0;x < all.size();x++){
			String str = all.get(x);
			System.out.println(str);
		}
	}
}
```

* 如果要在**集合中保存自定义的对象**，如果要正确**使用操作集合中的remove()和contains()方法**，必须保证在**自定义的类中明确覆写了equals()方法**。

List接口的子类**ArrayList**和**LinkedList**有什么区别：
* **ArrayList**采用**顺序式的结果进行数据保存**，并且可以**自动生成相应的索引信息**。
* **LinkedListed**集合保存的是**前后元素**，也就是说，它每一个节点中保存的是两个元素对象，一个是它的上一个节点，另一个是它对应的下一个节点，所以**LinkedList要比ArrayList占用更多的内存空间**，同时LinkedList比ArrayList多实现一个Queue队列数据接口。

ArrayList与Vector类最大的区别就是**Vector类中部分方法使用synchronized关键字声明（同步操作）**。

![ArrayList与Vector子类的区别](image/13.3ArrayList%E4%B8%8EVector%E5%AD%90%E7%B1%BB%E7%9A%84%E5%8C%BA%E5%88%AB.png)

## 13.4 Set子接口
* 在**Collection接口**中还有一个比较常用的**Set子接口**，Set子接口并没有像List接口一样进行了大量的扩充，而是**简单的继承了Collection接口**，也就是说**Set子接口无法使用get()方法通过索引取得保存的数据**。
* **Set接口**下有两个**常用的子类**，分别是：**HashSet（散列存放）**和**TreeSet（有序存放，默认升序）**。
* **TreeSet子类**保存的**内容可以进行排序**，但是排序是**依靠Comparable接口来实现的**，所以如果对象要存在TreeSet子类中，需要实现java.lang.Comparable接口。

**TreeSet可以利用Comparable接口实现重复元素的判断**，但是这样的操作只适合支持排序类的操作环境，而**HashSet子类**中如果要消除重复元素，必须依**靠Object类中的两个方法**：
1. **取得哈希码**：`public int hashCode()`
2. **对象比较**：`public boolean equals(Object obj)`

## 13.5 集合输出
* **Iterator（迭代器）**是**集合输出操作**中最为常用的接口，**在Collection接口中也提供了直接为Iterator接口实例化的方法**`public Iterator<E> interator()`，所以**任何集合类型都可以转换为Iterator接口输出**。

Iterator接口中定义的方法：
1. 判断是否还有内容：`public boolean hasNext()`
2. 取出当前内容：`public E next()`

**使用Iterator输出集合**：
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Iterator;

public class TestDemo{
	public static void main(String args[]){
		List<String> all = new ArrayList<String>();
		all.add("Hello");
		all.add("Hello");
		all.add("LiuFeng");
		Iterator<String> iter = all.iterator();
		while (iter.hasNext()){
			System.out.println(iter.next());
		}
	}
}
```

虽然利用**Iterator接口可以实现集合的迭代输出**，但是Iterator本身只能**由前向后**的输出，所以为了让输出变得更加灵活，在类集框架中提供了**ListIterator接口**，可以实现**双向迭代**，**ListIterator属于Iterator的子接口**，常用方法如下：
1. 判断是否有前一个元素：`public boolean hasPrevious()`
2. 取出前一个元素：`public E previous()`
3. 向集合追加数据：`public void add(E e)`
4. 修改集合数据：`public void set(E e)`

**使用ListIterator输出集合**：
```java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

public class TestDemo{
	public static void main(String args[]){
		List<String> all = new ArrayList<String>();
		all.add("First");
		all.add("Second");
		all.add("Third");
		ListIterator<String> iter = all.listIterator();
		System.out.println("从前向后输出：");
		while (iter.hasNext()){
			System.out.print(iter.next() + "\t");
		}
		System.out.println("\n" + "从后向前输出：");
		while (iter.hasPrevious()){
			System.out.print(iter.previous() + "\t");
		}
	}
}
```

## 13.6 偶对象保存：Map接口
**Collection每次只能保存一个对象**，属于**单值保存父接口**，在类集中提供了**保存偶对象的集合**：**Map集合**，利用Map集合可以**保存一对关联数据（key = value）**，在**java.util.Map接口**中常用方法如下：
1. 向集合中保存数据：`public V put(K key,V value)`
2. 根据key查找对应的value数据：`public V get(Object key)`
3. 将Map集合转换为Set集合：`public Set<Map.Entry<K,V>>entrySet()`
4. 取出全部的key：`public Set<K> keySet()`

* Map接口有两个常用的子类：**HashMap**和**Hashtable**。

**HashMap子类的使用**：
```java
import java.util.Map;
import java.util.HashMap;

public class TestDemo{
	public static void main(String args[]){
		Map<String,Integer> map = new HashMap<String,Integer>();
		map.put("壹",1);
		map.put("贰",2);
		map.put("叁",3);
		map.put("叁",33);
		map.put("空",null);
		map.put(null,0);
		System.out.println(map);
	}
}
```

**Map有如下特点**：
* 使用**HashMap**定义的Map集合是**无序存放**的。
* 如果发现了**重复的key**，会进行覆盖，用**新的内容覆盖旧的内容**。
* 使用HashMap子类保存数据时**key或value可以保存为null**。
* **Map**保存数据**主要**不是进行输出，而是**为了查找**。

* **Hashtable类中key或者value不允许为null（会出现java.lang.NullPointerException）。**

**HashMap与Hashtable的区别**：

![HashMap与Hashtable的区别](image/13.6HashMap%E4%B8%8EHashtable%E7%9A%84%E5%8C%BA%E5%88%AB.png)

* 使用Collection集合保存数据时所有的对象都是直接保存的，而用**Map集合**保存数据时，所**保存的key和value会自动包装为Map.Entry接口对象**，如果利用**Iterator进行迭代**，那么**next()方法读取数据时返回的都是Map.Entry接口对象**，此接口定义为：`public static interface Map.Entry<K,V>{}`

Map.Entry接口中常用的方法：
1. 取得数据中的key：`public K getKey()`
2. 取得数据中的value：`public V getValue()`
3. 修改数据中的value：`public V setValue(V value)`

利用Iterator实现Map接口的输出：
```java
import java.util.Map;
import java.util.Hashtable;
import java.util.Set;
import java.util.Iterator;

public class TestDemo{
	public static void main(String args[]){
		Map<String,Integer> map = new Hashtable<String,Integer>();
		map.put("壹",1);
		map.put("贰",2);
		map.put("叁",3);
		map.put("叁",33);
		Set<Map.Entry<String,Integer>> set = map.entrySet();
		Iterator<Map.Entry<String,Integer>> iter = set.iterator();
		while (iter.hasNext()){
			Map.Entry<String,Integer> data = iter.next();
			System.out.println(data.getKey() + "\t" + data.getValue());
		}
	}
}
```
* **Map接口可以使用任意类型来充当key和value**，但是要注意，作为**key的自定义类**必须要**覆写hashCode()方法和equals()方法**，因为要依靠这两个方法**判断key元素是否重复**。

## 13.7 Stack子类
**栈**是一种**动态对象数组**，采用**先进后出**的数据结构形式，此类定义为：`public class Stack<E> extends Vector<E>`

**java.util.Stack类**是Vector的子类，**Stack类常用方法如下**：
1. 数据入栈：`public E push(E item)`
2. 数据出栈，如果栈中没有数据，则调用此方法会抛出空栈异常（EmptyStackException）：`public E pop()`

**观察栈的操作**：
```java
import java.util.Stack;

public class TestDemo{
	public static void main(String args[]){
		Stack<String> all = new Stack<String>();
		all.push("Hello");
		all.push("Liu");
		all.push("Feng");
		System.out.println(all.pop());
		System.out.println(all.pop());
		System.out.println(all.pop());
	}
}
```

## 13.8 Properties子类
**Properties类**本身属于**Hashtable的子类**，但是由于Properties类都使用String数据类型进行操作，所以Properties类主要使用本类所定义的方法：
1. 设置属性：`public Object setProperty(String key,String value)`
2. 取得属性，如果key不存在则返回null：`public String getProperty(String key)`
3. 取得属性，如果key不存在则返回默认值：`public String getProperty(String key,String defaultValue)`
4. 通过输出流保存属性内容，输出的同时可以设置注释信息：`public void store(OutputStream out,String comments) throws IOException`
5. 通过输入流读取属性内容：`public void load(InputStream inStream) throws IOException`

## 13.9 Collections工具类
**java.util.Collections类**提供了很多**操作List、Set、Map集合的方法**，常用方法如下：
1. 实现集合数据追加：`public static <T> boolean addAll(Collection<? super T> c,T ... elements)`
2. 使用二分查找法查找集合数据：`public static <T> int binarySearch(List<? extends Comparable<? super T>> list,T key)`
3. 集合复制：`public static <T> void copy(List<? super T> dest,List<? extends T> src)`
4. 集合反转：`public static void reverse(List<?> list)`
5. 集合排序：`public static <T extends Comparable<? super T>> void sort(List<T> list)`

## 13.10 数据流
* 在Iterable接口中提供了一个实现集合输出的方法：`default void forEach(Consumer<? super T> action)`

利用forEach()方法输出：
```java
import java.util.List;
import java.util.ArrayList;

public class TestDemo{
	public static void main(String args[]){
		List<String> all = new ArrayList<String>();
		all.add("Hello");
		all.add("Liu");
		all.add("Feng");
		all.forEach(System.out :: println);
	}
}
```

在**JDK 1.8中为了简化集合数据处理的操作**，专门提供了一个**数据流操作接口**：**java.util.stream.Stream**，这个类可以**利用Collection接口中提供的default型方法实现Stream接口的实例化操作**：`default Stream<E> stream()`，Stream类中常用方法如下：
1. 返回元素个数：`public long count()`
2. 消除重复元素：`public Stream<T> distinct()`
3. 利用收集器接收处理后的数据：`public <R,A> R collect(Collector<? super T,A,R> collector)`
4. 数据过滤（设置断言型函数式接口）：`public Stream<T> filter(Predicate<? super T> predicate)`
5. 数据处理操作：`pubilc <R> Stream<R> map(Function<? super T,? extends R> mapper)`
6. 设置跳过的数据行数：`public Stream<T> skip(long n)`
7. 设置取出的数据个数：`public Stream<T> limit(long maxSize)`
8. 数据查询，要求全部匹配：`public boolean allMatch(Predicate<? super T> predicate)`
9. 数据查询，匹配任意一个：`public boolean anyMatch(Predicate<? super T> predicate)`
10. 或操作：`default Predicate<T> or(Predicate<? super T> other)`
11. 与操作：`default Predicate<T> and(Predicate<? super T> other)`

取消集合中的重复元素：
```java
import java.util.stream.Stream;
import java.util.stream.Collectors;
import java.util.List;
import java.util.ArrayList;

public class TestDemo{
	public static void main(String args[]){
		List<String> all = new ArrayList<String>();
		all.add("Hello");
		all.add("Hello");
		all.add("LiuFeng");
		all.add("World");
		Stream<String> stream = all.stream();
		List<String> newAll = stream.distinct().collect(Collectors.toList());
		newAll.forEach(System.out :: println);
	}
}
```