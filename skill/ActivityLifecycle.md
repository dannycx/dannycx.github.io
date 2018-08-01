# Activity
###### 状态：激活状态（位于当前任务栈顶）、暂停状态（失去焦点，但仍可见）、停止状态(被另一个activity完全覆盖)
#### 屏幕可上下左右四个方向翻转,设置screenOrientation=""属性若还可以翻转添加相机权限.
### 生命周期
- onCreate()设置布局 控件初始化
- onStart()注册一些监听 内容观察者,后台,可见,不可获得焦点
- onReStart() 
- onResume()再次对数据进行查询,前台,可见,可获得焦点 
- onPause()数据临时保存,后台,开启新的activity,会先执行完该方法,才去执行onResume
- onStop()取消监听,不可见
- onDestroy()对资源进行回收 cursor关闭cursor.close();   bitmap进行回收 bitmap.recycle();
- onSaveInstanceState(Bundle outState)内存不足时调用，保存数据  outState.putXxx(key,value)
- onRestoreInstansceState(Bundle saveInstanceState) 恢复数据  saveInstanceState.getXxx(key)

## Activity小知识
- 作为dialog的activity在清单文件配置theme：@android：style/
- 不希望重新创建activity实例，在清单文件配置configChanges="orientation|keyboardHidden"，重写activity的onConfigurationChanged()方法。
- 判断横竖屏方向
```java
private void adjustOrientation(){
  if(newConfig.orientation==Configuration.ORIENTATION_LANDSCAPE){
    //横
  }else if(newConfig.orientation==Configuration.ORIENTATION_PORTRAIT){
    //竖
  }
}
```

## 生命周期执行情况

```markdown
**情况一**
  开启activity:onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy,再打开:onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy
**情况二**
  onCreate,onStart,onResume中直接startActivity: A:onCreate-onStart-onResume-onPause,B:onCreate-onStart-onResume,A:onStop.后退键:B:onPause,A:onRestart-onStart-onResume-onPause,B:onCreate-onStart-onResume,B:onStop-onDestroy,A:onStop
**情况三**
  onCreate中直接startActivity,finish: A:onCreate,B:onCreate-onStart-onResume,A:onDestroy后退键:B:onPause-onStop-onDestroy
**情况四**
  onCreate,onStart,onResume中直接startActivity(对话框activity): A:onCreate-onStart-onResume-onPause,B:onCreate-onStart-onResume.后退键:B:onPause,A:onResume,B:onStop-onDestroy
**情况五**
  开启activity:onCreate-onStart-onResume,home:onPause-onStop,再打开:onRestart-onStart-onResume,后退键:onPause-onStop-onDestroy
**情况六**
  开启activity:onCreate-onStart-onResume,横竖屏切换:onPause-onSaveInstanceState(activity异常终止才调用)-onStop-onDestroy-onCreate-onStart-onResume,后退键:onPause-onStop-onDestroy
```


## Activity间传递数据
- Intent传递
```markdown
1. 传递基本类型数据putExtra(key,value);获取通过getXxxExtra(key)实现;
2. 通过Bundle（对HashMap封装，本身实现Parcelable序列化）传递数据，先将数据存储到Bundle中，通过intent.putExtras(bundle)传递，获取通过intent.getExtras()得到bundle对象，然后调用getXxx(key)得到传递过来数据;
3. 传递对象，类需实现Parcelable或Serializable接口，调用putExtra(key,object);通过getParcelableExtra(key)或getSerializableExtra(key);
4. 提供了部分方法可以传递集合数据。
```

## Activity返回数据

- 启动activity，调用startActivityForResult(),重写onActivityResult()方法，通过请求码和响应码判断，获取返回结果
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if(requestCode==?){
        if(data!=null){
            String name=data.getExtra("key")
        }
    }
}

Intent intent=new Intent(this,Src.class);
string value=adapter.getItem(position);
intent.putExtra("key",value);
setResult(result,intent);//结果码：标识从哪个activity返回数据
finish();
```
## 启动过程
###### 启动方法最终会调用startActivityForResult（），接着调用Instrumentation的execStartActivity（），最终一个activity的启动是通过调用ActivityManagerNative.getDefault().startActivity()完成的。
- AMS(ActivityManagerService)继承自ActivityManagerNative，ActivityManagerNative继承自Binder并实现了IActivityManager这个Binder接口，因此AMS也是一个Binder，它是IActivityManager的具体实现。ActivityManagerNative.getDefault()就是AMS。
- Instrumentation的execStartActivity（）中调用checkStartActivityResult(int result, Object intent)检查启动的Activity，常见的没有在清单文件配置这个activity异常就是在该方法中抛出的。
- activity的启动过程中ActivityStackSupervisor和ActivityStack之间传递顺序如下图


- IApplicationThread接口继承自IInterface接口是一个Binder对象，内部声明大量和activity以及service启动停止相关功能。
- 最终实现IApplicationThread接口的是ApplicationThread，该类是ActivityThread的内部类，最终activity的启动又回到了ApplicationThread中，通过scheduleLaunchActivity()启动activity
- scheduleLaunchActivity()启动activity是通过Handler（H）发消息方式启动的
- handleMessage（）根据消息类型处理,若为LAUNCH_ACTIVITY，调用handleLaunchActivity（），从该方法中可以看到Activity对象的创建是通过performLaunchActivity（）完成的。并且ActivityThread通过handleResumeActivity（）来调用被启动activity的onResume()。
#### performLaunchActivity（）任务
- 从ActivityClientRecord中获取待启动activity的组件信息
- 通过Instrumentation的newActivity()使用类加载器创建activity对象
- 通过LoadedApk的makeApplication()尝试创建application对象（也是通过类加载器创建的），若不为null直接返回，一个应用中只能存在一个Application对象，创建后会通过Instrumentation的callApplicationOnCreate()来实现Application的onCreate（）调用
- 创建ContextImpl对象，并通过activity的onAttach（）完成重要数据的初始化，ContextImpl通过activity的onAttach（）和activity建立关系，onAttach（）中还完成了window的创建，并建立自己和window关联，这样当window接收外部输入事件后，就可以将事件传递给activity
- 调用activity的onCreate（）：Instrumentation.callActivityOnCreate.onCreate()执行所以activity启动成功

## 禁止截屏
```java
WindowManager.LayoutParams.FLAG_SECURE;
```


