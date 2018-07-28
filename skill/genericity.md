# 泛型

## 定义泛型：Point<T>
###### 类名后加<T>,尖括号中字母可以是任何大写字母，意义是相同的。

## 类中使用泛型
```java
//定义变量
private T x ; 

public T getX(){ 
    return x ;  
}  

public void setX(T x){  
    this.x = x ;  
} 
```

## 泛型类的使用
```java
//IntegerPoint使用
Point<Integer> p = new Point<Integer>() ; 
p.setX(new Integer(100)) ; 
Log.d("Point", p.getX());  
 
Point<String> p = new Point<String>() ; 
p.setX(new String("abc")) ; 
Log.d("Point", p.getX()); 
```
