#  设计模式学习Summary
设计模式的一个简要回顾，对每一种设计模式的学习在本文末尾进行收录
## 
1. 设计模式的概念和目的  
设计模式是为了解决特定环境下的一些问题而产生的**常用**的解决方案。  
使用设计模式的目的我认为是为了使我们的程序更利于维护和扩展，易于阅读，代码健壮性高，另外代码重用使得我们开发成本得以降低。

2. 设计模式的来历  
GoF(Gang of Four)四人帮编写的书籍《设计模式》的问世影响非常深远，里面收编了**23个常用的设计模式**。  

3. 设计模式的四个基本要素  
 + 模式名称
 + 问题
 + 解决方案
 * 效果

4. 设计模式的分类  
 * 创建型：对象的创建会消耗掉系统的很多资源，所以单独对对象的创建进行研究，从而能够高效地创建对象就是创建型模式要探讨的问题。这里有5个具体的创建型模式可供研究。
 * 结构型：解决了对象的创建问题之后，对象的组成以及对象之间的依赖关系就成了开发人员关注的焦点，因为如何设计对象的结构、继承和依赖关系会影响到后续程序的维护性、代码的健壮性、耦合性等。对象结构的设计很容易体现出设计人员水平的高低
 * 行为型：对象的结构和对象的创建问题都解决了之后，就剩下对象的行为问题了，如果对象的行为设计的好，那么对象的行为就会更清晰，它们之间的协作效率就会提高，有11个具体的行为型模式可供研究。


## 索引



1. 创建型模式：
  1. [单例设计模式--学习笔记][Singleton]（Singleton）
  2. [工厂方法模式--学习笔记][Factory Method]（Factory Method）
  3. [抽象工厂模式--学习笔记][Factory Method]（Abstract Factory）
  4. 建造者模式（Builder）
  5. 原型模式（Prototype）


2. 结构型模式
 
 1. [装饰者模式--学习笔记][Decorator Method]（Decorator）
 2. [代理模式--学习笔记][Proxy Method]（Proxy）
 3. [适配器模式][Adapter Method]（Adapter）
 4. 桥接模式（Bridge）
 5. [外观模式][Facade]（Facade）
 6. 享元模式（Flyweight）
 7. [组合模式][Composite]（Composite）

3. 行为型模式（11种行为模式里类与类之间的关系：
第一类：通过父类与子类的关系进行实现。
第二类：两个类之间。
第三类：类的状态。
第四类：通过中间类）

 1. 观察者模式（Observer）第二类
 2. [模板设计模式--学习笔记][Template Method]（Template Method）第一类
 3. 命令模式（Command）第二类
 4. [状态模式][State]（State）第三类
 5. 职责链模式（Chain of Responsibility）第二类
 6. 解释器模式（Interpreter）第四类
 8. 访问者模式（Visitor）第四类
 9. [策略模式][Strategy]（Strategy）第一类
 10. 备忘录模式（Memento）第三类
 11. 迭代器模式（Iterator）第二类
 12. 中介者/调停者 模式（Mediator）第四类

其他 

1. [功能增强的四种方式][Enhanced method]

[Template Method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/template%20method.md (模板设计模式)
[Singleton]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/singleton.md 
[Factory Method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/factory%20method.md 
[Decorator Method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Decorator%20Method.md 
[Proxy Method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Proxy%20Method.md (代理模式)
[Enhanced method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/%E5%8A%9F%E8%83%BD%E5%A2%9E%E5%BC%BA%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%96%B9%E5%BC%8F.md (功能增强的四种方式)

[Adapter Method]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Adapter%20Method.md (适配器模式)
[Facade]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Facade.md (外观模式)
[Composite]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Composite.md (组合模式)
[Strategy]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/Strategy%20Pattern.md (策略模式)
[State]:https://github.com/a124779683/blog/blob/master/study/design%20patterns/State%20Pattern.md (状态模式)


