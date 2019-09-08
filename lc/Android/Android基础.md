## Android 开发基础
## 工具/环境
- Android Studio
- JDK 1.8
- SDK
## Activity生命周期
- onCreate 创建
- onStart 开始
    - 当onCreate过后就是这个状态 
    - 当onRestart的时候会从这里开始
- onResum 显示到界面
    - 当onStart过后就会进入这个状态
- onPause 暂停
    - 当应用在后台的时候就会处于该状态。
    - 当应用的当前页面被覆盖的时候就会有该状态啊。
- onStop 停止
    - 当应用在后台和退出的时候就会是该状态
- onDestroy 销毁
    - 当应用退出的时候就会是该状态。
- onRestart 从新开始
    - 当应用没有销毁，并且是在后台，然后重新启动了，就会触发这个状态。
## Activity
###### 注：`Acitvity`的名字不能包含大写字母
###### 注：创建一个新的`Activity`如果被引用了，那么必须要在`AndroidManifest.xml`里面注册，不然使用的时候会闪退。
## 控件
### 比较常用的控件
##### 控件view的通用属性：宽高，颜色，边距，是否可见，内容居中，点击事件等。
- TextView : 文本
    - **id**: 设置一个TextView的唯一ID
    - **singleLine**:将文本支显示在一行，后面的用省略号代替。
        - 值：`true` or `false`
    - **layout_width**:设置所占视图的宽度，其他的这种视图雷同。
        - 值：`match_parent` 和父控件一样
        - 值：`wrap_content` 自适应
        - 值：可以单独设置具体大小，但是必须要用`dp`为单位，比如20dp。
    - **textSize**: 设置文本的大小，其他雷同
        - 值：单独设置具体大小，但是必须要用`sp`为单位，比如20sp。
    - **maxLines**: 设置文本最大显示的行数，如果超过就不显示。
- EditText : 编辑框
    - **inputType**: 设置文本内容的类型，强制约束的。
        - 值：`text, phone, textPassword` 等
- ImageView : 图片视图
    - **src**: 资源文件，文件不会被拉伸
    - **background**: 背景，会被拉伸，如果和src同时存在，src会覆盖background。
- SeekBar : 滑动条
    - **progress**: 当前进度。
- RatingBar : 评分条
- ProgressBar : 进度条
- WebView : 加载网页
- ListView : 显示列表
- GridView : 显示表格式列表
- ScrollView : 内容可滚动视图
- SufaceView : 非常重要的绘图容器

## 布局
### 五大布局
#### LinearLayout
###### 线性布局
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    
    -- match_parent:宽度跟随父控件
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    
    // orientation 设置内容显示的方向 vertical 为竖向显示
    // orientation 设置内容显示的方向 horizontal 为横向显示
    android:orientation="vertical"
    
    // 设置控件里面的内容的对齐方式
    android:gravity="center"
    tools:context=".MainActivity">
    
    // 可以按照权重来分配距离，可以做水平分隔
    android:layout_weight="1 />
</LinearLayout>
```

#### RelativeLayout
###### 相对布局
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
<Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="hello"
        <!--在某个与元素的左边-->
        android:layout_toLeftOf="@+id/buttonPanel"
        <!--在某个与元素的上面-->
        android:layout_above="@+id/buttonPanel"
        <!--在某个与元素的下面-->
        android:layout_below="@+id/buttonPanel"
        <!--在某个与元素的右边-->
        android:layout_toRightOf="@+id/buttonPanel"
        <!--在父元素的某个方向-->
        android:layout_alignParentBottom="true"
        <!--依赖于某个元素的某个方向对齐，这里是左边-->
        android:layout_alignLeft="@+id/buttonPanel"
        />
</RelativeLayout>
```
#### FrameLayout
###### 帧布局
#### AbsoluteLayout
###### 绝对布局[已经弃用]
#### TableLayout
###### 标准布局

## 常用属性
- `<incloud />` 重用布局文件
    ```
    <include layout="@layout/activity_splash" />
    ```
- `<merge />` 减少视图层级,这里在merge里面的信息都不会显示。
    ```
    <merge xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <include layout="@layout/activity_splash" />
        <Button
            android:id="@+id/buttonPanel"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentBottom="true"
            android:text="button"
            android:layout_alignParentRight="true"/>
    
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="hello"
            android:layout_alignLeft="@+id/buttonPanel"
            android:layout_above="@+id/buttonPanel"/>
    </merge>
    ```
- `<ViewStub />` 需要时才加载

## 最重要的ListView
###### ListView就只是一个容器或者说是盒子，只需要向里面放东西就行了，现在市面上百分之99以上的app都用来ListView来渲染页面，它主要是用来做列表的。

