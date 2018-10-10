# LeakCanary简单使用

## 模块下build.gradle添加依赖
- debugCompile 'com.squareup.leakcanary:leakcanary-android:1.6'
- debugCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.6'

## Application类添加如下代码
```java
public class LeakCanaryApp extends Application {
  private LeakCanaryApp app;
  private RefWatcher watcher;
  
  @Override
  public void onCreate() {
      super.onCreate();
      app = this;
      if(LeakCanary.isInAnalyzerProcess(this)) {
        watcher = LeakCanary.DISABLED;
      }else {
        watcher = LeakCanary.install(this);
      }
      
  }
  
  public static RefWatcher getRefWatcher(){
    return app.watcher;
  }

}
```

## activity或fragment的onDestroy()中
```java
  @Override
  public void onDestroy() {
    RefWatcher watcher = LeakCanaryApp.getRefWatcher();
    watcher.watch(this);// 参数为检测对象
  }
```
