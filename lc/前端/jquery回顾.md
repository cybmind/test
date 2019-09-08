### jquery语法
- 基础语法 $("seletor").action();
- 美元符号定义jquery 
- 选择符(selector) "查询和查找"HTML元素
- jquery的action()执行对元素的操作
- 例如：
    - $(this).hide() - 隐藏元素
    - $("p").hide() - 隐藏所有段落

#### 当文档加载完成过后触发的函数
```
$(document).ready(function(){
    alert("文档加载完毕");
});
```
### HTML函数事件
#### 元素事件
-  单击事件click
    ```
    $("p").click(function(){
            $(this).hide();
        })
    ```
-  双击事件dblclick
    ```
    $("p").dblclick(function(){
            $(this).hide();
        })
    ``` 
- 当鼠标移动到按钮之上的时候mouseenter
    ```
    $("button").mouseenter(function(){
        num = num +1;
        if (num % 2 == 1) {
            $("p").text("p元素被修改了");
        }else{
            $("p").text("p有被")
        }

    })
    ```
- 添加一个bind（绑定事件）
    ```
    $(document).ready(function(){
        $("#buildEvent").bind("click",buildEvent1);
    });
    
    function buildEvent1(e) {
        console.log("helloworld");
    }
    ```
- 解除绑定
    ```
    // 当页面所有内容加载完成
    $(document).ready(function(){
        $("#buildEvent").bind("click",buildEvent1);
        $("#buildEvent").unbind("click",buildEvent1);
    });
    
    function buildEvent1(e) {
        console.log("helloworld");
    }
    ```

### JavaScript特效和动画
#### jquery的显示与隐藏
```
$(document).ready(function() {
    // 隐藏
   $("#hide").click(function(){
      $("p").hide(500);
   });

   $("#show").click(function(){
      $("p").show(500);
   });

   $("#toggle").click(function(){
       $("p").toggle(600);
   })
});
```

#### jquery的淡入淡出
```
$(document).ready(function(){

    // 淡出
    $("#fadeIn").click(function(){
        $("div").fadeIn(1000);
    })

    // 淡入
    $("#fadeOut").click(function () {
        $("div").fadeOut(1000);
    })

    // 淡入淡出
    $("#fadeToggle").click(function(){
        $("div").fadeToggle(1000);
    })

    // 自定义
    $("#fadeTo").click(function(){
        $("div").fadeTo(1000,0.1)
    })
});
```

#### jquery的回调函数，以及连续动画的调用
```
    $("#fadeIn").click(function(){
        $("div").fadeIn(1000,function () {
            console.log("hello")
        }).fadeOut(1000);
    })
```
### jquery操作元素
#### jquery捕获具体的内容
```
$(document).ready(function(){
    // 获取文本内容
    $("#btn1").click(function(){
        alert($("#pid").text())
    })

    // 获取带html 标签的内容
    $("#btn2").click(function(){
        alert($("#pid2").html())
    })

    // 获取input标签value值
    $("#btn3").click(function(){
        alert($("#input").val())
    })

    // 获取元素属性
    $("#btn4").click(function(){
        alert($("#aId").attr("href"))
    })
})
```
#### jquery元素的设置(替换)
```
    // 设置text元素内容
    $("#btn5").click(function(){
        $("#pid3").text("www.baidu.com");
    })

    // 设置html元素内容
    $("#btn6").click(function(){
        $("#pid4").html("<a href='www.baidu.com'>百度</a>");
    })

    // 设置inputvalue值
    $("#btn7").click(function(){
        $("#input2").val("www.baidu.com")
    })

    // 设置元素的属性值
    $("#btn8").click(function(){
        alert($("#a2").attr("href","http://www.baidu2.com"));
    })
    
    //  同时设置多个元素的属性值
    $("#btn8").click(function(){
       $("#a2").attr({
            "href":"http://www.baidu2.com",
            "title":"hello"
        });
    });
```

#### jquery元素内容的添加(追加)
```
$(document).ready(function () {
    // 在元素后面添加内容
    $("#btn1").click(function () {
        $("#pid1").append("使用append添加")

    })

    // 在元素后面添加内容
    $("#btn2").click(function () {
        $("#pid2").prepend("使用prepend添加")

    })

    // 从元素上面一行开始添加内容
    $("#btn3").click(function () {
        $("#pid3").before("使用before添加")

    })

    // 从元素下面一行开始添加内容
    $("#btn4").click(function () {
        $("#pid4").after("使用after添加")

    })

})
```

#### jquery删除元素
```
$(document).ready(function () {
    // remove删除的是真个元素
    $("#btn1").click(function () {
        $("#pid1").remove()

    })

    // empty只是将该元素里面的子元素和内容置空
    $("#btn2").click(function () {
        $("#pid2").empty()

    })
})
```

#### jquery对css的操作
```
        $(document).ready(function(){
            // 方式一
            $("#div1").css("width","100px");
            $("#div1").css("height","100px");
            $("#div1").css("background","red");

            // 方式二
            $("#div2").css({
                width:"100px",
                height:"100px",
                background:"blue"
            })

            // 方式三
            $("#div3").addClass("div1")

        })
```

#### jquery的css盒子模型
```
    // height() width() 获取元素的高度和宽度
    $("#btn1").width();

    // innerHeight() innerWidth() 获取元素的高度和宽度在加上内边距
    $("#btn2").innerHeight();
    
    // outerHeight() outerWidth() 获取元素的总高度和宽度不加外边距
    // outerHeight(true) outerWidth(true) 获取元素的总高度和宽度加外边距
    $("#btn2").outerHeight();
    $("#btn2").outerHeight(true);
```

####  jquery向下遍历元素
```
$("#id").children() 向下遍历元素，返回值是它的儿子辈元素，无法获取更深层次元素

$("#id").find(可选参数ID) 向下遍历元素，返回值是它的任意子元素
```