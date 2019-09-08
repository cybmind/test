## HTML的标签大全及其使用
### HTML有三大流
- 标准流
    - 区分行内元素和块级元素
- 浮动流
    - 不区分行内元素和块级元素
- 定位流：子绝父相
    - 相对定位：不脱离标准流，所以区分行内元素和块级元素
    - 绝对定位：
      - 脱离标准流，不区分行内元素和块级元素，默认情况下，它是相对于body进行定位的，如果祖先元素有定位流属性那么就是祖先元素为参考点的（静态定位除外）。
      - 如果是以body为参考点，那么就是以body的首屏为参考点，就算后面还有距离都不会定位过去。
      - 绝对定位会忽略父元素的padding属性
      - 如何让绝对定位的元素水平居中？可以设置left的50%,maring-left宽度的一半。高度也一样
    - 固定定位：脱离标准流，不区分行内和块级
    - 静态定位
### <!DOCTYPE>
- 功能：声明这个一个HTML5的版本，并且是一个HTML的文件。
### HTML基础标签
#### \<head>\</head>
- 标签内支持元素
    - lang: ```<head lang = "">``` 设置编码格式。
        - value : en | us
- 常用属性
    - meta：设置编码等信息
        - value : charset="UTF-8"
    - title：设置头信息
- 功能：用来定义一些头部信息，比如编码格式，标题等等。
#### \<body>\</body>
- 功能：存放HTML主体

### HTML标题
#### \<h1>...\<h6>

### HTML段落
#### \<p>\</p>
- 功能：如果用p标签申明的内容会换行
### HTML链接
#### \<a>\</a>
- 功能：定义一个超链接
- 例子：\<a href = "on.rongyipiao.com">点击前往mynote \</a>

### HTML图像
#### \<img>\</img>
- 功能：定义一张图片，可以是图片在本地的位置，也可以是在网络上面的位置。
- 例子：\<img src="image/测试图片.jpg">


### HTML特殊标签
#### \<br/>
- 功能：换行

### HTML标签通用属性
- class：规定元素的类名
- id   ：规定元素的唯一ID
- style：规定元素的样式
- title：规定元素的额外信息


### HTML格式化

标签 | 描述
---|---
\<b> | 定义粗体文本
\<big>|定义大号字
\<em>|定义着重文字
\<i>|定义斜体字
\<samll>|定义小号字
\<strong>|定义加重语气
\<sub>|定义下标字
\<sup>|定义上标字
\<ins>|定义插入字
\<del>|定义删除字


### HTMl5新增的元素
#### 新增的结构元素
-  section：页面中的一个内容模块，页眉页脚，或者其他部分
-  article：
-  aside：
-  header：头
-  hgroup：
-  footer：脚
-  nav：页面中导航链接内容
-  figure：文章中独立的但愿

#### 新增的其他元素
-   video：定义视频，或者视频刘
-   audio：定义音乐，或者音频流
-   embed
-   mark
-   progress
-   meter
-   time
-   ruby
-   rt
-   rp
-   wbr
-   canvas：画布，表示图形，用来绘制页面
-   command
-   details
-   datalist
-   datagrid
-   keygen
-   output
-   source
-   munu

#### 新增的input元素的类型
-   emial：表示输入框输入的一个eamil地址
-   url：表示文本框中输入的一个url地址
-   number：表示文本框里面输入的是数字
-   range：表示输入框里面的数字的范围
-   date pickers：关于日历的时间

### HTMl新增的全局属性
#### contentEditable
    允许用户编辑元素中的内容
#### designMode属性
    用来指定整个页面是否可以编辑，如果可以编辑，那么页面中所有用contentEditable属性指定的值都为true
#### hidden属性
    来标示该属性指向的值是否被浏览器渲染，删除该值就渲染，存在该值就不被渲染
#### spellcheck属性
    对用户输入的值进行语法检查，如果是错误就会提示。
#### tabindex属性
    对用户使用table时，下一次定位的table位置,也可以让一些不能获取到tab焦点的值获取到焦点
    <a href = "#" tabindex="1">hello</a>
    <a href = "#" tabindex="3">hello</a>
    <a href = "#" tabindex="2">hello</a>