### js变量
#### js变量用`var` 声明

### js数据类型
属性|描述
---|---
String|字符串
Number|数值
Boolean|布尔
Array|数组
Object|对象
null|空
-|未定义

### js事件
属性|描述
---|---
onClick|单击事件
onMouseOver|鼠标经过事件
onMouseOut|鼠标移出事件
onChange|文本内容改变事件
onSelect|文本框选中事件
onFocus|光标聚集事件
onBlur|移开光标事件
onLoad|网页加载事件
onUnload|关闭网页事件


### DOM操作HTML
#### JavaScript 能够改变页面中所有的HTML元素
- 改变HTML输出流document.write();
    - 绝对不要在文档加载完成之后使用document.write()。这会覆盖该文档
- 寻找元素
    - 通过id找到HTML元素 
    ```javascript
    function demo() {
        document.getElementById("id")
    }
    
    ```
    - 通过标签名找到HMLT元素,如果有相同的标签名，那么就是找到第一个，如果想要后面的可以在后面加 [下标] 来获取。
    ```javascript
    function demo() {
        document.getElementByTagName("tagName")
        document.getElementByTagName("tagName")[0];
    }
    ```
- 改变HTML内容
    - 使用innerHTML属性
     ```javascript
     function demo() {
         document.getElementById("id").innerHTMl="value";
     }
     ```
- 改变HTML属性
    - 使用attribute
    ```javascript
    function demo() {
        document.getElmentById("a").href="www.baidu.com";
    }
    ```
- getElementsByName 通过name获取元素
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    
    </head>
    <body>
    
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <script>
        function getName() {
           var x = document.getElementsByName("pn");
           var z = x[3].innerHTML = "world";
        }
        getName()
    </script>
    </body>
    </html>
    ```
- getElementsByTagName 通过标签名获取元素
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    
    </head>
    <body>
    
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <p name="pn"> hello </p>
    <script>
        function getName() {
           var x = document.getElementsByTagName("pn");
           var z = x[3].innerHTML = "world";
        }
        getName()
    </script>
    </body>
    </html>
    ```
- getAttribute 获取元素属性 
- setAttribute 设置元素属性
- childNodes 访问子节点
- parentNode 访问父节点
- caeateElement 创建元素节点
    ```
        
    function createNode() {

        var body = document.getElementsByTagName("body")[0];
        var input = document.createElement("input");
        input.type = "button";
        input.value= "点击我";
        body.appendChild(input)
    }
    createNode()
    ```
- caeatetextNode 创建文本节点
- insertBefore 插入节点
    ```
            function addNode() {
        var pn1 = document.getElementById("div");
        var pn2 = document.getElementById("pn2");
        var node = document.createElement("p");
        node.innerHTML = "world";
        pn1.insertBefore(node,pn2);
    }
    addNode()
    ```
- removeChild 删除节点
- offsetHeight 网页尺寸
    ```
        // 获取屏幕的宽高, 不带滚动条
    function getsize () {
        var width = document.body.offsetWidth;
        var height = document.body.offsetHeight;
        alert(width + "" +height)
    }
    ```
- scrollHeight 网页尺寸
#### JavaScript 能够改变页面中所有的css样式
- 通过DOM对象改变css
    - 语法
    ```javascript
    document.getElementById("id").style.property = new style;
    
    function demo() {
        document.getElementById("id").style.background = "blue";
    }
    ```
#### JavaScript 能够对页面中的所有事件作出反应，可以添加多个监听事件。
- DOM EventListener
    - addEventListener
        - 用于向指定元素添加事件监听器
        ```javascript
        document.getElementById("id").addEventListener("事件名", 要执行的函数);
        
        document.getElementById("id").addEventListener("click", function () {
           document.getElementById("id").innerHTML("哈哈哈");
        });
        ```
    - removeEventListener
        - 用于向指定元素移除事件监听器
- DOM 0级事件处理,把一个函数赋值给事件处理程序属性
    ```javascript
    var onclickDemo = document.getElementById("id");
    onclickDemo.onclick=function (){
        alert("hello world")
    }
    ```
    
