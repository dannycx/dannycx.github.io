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
- mScrollX 为负数时，内容向右滑，反之向左滑。
- mScrollY 为负数时，内容向下滑，反之向上滑。

### scrollBy() 和 scrollTo() 的区别
- scrollBy() 间接调用 scrollTo(),只是它是在当前滚动的基础上再进行偏移。
- computeScrollOffset() 方法会返回当前动画的状态，true 代表动画continue，false 代表动画over。
- 若动画没有结束，每次都会调用 computeScrollOffset() 方法，它会更新 mCurrentX 和 mCurrentY 的数值。
- 当调用 Scroller.startScroll() 时，调用了 invalidate() 方法,computeScroll()就会执行
- 所以运行 Scroller 的一般方法是在 View 中调用的地方编写如下代码
```java
public void startScroll(){
    //强制结束 mScroller 未完成的动画
    mScroller.forceFinished(true);
    int startX = getScrollX();
    int startY = getScrollY();
    // 调用 startScroll() 方法，其中的参数由开发者自己决定
    mScroller.startScroll(startX,startY,startX+dx,startY+dy,1000);
    // 让 View 重绘
    invalidate();
}

public void computeScroll() {
    super.computeScroll();
    if (mScroller.computeScrollOffset()) {
        Log.d(TAG, "滚动未结束。");
        // 通过获取 Scroller 中 mCurrentX、mCurrentY 的值，直接设置为 mScrollX、mScrollY
        // 在实际开发中，mCurrentX、mCurrentY 与 mScrollX、mScrollY 的关系由自己定义
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
- 启动动画
```java
方式一：最后调用方法二，默认时间为250 ms
public void startScroll(int startX, int startY, int dx, int dy) {}

方式二：
public void startScroll(int startX, int startY, int dx, int dy,int duration) {}
```

- Scroller的快速滚动功能 fling
```java
   /**
     * 快速滚动
     * @param startX 开始滚动时 X 坐标
     * @param startY 开始滚动时 Y 坐标
     * @param velocityX 开始滚动时 X 方向的初始速度
     * @param velocityY 开始滚动时 Y 方向的初始速度
     * @param minX 滚动过程，X 坐标不能小于这个数值
     * @param maxX 滚动过程，X 坐标不能大于这个值
     * @param minY 滚动过程，Y 坐标不能小于这个数值
     * @param maxY 滚动过程，Y 坐标不能大于这个数值
     */
    public void fling(int startX, int startY, int velocityX, int velocityY,
                      int minX, int maxX, int minY, int maxY) {}
```
- 快速滚动功能需要配合VelocityTracker（速度追踪器）使用，
```java
//1. 创建VelocityTracker
VelocityTracker mVelocityTracker = VelocityTracker.obtain();

//2. 添加MotionEvent 事件
mVelocityTracker.addMovement(event);

//3. 计算 x、y 轴的速度,
//第一个参数表明时间单位，1000 代表 1000 ms 也就是 1 s，计算 1 s 内滚动多少个像素,后面表示最大速度
mVelocityTracker.computeCurrentVelocity(1000,600.0f);

//4. 获取速度
float xVelocity = mVelocityTracker.getXVelocity();
float yVelocity = mVelocityTracker.getYVelocity();

//之后的事情根据获取的速度值做处理
```

## 小结
###### 理解手指滑动方向、内容滑动方向和 mScrollX、mScrollY 数值的关系。
- 手指向左滑动，内容将向右显示，这时 mScrollX > 0。
- 手指向右滑动，内容将向左显示，这时 mScrollX < 0.
- 手指向上滑动，内容将向下显示，这时 mScrollY < 0。
- 手指向下滑动，内容将向上显示，这时 mScrollY > 0.
```markdown
 1. View 滑动的关键属性 mScrollX 和 mScrollY。
 2. 系统处理滑动事件在 onDraw(Canvas) 之前，对 canvas 平移，canvas.translate(mLeft-mScrollX,mRight-mScrollY)。将坐标系从父组件转换到子 view 中。
 3. 处理 View 的滑动事件有 scrollBy() 和 scrollTo()，前一种是增量式（先在原基础上加偏移量，再调用scrollTo()方法），后一种直接到位。
 4. 实现平滑滚动的效果，除了 Scroller，还可以通过属性动画完成，还是针对 mScrollX 或者 mScrollY 的变化重绘。
 5. View 滚动的是内容，对于一个父组件而言，滚动的就是子view。所以想移动一个 View，就应该调用它的父组件的 scrollBy() 或者 scrollTo()。
 6. Scroller提供了滚动时改变相应数值，重写自定义View的computeScroll()，获取mCurrentX和mCurrentY，或根据自己的规则调用scrollTo()，实现平稳滚动效果。
 7. Scroller 提供快速滚动功能，在自定义 View 的 onTouchEvent()中获取相应方向的初始速度，调用 Scroller 的 startFling()。
```