### ListView 使用初体验
###### 实现一个列表，点击按钮跳转到一个页面，并且将数据展示到页面上。
- 第一步：创建一个页面，并且绑定一个按钮的点击事件。
    - 创建有按钮的跳转页面,创建类`MainActivity`
        ```
        public class MainActivity extends AppCompatActivity {
        
            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                
                // 给MainActivity设置视图为activity_main
                setContentView(R.layout.activity_main);
                
                // 监听按钮点击事件
                findViewById(R.id.button3).setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                    
                        Intent intent = new Intent(MainActivity.this, ListViewDemoActivity.class);
                        startActivity(intent);
        
                    }
                });
        }
        ```
        
    - 创建被监听按钮的`activity_main`视图页面
        ```
        <?xml version="1.0" encoding="utf-8"?>
        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="horizontal"
            android:gravity="center"
            tools:context=".MainActivity">
        
            <Button
                android:id="@+id/button3"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentTop="true"
                android:layout_centerHorizontal="true"
                android:text="点击我" />
        <RelativeLayout />
        ```
- 第二步：创建ListView的相关页面
    - 创建类`ListViewDemoActivity`，用它来装ListView的内容
    
        ```
        public class ListViewDemoActivity  extends Activity {
            private ListView phoneBookListView;
        
            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.activity_list_view_demo);
        
                // 首先找到ListView
                phoneBookListView = findViewById(R.id.list_view);
                
                // 构建一个电话簿的适配器，这个适配器是要用来存通讯录的人名。
                PhoneBookAdapter phoneBookAdapter = new PhoneBookAdapter(ListViewDemoActivity.this);
        
                // 使用适配器与数据列表进行绑定
                phoneBookListView.setAdapter(phoneBookAdapter);
            }
        }
        ```
    - 创建`PhoneBookAdapter`这个适配器，主要实现都在这个适配器里面完成
        ```
        public class PhoneBookAdapter extends BaseAdapter {
        
            /**
             * 全局上下文
             */
            private Context mContext;
        
            /**
             * LayoutInflater用来读取视图，也就是用它来解析视图的内容
             */
            private LayoutInflater mLayoutInflater;
        
            /**
             *初始化一个假数据
             */
            private String[] strings = {"小明", "小红", "小花"};
            
            public PhoneBookAdapter(Context context) {
                this.mContext = context;
                // LayoutInflater:使用一个系统的服务来初始化它
                mLayoutInflater = (LayoutInflater)context.getSystemService(context.LAYOUT_INFLATER_SERVICE);
            }
        
            /**
             * 有多少条数据
             *
             * @return
             */
            @Override
            public int getCount() {
                return strings.length;
            }
        
            /**
             * 返回某一条数据对象
             *
             * @param position
             * @return
             */
            @Override
            public Object getItem(int position) {
                return strings[position];
            }
        
            /**
             * 获取某个对象的对象id
             *
             * @param position
             * @return
             */
            @Override
            public long getItemId(int position) {
                return position;
            }
        
            /**
             * 返回一个视图
             *
             * @param position
             * @param convertView
             * @param parent
             * @return
             */
            @Override
            public View getView(int position, View convertView, ViewGroup parent) {
                // 使用LayoutInflater来找到一个视图并且赋值给convertView
                convertView = mLayoutInflater.inflate(R.layout.layout_phone_book_people, null);
                
                // 使用mLayoutInflater找到的View,通过它来获取一个TextView的视图
                TextView nameView = convertView.findViewById(R.id.name_text_view);
                nameView.setText(strings[position]);
                
                // 返回一个视图
                return convertView;
            }
        }
        
        ```
### ListView 的常用方法
##### 单个元素被点击:`setOnItemClickListener`
```
phoneBookListView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(ListViewDemoActivity.this, getUserList().get(position).getmUserName() + "被点击了", Toast.LENGTH_LONG).show();
            }
        });
```
##### 单个元素被长按:`setOnItemLongClickListener`
```
        // 给每个元素设置长按事件
        phoneBookListView.setOnItemLongClickListener(new AdapterView.OnItemLongClickListener() {

            /**
             * 它的返回值如果为true，那么click事件就会被吃掉，如果为false就不会被吃掉
             *
             * @param parent
             * @param view
             * @param position
             * @param id
             * @return
             */
            @Override
            public boolean onItemLongClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(ListViewDemoActivity.this, getUserList().get(position).getmUserName() + "被长按了", Toast.LENGTH_LONG).show();
                return false;
            }
        });
```
##### 整个ListView被点击:`setOnClickListener`
```
        /**
         * 整个ListView被点击的时候触发
         */
        phoneBookListView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                
            }
        });
```
##### 整个ListView被长按:`setOnLongClickListener`
```
        /**
         * 整个ListView被长按的时候触发
         */
        phoneBookListView.setOnLongClickListener(new View.OnLongClickListener() {
            /**
             /**
             * 它的返回值如果为true，那么click事件就会被吃掉，如果为false就不会被吃掉
             *
             * @param v
             * @return
             */
            @Override
            public boolean onLongClick(View v) {
                return false;
            }
        });
```
           