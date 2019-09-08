### 1，入口函数的区别
#### 如果同时写了两个入口函数会是那个先生效？
- 会是原生先生效
#### 看入口函数的代码的区别 
- 这里是使用原生和jquery同时获取一个网络上的图片的宽度
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <script src="jquery-3.3.1.js"></script>
    <script>
        window.onload = function(){
            console.log("js",document.getElementsByTagName("img")[0].width)
        }
        
        $(document).ready(function(){
            console.log("jquery",$("img")[0].width)
        })
    </script>
    
    <img src="http://g.search1.alicdn.com/img/i1/100555908/O1CN011tVuQX4WNQpiT3b_!!0-saturn_solar.jpg_220x220.jpg_.webp">
    </body>
    </html>
    ```
- 打开浏览器调试发现都打印出来了
 ![](http://uninote.com.cn/docs/1079089832/__pic/OUAUrPTx.png)
- 现在我们将浏览器缓存清除掉
![](http://uninote.com.cn/docs/1079089832/__pic/rHoe31IH.png)
- 现在在来刷新浏览器，查看宽度，发现jquery的宽度已经没有了。
![](http://uninote.com.cn/docs/1079089832/__pic/VSR5JBAG.png)
- 总结：原生的js是会等到dom节点加载完成并且图片也加载完成，jquery虽然也会等待dom节点加载完成，但是不会等图片加载完成，就会提前执行。

- 如果同时有多个原生和多个jquery的入口函数，那么原生的会被后面的替换掉，而jquery不会。
- jquery可以设置holdready属性来让jquery不加载