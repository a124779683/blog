# 状态模式（State Pattern）
>状态模式，又称状态对象模式（Pattern of Objects for States），状态模式是对象的行为模式。

>状态模式允许一个对象在其内部状态改变的时候改变其行为。这个对象看上去就像是改变了它的类一样。
--阎宏博士的《JAVA与模式》

## 状态模式所解决的问题
1. 在软件开发过程中，应用程序可能会根据不同的情况作出不同的处理。最直接的解决方案是将这些所有可能发生的情况全都考虑到。然后**使用if... else语句来做状态判断来进行不同情况的处理。但是对复杂状态的判断就显得“力不从心了”**。随着增加新的状态或者修改一个状体（if else(或switch case)语句的增多或者修改）可能会引起很大的修改，而程序的可读性，扩展性也会变得很弱。维护也会很麻烦。那么我就考虑只修改自身状态的模式。

2. 状态模式主要解决的是当**控制一个对象状态转换的条件表达式过于复杂时的情况**。把状态的判断逻辑转移到表示不同状态的一系列类当中，可以把复杂的判断逻辑简化。

## 状态模式中的角色及类图
1. 环境(Context)角色，上下文：定义客户端所感兴趣的接口，并且保留一个具体状态类的实例。这个具体状态类的实例给出此环境对象的现有状态。
1. 抽象状态(State)角色：定义一个接口，用以封装环境（Context）对象的一个特定的状态所对应的行为。
2. 具体状态(ConcreteState)角色：每一个具体状态类都实现了环境（Context）的一个状态所对应的行为。

