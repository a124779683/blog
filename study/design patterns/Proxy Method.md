# 代理模式
> 为其他对象提供一种代理，并以**控制**对这个对象的访问。

## 类图


### 组成成员
* 抽象角色(Subject)
* 代理角色(Proxy)
* 真实角色(RealSubject)

![类图](http://a1.qpic.cn/psb?/V12r9fmQ4ALxpC/6ZuzId0CNBc*.rFvyjPUfZcloxpokvxCXflC4EpGsz0!/b/dHEBAAAAAAAA&bo=twH1AAAAAAABB2M!&rf=viewer_4)


## 代理模式 和 装饰者模式 关系
相同点： 
 
* 代理模式和装饰者模式非常的相似，通过类图的比较我们也能看出他们差别很小，第一，无论是代理类还是被代理类，装饰类还是被装饰类，他们都实现了相同的接口（对于代理模式来说，可以不完全实现相同接口达到，因为代理强调控制，我们可以间接的达到这个功能）
* 他们都能实现部分AOP切面编程的能力。

区别：

* 代理类和被代理类是组合关系，被代理类的生命周期是由代理类控制的。从代码形式上来说，何时该初始化被代理的对象都是有代理类控制的。
* 装饰类和被装饰类是聚合关系。代码上我们一般会分别将他们创建出来，然后通过注入的方式将被装饰类包装起来而已。
* 关注点不一样（装饰者强调的是增强，代理强调的是控制，虽然他也有增强的功能）。首先，装饰模式中的装饰类和被装饰类关注同样的东西（*因此必须具有相同的接口*），目的在于增强这一部分的功能。而代理类与被代理类之间**关注更多的是代理类对被代理类的控制（proxy并不一定要求保持接口的一致性，只要能够实现间接控制，有时候损及一些透明性是可以接受的）**，代理类往往为了处理目标对象功能关系不太紧密的职责，比如权限控制，处理异常。
* 对于客户端，也就是使用者来说，装饰类和被装饰类都是公开透明的，单独使用哪一个都能达到效果。而代理模式对客户端，也就是调用者隐藏被代理类的具体实现，客户端必须使用代理类才能使用功能。

## 应用场景：
1. 想透明并且动态地给对象增加新的职责的时候（与装饰器一样）。
2. 给对象增加的职责，在未来存在增加或减少可能。
3. 用继承扩展功能不太现实的情况下，应该考虑用组合的方式。

## 代理实现的方式

### 1.静态代理

注意：大量使用静态代理会导致类的急剧膨胀，因为一个真实角色需要一个代理角色。

抽象角色

	abstract public class Subject{
	    abstract public void request();
	}

真实角色

	public class RealSubject extends Subject {
	
       public RealSubject(){ 

       }

       public void request(){ 
          System.out.println("From real subject.");
       }
	}

代理角色

	public class ProxySubject extends Subject {
	
		private RealSubject realSubject;  //以真实角色作为代理角色的属性
		
		public ProxySubject(){
		
		}
		
		public void request() {  //该方法封装了真实对象的request方法
		
			preRequest();  
		
			if( realSubject == null ){
				realSubject = new RealSubject();
			}
			
			realSubject.request();  //此处执行真实对象的request方法
			
			postRequest(); 
		}
		
		
		
		private void preRequest(){
			//something you want to do before requesting
		}
		
		
		
		private void postRequest(){
			//something you want to do after requesting
		}
	}

### 2.动态代理

抽象角色

	public interface BookFacade {  
	    public void addBook();  
	}  

真实角色

	public class ServiceImpl implements Service{
	
		@Override
		public void sayHello() {
			ServiceFactory.before();
			System.out.println("Hello world!");
			ServiceFactory.after();
		}
	
		@Override
		public void sayBye() {
			ServiceFactory.before();
			System.out.println("Bye bye!");
			ServiceFactory.after();
		}
	
		@Override
		public void sayHi() {
			ServiceFactory.before();
			System.out.println("Hi");
			ServiceFactory.after();
			
			ServiceFactory.other();
		}
	}

增强方法类

	public class ServiceFactory {
		public static void before(){
			System.out.println("前置日记：打印、启动事务等..");
		}
		
		public static void after(){
			System.out.println("后置日记：打印、关闭事务等..");
		}
		
		public static void other(){
			System.out.println("做其他的事..");
		}
	}


代理角色

	public class MyProxy implements InvocationHandler{
		
		// 目标对象，也就是我们主要的业务，主要目的要做什么事
		private Object delegate;
		
		/**
		 * 和你额外需要做得事情，进行绑定，返回一个全新的对象（写法，基本上固定的）
		 * @param delegate
		 * @return
		 */
		public Object bind(Object delegate){
			this.delegate = delegate;
			return Proxy.newProxyInstance(this.delegate.getClass().getClassLoader(), 
					this.delegate.getClass().getInterfaces(), this);
		}
		
		/**
		 * 你刚才需要执行的方法，都需要通过该方法进行动态调用
		 */
		@Override
		public Object invoke(Object proxy, Method method, Object[] args)
				throws Throwable {
			Object obj = null;
			// 执行前置的方法
			ServiceFactory.before();
			// 通过反射，执行目标方法，也就是你的主要目的
			obj = method.invoke(this.delegate, args);
			// 执行后置的方法
			ServiceFactory.after();
			// 返回值给调用者
			return obj;
		}
	
	}


### 3.Cglib动态代理 
JDK的动态代理机制只能代理实现了接口的类，而不能实现接口的类就不能实现JDK的动态代理，cglib是针对类来实现代理的，他的原理是对指定的目标类生成一个子类，并覆盖其中方法实现增强，但因为采用的是继承，所以不能对final修饰的类进行代理。 另外目标类必须有无参构造我们才能生成目标类的代理对象，否则会报Superclass has no null constructs but no arguements were given

	BookFacadeCglib cglib=new BookFacadeCglib();  
	BookFacadeImpl1 bookCglib=(BookFacadeImpl1)cglib.getInstance(new BookFacadeImpl1());  
	bookCglib.addBook();  



这里牵扯出我们进行[功能增强][more function]的四种方法

 1. 继承（前提有具体实现的父类，不能是接口）
 2. 装饰类（前提有接口）
 3. 代理（前提有接口）
 4. 字节码增强（CGLIB，javassist） 



[more function]:"http://www.baidu.com" (功能增强的四种方式)