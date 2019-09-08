### css选择器
属性|例子|描述
---|---|---
ID选择器|#div{}|使用#来选择
class选择器|.div{}|使用.来选择
属性|[title]{}|利用一个属性作为选择器，使用来该属性的标签就会使用这个样式
属性值选择器|[title=值]{}|可以是具体的某个值

#### css选择器分组
    h1,h2,h3,h4,h5,h6 {
        color: red;
        
    }
#### css继承
    如果这个属性没有自己的样式就会被继承
    body {
        color: green;
    }
#### css 派生选择器
    p a {
        这是p标签下面的a标签
    }

### css背景
属性|描述
---|---
background-attachment | 背景图像是否固定或者随着页面的其余部分滚动
background-color|设置元素的背景颜色
background-image|把图片设置为元素的背景
background-position|设置背景图片的起始位置
background-repeat|设置背景图片是否重复及如何重复
background-size|设置背景图片的大小

### css文本
属性|描述
---|---
color|文本颜色
direction|文本方向
line-height|行高
letter-spacing|字符间距
text-align|对其元素中的文本
text-decoration|像文本添加修饰
text-indent|所有元素中文本的首行
text-transform|元素中的字母
unicode-bidi|设置文本方向
white-space|元素中国呢空白的处理方式
word-spacing|字间距
text-shadow|文本阴影

### css字体
属性|描述
---|---
font-family|设置字体系列
font-size|设置字体大小
font-style|设置字体风格
font-variant|以小型答谢字特或正常字体显示
font-weight|设置字体的粗细

### css链接
属性|描述
---|---
a:link|鼠标点击之前的颜色
a:visited|鼠标点击过后的颜色
a:hover|鼠标划过的颜色
a:active|鼠标正在点击的颜色
text-decoration|去掉链接的下划线

### css列表
属性|描述
---|---
list-style|简写列表项
list-style-image|列表项图片
list-style-position|列表标志位置
list-style-type|列表类型

### css轮廓
属性|描述
---|---
outline|设置轮廓属性
outline-color|设置轮廓颜色
ontline-style|设置轮廓样式
outline-width|设置轮廓宽度

### css定位机制
- 普通流
    - 元素按照其在HTML中的位置顺序决定排布的过程
- 浮动
- 绝对布局


### css定位
属性|描述
---|---
position|把元素放在一个静态的，相对的，绝对的，或固定的位置中
top|元素向上的偏移量
left|元素向左的偏移量
right|元素向右的偏移量
bottom|元素向下的偏移量
overflow|设置元素溢出其区域发生的事情
clip|设置元素显示的形状
vertical-align|设置元素垂直对齐方式
z-index|设置元素的堆叠方式

### css浮动
属性|描述
---|---
float|浮动元素(left,right,none,inherit)
clear|哪个方向的位置不允许浮动(left,right,both,inherit)

### css盒子模型
属性|描述
---|---
margin|外边距
padding|内边距
border|边框
content|内容

### css边框
属性|描述
---|---
box-shadow|边框阴影　
border-radius|圆角边框
border-image|边框图片

### css3的常用操作
#### 对齐操作
- 使用margin属性进行水平对齐
- 使用position属性进行左右对齐
- 使用float属性进行左右对齐


#### 尺寸操作
属性|描述
---|---
height|设置元素高度
line-height|设置行高
max-height|设置元素最大高度
max-width|设置元素最大宽度
min-width|设置元素最小宽度
min-height|设置元素最小高度
width|设置元素宽度
#### 分类操作
属性|描述
---|---
clear|设置一个元素的侧面是否允许其他的浮动元素
cursor|规定当指向某个元素之上时显示的指针类型
display|设置是否显示元素及如何显示元素
float|设置元素向哪个方向移动
position|把元素定位到一个位置
visibility|设置元素是否可以或不可见
#### 导航栏
- 垂直导航栏
- 水平导航栏
- 导航栏效果

#### 图片操作
属性|描述
---|---
opacity|图片透明度
