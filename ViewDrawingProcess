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
