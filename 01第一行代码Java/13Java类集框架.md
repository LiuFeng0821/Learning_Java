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

