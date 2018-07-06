# java基础知识梳理

## Java上下转型：编译看左边，运行看右边

## List集合
#### ArrayList
- 基于数组的实现访问元素效率快（基于数组索引访问元素），插入删除数据效率慢（每次都要涉及多个元素位置发生偏移）。每次插入都要判断是否需要扩容，超出则每次以1.5 - 倍扩容，初始化时未指定空间大小，在第一次添加元素时会指定默认空间大小10，建议没出初始化ArrayList时指定空间大小，避免空间浪费问题。
- 支持快速随机访问，实现了RandomAccess接口（控接口）。
#### Vector
- 基于数组的实现，支持快速访问，是线程安全的，其扩容机制不完善，每次扩容原来容量大小的2倍
#### Stack
- 继承自Vector，在其基础上添加了栈的方法
#### LinkedList
- 基于链表实现，插入删除数据效率快（只需修改前后指针的指向即可），访问元素效率慢
- LinkedList可以当做栈和队列来使用，LinkedList实现了Deque接口，Deque接口又继承自Queue接口
#### SynchronizedList
- 继承自SynchronizedCollection，使用装饰着模式为List加了锁，从而使List同步安全，代替Vector和Stack。

### 集合区别
- List、Set继承自Collection接口，Map本身是个顶层接口。
- List有序，可重复，检索数据效率高，插入删除效率低，涉及多个元素位置变化。支持普通for循环遍历，增强for遍历，迭代器遍历,此处只提供迭代器遍历相关代码
```java
Iterator<Object> it = list.iterator();
while(it.hasNext()) {
  System.out.println((Object)it.next);
}
```
- Set无序，不可重复，重复会覆盖之前的，位置有HashCode决定，加入Set中的Object必须重写equal是（），检索数据效率低，插入删除效率高，不涉及元素位置变化。可使用迭代器遍历和增强for遍历。
###### 迭代器
```java
Set<String> set=new HashSet<>();
set.add("qqq");
set.add("www");
set.add("sss");
set.add("aaa");
Iterator<String> iterable = set.iterator();
while (iterable.hasNext()){
    System.out.println(iterable.next());
}
```
###### 增强for
```java
Set<String> set=new HashSet<>();
set.add("qqq");
set.add("www");
set.add("sss");
set.add("aaa");
for (String o : set) {
   System.out.println(o);
}
```
- HashSet数据无序，唯一，key可为null，放入的对象必须实现hashCode（），因为放入的对象是以hashCode码作为标识
- TreeSet是二叉树，有序，不允许存null值

- Map适合存储键值对数据。
- HashMap基于哈希表实现，散列表处理：开放定址法，链表法，HashMap使用链表法
- TreeMap非线程安全，基于红黑树（平衡二叉树）实现

- 线程安全类：Vector，Hashtable，SynchronizedList（代替Vector），StringBuffer
- 非线程安全类LinkedList，ArrayList，HashSet，HashMap，StringBuilder

## java类加载过程
###### Student student = new Student();
- 先找到Student.class文件，并加载到内存
- 执行类中static代码块
- 堆内存中开辟空间分配内存地址
- 堆内存中建立对象特有属性，并进行默认初始化
- 对属性显示初始化
- 对对象构造代码块初始化
- 对对象进行与之对应的构造函数进行初始化
- 将内存地址赋给栈内存中的student变量



## 相关链接

[Link](https://github.com/dannycx/dcxing111.github.io/blob/master/activity%E8%BF%94%E5%9B%9E%E6%95%B0%E6%8D%AE)

[Activity生命周期](https://github.com/dannycx/knowledge/blob/master/ActivityLifecycle.md)

[github代码上传](https://github.com/dannycx/knowledge/blob/master/github.md)

[View绘制](https://github.com/dannycx/knowledge/blob/master/ViewDrawingProcess.md)

[Android特性](https://github.com/dannycx/knowledge/edit/master/android_features.md)
