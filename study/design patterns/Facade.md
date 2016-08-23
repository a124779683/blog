# 外观模式（Facade）
>为子系统中的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

>外观模式用到的 设计原则 之一：迪米特原则，也就是最少知识原则。

### 外观模式所解决的问题
减少客户类和子系统类之间的耦合度，降低修改代码造成的影响。

## 外观模式中的角色及类图
1. 外观类(Facade)，外观类知道哪些子系统类负责处理请求，将客户的请求代理给恰当的子系统对象。
2. 子系统类(SubClasses)，子系统类集合实现了子系统的功能，处理外观类对象指派的任务。

![外观模式类图](http://a2.qpic.cn/psb?/V12r9fmQ4ALxpC/KeusI.7Tvyhi*XYwiWIAfRQTPYo7rN*yW1K92FXc28Y!/b/dKwAAAAAAAAA&bo=UgIJAgAAAAADB3k!&rf=viewer_4) 

## 外观模式的优缺点
* 优点  
	1. Facade模式封装了子系统复杂的交互和依赖关系，降低了客户端对子系统使用的复杂性
	2. 由于外观模式松散了客户端与子系统的耦合关系，让子系统内部的模块能更容易扩展和维护
	3. 通过合理使用Facade，可以帮助我们更好的划分访问的层次

* 缺点  
	1. 过多的或者是不太合理的Facade也容易让人迷惑，到底是调用Facade好呢，还是直接调用模块好
	2. 如果设计不当，增加新的子系统可能需要修改外观类的源代码，违背了开闭原则。

## 应用场景
1. 需要将设计进行分层时考虑Facade模式。
2. 在开发阶段，子系统往往因为重构变得越来越复杂，增加外观模式可以提供一个简单的接口，减少它们之间的依赖
3. 在维护一个遗留的大型系统时，可以这个系统已经非常难以维护和扩展，可以为新系统开发一个Facade类，来提供设计粗糙或高度复杂的遗留代码的比较清晰简单的接口，让新系统与Facade对象交互，Facade与遗留代码交互所有复杂的工作


## 外观模式简单实现

子系统

	class SubSystemA{

	    public void MethodA(){
	        //业务实现代码
	    }
	}
	
	class SubSystemB{

	    public void MethodB(){
	        //业务实现代码
	     }
	}
	
	class SubSystemC{
	    public void MethodC(){
	        //业务实现代码
	    }
	}

外观类

	class Facade{

	    private SubSystemA obj1 = new SubSystemA();
	    private SubSystemB obj2 = new SubSystemB();
	    private SubSystemC obj3 = new SubSystemC();
	
	    public void Method(){
	        obj1.MethodA();
	        obj2.MethodB();
	        obj3.MethodC();
	    }
	}

客户端调用

	class Program{

	    static void Main(string[] args){
	        Facade facade = new Facade();
	        facade.Method();
	    }
	}

## 应用实战
假设我们现在有一个需求，需求是我们需要一个文件加密系统能够对每一个文件里的内容进行加密并且将加密过后的内容保存起来。  
流程含：  

* 读取文件内容
* 加密内容
* 写入加密后的内容

分析后我们得出对于文件的操作，例如读取和写入可以用一个类来封装，加密功能用一个类封装。这样的话，这两个操作相对独立，遵循了单一职责的设计原则，也利于重用代码。

FileReader：文件读取类，充当子系统类。



    class FileReader
    {
        public string Read(string fileNameSrc) 
        {
	   		Console.Write("读取文件，获取明文：");
            FileStream fs = null;
            StringBuilder sb = new StringBuilder();
	  		 try
  	    	  {
                fs = new FileStream(fileNameSrc, FileMode.Open);
                int data;
    	       while((data = fs.ReadByte())!= -1) 
                {
    				sb = sb.Append((char)data);
    	       }
     	       fs.Close();
     	       Console.WriteLine(sb.ToString());
	   }
	   catch(FileNotFoundException e) 
            {
	       Console.WriteLine("文件不存在！");
	   }
	   catch(IOException e) 
            {
	       Console.WriteLine("文件操作错误！");
	   }
	   return sb.ToString();
        }
    }

CipherMachine：数据加密类，充当子系统类。

	class CipherMachine
	{
		public string Encrypt(string plainText) 
		{
			Console.Write("数据加密，将明文转换为密文：");
			string es = "";
			char[] chars = plainText.ToCharArray();
			foreach(char ch in chars) 
			{
			   string c = (ch % 7).ToString();
			   es += c;
			}
		    Console.WriteLine(es);
		return es;
		}
	}

FileWriter：文件保存类，充当子系统类。

	class FileWriter
	{
		public void Write(string encryptStr,string fileNameDes) 
		{
			Console.WriteLine("保存密文，写入文件。");
		    FileStream fs = null;
			try
		    {
		        fs = new FileStream(fileNameDes, FileMode.Create);
		        byte[] str = Encoding.Default.GetBytes(encryptStr);
		        fs.Write(str,0,str.Length);
		        fs.Flush();
		       	fs.Close();
			}	
			catch(FileNotFoundException e) 
			    {
			Console.WriteLine("文件不存在！");
			}
			catch(IOException e) 
			    {
			        Console.WriteLine(e.Message);
			   Console.WriteLine("文件操作错误！");
			}		
		}
	}

EncryptFacade：加密外观类，充当外观类。

    class EncryptFacade
    {
        //维持对其他对象的引用
         private FileReader reader;
        private CipherMachine cipher;
        private FileWriter writer;

        public EncryptFacade()
        {
            reader = new FileReader();
            cipher = new CipherMachine();
            writer = new FileWriter();
        }

        //调用其他对象的业务方法
         public void FileEncrypt(string fileNameSrc, string fileNameDes)
        {
            string plainStr = reader.Read(fileNameSrc);
            string encryptStr = cipher.Encrypt(plainStr);
            writer.Write(encryptStr, fileNameDes);
        }
    }

Program：客户端测试类

	class Program
	{
		static void Main(string[] args)
		{
		    EncryptFacade ef = new EncryptFacade();
		    ef.FileEncrypt("src.txt", "des.txt");
		    Console.Read();
		}
	}
## 参考资料
1. [深入浅出外观模式（一）](http://blog.csdn.net/lovelion/article/details/8258121)