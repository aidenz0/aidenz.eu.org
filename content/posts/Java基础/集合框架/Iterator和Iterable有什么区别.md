+++
date = '2025-10-16T14:52:00+08:00'
draft = true
title = 'Iterator和Iterable有什么区别'
+++

Iterator 是个接口，JDK 1.2 的时候就有了，用来改进 Enumeration 接口：

- 允许删除元素（增加了 remove 方法）
- 优化了方法名（Enumeration 中是 hasMoreElements 和 nextElement，不简洁）
 Iterator 的源码：
 ```java
 public interface Iterator<E> {
    // 判断集合中是否存在下一个对象
    boolean hasNext();
    // 返回集合中的下一个对象，并将访问指针移动一位
    E next();
    // 删除集合中调用next()方法返回的对象
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }
}
 ```
JDK 1.8 时，Iterable 接口中新增了 forEach 方法。该方法接受一个 Consumer 对象作为参数，用于对集合中的每个元素执行指定的操作。该方法的实现方式是使用 for-each 循环遍历集合中的元素，对于每个元素，调用 Consumer 对象的 accept 方法执行指定的操作。
```java
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```
该方法实现时首先会对 action 参数进行非空检查，如果为 null 则抛出 NullPointerException 异常。然后使用 for-each 循环遍历集合中的元素，并对每个元素调用 action.accept(t) 方法执行指定的操作。由于 Iterable 接口是 Java 集合框架中所有集合类型的基本接口，因此该方法可以被所有实现了 Iterable 接口的集合类型使用。
它对 Iterable 的每个元素执行给定操作，具体指定的操作需要自己写Consumer接口通过accept方法回调出来。
```java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
list.forEach(integer -> System.out.println(integer));
```
写浅显点就是：
```java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
list.forEach(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) {
        System.out.println(integer);
    }
});
```


