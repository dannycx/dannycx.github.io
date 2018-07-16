ObjectAnimator# Android 动画
- 帧动画：一张张图片组成，类似放电影效果，需要大量图片资源
- 补间动画（不改变控件实际位置）：旋转动画(RotateAnimation)、透明动画(AlphaAnimation)、缩放动画(ScaleAnimation)、平移动画(TranslateAnimation)
- 属性动画（改变控件实际位置）：ValueAnimator、ObjectAnimator

## 帧动画


## 补间动画

#### 透明动画
```java
java代码
AlphaAnimation alpha = new AlphaAnimation(0,1);
alpha.setDuration(1000);//时间
view.startAnimation(alpha);

xml文件 alpha.xml
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromAlpha="0"
    android:toAlpha="1"
    android:duration="3000"/>
view.startAnimation(AnimationUtils.loadAnimation(getActivity(),R.anim.alpha));
```

#### 旋转动画
```java
java代码
// new RotateAnimation(0,360,100,50);//从0-360度旋转,以（100，50）为中心点
// new RotateAnimation(0, 360, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);//从0-360度旋转,以自身中心点为中心点
RotateAnimation rotate = new RotateAnimation(0,360);//从0-360度旋转
rotate.setDuration(1000);
rotate.setFillAfter(true);//停在结束位置
view.startAnimation(rotate);

xml文件 rotate.xml
<?xml version="1.0" encoding="utf-8"?>
<rotate
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:toDegrees="360"
    android:pivotX="50%"
    android:pivotY="50%"
    android:duration="3000"/>
view.startAnimation(AnimationUtils.loadAnimation(getActivity(),R.anim.rotate));
```

#### 缩放动画
```java
java代码
// new ScaleAnimation(0, 1, 0, 1, 100, 50);//相对(100，50)点缩放
// new ScaleAnimation(0, 1, 0, 1, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);//相对自身中心点缩放一倍
ScaleAnimation scale = new ScaleAnimation(0, 1, 0, 1);//放大一倍
scale.setDuration(1000);
scale.setFillAfter(true);//停在结束位置
view.startAnimation(scale);

xml文件 scale.xml
<?xml version="1.0" encoding="utf-8"?>
<scale
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXScale="0"
    android:toXScale="1"
    android:fromYScale="0"
    android:toYScale="1"
    android:pivotX="50%"
    android:pivotY="50%"
    android:duration="3000"/>
view.startAnimation(AnimationUtils.loadAnimation(getActivity(),R.anim.scale));
``

#### 平移动画
```java
java代码
TranslateAnimation translate = new TranslateAnimation(0, 200, 0, 200);//相对自身位置右移200，下移200
translate.setDuration(1000);
translate.setFillAfter(true);//停在结束位置
view.startAnimation(translate);

xml文件 translate.xml
<?xml version="1.0" encoding="utf-8"?>
<translate
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXDelta="0"
    android:toXDelta="200"
    android:fromYDelta="0"
    android:toYDelta="200"
    android:duration="3000"/>
view.startAnimation(AnimationUtils.loadAnimation(getActivity(),R.anim.translate));
```

#### 组合动画
```java
java代码
AnimationSet set = new AnimationSet(true);//是否使用插补器
set.setDuration(2000);

AlphaAnimation alpha = new AlphaAnimation(0,1);
alpha.setDuration(1000);//时间
set.addAnimation(alpha);

TranslateAnimation translate = new TranslateAnimation(200, 0, 200, 0);//相对自身位置右移200，下移200
translate.setDuration(1000);
set.addAnimation(translate);
        
view.startAnimation(set);

xml文件 set.xml
<?xml version="1.0" encoding="utf-8"?>
<set
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="3000"
    android:shareInterpolator="true">

    <alpha
        android:fromAlpha="0"
        android:toAlpha="1"/>

    <translate
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:fromXDelta="200"
        android:fromYDelta="200"
        android:toXDelta="0"
        android:toYDelta="0"/>
</set>
view.startAnimation(AnimationUtils.loadAnimation(getActivity(),R.anim.set));
```

#### 动画监听
```java
TranslateAnimation translate = new TranslateAnimation(200, 0, 200, 0);//相对自身位置右移200，下移200
translate.setDuration(1000);
translate.setAnimationListener(new Animation.AnimationListener() {
    @Override
    public void onAnimationStart(Animation animation) {
       //开始
    }

    @Override
    public void onAnimationEnd(Animation animation) {
       //结束
    }

    @Override
    public void onAnimationRepeat(Animation animation) {
       //重复
    }
});
```



## 属性动画
