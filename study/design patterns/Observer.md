# 观察者模式（Observer）
>观察者模式，定义了对象之间的“一对多”的依赖关系，这样，当一个对象的状态发生变化时，所有依赖于这个对象的相关对象都被通知并自动更新

>有时被称作发布/订阅模式，观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题(Subject)对象。这个主题对象在状态发生变化时，会通知所有观察者对象，使它们能够自动更新自己。

## 观察者模式所解决的问题
将一个系统分割成一个一些类相互协作的类有一个不好的副作用，那就是需要维护相关对象间的一致性。我们不希望为了维持一致性而使各类紧密耦合，这样会给维护、扩展和重用都带来不便。观察者就是解决这类的耦合关系的。

## 观察者模式中的角色及类图
1. 抽象主题（Subject）：它把所有观察者对象的引用保存到一个聚集里，每个主题都可以有任何数量的观察者。抽象主题提供一个接口，可以增加和删除观察者对象。
2. 具体主题（ConcreteSubject）：将有关状态存入具体观察者对象；在具体主题内部状态改变时，给所有登记过的观察者发出通知。
3. 抽象观察者（Observer）：为所有的具体观察者定义一个接口，在得到主题通知时更新自己。
4. 具体观察者（ConcreteObserver）：实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题状态协调。

![观察者模式类图](http://oddqfb3p7.bkt.clouddn.com/Observer.png) 

## 观察者模式的优缺点
* 优点  
	1. 观察者和被观察者之间解耦了。双方只依赖于接口，而不是依赖具体实现。
	
* 缺点  
	1.  依赖关系并未完全解除，抽象通知者依旧依赖抽象的观察者。

## 应用场景
　　当一个对象的改变需要给变其它对象时，而且它不知道具体有多少个对象有待改变时。

　　一个抽象某型有两个方面，当其中一个方面依赖于另一个方面，这时用观察者模式可以将这两者封装在独立的对象中使它们各自独立地改变和复用。

## 观察者两种模型--推（PUSH）模型和拉（PULL）模型

　　●　　推模型

　　　　 主题对象向观察者推送主题的详细信息，不管观察者是否需要，推送的信息通常是主题对象的全部或部分数据。

　　●　　拉模型

　　　　 主题对象在通知观察者的时候，只传递少量信息。如果观察者需要更具体的信息，**由观察者主动到主题对象中获取**，相当于是观察者从主题对象中拉数据。一般这种模型的实现中，会把主题对象自身通过update()方法传递给观察者，这样在观察者需要获取数据的时候，就可以通过这个引用来获取了。

### 两种模式的比较

　　■　　推模型是假定主题对象知道观察者需要的数据；而拉模型是主题对象不知道观察者具体需要什么数据，没有办法的情况下，干脆把自身传递给观察者，让观察者自己去按需要取值。

　　■　　推模型可能会使得观察者对象难以复用，因为观察者的update()方法是按需要定义的参数，**可能无法兼顾没有考虑到的使用情况**。这就意味着出现新情况的时候，就可能提供新的update()方法，或者是干脆重新实现观察者；而拉模型就不会造成这样的情况，因为拉模型下，update()方法的参数是主题对象本身，这基本上是主题对象能传递的最大数据集合了，基本上可以适应各种情况的需要。

 JAVA标准库当中的Observable类(Subject)和Observer接口结合了推模型和拉模型两种模式。

	public interface Observer {
		//即传递了被观察者自身（PULL，可能获取数据最大情况），也传递数据对象（PUSH）。
	    void update(Observable o, Object arg);
	}


	public class Observable {
	    private boolean changed = false;

		...

	    /**
	     * 如果本对象有变化（那时hasChanged 方法会返回true）
	     * 调用本方法通知所有登记的观察者，即调用它们的update()方法
	     * 传入this和arg作为参数
	     */
	    public void notifyObservers(Object arg) {
	
	        Object[] arrLocal;
	
	    synchronized (this) {
	
	        if (!changed)
	                return;
	            arrLocal = obs.toArray();
	            clearChanged();
	        }
	
	        for (int i = arrLocal.length-1; i>=0; i--)
	            ((Observer)arrLocal[i]).update(this, arg);
	    }
	
	    /**
	     * 将“已变化”设置为true,更新之前必须调用这个方法
	     */
	    protected synchronized void setChanged() {
	    changed = true;
	    }
	
	    /**
	     * 将“已变化”重置为false
	     */
	    protected synchronized void clearChanged() {
	    changed = false;
	    }
	
	    /**
	     * 检测本对象是否已变化
	     */
	    public synchronized boolean hasChanged() {
	    return changed;
	    }
	}

### 观察者模式简单实现

	public class Watched extends Observable{
	    
	    private String data = "";
	    
	    public String getData() {
	        return data;
	    }
	
	    public void setData(String data) {
	        
	        if(!this.data.equals(data)){
	            this.data = data;
	            setChanged();
	        }
	        notifyObservers();//这里用的是PULL模式，相当于notifyObservers(null)
	    }
	}

	public class Watcher implements Observer{
	    
	    public Watcher(Observable o){
	        o.addObserver(this);
	    }
	    
	    @Override
	    public void update(Observable o, Object arg) {
	        
	        System.out.println("状态发生改变：" + ((Watched)o).getData());
	    }
	}

	public class Test {
	
	    public static void main(String[] args) {
	        
	        //创建被观察者对象
	        Watched watched = new Watched();
	        //创建观察者对象，并将被观察者对象登记
	        Observer watcher = new Watcher(watched);
	        //给被观察者状态赋值
	        watched.setData("start");
	        watched.setData("run");
	        watched.setData("stop");
	
	    }
	}

## Thanks
1. [《JAVA与模式》之观察者模式 ](http://www.cnblogs.com/java-my-life/archive/2012/05/16/2502279.html)
2. [设计模式学习笔记-观察者模式 ](http://www.cnblogs.com/wangjq/archive/2012/07/12/2587966.html)