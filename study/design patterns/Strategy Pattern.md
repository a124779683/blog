# 策略模式（Strategy Pattern）
> 定义一组算法，对每一种进行封装，使他们可以相互交换。此模式可以使算法独立于使用它的客户程序而变化。--GOF
>
> 略模式属于对象的行为模式。其用意是针对一组算法，**将每一个算法封装到具有共同接口的独立的类中**，从而使得它们可以相互替换。策略模式使得算法可以在不影响到客户端的情况下发生变化。--阎宏博士的《JAVA与模式》

### 策略模式所解决的问题
策略模式易于扩展，遵循了开闭原则，我们增加策略的同时，不会影响到其他原有策略。策略模式的重心不是如何实现算法，而是如何组织、调用这些算法，从而让程序结构更灵活，具有更好的维护性和扩展性。

## 策略模式中的角色及类图
1. 上下文环境角色(Context)，持有一个或一系列实现或者继承抽象策略角色的具体角色，
1. 抽象策略角色(Strategy)，定义了所有算法共同的接口。 Context使用这个接口来调用具体策略（ConcreteStrategy）定义的算法。
2. 具体策略角色(Concrete Strategy)，包装了相关的算法或行为，没一种算法都是独立封装的。

![策略模式类图](http://oddqfb3p7.bkt.clouddn.com/Strategy.png) 

## 策略模式的优缺点
* 优点  
	1. 略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族。恰当使用继承可以把公共的代码移到父类里面，从而避免代码重复。
	3. 使用策略模式可以避免使用多重条件(if-else)语句。多重条件语句不易维护，它把采取哪一种算法或采取哪一种行为的逻辑与算法或行为的逻辑混合在一起，统统列在一个多重条件语句里面，比使用继承的办法还要原始和落后。

* 缺点  
	1. **客户端需要理解所有具体策略类之间的区别**，以便选择合适的算法，这也是策略模式的缺点之一，在一定程度上增加了客户端的使用难度。
	2. 虽然我们把每个算法都独立封装了，但是这有可能会导致类的泛滥，就像模板方法一样。由于策略模式把每个具体的策略实现都单独封装成为类，如果备选的策略很多的话，那么对象的数目就会很可观。

## 应用场景
1. 好多不同情况需要使用不同策略来进行处理和计算的情景。  
假设现在要设计一个贩卖各类书籍的电子商务网站的购物车系统。一个最简单的情况就是把所有货品的单价乘上数量，但是实际情况肯定比这要复杂。比如，本网站可能对所有的高级会员提供每本20%的促销折扣；对中级会员提供每本10%的促销折扣；对初级会员没有折扣。

	根据描述，折扣是根据以下的几个算法中的一个进行的：

	算法一：对初级会员没有折扣。

	算法二：对中级会员提供10%的促销折扣。

	算法三：对高级会员提供20%的促销折扣。

## 策略模式简单实现

	public interface MemberStrategy {
		/**
		 * 计算图书的价格
		 * @param booksPrice    图书的原价
		 * @return    计算出打折后的价格
		 */
		public double calcPrice(double booksPrice);
	}

	//初级会员折扣算法
	public class PrimaryMemberStrategy implements MemberStrategy {
		
		@Override
		public double calcPrice(double booksPrice) {
		
		System.out.println("对于初级会员的没有折扣");
		return booksPrice;
		}
		
	}

	 //中级会员折扣算法
	public class IntermediateMemberStrategy implements MemberStrategy {
	
		@Override
		public double calcPrice(double booksPrice) {
		
		System.out.println("对于中级会员的折扣为10%");
		return booksPrice * 0.9;
		}
		
	}
	
	//高级会员折扣算法
	public class AdvancedMemberStrategy implements MemberStrategy {
	
	    @Override
	    public double calcPrice(double booksPrice) {
	        
	        System.out.println("对于高级会员的折扣为20%");
	        return booksPrice * 0.8;
	    }
	}

	//Context上下文类
	public class Price {

	    //持有一个具体的策略对象
	    private MemberStrategy strategy;

	    /**
	     * 构造函数，传入一个具体的策略对象
	     * @param strategy    具体的策略对象
	     */
	    public Price(MemberStrategy strategy){
	        this.strategy = strategy;
	    }
	    
	    /**
	     * 计算图书的价格
	     * @param booksPrice    图书的原价
	     * @return    计算出打折后的价格
	     */
	    public double quote(double booksPrice){
	        return this.strategy.calcPrice(booksPrice);
	    }
	}

	//客户端调用
	public class Client {
	
	    public static void main(String[] args) {
	        //选择并创建需要使用的策略对象
	        MemberStrategy strategy = new AdvancedMemberStrategy();
	        //创建环境
	        Price price = new Price(strategy);
	        //计算价格
	        double quote = price.quote(300);
	        System.out.println("图书的最终价格为：" + quote);
	    }
	}

## Thanks
1. [《JAVA与模式》之策略模式 ](http://www.cnblogs.com/java-my-life/archive/2012/05/10/2491891.html)