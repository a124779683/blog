OOM的两大根本原因是（1）加载过大的对象和（2）内存泄漏

## 一、加载过大的对象
     这里暂时只对图片处理做出规范。

图片处理

*	图片格式的选择，尽量选择JPG图片，尽可能使用shape图形资源（ JPG 不适用于所含颜色很少、具有大块颜色相近的区域或亮度差异十分明显的较简单的图片。对于需要高保真的较复杂的图像，PNG 虽然能无损压缩，但图片文件较大）

*	大图小用使用 inSampleSize。在图片资源本身较大，或者适当地采样并不会影响视觉效果的条件下，这时候我们输出地目标可能相对较小，对图片分辨率、大小要求不是非常的严格
*	小图大用用矩阵Matrix
*	Bitmap对象使用完后调用recycle()释放内存，才设置为null
*	合理选择Bitmap的像素格式。 
格式	描述
ALPHA_8	只有一个alpha通道
ARGB_4444	这个从API 13开始不建议使用，因为质量太差
ARGB_8888	ARGB四个通道，每个通道8bit
RGB_565	每个像素占2Byte，其中红色占5bit，绿色占6bit，蓝色占5bit

## 二、内存泄漏

使用LeakCanary内存检查工具配合，避免对象的内存泄漏

1. Context 没有合理释放造成的内存泄漏，这个情况是最常见的。常见于Activity和Fragment泄漏，其在onDestroy后还被其他地方引用导致无法被回收掉，这是最糟糕的溢出情况。

2. Context尽量选择ApplicationContext，如果必须使用Context（例如Dialog,PopupWindow,View），Context做为参数传递时，接收参数的类使用WeakReference代替强引用。比如: WeakReference<Context> ref = new WeakReference<>(context);

3. 避免静态成员变量引用资源耗费过多的对象，尤其是Context。例如：public static Context mContext ; 常见于一些工具类中使用Context

4. 集合中的对象没有清理造成的内存泄漏
当集合不需要该对象时，我们应当及时将对象从集合中清理掉

5. Handler的使用(内部类，线程同样适用)，尽可能避免使用内部类
我们使用Handler大多喜欢将Handler作为内部类使用，Handler改为静态类或者静态引用，因为非静态内部类会只有所属类的实例，如果所属类已经结束生命周期（例如Activity），但内部类方法还在执行，也就使Activity无法释放，而静态类这不会有这样的问题。而且大多数Handler都是用于异步通知或者进行延时操作，更加容易导致泄漏。
如果确实需要使用Context,使用弱引用。
onDestroy被调用时，必须取消所有handler任务。

6. 注意监听器的注销。
我们在程序中设置很多监听和回调函数。参考广播、服务应该在合理生命周期里注册绑定和反注册解绑。
有setOnXXXListener就必须有及时remove掉这个Listener的方法。

7. 使用优化过的数据集合。
API当中提供了一些优化过后的数据集合工具类，如SparseArray，SparseBooleanArray，以及LongSparseArray等，使用这些API可以让我们的程序更加高效。传统Java API中提供的HashMap工具类会相对比较低效，因为它需要为每一个键值对都提供一个对象入口，而SparseArray就避免掉了基本数据类型转换成对象数据类型的时间。不过这几个Map是否合适自己自己要针对情况使用。

8. 对象的重复利用
减少对象的重复创建，降低内存的分配和回收。

9. 字符串操作
大量的字符串拼接操作考虑使用StringBuilder替代频繁的+ 字符串连接符。（StringUtils提供getStringFromResource(int id,Object... Args)获取资源文件中字符串拼接方法）

10. WebView泄漏问题（后期修改）
根治方法是为WebView开启另外进程，通过AIDL与主进程进行通信，WebView所在进程选择合适的时机进行销毁。

11. 数据库操作Cursor对象是否及时关闭

## 三、使用策略优化
1. 谨慎使用largeHeap。使用largeHeap会影响系统整体的用户体验，并且会使得每次GC时间更长。

2. onTrimMemory(int level),该方法是在4.0引进，一共有8种级别。这个方法会被频繁调用，我们在收到不同级别通知应该做响应释放资源的操作，占用越少的进程越容易被保存下载。
3. 资源文件选择合适的文件夹进行存放。如果不希望图片被拉伸应该放置在nodpi目录下

4. try catch大内存分配的操作

5. 谨慎使用static对象，静态成员生命周期和应用进程保持一致，应该谨慎使用static对象

6. 留意单例对象中不合理的持有。虽然单例简单实用，但是生命周期同上，不合理使用容易出现对象的泄漏

7. 优化布局层次，减少内存消耗。自定义控件使用merge标签

8. 谨慎使用第三方库，踩过最大的坑就是第三方自定义控件导致activity泄漏。

9. 使用ProGuard提出不需要代码，减少mapping代码需要的内存空间

10. 考虑不同的实现方式来优化内存占用。例如帧动画 改为 属性动画。
