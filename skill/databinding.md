# Databinding使用
###### 双向绑定实现数据和UI绑定的框架，基于MVVM模式实现

## 使用
###### Java模块 build.gradle 中添加
```java
android {
    ....
    dataBinding {
        enabled = true
    }
}
```
###### 注意：如果app依赖了一个使用 Data Binding 的库，那么app module 的 build.gradle 也必须配置 Data Binding。

## 简单实现,实现基本数据绑定,按钮事件触发,及线程任务执行
###### 提供一个javabean
```java
public class Student {
    private String mNo;
    private String mName;

    public String getNo() {
        return mNo;
    }

    public String getName() {
        return mName;
    }

    public Student(String no, String name) {
        mNo = no;
        mName = name;
    }
}
```
###### 布局
```java
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <!-- 变量user， 描述了一个布局中会用到的属性 -->
        <variable name="student" type="com.danny.databinding.model.Student"/>
        <variable name="event" type="com.danny.databinding.model.Event"/>
        <variable name="task" type="com.danny.databinding.model.Task"/>
    </data>

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView android:layout_width="wrap_content"
                  android:layout_height="wrap_content"
                  android:text="@{student.no}"/>

        <!-- 布局文件中的表达式使用 “@{}” 的语法 -->
        <TextView android:layout_width="wrap_content"
                  android:layout_height="wrap_content"
                  android:text="@{student.name}"/>

        <!-- 注意：函数名和监听器对象必须对应 -->
        <!-- 函数调用也可以使用 `.` , 如handler.onClickFriend , 不过已弃用 -->
        <Button
            android:text="点下试试"
            android:onClick="@{event::click}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>

        <Button
            android:text="执行任务"
            android:onClick="@{() -> event.onTaskClick(task)}"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"/>
    </LinearLayout>
</layout>
```
###### Activity代码实现(此处setStudent/setEvent/setTask方法会显示红色,不用管编译运行没有问题)
```java
        ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
//        所有的 set 方法也是根据布局中 variable 名称生成的
//        variableId不能为0, 否则会出现数据无法显示现象
//        binding.setVariable(0, new Student("1111100000","张三"));
//        binding.setVariable();// 有多个对象数据时,不要使用该方法,会抛出类型转换异常
        binding.setStudent(new Student("1111100000","张三"));
        binding.setEvent(new Event(this));
        binding.setTask(new Task(this));
```
