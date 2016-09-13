# UML类图学习笔记
## 一、类的UML图示：
如定义一个Employee类，它包含属性name、age和email，以及操作modifyInfo()，在UML类图中该类如图所示：  
![employee class](http://oddqfb3p7.bkt.clouddn.com/uml_1.jpg)

类图2包含一个内部类

![](http://oddqfb3p7.bkt.clouddn.com/uml_inner.png)

上面这个类图1由三部分（在Java语言中允许出现内部类，也有可能有四部分，如类图2）组成，每个部分用实线分割开来。

	第一部分是类名。
	第二部分是类的成员变量。表示方法：可见性 名称:类型 [ = 缺省值 ]
	其中：
	- “可见性”表示该属性对于类外的元素而言是否可见，包括公有(public)、私有(private)和受保护(protected)三种，在类图中分别用符号+、-和#表示。
	- “名称”表示属性名，用一个字符串表示。
	- “类型”表示属性的数据类型，可以是基本数据类型，也可以是用户自定义类型。
	- “缺省值”是一个可选项，即属性的初始值。
	第三部分是成员方法。表示方式为：可见性 名称(参数列表) [ : 返回类型]
	其中：
	- “可见性”的定义与属性的可见性定义相同。
	- “名称”即方法名，用一个字符串表示。
	- “参数列表”表示方法的参数，其语法与属性的定义相似，参数个数是任意的，多个参数之间用逗号“，”隔开。
	- “返回类型”是一个可选项，表示方法的返回值类型，依赖于具体的编程语言，可以是基本数据类型，也可以是用户自定义类型，还可以是空类型(void)，如果是构造方法，则无返回类型。


## 二、类与类之间的关系（1）
1. 关联关系  
关联(Association)关系是类与类之间最常用的一种关系，它是一种结构化关系，用于表示一类对象与另一类对象之间有联系，如汽车和轮胎、师傅和徒弟、班级和学生等等。在UML类图中，用实线连接有关联关系的对象所对应的类，在使用Java、C#和C++等编程语言实现关联关系时，通常将一个类的对象作为另一个类的成员变量。
![](http://oddqfb3p7.bkt.clouddn.com/uml_1dependency.png)

	* 双向关联
	
	默认情况下，关联是双向的。例如：顾客(Customer)购买商品(Product)并拥有商品，反之，卖出的商品总有某个顾客与之相关联。因此，Customer类和Product类之间具有双向关联关系，如图2所示：
	![](http://oddqfb3p7.bkt.clouddn.com/uml_1dependency2.png)

	* 单向关联
	
	类的关联关系也可以是单向的，单向关联用带箭头的实线表示。例如：顾客(Customer)拥有地址(Address)，则Customer类与Address类具有单向关联关系，如图3所示：

		public class Customer {
			private Address address;
			……
		}
		
		public class Address {
			……
		}

	* 自关联

     在系统中可能会存在一些类的属性对象类型为该类本身，这种特殊的关联关系称为自关联。例如：一个节点类(Node)的成员又是节点Node类型的对象，如图4所示：

		public class Node {
			private Node subNode;
			……
		}


	* 多重性关联

 	多重性关联关系又称为重数性(Multiplicity)关联关系，表示两个关联对象在数量上的对应关系。在UML中，对象之间的多     重性可以直接在关联直线上用一个数字或一个数字范围表示。对象之间可以存在多种多重性关联关系，常见的多重性表示方式如所示：

	多重性说明:

	1..1
	表示另一个类的一个对象只与该类的一个对象有关系

	0..*
	表示另一个类的一个对象与该类的零个或多个对象有关系

	1..*
	表示另一个类的一个对象与该类的一个或多个对象有关系

	0..1
	表示另一个类的一个对象没有或只与该类的一个对象有关系

	m..n
	表示另一个类的一个对象与该类最少m，最多n个对象有关系 (m≤n)
	
	例如：一个界面(Form)可以拥有零个或多个按钮(Button)，但是一个按钮只能属于一个界面，因此，一个Form类的对象可以与零个或多个Button类的对象相关联，但一个Button类的对象只能与一个Form类的对象关联，如图5所示：


2. 聚合关系
	
	聚合(Aggregation)关系表示整体与部分的关系，表示has-a的关系。在聚合关系中，成员对象是整体对象的一部分，但是成员对象可以脱离整体对象独立存在。在UML中，聚合关系用带空心菱形的直线表示。例如：汽车发动机(Engine)是汽车(Car)的组成部分，但是汽车发动机可以独立存在，因此，汽车和发动机是聚合关系，在代码实现聚合关系时，成员对象通常作为构造方法、Setter方法或业务方法的参数注入到整体对象中。
![](http://oddqfb3p7.bkt.clouddn.com/uml_contains.png)

3. 组合关系
	
	组合(Composition)关系也表示类之间整体和部分的关系，表示contains-a的关系。但是在组合关系中整体对象可以控制成员对象的生命周期，一旦整体对象不存在，成员对象也将不存在，成员对象与整体对象之间具有同生共死的关系。在UML中，组合关系用带实心菱形的直线表示。例如：人的头(Head)与嘴巴(Mouth)，嘴巴是头的组成部分之一，而且如果头没了，嘴巴也就没了，因此头和嘴巴是组合关系 。在代码实现组合关系时，通常在整体类的构造方法中直接实例化成员类
![](http://oddqfb3p7.bkt.clouddn.com/uml_composition.png)

		public class Head {
		  private Mouth mouth;
		
		  public Head() {
		  mouth = new Mouth(); //实例化成员类
		  }
		  ……
		}
		
		public class Mouth {
		  ……
		}


	PS.**聚合和组合的区别**  
　　这两个比较难理解，重点说一下。聚合和组合的区别在于：聚合关系是“has-a”关系，组合关系是“contains-a”关系；聚合关系表示整体与部分的关系比较弱，而组合比较强；聚合关系中代表部分事物的对象与代表聚合事物的对象的生存期无关，一旦删除了聚合对象不一定就删除了代表部分事物的对象。组合中一旦删除了组合对象，同时也就删除了代表部分事物的对象。从代码上说，聚合一般是通过注入的方式将部分的对象注入到整体对象中。而组合是通过在整体内部初始化部分的对象，这样部分对象的生命是跟整体息息相关的。


4. 依赖关系

     依赖(Dependency)关系是一种使用关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需要表示一个事物使用另一个事物时使用依赖关系。大多数情况下，依赖关系体现在某个类的方法使用另一个类的对象作为参数。在UML中，依赖关系用带箭头的虚线表示，由依赖的一方指向被依赖的一方。例如：驾驶员开车，在Driver类的drive()方法中将Car类型的对象car作为一个参数传递，以便在drive()方法中能够调用car的move()方法，且驾驶员的drive()方法依赖车的move()方法，因此类Driver依赖类Car。
![](http://oddqfb3p7.bkt.clouddn.com/uml_dependency_relation.png)

    依赖关系通常通过三种方式来实现，第一种也是最常用的一种方式是如图1所示的将一个类的对象作为另一个类中方法的参数，第二种方式是在一个类的方法中将另一个类的对象作为其局部变量，第三种方式是在一个类的方法中调用另一个类的静态方法。图1对应的Java代码片段如下：

		public class Driver {
		  public void drive(Car car) {
		  car.move();
		  }
		  ……
		}
		
		public class Car {
		  public void move() {
		  ......
		  }
		  ……
		}

5. 泛化关系  
 泛化(Generalization)关系也就是继承关系，用于描述父类与子类之间的关系，父类又称作基类或超类，子类又称作派生类。在UML中，泛化关系用带空心三角形的直线来表示。在代码实现时，我们使用面向对象的继承机制来实现泛化关系，如在Java语言中使用extends关键字、在C++/C#中使用冒号“：”来实现。例如：Student类和Teacher类都是Person类的子类，Student类和Teacher类继承了Person类的属性和方法，Person类的属性包含姓名(name)和年龄(age)，每一个Student和Teacher也都具有这两个属性，另外Student类增加了属性学号(studentNo)，Teacher类增加了属性教师编号(teacherNo)，Person类的方法包括行走move()和说话say()，Student类和Teacher类继承了这两个方法，而且Student类还新增方法study()，Teacher类还新增方法teach()。如图2所示：
![](http://oddqfb3p7.bkt.clouddn.com/uml_generalization.png)

		//父类
		public class Person {
		protected String name;
		protected int age;
		
		public void move() {
		  ……
		}
		
		  public void say() {
		  ……
		  }
		}
		
		//子类
		public class Student extends Person {
		private String studentNo;
		
		public void study() {
		  ……
		  }
		}
		
		//子类
		public class Teacher extends Person {
		private String teacherNo;
		
		public void teach() {
		  ……
		  }
		}


6. 接口与实现关系  
 在很多面向对象语言中都引入了接口的概念，如Java、C#等，在接口中，通常没有属性，属性默认都是常量。而且所有的操作都是抽象的，只有操作的声明，没有操作的实现。UML中用与类的表示法类似的方式表示接口，如图3所示：  
![](http://oddqfb3p7.bkt.clouddn.com/uml_interface.png)  
接口之间也可以有与类之间关系类似的继承关系和依赖关系，但是接口和类之间还存在一种实现(Realization)关系，在这种关系中，类实现了接口，类中的操作实现了接口中所声明的操作。在UML中，类与接口之间的实现关系用带空心三角形的虚线来表示。例如：定义了一个交通工具接口Vehicle，包含一个抽象操作move()，在类Ship和类Car中都实现了该move()操作，不过具体的实现细节将会不一样，如图4所示：
![](http://oddqfb3p7.bkt.clouddn.com/uml_impl.png)

	实现关系在编程实现时，不同的面向对象语言也提供了不同的语法，如在Java语言中使用implements关键字，而在C++/C#中使用冒号“：”来实现。图4对应的Java代码片段如下：

		public interface Vehicle {
		     public void move();
		}
		
		public class Ship implements Vehicle {
		     public void move() {
		         ……
		         }
		}
		
		public class Car implements Vehicle {
		     public void move() {
		         ……
		         }
		}

![](http://oddqfb3p7.bkt.clouddn.com/uml_summary.png)