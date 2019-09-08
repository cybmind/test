### 持续更新中
### 首先看一下什么是`Arthas` (阿尔萨斯)
`Arthas` 是Alibaba开源的Java诊断工具，它能为我们带来什么呢？

	这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？
	我改的代码为什么没有执行到？难道是我没 commit？分支搞错了？
	遇到问题无法在线上 debug，难道只能通过加日志再重新发布吗？
	线上遇到某个用户的数据处理有问题，但线上同样无法 debug，线下无法重现！
	是否有一个全局视角来查看系统的运行状况？
	有什么办法可以监控到JVM的实时运行状态？

Arthas支持JDK 6+，采用命令行交互模式，同时提供丰富的 Tab 自动补全功能，进一步方便进行问题的定位和诊断。
[Arthas下载地址](http://repository.sonatype.org/service/local/artifact/maven/redirect?r=central-proxy&g=com.taobao.arthas&a=arthas-packaging&e=zip&c=bin&v=LATEST "Arthas下载地址")

### 安装和使用
 下载完成过后是一个压缩包，
![](http://on.rongyipiao.com/docs/1079089832/__pic/0R9vNZbI.png)
先使用`unzip`命令解压, 解压完成过后有如下文件。
![](http://on.rongyipiao.com/docs/1079089832/__pic/LSdwHmZV.png)
直接使用`java -jar arthas-boot.jar`启动即可运行
![](http://on.rongyipiao.com/docs/1079089832/__pic/PwS6dM9m.png)
运行时能够看到有4个Java进程，我们只需要选择我们自己的java进程就可以了，我这里的Java进程是一个SpringBoot进程
也就是上图的第三个进程DemoAppliaction,
![](http://on.rongyipiao.com/docs/1079089832/__pic/xxOwawMY.png)
直接输入 3 在按enter键选择该进程。

### 命令列表
- dashboard
这是一个很强大的命令，能够实时的了解jvm的内存使用青情况，先看看dashboard的输出吧，它的输出是实时的内存信息，每隔100ms打印这100ms里面的qps，jvm内存等情况。
![](http://on.rongyipiao.com/docs/1079089832/__pic/Ve72mQ11.png)
	- Thread 区域
	线程信息
	- Memory区域
	- heap：总堆信息
	- ps_eden_space：堆的新生代eden区内存情况
	- ps_survivor_space：堆的survivor区内存情况
	- ps_old_gen：堆的老年代使用情况
	- nonheap：非堆区内存使用情况，这个地方一般都是使用90%以上
	- code_cache：代码缓存区域
	- metaspace：元空间
	- GC区域
	- gc.ps_scavenge.count: 新生代垃圾收集次数
	- gc.ps_scavenge.time(ms)： 新生代垃圾收集清除的总耗时
	- gc.ps_marksweep.count：全堆扫描收集垃圾次数
	- gc.ps_marksweep.time: 全堆扫描收集垃圾总耗时
	- Runtime区域
	- Tomcat区域
###### 注：如果系统有无缘无故卡顿的情况可以分析GC区域。
- thread 命令
	- `thread` 如果不带任何参数，查看所有进行中的线程
	![](http://on.rongyipiao.com/docs/1079089832/__pic/JXwsbZfd.png)
	- `thread -b`  查看使其他线程阻塞的罪魁祸首
		模拟一个启动了两个线程，然后都去调用threadTest1方法，可以看到会有一个线程先进去，另一个线程就在等待着第一个执行完，这个时候另一个线程就在等待着释放锁，这个时候就阻塞了。
	```Java
	package com.test.demo;

	public class ThreadTest {
		public synchronized void threadTest1() {
			try{
				Thread.sleep(60000);
				System.out.println(Thread.currentThread().getName()+ "正在执行");
			} catch (Exception e) {

			}
		}

		public static void main(String[] args) {
			ThreadTest threadTest = new ThreadTest();
			new Thread(() -> threadTest.threadTest1()).start();
			new Thread(() -> threadTest.threadTest1()).start();
		}
	}
	```
	使用`thread -b` 命令查看阻塞的线程, 可以看到Thread-0就是这个罪魁祸首，一下就把它揪出来了。
	![](http://on.rongyipiao.com/docs/1079089832/__pic/wKIdbPrS.png)
- jad 命令
	- 功能：可以对类进行反编译
	- 使用方法 + 参数信息
		- `jad 包名加类名` jad 类和所在包的全路径
			- 例子：`jad com.test.demo.controller.UserController`
			- 用途参考
				- 如果发现修改的代码发布出去不生效，可以通过该方法查看是否成功发布，是否是分支提交错了，或者打包错误等等。
		- `jad 包名加类名 方法名` jad 类全路径加名字 方法名
			- 例子：`jad com.test.demo.controller.UserControlelr getUser`
			- 用途参考
				- 如果当类太大时，就不方便查看所有方法的反编译信息了，所以可以直接对方法进行反编译。
- sc 命令
	- 功能：可以查找所有已经被ClassLoader加载到jvm的类。
	- 使用方法 + 参数信息
		- `sc 包名加类名` sc 类所在包的全路径,也可以不用全路径，可以使用模糊`*`
			- 例子：`sc com.test.demo`
			- 例子：`sc com.test.*`
			- 用途参考
				- 如果我们新建了一个类，然后发布到服务器上面，但是去调用该类的时候爆出`could not find Class`等关于找不到类的错误信息时，就可以使用`sc` 命令来查看这个类是否被ClassLoader加载到了jvm里面。
- sm 命令
	- 功能：查看一个类的所有方法。
	- 使用方法 
		- `sm 包名加类名`
		- 例子 + 参数
			- 没有任何参数 `sm com.test.demo.controlelr.UserController`
			- -d 查看每个方法的详情 `sm -d com.test.demo.controlelr.UserController`
- dump 命令
	- 功能：备份一个class文件
	- 使用方法
		- `dump 包名加类名`
			- 例子: `dump com.test.demo.controller.UserController`
		- 用途参考
			- 后面有一个redefine功能是用来直接替换jvm中已经加载好的class信息，但是一但被替换过后就没法还原了，所以这里有个备份，到时候用这个备份的再去替换一下已经被替换过的类。
- redefine 命令
- monitor 命令 | 方法内部调用路径，并输出方法路径上的每个节点上耗时
- trace 命令 | 方法内部调用路径，并输出方法路径上的每个节点上耗时
- tt 命令 | 记录下当前方法的每次调用环境现场。