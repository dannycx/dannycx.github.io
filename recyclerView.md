# RecyclerView使用

## 简单实用
### 1.添加依赖
- implementation 'com.android.support:recyclerview-v7:26.+'

### 2.布局使用RecyclerView 
```java
<android.support.v7.widget.RecyclerView
        android:id="@+id/recycler"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

### 3.创建ViewHolder
```java
public class Holder extends RecyclerView.ViewHolder {
    ImageView icon;
    TextView textView;
    //构造方法-必须
    public Holder(View itemView) {
        super(itemView);
        icon = itemView.findViewById(R.id.item_icon);
        textView = itemView.findViewById(R.id.item_title);
    }
}  
```

### 4.创建Adapter
```java
private class Adapter extends  RecyclerView.Adapter<MyHolder>{
       private ArrayList<String> mLists;
       
       public Adapter(ArrayList<String> lists){
          mLists = lists;
       }
       
       //OnCreateViewHolder用来给初始化Holder
       public Holder onCreateViewHolder(ViewGroup parent, int viewType) {
            //参数：1.是打气筒 2.添加到parent 3.判断条件 true
            View view = LayoutInflater.from(getContext()).inflate(R.layout.main_recyclerview_item, parent, false);
            Holder holder = new Holder(view);
            return holder;
        }
        
        //给holder控件设置数据
        public void onBindViewHolder(Holder holder, int position) {
            String title = mLists.get(position);
            holder.title.setText(title);
            holder.icon.setImageResource(R.drawable.icon);
        }
        
        //子view个数
        public int getItemCount() {
            return mLists.size();
        }
```

### 5.RecyclerView设置数据
```java
    privqte void initRecyclerView(){
        //获取RecyclerView控件
        RecyclerView rv = findViewById(R.id.recycler);
        //设置线性管理器，item可水平或垂直LinearLayout.HORIZONTAL, LinearLayout.VERTICAL
        //表格管理器 new GridLayoutManager(this, 3)-3列
        rv.setLayoutManager(new LinearLayoutManager(this, LinearLayout.HORIZONTAL, true));
        //设置Adapter
        rv.setAdapter(new MyAdapter(initList()));
    }
    
    privqte ArrayList<String> initList(){
        ArrayList<String> list = new ArrayList<>;
        for(int i=0; i<20; i++){
            list.add("item-" + i);
        }
        return list;
    }
```

## 进阶
### RecyclerView通用适配器的封装
- 优点：数量动态 类型不限 Map<Integer，View>    
- 作用：封装了Adapter编写的冗余代码，提供简洁的基类

### BaseHolder封装
```java
/**
 * 抽取BaseHolder继承RecyclerView.ViewHolder
 */
public class BaseHolder extends RecyclerView.ViewHolder {
    /**
     * 不写死控件变量，而采用SparseArray方式，Android中代替HashMap的集合
     * 节省内存（避免了对key的自动装箱，内部对数据还采取了压缩的方式来表示稀疏数组的数据）
     * SparseArray稀疏数组：两个一维数组来保存数据，一个用来存key，一个用来存value
     * HashMap哈希表：数组+链表
     */
     private SparseArray<Integer, View> mViews = new SparseArray<>();
    
     public BaseHolder(View itemView) {
         super(itemView);
     }
     
     /**
      * 获取控件的方法
      */
     public<T> T getView(Integer viewId){
        //根据保存变量的类型 强转为该类型
        View view = mViews.get(viewId);
        if(view==null){
           view= itemView.findViewById(viewId);
            //缓存
            mViews.put(viewId,view);
        }
        return (T)view;
     }
    
     /**
      * 传入文本控件id和设置的文本值，设置文本
      */
     public BaseHolder setText(Integer viewId, String value){
         TextView textView = getView(viewId);
         if (textView != null) {
             textView.setText(value);
         }
         return this;
     }
     /**
      * 传入图片控件id和资源id，设置图片
      */
     public BaseHolder setImageResource(Integer viewId, Integer resId) {
         ImageView imageView = getView(viewId);
         if (imageView != null) {
             imageView.setImageResource(resId);
         }
         return this;
     }
     //...还可以扩展出各种控件。
}
```

### BaseAdapter封装
```java
    /** 
     * 封装的时候，参数可以由外部的构造函数或者set方法传递
     */
    public class BaseAdapter<T> extends RecyclerView.Adapter<BaseHolder> {
        private ArrayList<T> mList = new ArrayList<>();
        private int mLayoutId;
        
        public BaseAdapter(int layoutId,List<T> list){
            this.mLayoutId = layoutId;
            this.mList = list;
        }
        
        //onCreateViewHolder用来给RecyclerView创建缓存（初始化Holder）
        @Override
        public BaseHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            View view =   LayoutInflater.from(parent.getContext()).inflate(mLayoutId, parent, false);
            BaseHolder holder = new BaseHolder(view);
            return holder;
        }
        
       //onBindViewHolder给Holder控件设置数据
        @Override
        public void onBindViewHolder(BaseHolder holder, int position) {
            T item = mList.get(position);
            convert(holder,item);
        }
        
        protected void convert(BaseHolder holder, T item) {
             //什么都不做，交给子处理
        }
        
        //item个数
        @Override
        public int getItemCount() {
            return mList.size();
        }
    }
```

### 继承实现
```java
/**
 * extends  继承父类的代码。
 * 覆盖/重写父类方法
 */
public class MyAdapter extends BaseAdapter<String> {

    public MyAdapter(ArrayList<String> list) {
        super(R.layout.recycler_view_item, list);
    }
    
    @Override
    protected void convert(BaseHolder holder, String item) {
        holder.setText(R.id.item_title,item).setImageResource(R.id.image,R.drawable.icon);
    }
}
```

### 

