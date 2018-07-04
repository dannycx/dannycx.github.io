# view

## view绘制流程
- 绘制流程从ViewRoot的performTraversals方法开始，经过measure测量view宽高，layout确定view在父布局位置，draw负责将view绘制在屏幕上

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

### scrollBy() 和 scrollTo() 的区别
- scrollBy() 间接调用 scrollTo(),只是它是在当前滚动的基础上再进行偏移。
### mScrollX 与 mScrollY 相关。
