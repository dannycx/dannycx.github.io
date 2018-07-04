# view

## view绘制流程
- 绘制流程从ViewRoot的performTraversals方法开始，经过measure测量view宽高，layout确定view在父布局位置，draw负责将view绘制在屏幕上
```markdown
 View 绘制流程
1. 绘制背景
2. 绘制内容
3. 绘制 children
4. 如果有需要，绘制渐隐(fading) 效果
5. 绘制装饰物 （scrollbars） 
```

## MeasureSpec

- 一个32位int值，高2为代表测量模式SpecMode，低30位代表该模式下的规格大小SpecSize.
- 三种模式：
- UNSPECIFIED:父容器对view不限制，要多大给多大
- EXACTLY:精确大小，指定一个值和MATCH_PARENT,都是该模式
- AT_MOST:父容器指定一个大小，子view不能超出该值，WRAP_CONTENT就是该模式


![](https://github.com/dannycx/knowledge/blob/master/image/viewMeasure.png) 


## view滑动
###### 对于一个 View 或者 ViewGroup 而言，滚动针对的是它的内容，View 的内容体现在它要绘制的内容上面，ViewGroup 的内容相当于它的所有子 View。
- ScrollView：滚动针对的只是view的显示内容进行了位移。
- RecyclerView：滚动针对的是它的子View。RecyclerView本身尺寸和位置并没有发生变化，只是子View 的显示区域进行了位移。

- scrollBy() 和 scrollTo()，而归根到底，如果要设置 mScrollX 或者 mScrollY，最终调用的还是 scrollTo()

### mScrollX 与 mScrollY 相关。
###### 如果你想翻看前面或者是上面的内容，mScrollX 和 mScrollY 取值为正数，如果想翻看后面或者是下面的内容，mScrollX 和 mScrollY 取值为负。
```markdown
手指向左滑动，内容将向右显示，这时 mScrollX > 0。
手指向右滑动，内容将向左显示，这时 mScrollX < 0.
手指向上滑动，内容将向下显示，这时 mScrollY < 0。
手指向下滑动，内容将向上显示，这时 mScrollY > 0.
```
- mScrollX 为负数时，内容向右滑，反之向左滑。
- mScrollY 为负数时，内容向下滑，反之向上滑。

### scrollBy() 和 scrollTo() 的区别
- scrollBy() 间接调用 scrollTo(),只是它是在当前滚动的基础上再进行偏移。
- computeScrollOffset() 方法会返回当前动画的状态，true 代表动画continue，false 代表动画over。
- 若动画没有结束，每次都会调用 computeScrollOffset() 方法，它会更新 mCurrentX 和 mCurrentY 的数值。
- 当调用 Scroller.startScroll() 时，调用了 invalidate() 方法,computeScroll()就会执行
```java
public void computeScroll() {
    super.computeScroll();
    if (mScroller.computeScrollOffset()) {
        Log.d(TAG, "滚动未结束。");
        scrollTo(mScroller.getCurrX(),mScroller.getCurrY());
        if (mScroller.getCurrX() == getScrollX() && mScroller.getCurrY() == getScrollY() ) {//避免重复请求
            postInvalidate();//一直调用，类似消息机制
        }
    } else {
        Log.d(TAG, "滚动结束！");
    }
}
```
## Scroller使用
- 创建

```java
方式一：
Scroller mScroller = new Scroller(context);
```

```java
方式二：指定动画插值器
AccelerateDecelerateInterpolator  //先加速后减速
AccelerateInterpolator  //加速
AnticipateInterpolator  //运动时先向反方向运动一定距离，再向正方向目标运动
BounceInterpolator  //模拟弹球轨迹
DecelerateInterpolator  // 减速
LinearInterpolator  //匀速

AccelerateInterpolator interpolator = new AccelerateInterpolator(1.2f);
Scroller mScroller = new Scroller(context,interpolator);
```

