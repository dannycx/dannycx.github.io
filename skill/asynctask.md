# AsyncTask
###### 内部使用java的线程池和Handler来实现异步任务以及与UI线程的交互。
onPreExecute()，onPostExecute（），doInBackground()，onProgressUpdate（）;
第一个泛型为doInBackground()方法参数：后台下载
第二个泛型为onProgressUpdate（）方法参数：进度更新
第三个泛型为doInBackground()方法返回值类型，onPostExecute（）方法参数：结束处理
