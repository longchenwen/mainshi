# 集合

## 一.Map容器包含:
	1.HashMap
	2.TreeMap
	3.LinkedHashMap
	4.HashTable
	5.ConcurrentHasMap(不是map容器中)
## HashMap 和 Hashtable 有什么区别？
	1.HashMap线程不安全,Hashtable线程安全
	2.HashMap key,value 可以是null,Hashtable 不可以
	3.HashMap底层结构是组数+链表,HashTable底层是红黑树
## 如何决定使用 HashMap 还是 TreeMap?
	TreeMap<K,V>的Key值是要求实现java.lang.Comparable，所以迭代的时候TreeMap默认是按照Key值升序排序的；
	TreeMap的实现也是基于红黑树结构。
	而HashMap<K,V>的Key值实现散列hashCode(),分布是散列的均匀的，不支持排序；数据结构主要是桶(数组),链表或红黑树。
## HashMap和HashSet的区别:
	1.实现的接口不同
	2.HashMap是key,value存值,HashSet存对象
	3.HashMap中使用键对象来计算hashcode值,HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的		相等性，如果两个对象不同的话，那么返回false
	4.HashMap 比加快,key是唯一的
## HashMap的原理:
	HashMap是基于hashing(哈希)的原理，我们使用put(key, value)存储对象到HashMap中，
	使用get(key)从HashMap中获取对象。当我们给put()方法传递键和值时，
	我们先对键调用hashCode()方法，计算并返回的hashCode是用于找到Map数组的bucket位置来储存Node 对象。
	这里关键点在于指出，HashMap是在bucket中储存键对象和值对象，作为Map.Node
## HashMap put()步骤:
	1.对key求hash值,然后计算下标
	2.如果没有碰撞,直接放入桶中
	3.如果碰撞了,以链表的方式链接到后面
	4.如果链表长度超过阀值,就把链表转化成红黑树
	5.如果链表长度低于6,就把红黑树转回链表
	6.如果节点已经存在就替换旧值
	7.如果桶子满了(容量16*加载银子0.75),就需要resize(扩容2倍后重排)
## 如果两个键的hashcode相同，你如何获取值对象?(**)
	找到bucket位置之后，会调用keys.equals()方法去找到链表中正确的节点
## 你了解重新调整HashMap大小存在什么问题吗?
	多线程情况下,产生竞争,最终出现死循环(jdk.1.8之前),1.8jdk之后死循环已经解决,可能出现数据丢失问题
	
