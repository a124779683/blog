# 单例
>保证一个类仅有一个实例，并提供一个访问它的全局访问点。

## 单例设计模式五种写法
1. 饿汉式

		public class HungrySingleton {
		
			private static HungrySingleton instance = new HungrySingleton();
		
			private HungrySingleton(){
			}

		    public static HungrySingleton getInstance(){
				return instance;
			}
		}



2. 懒汉式(线程不安全)

		public class LazySingleton {
		
			private static LazySingleton instance = null;
		
			private LazySingleton(){
			}
		
		    public static LazySingleton getInstance(){
				if(instance == null)
					instance = new LazySingleton();
				return instance;
			}
		}

3. 懒汉式(线程安全,直接加方法锁，高并发性能不好)

		public class LazySingleton {
		
			private static LazySingleton instance = null;
		
			private LazySingleton(){
			}
		
		    public static synchronized LazySingleton getInstance(){
				if(instance == null)
					instance = new LazySingleton();
				return instance;
			}
		}


4. 懒汉式(线程安全,双重验证锁)  
与上面的懒汉式不同的是，在实例化的区域加锁，锁内再次判断是否存在，因为如果两个线程同时判断到了instance为null的时候，无论谁占据锁，都不可能阻止另外一条线程重新实例化一个对象。 变量使用volatile修饰，线程能够自动发现volatile变量的最新值。

		public class DoubleCheckSingleton {
		
			private volatile static DoubleCheckSingleton instance = null;
		
			private DoubleCheckSingleton(){
			}
		
		    public static DoubleCheckSingleton getInstance(){
				if(instance == null){

					synchronized (DoubleCheckSingleton.class){
						if(instance == null){
							instance = new DoubleSingleton();
						}
					}

					instance = new DoubleCheckSingleton();
				}
				return instance;
			}
		}

5. Intialization on demand holder(使用静态内部类)  
这种方式其实也是使用了静态的属性和静态代码块是在类被加载进来的时候只会串行执行的特性，来保证在多线程中不会出现安全问题

		public class LazyLoadInnerClassSingleton {
		
			private static LazyLoadInnerClassSingleton instance = null;

		  	private static class LazyHolder {
				public static LazyLoadInnerClassSingleton singleton = new LazyLoadInnerClassSingleton();
			}

			private LazyLoadInnerClassSingleton(){
			}
		
		    public static LazyLoadInnerClassSingleton getInstance() {
				return LazyHolder.singleton;
			}
		}
[callback]:http://www.baidu.com (回调)