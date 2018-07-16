# android 特性

###### 水波纹效果，Android5.0之后效果，在drawable下新建ripple.xml，新标签ripple支持水波纹效果,直接设置给Button的背景即可看到效果
```java
normal.xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle">
    <solid android:color="#8cc476" />
    <corners android:radius="0dp" />
</shape>

ripple.xml
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
        android:color="#FF2194e1">
    <item android:drawable="@drawable/normal_bg" />
</ripple>
```
