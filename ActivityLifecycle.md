# Activity生命周期

状态：激活状态（位于当前任务栈顶）、暂停状态（失去焦点，但仍可见）、停止状态(被另一个activity完全覆盖)
##onCreate()设置布局 控件初始化

##onStart()注册一些监听 内容观察者

##onReStart()

##onResume()再次对数据进行查询

##onPause()数据临时保存

##onStop()取消监听

##onDestroy()对资源进行回收 cursor关闭cursor.close();   bitmap进行回收 bitmap.recycle();



onSaveInstanceState(Bundle outState)内存不足时调用，保存数据  outState.putXxx(key,value)
onRestoreInstansceState(Bundle saveInstanceState)   恢复数据  saveInstanceState.getXxx(key)



-完整生命周期：create-start-resume可视
后退：pause-stop-detroy
打开一个activity：onPause-onstop
作为dialog在清单文件配置theme：@android：style/



不希望重新创建activity实例：configChanges="orientation|keyboardHidden"指定要捕获“屏幕方向”，“键盘显示隐藏”变化，
当捕获后调用activity的onConfigurationChanged()方法。
##
onConfigurationChanged(Configuration newConfig){
if(newConfig.orientation==Configuration.ORIENTATION_LANDSCAPE){
横
}else if(newConfig.orientation==Configuration.ORIENTATION_PORTRAIT){
竖
}
##


任务栈：
进程：
