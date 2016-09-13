# 组合模式（Composite Pattern）
>将对象组合成树形结构以表示‘部分-整体’的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
>
>掌握组合模式的重点是要理解清楚 “部分/整体” 还有 ”单个对象“ 与 "组合对象" 的含义。

### 组合模式所解决的问题
组合模式解耦了客户程序与复杂元素内部结构，从而使客户程序可以像处理简单元素一样来处理复杂元素。（外观模式也可以使客户端与实际工作类之间达到一定解耦的目的）

## 组合模式中的角色及类图
1. 抽象构件(Component)，实现类共有接口，用于访问和管理Component子部件。
1. 叶子构件(Leaf)，叶子结点没有子结点。
2. 树枝构件(Composite)，用来存储叶子部件，在Component接口中实现与子部件有关操作，如增加(add)和删除(remove)等。

![组合模式类图](http://oddqfb3p7.bkt.clouddn.com/composite.jpg) 

## 外观模式的优缺点
* 优点  
	1. 使用了组合模式后，我们可以看看，如果想增加一个树枝节点、树叶节点是不是都很容易呀，只要找到它的父节点就成，非常容易扩展，符合开闭原则，对以后的维护非常有利。
	3. 高层模块调用简单。一棵树形机构中的所有节点都是Component，局部和整体对调用者来说没有任何区别，也就是说，高层模块不必关心自己处理的是单个对象还是整个组合结构，简化了高层模块的代码。

* 缺点  
	1. 使用组合模式后，控制树枝构件的类型不太容易。
	2. 用**继承的方法来增加新的行为很困难**。直接使用了实现类！这在面向接口编程上是很不恰当的，与依赖倒置原则冲突，读者在使用的时候要考虑清楚，它限制了你接口的影响范围。

## 应用场景
1. 你想表示对象的部分-整体层次结构。
2. 你希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。


## 组合模式简单实现
    public abstract class Component
    {
        protected string name;
        public string Name
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }
        //添加部件
        public abstract void Add(Component component);
        //删除部件
        public abstract void Remove(Component component);
        //遍历所有子部件
        public abstract void eachChild();
    }

    //叶子类，这里是 透明的组合模式
    public class Leaf : Component
    {
        //叶子节点不具备添加的能力，所以不实现
        public override void Add(Component component)
        {
            throw new NotImplementedException();
        }

        //叶子节点不具备添加的能力必然也不能删除
        public override void Remove(Component component)
        {
            throw new NotImplementedException();
        }

        //叶子节点没有子节点所以显示自己的执行结果
        public override void eachChild()
        {
            Console.WriteLine("{0}执行了..",name);
        }
    }

    //组合类
    public class Composite : Component
    {
        //用来保存组合的部件
        List<Component> myList = new List<Component>();

        //添加节点 添加部件
        public override void Add(Component component)
        {
            myList.Add(component);
        }

        //删除节点 删除部件
        public override void Remove(Component component)
        {
            myList.Remove(component);
        }

        //遍历子节点
        public override void eachChild()
        {
            Console.WriteLine("{0}执行了..", name);
            foreach (Component c in myList)
            {
                c.eachChild();
            }
        }
    }
    static void Main(string[] args)
    {
            //构造根节点
            Composite rootComponent = new Composite();
            rootComponent.Name = "根节点";

            //添加两个叶子几点，也就是子部件
            Leaf l = new Leaf();
            l.Name = "叶子节点一";
            Leaf l1 = new Leaf();
            l1.Name = "叶子节点二";

            rootComponent.Add(l);
            rootComponent.Add(l1);

            //遍历组合部件
            rootComponent.eachChild();
     }

## 透明的组合模式与安全的组合模式
上面代码示例的是透明的组合模式，他把对叶子部件的操作都定义在了抽象类当中，然后在实际叶子部件中不做任何处理，或者抛出一个异常。这种方式对于客户端来说是完全透明的。

而安全的组合模式中，叶子节点是没有add,remove等对子节点操作的方法的。所有对子节点的操作都从抽象类中移到组合构件（Composite）中去提供。

PS：透明的组合模式是牺牲了安全性的，因为只有在运行时才能暴露出错误的使用方式。这个地方得我们自己权衡使用~

## Thanks
1. [组合模式](http://baike.baidu.com/link?url=ULs3swy8KYUGefLxv5lV0NOSmKrWtCrIYXgMUF4hjrbvAK4qINyPsaDtgF3NJzXPgXADF2Tg8o0GnbfvtxhEJq)