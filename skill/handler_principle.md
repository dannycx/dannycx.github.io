# Handler原理

## handler是什么
- 是消息处理的机制,我们可以发送消息，也可以处理消息
 
## 为什么要有Handler
- Android在设计的时候，封装了一套消息创建、传递、处理机制，如果不遵循这样的机制就没办法更新UI信息，就会抛出异常。
 
## handler怎么用
- post(Runnable);
- postDelayed(Runnable ,long);
- sentMessage
- sentMessageDelayed
 
## Android为什么要设置只能通过Handler机制更新UI
- 最根本的问题解决多线程并发的问题；
      假设如果在一个Activity中，有多个线程去更新UI，并且都没有加锁机制，会产生更新界面混乱；
      如果对更新UI的操作都加锁处理的话会性能下降
      更新UI的操作，都是在主线程的消息队列当中轮询处理的。
 
## handler的原理是什么
- handler封装消息的发送（主要包括消息发送给谁）
- Looper——消息封装的载体。
       （1）内部包含一个MessageQueue，所有的Handler发送的消息都走向这个消息队列；
       （2）Looper.loop方法，就是一个死循环，不断地从MessageQueue取消息，如果有消息就处理消息，没有消息就阻塞。
- MessageQueue，一个消息队列，添加消息，处理消息
- handler内部与Looper关联，handler->Looper->MessageQueue,handler发送消息就是向MessageQueue队列发送消息。
###### 总结：handler负责发送消息，Looper负责接收handler发送的消息，并把消息回传给handler自己。MessageQueue存储消息的容器。
 
## HandlerThread的作用是什么
- HandlerThread thread=new HandlerThread("handler thread");自动包含等待机制，等Looper创建好了，才创建Handler，避免出现空指针异常。
 
## 主线程
- ActivityThread 默认创建main线程，main中默认创建Looper，Looper默认创建MessageQueue
- threadLocal保存线程的变量信息，方法包括：set，get,设置获取Looper对象
 
## Android更新UI的方式
- runOnUIThread
- handler post
- handler sendMessage
- view post
 
## 非UI线程真的不能更新UI吗
- 不一定，之所以子线程不能更新界面，是因为Android在线程的方法里面采用checkThread进行判断是否是主线程，而这个方法是在ViewRootImpl中的，
- 这个类是在onResume里面才生成的，因此，如果这个时候子线程在onCreate方法里面生成更新UI，而且没有做阻塞，就是耗时多的操作，还是可以更新UI的。
 
## 使用Handler遇到的问题
- 比如说子线程更新UI，是因为触发了checkThread方法检查是否在主线程更新UI，还有就是子线程中没有Looper，这个原因是因为Handler的机制引起的
- 因为Handler发送Message的时候，需要将Message放到MessageQueue里面，而这个时候如果没有Looper的话，就无法循环输出MessageQueue了
- 这个时候就会报Looper为空的错误。
 
## 主线程怎么通知子线程
- 可以利用HandlerThread进行生成一个子线程的Handler，并且实现handlerMessage方法，然后在主线程里面也生成一个Handler
- 然后通过调用sendMessage方法进行通知子线程。同样，子线程里面也可以调用sendMessage方法进行通知主线程。
- 这样做的好处比如有些图片的加载啊，网络的访问啊可能会比较耗时，所以放到子线程里面做是比较合适的。

Handler的使用
方式一： post(Runnable)

    new Thread(new Runnable() {
    @Override
    public void run() {

      /**
         耗时操作
       */
     handler.post(new Runnable() {
         @Override
         public void run() {
             /**
               更新UI
              */
         }
     });

    }
    }).start();

方式二： sendMessage(Message)
```java
    private Handler handler = new Handler(){

    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        switch (msg.what) {      //判断标志位
            case 1:
                /**
                  获取数据，更新UI
                 */
                break;
        }
     }

    };

    public class WorkThread extends Thread {

    @Override

     public void run() {
       super.run();
    /**
      耗时操作
     */
       

    Message msg =Message.obtain(); //从全局池中返回一个message实例，避免多次创建message（如new Message）
    msg.obj = data;
    msg.what=1; //标志消息的标志
    handler.sendMessage(msg);
    }

    new WorkThread().start();
```

## Handler
- 在App初始化的时候会执行ActivityThread的main方法,该方法中调用Looper.prepareMainLooper();Looper.loop();
- 每个线程只能创建一个Looper
### Looper.prepareMainLooper()中调用prepare(false)
- prepare()方法初始化了一个Looper对象并关联在一个MessageQueue对象，并且一个线程中只有一个Looper对象，只有一个MessageQueue对象。
- Handler的构造方法则在Handler内部维护了当前线程的Looper对象
###### 消息队列(采用单链表的数据结构来存储消息列表。）
- 使用Handler发消息，会将该Handler对象也存入消息中，跟踪代码发现最后会调用MessageQueue的enqueueMessage(Message msg, long when)方法，在该方法中处理消息，
- 若当前消息队列中没有消息，消息触发时间等于0或者消息触发时间小于消息队列中的第一条消息触发时间，则将该消息作为第一条消息，将其指针指向之前链表中的第一条消息。
- 若不满足上述条件就会循环遍历消息队列，将新的消息插入到消息队列中的指定位置，源码如下：
```java
msg.when = when;
Message p = mMessages;
boolean needWake;
if (p == null || when == 0 || when < p.when) {
       // New head, wake up the event queue if blocked.
       msg.next = p;
       mMessages = msg;
       needWake = mBlocked;
} else {
       // Inserted within the middle of the queue.  Usually we don't have to wake
       // up the event queue unless there is a barrier at the head of the queue
       // and the message is the earliest asynchronous message in the queue.
       needWake = mBlocked && p.target == null && msg.isAsynchronous();
       Message prev;
       for (;;) {
            prev = p;
            p = p.next;
            if (p == null || when < p.when) {
                break;
            }
            if (needWake && p.isAsynchronous()) {
                needWake = false;
            }
        }
        msg.next = p; // invariant: p == prev.next
        prev.next = msg;
}
```
### Looper.Loop()方法
- 开启一个死循环，不断的判断MessageQueue中的消息是否为空，如果为空则直接return。
- 不断执行queue.next()方法，从消息队列中取消息，取到就调用handler的dispatchMessage(msg)，该方法中先判断消息中是否有Runnable对象，有的话直接执行Runnable的run()方法，在判断初始化Handler时是否传递Runnable对象，传的化直接执行Runnable的run()方法
- 如果没传的话，会直接调用我们创建handler时重写的handlerMessage方法。由于Handler对象是在主线程中创建的，所以handler的handlerMessage方法的执行也会在主线程中。
```java
public void dispatchMessage(Message msg) {
    if (msg.callback != null) {
        handleCallback(msg);
    } else {
        if (mCallback != null) {
            if (mCallback.handleMessage(msg)) {
               return;
            }
        }
        handleMessage(msg);
    }
}
```

- 子线程中定义Handler，则标准的写法为：
```java
// 初始化该线程Looper，MessageQueue，执行且只能执行一次
Looper.prepare();
// 初始化Handler对象，内部关联Looper对象
Handler mHandler = new Handler() {
     @Override
     public void handleMessage(Message msg) {
            super.handleMessage(msg);
     }
};
// 启动消息队列出栈死循环
Looper.loop();
```
- 管道机制：内部维护
```java
Looper.prepare();
Handler mHandler = new Handler() {
   @Override
   public void handleMessage(Message msg) {
      if (msg.what == 101) {
         Log.i(TAG, "在子线程中定义Handler，并接收到消息。。。");
       }
   }
};
Looper.loop();
```
