# Activity生命周期

###### 状态：激活状态（位于当前任务栈顶）、暂停状态（失去焦点，但仍可见）、停止状态(被另一个activity完全覆盖)
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
```


-startActivityForResult(intent,requestCode);**请求码：标识请求来源**
**需返回数据的activity将数据传回然后关闭**

-listView数据
-写个Adapter实现BaseAdapter
-定义数组String[] names={"张三丰","张无忌","杨逍","白眉鹰王","青翼蝠王","金毛狮王",""};
-getCount()
-getView()
-getItem()
-getItemId()
-listView.setAdapter();
-listView.setOnItemClickListener(new OnItemClickListener(){
-   public void onItemClick(Adapter<?> parent,View view,int position,int id){
-      Intent intent=new Intent(this,Src.class);
-      string value=adapter.getItem(position);
-      intent.putExtra("key",value);
-      setResult(resu,intent);结果码：标识从哪个activity返回数据
-      finish();
-    }
-});

-
