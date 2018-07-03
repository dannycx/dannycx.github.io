# java基础
知识梳理

## Java上下转型：编译看左边，运行看右边

## List集合
** ArrayList
- 基于数组的实现访问元素效率快（基于数组索引访问元素），插入删除数据效率慢（每次都要涉及多个元素位置发生偏移）。每次插入都要判断是否需要扩容，超出则每次以1.5 - 倍扩容，初始化时未指定空间大小，在第一次添加元素时会指定默认空间大小10，建议没出初始化ArrayList时指定空间大小，避免空间浪费问题。
- 支持快速随机访问，实现了RandomAccess接口（控接口）。
** Vector
- 基于数组的实现，支持快速访问，是线程安全的，其扩容机制不完善，每次扩容原来容量大小的2倍
** Stack
- 继承自Vector，在其基础上添加了栈的方法
** LinkedList
- 基于链表实现，插入删除数据效率快（只需修改前后指针的指向即可），访问元素效率慢
- LinkedList可以当做栈和队列来使用，LinkedList实现了Deque接口，Deque接口又继承自Queue接口
** SynchronizedList
- 继承自SynchronizedCollection，使用装饰着模式为List加了锁，从而使List同步安全，代替Vector和Stack。
