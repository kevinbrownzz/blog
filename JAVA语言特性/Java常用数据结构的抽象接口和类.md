---
title: JAVA中常用数据结构的抽象接口和类
date: 2024-2-15 09:13:40
type: Java
categories: Java
cover: https://javaguide.cn/logo.svg
---

# 一些关于JAVA中常用数据结构的抽象接口和类

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/12/5/16ed483e9411ec98~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)
## 都定义在java.utils包中，import后可以ctrl+B转到定义进行详细的源码阅读，抽象数据结构的接口和类的源码都相对简单，对能力的提升有很大帮助。
### Stack的消失
自从Java 1.6开始，官方推荐使用java.util.Deque接口来实现栈的功能，而不是直接使用java.util.Stack类。Deque接口提供了一个更完整和一致的双端队列接口，可以通过其实现类如LinkedList等来模拟栈的行为。使用Deque作为栈的好处是它比Stack类有更好的性能和更广泛的功能。


### Collection 接口的实现类

ArrayList  
LinkedList  
HashSet  
LinkedHashSet  
TreeSet  
PriorityQueue  
ArrayDeque  
Vector  
Stack  
   
### List 接口的实现类

ArrayList  
LinkedList  
Vector  
Stack  

### Set 接口的实现类

HashSet  
LinkedHashSet  
TreeSet  
EnumSet：专门为枚举类型设计的集合类。  
CopyOnWriteArraySet：线程安全的Set，适用于并发访问。

### SortedSet 接口的实现类

TreeSet  

### NavigableSet 接口的实现类

TreeSet  
ConcurrentSkipListSet：一个线程安全的可排序集合。  

### Queue 接口的实现类
 
LinkedList  
PriorityQueue  
ArrayDeque  
ConcurrentLinkedQueue：一个适用于并发应用程序的队列。  
LinkedBlockingQueue  
ArrayBlockingQueue    
PriorityBlockingQueue  
DelayQueue：一个使用优先级队列实现的、其中元素只能在其指定的延迟时间之后被消费的队列。  
SynchronousQueue：一个不存储元素的阻塞队列，每个插入操作必须等待另一个线程的移除操作，反之亦然。  
LinkedTransferQueue：一个由链表结构组成的无界阻塞TransferQueue队列。  

### Deque 接口的实现类

ArrayDeque  
LinkedList  
LinkedBlockingDeque：一个线程安全的阻塞双端队列。  
ConcurrentLinkedDeque：一个适用于并发应用程序的双端队列。

### Map 接口的实现类

HashMap  
LinkedHashMap  
TreeMap  
Hashtable  
WeakHashMap：一个使用弱键的哈希表。  
IdentityHashMap：一个使用引用相等而非对象内容相等来比较键的哈希表。  
EnumMap：专门为枚举键设计的映射。  
ConcurrentHashMap：一个线程安全的哈希表。  

### SortedMap 接口的实现类

TreeMap

### NavigableMap 接口的实现类

TreeMap  
ConcurrentSkipListMap：一个线程安全的可排序映射


# 实现类中的常用操作
ArrayList

add(E e)：添加元素到列表末尾。  
get(int index)：获取指定位置上的元素。  
remove(int index)：移除指定位置上的元素。  
size()：返回列表中的元素数。  


LinkedList

addFirst(E e)和addLast(E e)：分别在列表开头和末尾添加元素。  
getFirst()和getLast()：分别获取列表开头和末尾的元素。  
removeFirst()和removeLast()：分别移除列表开头和末尾的元素。  


HashSet

add(E e)：添加元素到集合中。  
contains(Object o)：判断集合中是否包含指定的元素。  
remove(Object o)：从集合中移除指定的元素。  
size()：获取集合的大小。


TreeSet

add(E e)：向集合添加元素。  
first()：返回集合中最小的元素。    
last()：返回集合中最大的元素。  
higher(E e)：返回集合中大于给定元素的最小元素。  
lower(E e)：返回集合中小于给定元素的最大元素。  



HashMap


put(K key, V value)：将指定的值与此映射中的指定键关联。  
get(Object key)：返回指定键所映射的值。  
remove(Object key)：移除指定键的映射关系。    
containsKey(Object key)：判断映射中是否包含指定键的映射关系。


Stack
 
push(E item)：把项压入堆栈顶部。  
pop()：移除并返回堆栈顶部的对象。  
peek()：查看堆栈顶部的对象而不移除它。  
empty()：测试堆栈是否为空。

Queue

offer(E e)：添加元素到队列尾部。  
poll()：移除并返回队列头部的元素。  
peek()：获取但不移除队列头部的元素。

Deque

addFirst(E e)和addLast(E e)：在双端队列的前端/后端添加元素。  
removeFirst()和removeLast()：移除双端队列的前端/后端元素。  
getFirst()和getLast()：获取但不移除双端队列的前端/后端元素。

ConcurrentHashMap
  
put(K key, V value)：添加键值对到映射。  
get(Object key)：根据键获取值。    
remove(Object key)：根据键移除键值对。  
size()：返回映射的键值对数量