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
