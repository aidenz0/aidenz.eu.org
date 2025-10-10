# List、Set、Queue和Map

![](/asset/images/Pasted%20image%2020250916095137.png)

Java集合框架分类两条大的支线：
1. Collection，主要由 List、Set、Queue 组成：

- List 代表有序、可重复的集合，典型代表就是封装了动态数组的 ArrayList和封装了链表的LinkedList；
- Set 代表无序、不可重复的集合，典型代表就是 HashSet 和 TreeSet；
- Queue 代表队列，典型代表就是双端队列 ArrayDeque，以及优先级队列PriorityQueue。
2. Map，代表键值对的集合，典型代表就是HashMap。
## 1. list
List 的特点是存取有序，可以存放重复的元素，可以用下标对元素进行操作。
### Arraylist
```java
package collection.collection_demo;  
  
import java.util.ArrayList;  
  
public class ArrayList_demo {  
    public static void main(String[] args) {  
        ArrayList<String> lists = new ArrayList<String>();  
        lists.add("王二");  
        lists.add("沉默");  
        lists.add("陈清扬");  
        for (int i = 0; i < lists.size(); i++) {  
            System.out.println(lists.get(i));  
        }  
        System.out.println("--------------------");  
        for (String str : lists) {  
            System.out.println(str);  
        }  
        System.out.println("--------------------");  
        lists.remove(0);  
        for(String str : lists) {  
            System.out.println(str);  
        }  
        System.out.println("--------------------");  
        lists.set(0, "123");  
        for(String str : lists) {  
            System.out.println(str);  
        }  
        System.out.println("--------------------");  
    }  
  
}
```

- ArrayList 是由数组实现的，支持随机存取，也就是可以通过下标直接存取元素；
- 从尾部插入和删除元素会比较快捷，从中间插入和删除元素会比较低效，因为涉及到数组元素的复制和移动；
- 如果内部数组的容量不足时会自动扩容，因此当元素非常庞大的时候，效率会比较低。
### LinkedList
```java
package collection.collection_demo;  
  
import java.util.LinkedList;  
  
public class LinkedList_demo {  
    public static void main(String[] args) {  
        LinkedList<String> linkedList = new LinkedList<String>();  
        linkedList.add("a");  
        linkedList.add("b");  
        linkedList.add("c");  
        linkedList.add("d");  
        for(String s : linkedList){  
            System.out.println(s);  
        }  
        System.out.println("----------------------");  
        linkedList.remove("a");  
        for(String s : linkedList){  
            System.out.println(s);  
        }  
        System.out.println("----------------------");  
        linkedList.set(0, "e");  
        for(String s : linkedList){  
            System.out.println(s);  
        }  
        System.out.println("----------------------");  
    }  
}
```
- LinkedList 是由双向链表实现的，不支持随机存取，只能从一端开始遍历，直到找到需要的元素后返回；
- 任意位置插入和删除元素都很方便，因为只需要改变前一个节点和后一个节点的引用即可，不像 ArrayList 那样需要复制和移动数组元素；
- 因为每个元素都存储了前一个和后一个节点的引用，所以相对来说，占用的内存空间会比 ArrayList 多一些
### Vector 和 Stack
List 的实现类还有一个 Vector，是一个元老级的类，比 ArrayList 出现得更早。ArrayList 和 Vector 非常相似，只不过 Vector 是线程安全的，像 get、set、add 这些方法都加了 `synchronized` 关键字，就导致执行效率会比较低，所以现在已经很少用了。
Stack 是 Vector 的一个子类，本质上也是由动态数组实现的，只不过还实现了先进后出的功能（在 get、set、add 方法的基础上追加了 pop「返回并移除栈顶的元素」、peek「只返回栈顶元素」等方法），所以叫栈。
## 2. Set
Set 的特点是存取无序，不可以存放重复的元素，不可以用下标对元素进行操作，和 List 有很多不同。
### HashSet
HashSet 其实是由 HashMap 实现的，只不过值由一个固定的 Object 对象填充，而键用于操作。来简单看一下它的源码。
```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    private transient HashMap<E,Object> map;

    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
}
```
### LinkedHashSet

LinkedHashSet 是一种基于哈希表实现的 Set 接口，它继承自 HashSet，并且使用链表维护了元素的插入顺序。因此，它既具有 HashSet 的快速查找、插入和删除操作的优点，又可以维护元素的插入顺序。

### TreeSet
与 TreeMap 相似，TreeSet 是一种基于红黑树实现的有序集合，它实现了 SortedSet 接口，可以自动对集合中的元素进行排序。按照键的自然顺序或指定的比较器顺序进行排序。
## 3. Queue
### ArrayDeque
ArrayDeque 是一个基于数组实现的双端队列，为了满足可以同时在数组两端插入或删除元素的需求，数组必须是循环的，也就是说数组的任何一点都可以被看作是起点或者终点。



### LinkedList
LinkedList 一般应该归在 List 下，只不过，它也实现了 Deque 接口，可以作为队列来使用。等于说，LinkedList 同时实现了 Stack、Queue、PriorityQueue 的所有功能。

```Java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{}
```

换句话说，LinkedList 和 ArrayDeque 都是 Java 集合框架中的双向队列（deque），它们都支持在队列的两端进行元素的插入和删除操作。不过，LinkedList 和 ArrayDeque 在实现上有一些不同：

