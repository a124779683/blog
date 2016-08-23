# 适配器模式（Adapter）
>将一个类的接口转换成客户希望的另外一个接口。Adapter 模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

### 适配器所解决的问题
做兼容！！！！使不兼容的两个接口的类可以在一起工作**,做法是将类自己的接口包裹在一个已存在的类中。

## 适配器模式中的角色及类图
1. 目标接口(Target)，客户所需要调用的接口
2. 适配器类(Adapter)，实现了目标接口的类
3. 需要适配的类（Adaptee）

![](http://a3.qpic.cn/psb?/V12r9fmQ4ALxpC/jIn.01ye3qn0.Sb.6KeobX84d*EFFpl0CELOnrLMVDw!/b/dK0AAAAAAAAA&bo=PwLXAAAAAAAFB84!&rf=viewer_4) 


## 适配器模式的优缺点
* 优点  
适配器模式也是一种包装模式，它与装饰模式同样具有包装的功能，此外，对象适配器模式还具有委托的意思。总的来说，**适配器模式属于补偿模式，专用来在系统后期扩展、修改时使用**。

* 缺点  
**过多的使用适配器，会让系统非常零乱**，不易整体进行把握。比如，明明看到调用的是 A 接口，其实内部被适配成了 B 接口的实现，一个系统如果太多出现这种情况，无异于一场灾难。因此如果不是很有必要，可以不使用适配器，而是直接对系统进行重构。

## 对象适配器模式

目标接口

	/*
	 * 定义客户端使用的接口，与业务相关
	 */
	public interface Target {
		 /*
		 * 客户端请求处理的方法
		 */
		public void request();
	}

被适配的类

	/*
	 * 已经存在的接口，这个接口需要配置
	 */
	public class Adaptee {
		 /*
		 * 原本存在的方法
		 */
		public void specificRequest(){
			//业务代码
		}
	}


适配器类

	/*
	 * 适配器类
	 */
	public class Adapter implements Target{
		 /*
		 * 持有需要被适配的接口对象
		 */
	
		private Adaptee adaptee;
	
		/*
		 * 构造方法，传入需要被适配的对象
		 * @param adaptee 需要被适配的对象
		 */
		public Adapter(Adaptee adaptee){
			this.adaptee = adaptee;
		}
	
		@Override
		public void request() {
			// TODO Auto-generated method stub
			adaptee.specificRequest();
		}
	}

### 我们来看一个实际开发过程中可能遇到的一个对象适配器案例
我们需求是做一个日志记录功能，以前可能是使用数据库进行数据存储。后面我们可能需要更换成文件存储的形式，但是对于客户端来说我们不希望更换接口调用的方式，我们还希望使用原来的调用方式。虽然文件存储方式的接口可能和数据库存储接口不一致，我们也不想去修改接口来达到这个目的。那就可以使用适配器模式。

被适配角色接口（Adaptee Interface）

	/*
	 * 读取日志文件，从文件里面获取存储的日志列表对象
	 * @return 存储的日志列表对象
	 */
	public interface LogFileOperateApi {
		 public List<LogBean> readLogFile();
		 /**
		 * 写日志文件，把日志列表写出到日志文件中去
		 * @param list 要写到日志文件的日志列表
		 */
		 public void writeLogFile(List<LogBean> list);
	}

被适配角色实现类（Adaptee）

	/*
	 * 实现对日志文件的操作
	 */
	public class LogFileOperate implements LogFileOperateApi{
		 /*
		 * 设置日志文件的路径和文件名称
		 */
		private String logFileName = "file.log";

		/*
		 * 构造方法，传入文件的路径和名称
		 */
		public LogFileOperate(String logFilename){
			if(logFilename!=null){
			this.logFileName = logFilename;
			}
		}
		
		@Override
		public List<LogBean> readLogFile() {
			// TODO Auto-generated method stub
			List<LogBean> list = null;
			ObjectInputStream oin =null;
			//业务代码
			return list;
		}
		
		@Override
		public void writeLogFile(List<LogBean> list) {
			// TODO Auto-generated method stub
			File file = new File(logFileName);
			ObjectOutputStream oout = null;
			//业务代码
		}
	
	}


适配器接口(Target)，也就是目标接口

	public interface LogDbOpeApi {
		 /*
		 * 新增日志
		 * @param 需要新增的日志对象
		 */
		public void createLog(LogBean logbean);
	}

接口实现的具体角色
	
	/*
	 * 适配器对象，将记录日志到文件的功能适配成数据库功能
	 */
	public class LogAdapter implements LogDbOpeApi{
		 private LogFileOperateApi adaptee;

		 public LogAdapter(LogFileOperateApi adaptee){
		 	this.adaptee = adaptee;
		 }

		@Override
		public void createLog(LogBean logbean) {
			// 这里使用文件存储的接口去实现存储功能，间接兼容了文件存储接口，有点像代理呵
			List<LogBean> list = adaptee.readLogFile();
			list.add(logbean);
			adaptee.writeLogFile(list);
		}
	}

## 双向适配器
适配器也可以实现双向的适配，前面所讲的都是把 Adaptee 适配成为 Target，其实也可以把 Target 适配成为 Adaptee。也就是说这个适配器可以同时当作 Target 和 Adaptee 来使用。

使用的正确姿势：
**双向适配器同时实现了 Target 和 Adaptee 的接口**，使得双向适配器可以在 Target 或 Adaptee 被使用的地方使用，以提供对所有客户的透明性。尤其在两个不同的客户需要用不同的地方查看同一个对象时，适合使用双向适配器。

以上面案例来实现双向适配器

	/*
	 * 双向适配器对象案例
	 */
	public class TwiceAdapter implements LogDbOpeApi,LogFileOperateApi {
	 
		/*
		 * 持有需要被适配的文件存储日志的接口对象
		 */
		private LogFileOperateApi fileLog;

		/*
		 * 持有需要被适配的 DB 存储日志的接口对象
		 */
		private LogDbOpeApi dbLog;
		
		public TwiceAdapter(LogFileOperateApi fileLog,LogDbOpeApi dbLog){
			this.fileLog = fileLog;
			this.dbLog = dbLog;
		}

		@Override
		public List<LogBean> readLogFile() {
			// TODO Auto-generated method stub
			return null;
		}
		
		@Override
		public void writeLogFile(List<LogBean> list) {
			// TODO Auto-generated method stub
		}
		
		@Override
		public void createLog(LogBean logbean) {
			// TODO Auto-generated method stub
			List<LogBean> list = fileLog.readLogFile();
			list.add(logbean);
			fileLog.writeLogFile(list);
		}
	}

## 类适配器模式（这在单继承语言里很少使用，一般在C++ 和Python使用较多）
采用多重继承对一个接口与另一个接口进行匹配。由于 Java 不支持多重继承，所以到目前为止还没有涉及。  
还是用上面例子说明:

	/*
	 * 类适配器对象案例
	 */
	public class ClassAdapter extends LogFileOperate implements LogDbOpeApi{
	
		public ClassAdapter(String logFilename) {
			super(logFilename);
			// TODO Auto-generated constructor stub
		}
		
		@Override
		public void createLog(LogBean logbean) {
			// TODO Auto-generated method stub
			List<LogBean> list = this.readLogFile();
			list.add(logbean);
			this.writeLogFile(list);
		}
	}

## 适配器模式和装饰者模式及代理模式
适配器模式有装饰的功能，同时在对象适配器模式下还有委托，也就是代理的功能。但是他们不完全一样，代理模式很多情况下代理角色和被代理角色间实现的都是相同的接口，强调对被代理对象的控制。而适配器模式则是完全两个不相同的接口要使得他们能够兼容。他们的目的是不一样的，一个是为了控制，另一个是为了兼容。因此他们在使用方式和具体实现上还是有区别的。

## 参考资料
1. [适配器模式原理及实践](http://www.ibm.com/developerworks/cn/java/j-lo-adapter-pattern/index.html#toggle)
2. [适配器模式笔记](http://www.cnblogs.com/wangjq/archive/2012/07/09/2582485.html)