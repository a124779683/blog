## 一、RxJava基础

###名字解释：
* Observable 能被观察到的对象，他是事件的产生者
* Subscriber 订阅者，他是事件的消费者
* Operators 操作符，对 Observable 发出的事件进行修改和变换
* subscribeOn 订阅的事件产生和变换在哪个线程
* subscribe 订阅的动作

 RxJava最核心的就是Observable和Subscriber，由可被观察的对象发出事件由订阅者来处理。其中他们之间的关系由subscribe这个动作进行关联，只有进行关联后才会发出事件。RxJava是一个扩展的观察者模式

###Operators 操作符
操作符是为了解决进行对象变化使用的，因为Subscriber应该专注于Observable发出的事件，而不是去修改他的事件。
操作符又分为转换类，过滤类和组合类

#### 转换类操作符(map flatMap concatMap flatMapIterable switchMap scan groupBy ...)；

* map操作符

     对Observable发射的数据都应用一个函数（Func1），然后将原始类型的事件转换成其他类型。最后map()方法返回一个新的Observable。

* flatMap操作符

     flatMap和map操作符很相像，flatMap发送的是一个全新的Observable，map操作符发送的是应用函数后返回的结果集。应用场景，例如多个不同网络请求有依赖关系，必须先请求A请求才能请求B请求。我们就需要用flatMap变化生成一个全新的Observable。

* concatMap操作符

     与flatMap功能一样。map操作符都是进行类型变换使用的，他们最后都能完成同样的功能，不同的是flatMap是无序的，concatMap是有序的。

#### 过滤类操作符(fileter take takeLast takeUntil distinct distinctUntilChanged skip skipLast ...)；


####  组合类操作符(merge zip join concat combineLatest and/when/then switch startSwitch ...)。

* concat操作符

     从concatMap操作我们知道，concat操作符肯定也是有序的，而concat操作符是接收若干个Observables，发射数据是有序的，不会交叉。应用场景，使用 concat操作符 对缓存进行检查，如：内存缓存、本地缓存、网络，那一层有数据立即返回。需要搭配first或者takeFirst使用

#### 其他操作符（条件与布尔操作）

* buffer操作符,List操作符

     buffer和List都是用于缓冲一组Observable对象使用的。应用的场景，需要返回一组数据时候，例如压缩一批文件，需要返回一批压缩过后的文件。

* all操作符

     判断Observable发射的数据是否满足某个条件。如果原始Observable正常终止并且每一项数据都满足条件，就返回true；如果原始Observable的任何一项数据不满足条件就返回False

* amb操作符

     它只发射其中首先发射数据或通知（onError或onCompleted）的那个Observable的所有数据，而其他所有的Observable的发射物将被丢弃。类似的对象方法ambWith。Observable.amb(o1,o2)和o1.ambWith(o2)是等价的

* first,takeFirst,firstOrDefault

     first是只发射第一个事件，takeFirst接收一个函数做为判断是否发射第一个事件的条件，其他被丢弃，firstOrDefault是在takeFirst的基础上加上一个默认值，如果没有任何一个满足条件的话则会发送默认值。

* take,takeLast

	发射前几个和后几个事件，其他事件不发射。

### Scheduler线程控制
- RxJava默认在哪条线程订阅就在哪条线程执行，当然我们也可以按照以下五种线程模式来指定我们事件产生和消费的线程
- Schechule.io IO线程，他是一个无数量上限，但是能够重用的线程池。他的效率高于newThread()
- Schechule.newThread 默认都会开启一条新的线程
- Schechule.computation
- Schechule.immediate 默认线程，就是当前产生的线程
- AndroidSchechule.mainThread 扩展的安卓主线程