- DOM 不同的浏览器处理不同的事件等级
    ```javascript
        var x = document.getElementById("id");
        if(x.addEventListener) {
            // 如果支持2级事件，那么就添加二级事件
            x.addEventListener("click", demo);
        } else if(x.attachEvent) {
            // 如果IE浏览器那么只支持attachEvent事件
            x.attachEvent("onclick", demo)
        } else {
            // 如果都不支持采用0级事件
            x.onclick = demo();
        }
        
        function demo(){
            alert("hello world");
        }
    ```
###### 注：DOM一级事件会被覆盖，二级事件不会被覆盖

### 获取事件相关信息
#### 获取事件的类型
```javascript
document.getElementById("id").addEventListener("click", showtype);

function showtype(event) {
    alert(event.type)
}
```
#### 获取事件的目标
```javascript
document.getElementById("a").addEventListener("click", showtype);

function showtype(event) {
    alert(event.target)
}
```

#### 模拟一个事件冒泡,事件冒泡就是外面的事件也会执行
- 阻止事件冒泡 `event.stopPropagation()`
```html
<body>
<div id="x">
    <input id = "a" type="button" value="提交">
</div>
<script>
    document.getElementById("a").addEventListener("click", showtype);
    document.getElementById("x").addEventListener("click", div)
    function showtype(event) {
        alert(event.target)
        event.stopPropagation(); // 阻止事件冒泡
    }
    function div(e) {
        alert("div")
    }
</script>
</body>
```
- 阻止事件默认行为（默认行为就是它本来应该做的事）`        event.preventDefault()
`
	```html
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Title</title>

	</head>
	<body>
	<a href="//www.baidu.com" id="aid">百度一下！</a>

	<script>
		document.getElementById("aid").addEventListener("click", stoprun);
		function stoprun(event) {
			event.preventDefault()
		}
	</script>
	</body>
	</html>
```

###  设置延时
#### setTimeOut(function(),time)

### 数组
- 元素合并
    ```
    var a = [1,2,3]
    var b = [4,5,6]
    var c = a.concat(b);
    ```
- 数组排序
    ```
    var a = [1,24,32,52,244,,2,1]
    var c = a.sort();
    ```
- 升序降序
    ```
    var a = [1,24,32,52,244,,2,1]
    a.sort(function(a,b){
        a-b // 升序
        b-a // 降序
    });
    ```
- 追加元素
    ```
        var a = [1,24,32,52,244,,2,1]
        a.push(90)
    ```
- 元素反转
    ```
      var a = [1,24,32,52,244,,2,1]
      a.reverse();
    ```
    
### Math对象
- round() 四舍五入
- random() 随机数
- max() 返回最大值
- min() 返回最小值
- abs() 返回绝对值



### Window对象
- window.innerHeight - 浏览器的内部高度
- window.innerWidth - 浏览的内部宽度
- window.open 打开窗口
- window.open 关闭窗口
### 计时器
- setInterval : 到达指定时间过后一直执行函数
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p id="pname"></p>
<script>
    
    var x = setInterval(function() {
        getTime()
    }, 1000);

    function getTime() {
        var date = new Date();
       var time =  date.toLocaleTimeString()
        document.getElementById("pname").innerHTML = time;
    }
    
    function stopTime() {
        clearInterval(x)
    }
</script>
</body>
</html>
```
- setTimeout : 延时执行,只执行一次
```
    function time() {
        alert(new Date().toLocaleTimeString())
        setTimeout(function(){time()}, 3000)
    }
    
    clearTimeout()
```
### History对象
### Location对象
属性|描述
---|---
location.hostname|返回web主机的域名
location.pathname|返回当前页面的路径和文件名
location.prot|返回web主机的端口
location.protoclo|返回锁使用的web协议(http或https)
location.href|属性返回当前页面的url
location.assign()|方法加载新的文档
### Screen对象
属性|描述
---|---
screen.availWidth|可用的屏幕宽度
screen.availHeight|可用的屏幕高度
screen.Height|屏幕高度
screen.Width|屏幕宽度
### Navigator对象
### 弹出窗口
### Cookies