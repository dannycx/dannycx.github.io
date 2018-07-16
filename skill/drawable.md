# XML 图片选择器、shape图形

## selector选择器
#### 按钮选择器(实现按下一种效果和正常一种效果)
```java
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" android:drawable="@drawable/pressed"/>
    <item android:drawable="@drawable/normal"/>
</selector>
```

## 水波纹效果，Android5.0之后效果，在drawable下新建ripple.xml，新标签ripple支持水波纹效果,直接设置给Button的背景即可看到效果
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

## 相机效果
```java
<vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:width="24dp"
        android:height="24dp"
        android:viewportHeight="24.0"
        android:viewportWidth="24.0">
    <path
        android:fillColor="#FF000000"
        android:pathData="M12,12m-3.2,0a3.2,3.2 0,1 1,6.4 0a3.2,3.2 0,1 1,-6.4 0"/>
    <path
        android:fillColor="#FF000000"
        android:pathData="M9,2L7.17,4H4c-1.1,0 -2,0.9 -2,2v12c0,1.1 0.9,2 2,2h16c1.1,0 2,-0.9 2,-2V6c0,-1.1 -0.9,-2 -2,-2h-3.17L15,2H9zm3,15c-2.76,0 -5,-2.24 -5,-5s2.24,-5 5,-5 5,2.24 5,5 -2.24,5 -5,5z"/>
</vector>

```
