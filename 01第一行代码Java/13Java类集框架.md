# 13 Java类集框架

## 13.1 类集框架简介

## 13.2 单对象保存父接口：Collection
java.util.Collection是进行单对象保存的最大父接口，即每次利用Collection接口都只能保存一个对象信息，单对象保存父接口定义如下：
`public interface Collection<E> extends Iterable<E>`

在Collection接口中定义了9个常用操作方法：
1. 向集合里面保存数据：`public boolean add(E e)`
2. 追加一个集合：`public boolean addAll(Collection<? extends E> c)`
3. 清空集合，根元素为null：`public void clear()`
4. 判断是否包含指定的内容，需要equals()支持：`public boolean contains(Object o)`
5. 判断是否是空集合（不是null）：`public boolean isEmpty()`
6. 删除对象，需要equals()支持：`public boolean remove(Object o)`
7. 取得集合中保存的元素个数：`public int size()`
8. 将集合变为对象数组保存：`public Object[] toArray()`
9. 为Iterator接口实例化（Iterable接口定义）：`public Iterator<E> iterator()`

* Collection接口虽然是单对象集合的最大父接口，但是Collection接口本身存在一个问题，它无法区分保存的数据是否重复。所以在实际的开发中，往往使用它的两个子接口：List子接口（数据允许重复）和Set子接口（数据不允许重复）。

![Collection及其子接口继承关系](image/13.2Collection%E5%8F%8A%E5%85%B6%E5%AD%90%E6%8E%A5%E5%8F%A3%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB.png)

## 13.3 List子接口
