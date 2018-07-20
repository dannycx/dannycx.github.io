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

