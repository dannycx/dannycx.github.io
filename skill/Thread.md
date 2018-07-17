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