- 底层实现方式不同：LinkedList 是基于链表实现的，而 ArrayDeque 是基于数组实现的。
- 随机访问的效率不同：由于底层实现方式的不同，LinkedList 对于随机访问的效率较低，时间复杂度为 O(n)，而 ArrayDeque 可以通过下标随机访问元素，时间复杂度为 O(1)。
- 迭代器的效率不同：LinkedList 对于迭代器的效率比较低，因为需要通过链表进行遍历，时间复杂度为 O(n)，而 ArrayDeque 的迭代器效率比较高，因为可以直接访问数组中的元素，时间复杂度为 O(1)。
- 内存占用不同：由于 LinkedList 是基于链表实现的，它在存储元素时需要额外的空间来存储链表节点，因此内存占用相对较高，而 ArrayDeque 是基于数组实现的，内存占用相对较低。

因此，在选择使用 LinkedList 还是 ArrayDeque 时，需要根据具体的业务场景和需求来选择。如果需要在双向队列的两端进行频繁的插入和删除操作，并且需要随机访问元素，可以考虑使用 ArrayDeque；如果需要在队列中间进行频繁的插入和删除操作，可以考虑使用 LinkedList。
### PriorityQueue
PriorityQueue 是一种优先级队列，它的出队顺序与元素的优先级有关，执行 remove 或者 poll 方法，返回的总是优先级最高的元素。
```java
package collection.collection_demo.priorityQueue;  
  
import java.util.Comparator;  
import java.util.PriorityQueue;  
  
public class PriorityQueue_demo {  
    public static void main(String[] args) {  
  
        PriorityQueue<Student> priorityQueue = new PriorityQueue<>(new StudentComparator());  
        priorityQueue.offer(new Student("张三", 83, 23));  
        priorityQueue.offer(new Student("李四", 12, 45));  
        priorityQueue.offer(new Student("王五", 1, 4));  
  
        System.out.println(priorityQueue);  
  
        while (!priorityQueue.isEmpty()) {  
            Student student = priorityQueue.poll();  
            System.out.println(student );  
        }  
    }  
  
}  
class Student{  
    String name;  
    int ChineseScore, MathScore;  
  
    Student(String name, int chineseScore, int mathScore){  
        this.name=name;  
        this.ChineseScore = chineseScore;  
        this.MathScore = mathScore;  
    }  
    @Override  
    public String toString() {  
        return this.name + (this.ChineseScore + this.MathScore);  
    }  
}  
class StudentComparator implements Comparator<Student> {  
    @Override  
    public int compare(Student o1, Student o2) {  
        return  Integer.compare(o1.ChineseScore + o1.MathScore, o2.ChineseScore + o2.MathScore);  
    }  
}
```

Student 是一个学生对象，包含姓名、语文成绩和数学成绩。

StudentComparator 实现了 Comparator 接口，对总成绩做了一个排序。
## 4. Map
### hashMap
HashMap 实现了 Map 接口，可以根据键快速地查找对应的值——通过哈希函数将键映射到哈希表中的一个索引位置，从而实现快速访问。
这里先大致了解一下 HashMap 的特点：

- HashMap 中的键和值都可以为 null。如果键为 null，则将该键映射到哈希表的第一个位置。
- 可以使用迭代器或者 forEach 方法遍历 HashMap 中的键值对。
- HashMap 有一个初始容量和一个负载因子。初始容量是指哈希表的初始大小，负载因子是指哈希表在扩容之前可以存储的键值对数量与哈希表大小的比率。默认的初始容量是 16，负载因子是 0.75。
```java
package collection.collection_demo.hashmap;  
  
import java.util.HashMap;  
  
public class HashMap_demo {  
    public static void main(String[] args) {  
        HashMap<String, String> map = new HashMap<>();  
        map.put("张三", "zhangsan");  
        map.put("李四", "lisi");  
        map.put("王五", "wangwu");  
  
        String s = map.get("李四");  
        System.out.println(s);  
        for(String key : map.keySet()) {  
            System.out.println(key + " : " + map.get(key));  
        }  
        map.remove("李四");  
        System.out.println(map);  
  
    }  
}
```
### LinkedHashMap
HashMap 已经非常强大了，但它是无序的。如果我们需要一个**插入有序**的 Map，就要用到 LinkedHashMap。LinkedHashMap 是 HashMap 的子类，它使用链表来记录插入/访问元素的顺序。

LinkedHashMap 可以看作是 HashMap + LinkedList 的合体，它使用了哈希表来存储数据，又用了双向链表来维持顺序。

来一个简单的例子
```java
package collection.collection_demo.linkerhashmap;  
  
import java.util.HashMap;  
import java.util.LinkedHashMap;  
  
public class LinkedHashMap_demo {  
    public static void main(String[] args) {  
        LinkedHashMap<String, String> map = new LinkedHashMap<>();  
        map.put("E", "F");  
        map.put("B", "C");  
        map.put("A", "B");  
        map.put("D", "E");  
        map.put("C", "D");  
        System.out.println(map);  
        for (String key : map.keySet()) {  
            System.out.println(key + ":" + map.get(key));  
        }  
        System.out.println("---------------------------------");  
        map.remove("B");  
        for (String key : map.keySet()) {  
            System.out.println(key + ":" + map.get(key));  
        }  
        System.out.println("---------------------------------");  
    }  
}
```



### TreeMap
实现了 SortedMap 接口，可以自动将键按照自然顺序或指定的比较器顺序排序，并保证其元素的顺序。内部使用红黑树来实现键的排序和查找。
