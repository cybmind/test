## 1. Hello World
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
</head>
<body>
<div id="app">
   <p>{{message}}</p>
</div>
<script>
    // 创建一个Vue对象，第一个参数为要操作饿元素，第二个参数为要为元素绑定的数据（元素可以选择是否取值）
    var vue = new Vue({
        el: "#app",
        data: {
            message:"hello world"
        }
    })
</script>
</body>
</html>
```

## 2. v-cloak, v-text, v-html 的作用以及用法
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
</head>
<style>
    [v-cloak]{
        display: none;
    }
</style>
<body>

<div id="app">
    <!-- v-cloak 可以将{{msg}}这个明文隐藏掉-->
    <p v-cloak>{{msg}}</p>

    <!-- v-text 可以渲染文本，并且会覆盖元素里面的内容，它和插值表达式的区别在于它不会被直接显示出来，而且会覆盖元素-->
    <p v-text="msg"></p>


    <!-- 如果想要显示html，必须使用v-html -->
    <p>{{msg2}}</p>
    <p v-text="msg2"></p>
    <p v-html="msg2"></p>

</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            msg: "hello world",
            msg2: "<h1>测试h1标签是否显示为html</h1>"
        }
    })
</script>
</body>
</html>
```
## 3. v-bind的作用以及使用方式
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
</head>
<body>

<div id="app">

    <!-- v-bind 使用方式是在属性前面加上v-bind: 属性里面的值写成变量名-->
    <input type="button" value="按钮1" v-bind:title="title">
    <!-- v-bind指定的值会用js的方式去解析，所以这里title是一个变量，变量可以添加字符串-->
    <input type="button" value="按钮2" v-bind:title="title + '123'">
    <!-- v-bind简写，直接使用冒号-->
    <input type="button" value="按钮3" :title="title + '123'">

</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            title: "测试x-bind的作用"
        }
    })
</script>
</body>
</html>
```

## 4. v-on使用方式
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="js/vue.js"></script>
</head>
<body>

<div id="app">
<p>{{title}}</p>
    <input type="button" value="按钮" v-on:click="dianji">
</div>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            title: "测试x-bind的作用"
        },
        methods:{
            dianji:function () {
                alert("点击我")
            }
        }
    })
</script>
</body>
</html>
```

## 事件修饰符
```
阻止事件的默认行为：@click.prevent
阻止事件冒泡：@click.stop
事件捕获机制：@click.capture  从祖先级别的事件往下执行（先执行祖先级别的事件）
事件只触发一次：@click.once
```