# 开机自启
###### 应用一定要安装到手机内存，不要安装到内存卡
#### 添加开机自启权限(必须)
```java
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

#### 静态注册广播
```java
<receiver android:name=".BootReceiver">
     <intent-filter>
         <action android:name="android.intent.action.BOOT_COMPLETED"/>
     </intent-filter>
</receiver>
```

#### 广播
```java
@Override
public void onReceive(Context context, Intent intent) {
    Log.d(TAG, "onReceive: ");
    String action = intent.getAction();
    if (action.equals("android.intent.action.BOOT_COMPLETED")) {
         //startUi（）
         //startService()
    }
}
```

#### 服务自启
```java
//开机自启service
private void startService(){
    Intent service = new Intent(context, BootService.class);
    context.startService(service);
}
```

#### 界面自启
```java
//开机自启activity
private void startUi（）{
    Intent activity = new Intent(context, MainActivity.class);
    activity.setAction("android.intent.action.MAIN");
    activity.addCategory("android.intent.category.LAUNCHER");
    activity.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    context.startActivity(activity);
}
```
