# 集合
0.集合结构图如下:
	![image](https://github.com/longchenwen/mainshi/blob/master/src/img/collection.jpg)		
## 一.Map容器包含:
	1.HashMap
	2.TreeMap
	3.LinkedHashMap
	4.HashTable
	5.ConcurrentHasMap
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
	
## 在多并发情况下不适用HashMap,HashTable,使用ConcurrentHasMap的原因?
* 线程不安全的HashMap,因为多线程环境下，使用Hashmap进行put操作会引起死循环，导致CPU利用率接近100%，所以在并发情况下不能使用HashMap.
* 效率低下的HashTable容器,HashTable容器使用synchronized来保证线程安全，但在线程竞争激烈的情况下HashTable的效率非常低下。因为当一个线程访问	HashTable的同步方法时，其他线程访问HashTable的同步方法时，可能会进入阻塞或轮询状态。如线程1使用put进行添加元素，线程2不但不能使用put方法添加元素，并且也不能使用get方法来获取元素，所以竞争越激烈效率越低。
### ConcurrentHasMap 锁分段技术:
* HashTable容器在竞争激烈的并发环境下表现出效率低下的原因，是因为所有访问HashTable的线程都必须竞争同一把锁，那假如容器里有多把锁，每一把锁用于锁容器其中一部分数据，那么当多线程访问容器里不同数据段的数据时，线程间就不会存在锁竞争，从而可以有效的提高并发访问效率，这就是ConcurrentHashMap所使用的锁分段技术，首先将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。有些方法需要跨段，比如size()和containsValue()，它们可能需要锁定整个表而而不仅仅是某个段，这需要按顺序锁定所有段，操作完毕后，又按顺序释放所有段的锁。这里“按顺序”是很重要的，否则极有可能出现死锁，在ConcurrentHashMap内部，段数组是final的，并且其成员变量实际上也是final的，但是，仅仅是将数组声明为final的并不保证数组成员也是final的，这需要实现上的保证。这可以确保不会出现死锁，因为获得锁的顺序是固定的.
![img](https://github.com/longchenwen/mainshi/blob/master/src/img/concurrentHasmap%E7%BB%93%E6%9E%84%E5%9B%BE.png)
结构如下图所示:
	
## 二.List集合主要成员:
	1.Arraylist
	2.LinkedList
## ArrayList 和 LinkedList 的区别是什么？
	1.Vector的方法都是同步的(Synchronized),是线程安全的(thread-safe)，而ArrayList的方法不是，由于线程的同步必然要影响性能，因此,ArrayList的		性能比Vector好。 
	2.当Vector或ArrayList中的元素超过它的初始大小时,Vector会将它的容量翻倍,而ArrayList只增加50%的大小，这样,ArrayList就有利于节约内存空间。
## Set集合主要成员:
	1.HashSet(无序)
	2.LinkedHashSet
	3.TreeSet(有序)
	
