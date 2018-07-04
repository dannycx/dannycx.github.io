# Activity生命周期

状态：激活状态（位于当前任务栈顶）、暂停状态（失去焦点，但仍可见）、停止状态(被另一个activity完全覆盖)
## onCreate()
- 设置布局 控件初始化
## onStart()
- 注册一些监听 内容观察者,后台,可见,不可获得焦点
## onReStart()
## onResume()
- 再次对数据进行查询,前台,可见,可获得焦点

## onPause()
- 数据临时保存,后台,开启新的activity,会先执行完该方法,才去执行onResume

## onStop()
- 取消监听,不可见

## onDestroy()
- 对资源进行回收 cursor关闭cursor.close();   bitmap进行回收 bitmap.recycle();
## onSaveInstanceState(Bundle outState)
- 内存不足时调用，保存数据  outState.putXxx(key,value)
## onRestoreInstansceState(Bundle saveInstanceState) 
- 恢复数据  saveInstanceState.getXxx(key)



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
/**
 * onStart:
 * onResume:
 * onPause:
 * onStop:
 *
 * 情况一:
 *      开启activity:onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy,再打开:onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy
 * 情况二:
 *      onCreate,onStart,onResume中直接startActivity: A:onCreate-onStart-onResume-onPause,B:onCreate-onStart-onResume,A:onStop
 *      后退键:B:onPause,A:onRestart-onStart-onResume-onPause,B:onCreate-onStart-onResume,B:onStop-onDestroy,A:onStop
 * 情况三:
 *      onCreate中直接startActivity,finish: A:onCreate,B:onCreate-onStart-onResume,A:onDestroy
 *      后退键:B:onPause-onStop-onDestroy
 * 情况四:
 *      onCreate,onStart,onResume中直接startActivity(对话框activity): A:onCreate-onStart-onResume-onPause,B:onCreate-onStart-onResume
 *      后退键:B:onPause,A:onResume,B:onStop-onDestroy
 * 情况五:
 *      开启activity:onCreate-onStart-onResume,home:onPause-onStop,再打开:onRestart-onStart-onResume,后退键:onPause-onStop-onDestroy
 * 情况六:
 *      开启activity:onCreate-onStart-onResume,横竖屏切换:onPause-onSaveInstanceState(activity异常终止才调用)-onStop-onDestroy-onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy
 *
 *
 *
 * 屏幕旋转不重新创建:   待验证
 *      清单文件,activity标签下-android:configChanges="orientation"
 */
