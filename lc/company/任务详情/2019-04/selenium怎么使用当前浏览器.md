1. selenium怎么使用现有浏览器的数据 
找了很多方案，也试了很多方案，最终得出的结论是selenium是不支持在当前打开的浏览器里面新增一个窗口的形式来执行，但是可以通过另一种方式，在启动的时候给浏览器指定一个dubug模式，然后selenium在执行的时候就可以去调用这个浏览器
https://blog.csdn.net/qq_36328915/article/details/86608844
```
./Google\ Chrome --flag-switches-begin --flag-switches-end --remote-debugging-port=9222
```
(5.5小时)

2. 掘金发布文章采用post方式来进行发布
git节点 dac5302840eae1781e46fdf44cf06336f917f581 
(2.5小时)

