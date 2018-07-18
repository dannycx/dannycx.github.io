# Android线程及线程池
###### 线程是操作系统的最小调度单位，系统中存在大量线程时，通过时间片轮转方式调度
###### 线程池可以避免因为频繁创建和销毁线程带来的系统开销，通过Executor派生特定类型线程池
###### AsyncTask（线程池实现），HandlerThread、IntentService（线程实现）

## 主线程与子线程
- 主线程是进程所拥有的线程，Java默认一个进程只有一个线程（主线程），主线程处理界面交互，须具有较高的响应速度，否则会出现卡顿现象。
- 由于主线程不能执行耗时操作，所以子线程出现了。（网络请求，I/O操作）

## AsyncTask
###### 内部封装线程池和Handler，方便在子线程中更新UI
- 轻量级异步任务类，在线程池中执行耗时操作，然后将结果和进度丢到主线程中。
- 泛型参数Params（参数类型）、Progress（进度类型）、Result（结果类型）
- 第一个泛型为doInBackground()方法参数：后台下载
- 第二个泛型为onProgressUpdate（）方法参数：进度更新
- 第三个泛型为doInBackground()方法返回值类型，onPostExecute（）方法参数：结束处理
- 主线程方法：onPreExecute()，onPostExecute（），onProgressUpdate（）;
- 子线程方法：doInBackground()，通过publishProgress()回调onProgressUpdate（）


## HandlerThread
###### 消息循环的线程，内部可以使用Handler

## IntentService
###### 一个服务，它不容易被系统杀死，从而可以尽量保证任务执行。系统对其封装，使其可以更方便的执行后台任务，内部采用HandlerThread执行任务，任务执行完后IntentService自动退出，


## 线程池
###### Android中提供了四种线程池
- FixedThreadPool：固定大小，使用无界的LinkedBlockingQueue，适用于稳定、固定的正规并发线程，多用于服务器端
- CachedThreadPool：线程数不是一直增加，先判断有没有空闲的，有就使用，没有就创建，适用于执行周期短任务，使用SynchronousQueue作为阻塞队列
- SingleThreadExecutor：线程池中只有一个线程，适用于有明确执行顺序不影响主线程的任务，任务按顺序执行，使用无界的LinkedBlockingQueue
- 

### 参数
#### corePoolSize:线程池核心线程数量
- 如果池中线程数量少于核心线程池数量，则直接新建线程处理当前任务
- 核心线程池空闲不会被回收
- 当池中无空闲线程时，新任务将被添加到阻塞队列

#### maximumPoolSize：线程池最大线程数量
- 当阻塞队列已满，并且有新任务还在入队时，创建新的线程处理，直到线程数大于maximumPoolSize
- 超出corePoolSize部分的线程超过空闲时间后会被回收
- 当线程已经超出corePoolSize，并且队列容量已满，则拒绝入队

#### keepAliveTime unit：线程存活时间
- 当线程超出corePoolSize时生效
- 线程空余keepAliveTime后，将被回收

#### workQueue：线程使用阻塞队列
#### threadFactory：创建线程池工厂
- 用于控制创建线程或者销毁线程时加入其它逻辑

#### handler：线程池拒绝策略
- 直接丢弃（DiscardPolicy）
- 丢弃队列中最老的任务(DiscardOldestPolicy)。
- 抛异常(AbortPolicy)
- 将任务分给调用线程来执行(CallerRunsPolicy)