![状态模式类图](http://oddqfb3p7.bkt.clouddn.com/State%20Pattern.png) 

## 状态模式的优缺点
* 优点  
	1. 封装了转换规则。 
	2. 枚举可能的状态，在枚举状态之前需要确定状态种类。 
	3. 将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。 
	4. 允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块。 
	5. 可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。

* 缺点  
	1. 状态模式的使用必然会增加系统类和对象的个数。 
	2. 状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。 
	3. 状态模式对“开闭原则”的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态；而且修改某个状态类的行为也需修改对应类的源代码。

## 应用场景
　　考虑一个在线投票系统的应用，要实现控制同一个用户只能投一票，如果一个用户反复投票，而且投票次数超过5次，则判定为恶意刷票，要取消该用户投票的资格，当然同时也要取消他所投的票；如果一个用户的投票次数超过8次，将进入黑名单，禁止再登录和使用系统。

　　要使用状态模式实现，首先需要把投票过程的各种状态定义出来，根据以上描述大致分为四种状态：正常投票、反复投票、恶意刷票、进入黑名单。然后创建一个投票管理对象（相当于Context）。

	

### 上面场景状态模式的实现

	//抽象状态类
	public interface VoteState {
	    /**
	     * 处理状态对应的行为
	     * @param user    投票人
	     * @param voteItem    投票项
	     * @param voteManager    投票上下文，用来在实现状态对应的功能处理的时候，
	     *                         可以回调上下文的数据
	     */
	    public void vote(String user,String voteItem,VoteManager voteManager);
	}
	
	//具体状态类——正常投票
	public class NormalVoteState implements VoteState {
	
	    @Override
	    public void vote(String user, String voteItem, VoteManager voteManager) {
	        //正常投票，记录到投票记录中
	        voteManager.getMapVote().put(user, voteItem);
	        System.out.println("恭喜投票成功");
	    }
	
	}
	
	//具体状态类——重复投票
	public class RepeatVoteState implements VoteState {
	
	    @Override
	    public void vote(String user, String voteItem, VoteManager voteManager) {
	        //重复投票，暂时不做处理
	        System.out.println("请不要重复投票");
	    }
	
	}
	
	//具体状态类——恶意刷票
	public class SpiteVoteState implements VoteState {
	
	    @Override
	    public void vote(String user, String voteItem, VoteManager voteManager) {
	        // 恶意投票，取消用户的投票资格，并取消投票记录
	        String str = voteManager.getMapVote().get(user);
	        if(str != null){
	            voteManager.getMapVote().remove(user);
	        }
	        System.out.println("你有恶意刷屏行为，取消投票资格");
	    }
	
	}

	//具体状态类——黑名单
	public class BlackVoteState implements VoteState {
	
	    @Override
	    public void vote(String user, String voteItem, VoteManager voteManager) {
	        //记录黑名单中，禁止登录系统
	        System.out.println("进入黑名单，将禁止登录和使用本系统");
	    }
	
	}

	//环境类 Context
	public class VoteManager {
	    //持有状体处理对象
	    private VoteState state = null;
	    //记录用户投票的结果，Map<String,String>对应Map<用户名称，投票的选项>
	    private Map<String,String> mapVote = new HashMap<String,String>();
	    //记录用户投票次数，Map<String,Integer>对应Map<用户名称，投票的次数>
	    private Map<String,Integer> mapVoteCount = new HashMap<String,Integer>();
	    /**
	     * 获取用户投票结果的Map
	     */
	    public Map<String, String> getMapVote() {
	        return mapVote;
	    }
	    /**
	     * 投票
	     * @param user    投票人
	     * @param voteItem    投票的选项
	     */
	    public void vote(String user,String voteItem){
	        //1.为该用户增加投票次数
	        //从记录中取出该用户已有的投票次数
	        Integer oldVoteCount = mapVoteCount.get(user);
	        if(oldVoteCount == null){
	            oldVoteCount = 0;
	        }
	        oldVoteCount += 1;
	        mapVoteCount.put(user, oldVoteCount);
	        //2.判断该用户的投票类型，就相当于判断对应的状态
	        //到底是正常投票、重复投票、恶意投票还是上黑名单的状态
	        if(oldVoteCount == 1){
	            state = new NormalVoteState();
	        }
	        else if(oldVoteCount > 1 && oldVoteCount < 5){
	            state = new RepeatVoteState();
	        }
	        else if(oldVoteCount >= 5 && oldVoteCount <8){
	            state = new SpiteVoteState();
	        }
	        else if(oldVoteCount > 8){
	            state = new BlackVoteState();
	        }
	        //然后转调状态对象来进行相应的操作
	        state.vote(user, voteItem, this);
	    }
	}

	//客户端类
	public class Client {
	
		public static void main(String[] args) {
		    
		    VoteManager vm = new VoteManager();
		    for(int i=0;i<9;i++){
		        vm.vote("u1","A");
		    }
		}
	
	}

结果：
![](http://oddqfb3p7.bkt.clouddn.com/StateResult.png)

　　从上面可以看出，环境类Context的行为request()是委派给某一个具体状态类的。通过使用多态性原则，可以动态改变环境类Context的属性State的内容，使其从指向一个具体状态类变换到指向另一个具体状态类，从而使环境类的行为request()由不同的具体状态类来执行。状态的转换基本上都是内部行为，主要在状态模式内部来维护。比如对于投票的人员，任何时候他的操作都是投票，但是投票管理对象的处理却不一定一样，会根据投票的次数来判断状态，然后根据状态去选择不同的处理。

## 策略和状态模式的区别
　状态模式(state pattern)和策略模式(strategy pattern)的实现方法非常类似，都是利用多态把一些操作分配到一组相关的简单的类中，因此很多人认为这两种模式实际上是相同的。然而
　　在现实世界中，策略（如促销一种商品的策略）和状态（如同一个按钮来控制一个电梯的状态，又如手机界面中一个按钮来控制手机）是两种完全不同的思想。当我们对状态和策略进行建模时，这种差异会导致完全不同的问题。例如，对状态进行建模时，状态迁移是一个核心内容；然而，在选择策略时，迁移与此毫无关系。另外，策略模式允许一个客户选择或提供一种策略，而这种思想在状态模式中完全没有。
　　一个策略是一个计划或方案，通过执行这个计划或方案，我们可以在给定的输入条件下达到一个特定的目标。策略是一组方案，他们可以相互替换；选择一个策略，获得策略的输出。策略模式用于随不同外部环境采取不同行为的场合。我们可以参考微软企业库底层Object Builder的创建对象的strategy实现方式。
　　而状态模式不同，对一个状态特别重要的对象，通过状态机来建模一个对象的状态；状态模式处理的核心问题是状态的迁移，因为在对象存在很多状态情况下，对各个business flow，各个状态之间跳转和迁移过程都是及其复杂的。例如一个工作流，审批一个文件，存在新建、提交、已修改、HR部门审批中、老板审批中、HR审批失败、老板审批失败等状态，涉及多个角色交互，涉及很多事件，这种情况下用状态模式(状态机)来建模更加合适；把各个状态和相应的实现步骤封装成一组简单的继承自一个接口或抽象类的类，通过另外的一个Context来操作他们之间的自动状态变换，通过event来自动实现各个状态之间的跳转。在整个生命周期中存在一个状态的迁移曲线，这个迁移曲线对客户是透明的。我们可以参考微软最新的WWF 状态机工作流实现思想。
　　在状态模式中，状态的变迁是由对象的内部条件决定，外界只需关心其接口，不必关心其状态对象的创建和转化；而策略模式里，采取何种策略由外部条件(C)决定。

1. 都封装在具体的类中。策略模式把不同的算法封装到不同的子类；状态模式把不同状态下的行为封装到与之对应的状态子类中。
2. 都使用的合成，代理了上下文的相关操作
3. 一个状态对象封装了与状态相关的行为，并控制状态之间的转换。策略对象封装了算法，算法之间不存在切换。
4. 状态模式隐藏了状态接口和子类。策略模式中，客户对象可以在运行时选择不同的算法，策略接口和具体实现对他是可见的，但是状态模式中，客户对象只能操作Context对象，状态接口和子类对他是不可见的。

## Thanks
1. [《JAVA与模式》之状态模式](http://www.cnblogs.com/java-my-life/archive/2012/06/08/2538146.html)
2. [状态模式和策略模式的区别与联系](http://zhidao.baidu.com/link?url=GfxJ43fxVn5jKLMyGuUL0Q96YLLjUQKrIeiRGvLIZvyPv676Cm7HPVB_BsNe7IPrbkQXRqXxMos4_g_AtJsyj4up36K4aevil9vTkyc7tpi)
3. [设计模式 ( 十七) 状态模式State（对象行为型）](http://blog.csdn.net/hguisu/article/details/7557252)