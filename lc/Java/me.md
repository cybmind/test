## me
- 能够被确定不会更改的变量，和不能被更改的变量加上`final`修饰, 可以避免一些奇怪的bug
- 尽量使类中所有的变量都是私有的，只能通过方法来访问它们
- 数组标识：要用"int[] packets"，而不是"int packets[]"，后一种永远也不要用

### 注释
```
/**
     * Tracks connecting to the first proxy.
     *
     * @param proxy     the proxy connected to
     * @param secure    {@code true} if the route is secure,
     *                            {@code false} otherwise
     */
    public final void connectProxy(final HttpHost proxy, final boolean secure) {
        Args.notNull(proxy, "Proxy host");
        Asserts.check(!this.connected, "Already connected");
        this.connected  = true;
        this.proxyChain = new HttpHost[]{ proxy };
        this.secure     = secure;
    }
```

### 线性表
- 数组如何实现随机访问？随机元素的地址 = 连续内存空间的基础地址 + 随机元素 * 数据类型的大小
- 数组查询数据的时间复杂度是多少？O(1)？其实描述准确的话应该是通过下标随机访问是O(1)，因为如果通过二分查找就是O(logn)了
- 数组删除和修改速度低效的背后原因是什么？
- 如果能确定数组长度，但是又是使用的arrayList，那么一定要创建定长容器，因为ArrayList底层是数组，数组要扩容必须要找到另一个连续的内存空间，然后将数据复制过去，这个过程ArrayList帮我们做了，所以ArrayList在扩容的时候也会去copy数据并且复制数据，而且还要申请内存地址，申请内存地址是很耗时的。
- 为什么数组是从0而不是从1？，因为内存的寻址公式为 元素的地址 = 连续内存空间的基础地址 + 元素位置 * 数据类型的大小， 如果是从1开始那么久便成了 元素的地址 = 连续内存空间的基础地址 + （元素位置-1） * 数据类型的大小

### 链表