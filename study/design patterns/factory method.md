# 工厂模式
>为创建对象定义一个**接口**，让子类决定实例化哪个类。工厂方法让一个类的实例化延迟到子类。客户端不需了解是如何实例化对象的。

>工厂模式一共有三种实现方式，简单工厂，工厂方法和抽象工厂。简单工厂缺点太过明显，我们这里不做太多了解。

## 工厂模式的优势及应用场景
优势：  
使用工厂模式，可以避免客户端依赖于他所使用接口的具体实现（面向接口），并且在构造复杂的对象的时候我们就要考虑使用工厂模式了，这样也能较少客户端的使用难度。  

应用场景：

* **客户端不知道它要创建的具体是哪一个子类**（多种形态）
* 一个类想要由自己的子类来定义某对象的创建过程
* 类将创建某对象的职责代理给一些帮助子类中的一个，并且你想要将哪一个子类作为代理的信息进行局部化。

## 简单工厂模式（Simple Factory）
简单工厂模式又称静态工厂方法模式（static factory method）。从命名上就可以看出这个模式一定很简单，仅使用一个工厂类负责对象的实例化。在简单工厂模式中,一个**工厂类处于对产品类实例化调用的中心位置上**,它决定那一个产品类应当被实例化。简单工厂由三个要素组成：

* 抽象产品角色
* 具体产品角色
* 工厂角色

**使用场景：能预测到所有产品类的情况，就用简单工厂，否则就工厂方法**  
优点：客户端不需要修改代码。  
缺点：当需要增加新的实现了接口的子类的时候，**不仅需新加子类，还要修改工厂类**，违反了开闭原则。

	// 1抽象产品角色：它一般是具体产品继承的父类或者实现的接口。在java中由接口或者抽象类来实现。
	public interface people{
	      public void say();
	}
	 
	// 2具体产品角色：工厂类所创建的对象就是此角色的实例。在java中由一个具体类实现。
	public class chinese implements people{
	      public void say(){
	           System.out.println("说中国话");
	     }
	}
	 
	// 2具体产品角色：工厂类所创建的对象就是此角色的实例。在java中由一个具体类实现。
	public class american implements people{
	      public void say(){
	           System.out.println("speak english");
	     }
	}
	 
	// 3工厂类，处于实例化调用的中心位置。这是本模式的核心，含有一定的商业逻辑和判断逻辑。在java中它往往由一个具体类实现。
	public class peopleFactory{
	      public static people create(int type){
	           if(type==1){
	                  return new chinese();
	           }else if(type==2){
	                 return new american();
	           }
	     }
	}


除上面实现方式，我们可以直接在每个类里实现一个**静态的工厂方法**，就不需要额外的去增加一个工厂类了。最为典型的就是Integer，String等的valuesOf方法。

    public static Integer valueOf(int i){
		if(i >= -128 && i<= IntegerCache.high){
			return IntegerCache.cache[i+128];
		}else{
			return new Integer(i);
		}
	}


## 工厂方法模式（Factory Method）
工厂方法模式是**简单工厂模式的进一步抽象化和推广**，工厂方法模式里**不再只由一个工厂类决定那一个产品类应当被实例化**,这个决定被交给抽象工厂的子类去做。在上面的简单工厂模式中，工厂是一个具体的类，而在工厂方法中，我们需要把工厂像他所实例化的产品一样也抽象出来。

工厂方法由四个要素组成：

* 抽象工厂角色（相对于简单工厂增加的）： 这是工厂方法模式的核心，它与应用程序无关。是具体工厂角色必须实现的接口或者必须继承的父类。在java中它由抽象类或者接口来实现。
* 具体工厂角色：它含有和具体业务逻辑有关的代码。由应用程序调用以创建对应的具体产品的对象。
* 抽象产品角色：它是具体产品继承的父类或者是实现的接口。在java中一般有抽象类或者接口来实现。
* 具体产品角色：具体工厂角色所创建的对象就是此角色的实例。在java中由具体的类来实现。

工厂方法模式使用继承自抽象工厂角色的多个子类来代替简单工厂模式中的“上帝类”。正如上面所说，这样便分担了对象承受的压力；而且这样使得结构变得灵活 起来——当有新的产品（即暴发户的汽车）产生时，只要按照抽象产品角色、抽象工厂角色提供的合同来生成，那么就可以被客户使用，而不必去修改任何已有的代 码。可以看出工厂角色的结构也是符合开闭原则的！

### 缺点
可以看出工厂方法的加入，使得对象的数量成倍增长。**当产品种类非常多时，会出现大量的与之对应的工厂对象，这不是我们所希望的**。因为如果不能避免这种情 况，**可以考虑使用简单工厂模式与工厂方法模式相结合的方式来减少工厂类**：即对于产品树上类似的种类（一般是树的叶子中互为兄弟的）使用简单工厂模式来实现。
	
	//抽象产品角色
	public interface Moveable {
	    void run();
	}
	//具体产品角色
	public class Plane implements Moveable {
	    @Override
	    public void run() {
	        System.out.println("plane....");
	    }
	}
	
	public class Broom implements Moveable {
	    @Override
	    public void run() {
	        System.out.println("broom.....");
	    }
	}
	
	//抽象工厂
	public abstract class VehicleFactory {
	    abstract Moveable create();
	}
	//具体工厂
	public class PlaneFactory extends VehicleFactory{
	    public Moveable create() {
	        return new Plane();
	    }
	}
	public class BroomFactory extends VehicleFactory{
	    public Moveable create() {
	        return new Broom();
	    }
	}
	//测试类
	public class Test {
	    public static void main(String[] args) {
	        VehicleFactory factory = new BroomFactory();
	        Moveable m = factory.create();
	        m.run();
	    }
	}

## 抽象工厂
抽象工厂模式中，抽象产品 (AbstractProduct) 可能是一个(相当于退化到工厂方法模式)或多个，从而构成一个或多个产品族(Product Family)。 在**只有一个产品族的情况下，抽象工厂模式实际上退化到工厂方法模式**。

	//抽象工厂类，下面有三个抽象的产品，三个产品都是抽象的。
	public abstract class AbstractFactory {
	    public abstract Vehicle createVehicle();
	    public abstract Weapon createWeapon();
	    public abstract Food createFood();
	}
	//具体工厂类，其中Food,Vehicle，Weapon是抽象类，
	public class DefaultFactory extends AbstractFactory{
	    @Override
	    public Food createFood() {
	        return new Apple();
	    }
	    @Override
	    public Vehicle createVehicle() {
	        return new Car();
	    }
	    @Override
	    public Weapon createWeapon() {
	        return new AK47();
	    }
	}
	//测试类
	public class Test {
	    public static void main(String[] args) {
	        AbstractFactory f = new DefaultFactory();
	        Vehicle v = f.createVehicle();
	        v.run();
	        Weapon w = f.createWeapon();
	        w.shoot();
	        Food a = f.createFood();
	        a.printName();
	    }
	}