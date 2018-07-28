# 泛型
###### 所有的泛型变量都是派生自Object，所以在填充泛型变量时，只能使用派生自Object的类

## 字母规范(建议)
- E — Element，常用在java Collection里，如：List<E>,Iterator<E>,Set<E>
- K,V — Key，Value，代表Map的键值对
- N — Number，数字
- T — Type，类型，如String，Integer,Boolean等等

## 泛型类
### 定义
###### 类名后加<T>,尖括号中字母可以是任何大写字母，意义是相同的。
```java
class A<T>{}    
```

### 类中使用
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

### 泛型类的使用
```java
A<Integer> p = new A<Integer>() ; 
p.setX(new Integer(100)) ; 
Log.d("A", p.getX());  
 
A<String> p = new A<String>() ; 
p.setX(new String("abc")) ; 
Log.d("A", p.getX()); 
```

### 泛型实现的优势
- 不用强制转换
- 在setXxx()时如果传入类型不对，编译时会报错

## 多泛型变量
### 定义
##### 尖括号中可定义多个泛型变量,用逗号隔开即可
```java
class AA<T,U>{}
```

## 泛型接口
### 定义
```java
interface Info<T>{        // 在接口上定义泛型
    // 定义抽象方法，抽象方法的返回值就是泛型类型  
    T getXxx() ; 
    void setXxx(T x);
}  
```
### 使用
#### 非泛型类实现
```java
class InfoImpl implements Info<Boolean>{	// 定义泛型接口的子类
    private boolean var ;				// 定义属性
    public InfoImpl(boolean var){		// 通过构造方法设置属性内容
        this.setVar(var) ;
    }
    @Override
    public void setVar(boolean var){
        this.var = var ;
    }
    @Override
    public Boolean getVar(){
        return this.var ;
    }
}

InfoImpl i = new InfoImpl(true);// 使用
```

#### 泛型类实现
```java
class InfoImpl<T> implements Info<T>{	// 定义泛型接口的子类
	private T var ;				// 定义属性
	public InfoImpl(T var){		// 通过构造方法设置属性内容
		this.setVar(var) ;	
	}
	public void setVar(T var){
		this.var = var ;
	}
	public T getVar(){
		return this.var ;
	}
}

Info<String> i = new InfoImpl<String>("hello");// 使用
```

#### 泛型类中定义三个泛型变量T,M,N并且把第三个泛型变量N用来填充接口Info
```java
class InfoImpl<T,M,N> implements Info<N>{	// 定义泛型接口的子类
     private T x;
     private M y;
     private N var;
     
     public InfoImpl(N var){		// 通过构造方法设置属性内容
         this.setVar(var) ;
     }
     public void setVar(N var){
         this.var = var ;
     }
     public N getVar(){
         return this.var ;
     }
 }
 
 InfoImpl<String,Double,Integer> i = new InfoImpl<String,Double,Integer>(100);// 使用
```

## 泛型函数
### 定义
```java
//静态函数
public static  <T> void sMethod(T a){
   Log.d("genericity","sMethod: "+a.toString());
}
//普通函数
public  <T> void mMethod(T a){
   Log.d("genericity","mMethod: "+a.toString());
}
```
### 使用
```java
//静态方法
Genericity.sMethod("genericity");//使用方法一
Genericity.<String>sMethod("genericity");//使用方法二(建议)
 
//常规方法
Genericity genericity = new Genericity();
genericity.mMethod(new Integer(100));//使用方法一
genericity.<Integer>mMethod(new Integer(100));//使用方法二(建议)
```

## 泛型在返回值中使用
```java
public static <T> List<T> parseArray(String response,Class<T> object){// fastJson简单使用
    List<T> modelList = JSON.parseArray(response, object);
    return modelList;
}
```

## 泛型数组
```java
//定义
public static <T> T[] array(T... arr){  // 可变参数  
       return arr;            // 返回泛型数组  
}  

//使用
String str[] = array("hello","world","hello","haha","heihei") ;
String[] result = array(str) ;
```

## 泛型之类型绑定：extends
### 定义
#### <T extends Type>
- T 是 Type的子类型,extends不等同于继承
- T 和 Type可以是类，也可以是接口
- T 是 Type基础上创建的，具有Type的功能。

### 绑定
```java
public interface Comparable<T> {// 定义泛型接口
    boolean compareTo(T t);
}

//添加extends Comparable后，就可以Comparable里的函数
public static <T extends Comparable>  T min(T... t){
    T small = t[0];
    for(T item:t){
        if (small.compareTo(item)){
            small = item;
        }
    }
    return small;
}
```

### 作用
- 对填充的泛型加以限定 
- 使用泛型变量T时，可以使用Type内部的函数。

### 通配符
#### 无边界通配符：？
- 任意A实例都是可以传给a的：比如这里的A<String>(),A<Object>()都是可以的

#### ？与T的区别
- 无任何关联
- 泛型变量T不能在代码用于创建变量，只能在类，接口，函数中声明以后，才能使用
- ？只能用于填充泛型变量T,是用来填充T的一种方式
- 通配符只能用于填充泛型变量T,不能用于定义变量

#### 使用
```java
// 正确使用
A<?> a;
a = new A<String>();

// 错误使用
? x;
a = new A<?>();
```

#### 通配符？的extends绑定
- 例:值只能是数值-包括边界自身
```java
A<? extends Number> a;
```
- 从根本上解决数据乱填充A的问题，要在泛型类定义时加上<T extends Number>
```java
class Point<T extends Number> {}
```
- 利用<? extends Number>定义的变量，只可取值，不可修改

#### 通配符？的super绑定
###### <? extends XXX>指填充为派生于 XXX 的任意子类，<? super XXX>指填充为任意 XXX 的父类
- 包括边界
- super通配符实例内容,能存不能取

#### 小结
- 如果你想从一个数据类型里获取数据，使用 ? extends 通配符（能取不能存）
◆ 如果你想把对象写入一个数据结构里，使用 ? super 通配符（能存不能取）
◆ 如果你既想存，又想取，那就别用通配符。



