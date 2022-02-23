# [Java核心技术] static、final和常量设计


Java 核心技术读书笔记——Java static、final和常量设计

<!--more-->

## 1 static

`static` 静态的，`Java`中特殊的关键字，可作用在**变量**、**方法**、**类**、**匿名方法块**

### 1.2 静态变量

* 静态变量，类共有成员
  ```Java

  public class Potato {
    static int price = 5;
    String content = "";
    public Potato(int price, String content)
    {
      this.price = price;
      this.content = content;
    } 
    public static void main(String[] a)
    {
      System.out.println(Potato.price); //Potato.content    wrong
      System.out.println("----------------------------------");
      Potato obj1 = new Potato(10,"青椒土豆丝");
      // 以下两行都输出10，Potato.price 和 obj1.price 在内存中是同一个东西
      System.out.println(Potato.price); 
      System.out.println(obj1.price);
      
      System.out.println("----------------------------------");
      Potato obj2 = new Potato(20,"酸辣土豆丝");
      System.out.println(Potato.price);
      System.out.println(obj2.price);
      
    }
  }

  /**
   * 
   * 输出结果：
   * 5
   * ----------------------------------
   * 10
   * 10
   * ----------------------------------
   * 20
   * 20
   * 
   */

  ```
* `static`变量只依赖于类存在（通过类访问即可），不依赖于对象实例存在。即可以用过`Potato.price`即可访问
* 所有的对象实例，如上例中的`obj1`和`obj2`关于`price`变量的值共享存储在一个共同的空间（栈）

### 1.2 静态方法
* 静态方法也无需通过对象来引用，而通过类名可以直接引用。
* 静态方法中，只能使用静态变量，不能使用非静态变量。
* 静态方法不能调用非静态方法。
  ```Java
  public class StaticMethodTest {
    int a = 111111;
    static int b = 222222;
    public static void hello()
    {
      System.out.println("000000");
      System.out.println(b);
      //System.out.println(a);  //error, cannot call non-static variables
      //hi()                    //error, cannot call non-static method
    }
    public void hi()
    {
      System.out.println("333333");
      hello();                  //ok, call static methods
      System.out.println(a);    //ok, call non-static variables
      System.out.println(b);    //ok, call static variables
    }
    public static void main(String[] a)
    {
      StaticMethodTest.hello();
      //StaticMethodTest.hi(); //error, 不能使用类名来引用非静态方法
      StaticMethodTest foo = new StaticMethodTest();
      foo.hello();  //warning, but it is ok
      foo.hi();     //right
    }
  }


  ```
### 1.3 static修饰类（内部类）
  使用机会较少

### 1.4 static块
* `static块`只在类第一次被加载时调用，即程序运行期间，这段代码只运行一次
* 执行顺序：`static块` > `匿名块` > `构造函数`
* 不建议编写块代码，块代码会给程序带来混淆。建议将块代码封装成函数再调用。
  ```Java
  // StaticBlock.java
  class StaticBlock
  {
    //staticl block > anonymous block > constructor function  
    
    // 静态代码块
    static
    {
      System.out.println("22222222222222222222");
    }
    // 匿名代码块
    {
      System.out.println("11111111111111111111");
    }
    // 构造函数
    public StaticBlock()
    {
      System.out.println("33333333333333333333");
    }
    // 匿名代码块
    {
      System.out.println("44444444444444444444");
    }
  }

  //StaticBlockTest.java
  public class StaticBlockTest {

    public static void main(String[] args) {
      System.out.println("000000000000000");
      // TODO Auto-generated method stub
      StaticBlock obj1 = new StaticBlock();
      StaticBlock obj2 = new StaticBlock();
    }

  }

  /**
   * 
   * 运行结果：
   * 000000000000000
   * 22222222222222222222
   * 11111111111111111111
   * 44444444444444444444
   * 33333333333333333333
   * 11111111111111111111
   * 44444444444444444444
   * 33333333333333333333
   * 
   */
  ```
## 2 单例模式（Singleton）
* 单例模式（单态模式，Singleton），保证一个类有且只有一个对象
  * 采用 `static` 来共享对象实例
  * 采用 `private` 构造函数，防止外界 `new` 操作
  * 限定某一个类在整个程序运行过程中，只能保留一个实例对象在内存空间

  ```Java
  public class Singleton {
    private static Singleton obj = new Singleton(); //共享同一个对象
    private String content;
    
    private Singleton()  //确保只能在类内部调用构造函数
    {
      this.content = "abc";
    }
    
    public String getContent()  {
      return content;
    }
    public void setContent(String content) {
      this.content = content;
    } 
    
    public static Singleton getInstance() {
      //静态方法使用静态变量
      //另外可以使用方法内的临时变量，但是不能引用非静态的成员变量
      return obj;
    }
  
  
    public static void main(String[] args) {
      Singleton obj1 = Singleton.getInstance();
      System.out.println(obj1.getContent());  // abc
      
      Singleton obj2 = Singleton.getInstance();
      System.out.println(obj2.getContent());  // abc
      
      obj2.setContent("def");
      System.out.println(obj1.getContent());
      System.out.println(obj2.getContent());
      
      System.out.println(obj1 == obj2); // true, obj1和obj2指向同一个对象
    }
  }
  ```

* 单例模式是`GoF`的23种设计模式（Design Pattern）中经典的一种，属于创造型模型类型。
{{% admonition note "笔记" %}}
**设计模式**：在软件开发过程中，经过验证的，用于解决在特定环境下的、重复出现的、特定问题的结局方案。
{{% /admonition %}}

## 3 final

* `Java` 的 `final`关键字可以用来修饰类、方法、字段
* `final`的类，不能被继承

  ```Java
  final public class FinalFather {

  }
  class Son1 extends FinalFather
  {
    
  }
  ```
* 父类中如果有`final`的方法，子类中不能改写此方法
  ```Java
  // FinalMethodFather.java
  public class FinalMethodFather {
    public final void f1()
    {
    
    }
  }

  //FinalMethodSon.java
  public class FinalMethodSon extends FinalMethodFather{
    public void f1() // error Cannot override the final method from FinalMethodFather
    {
    
    }
  }
  ```
* `final`的变量，不能再次赋值
  * 如果是基本型的变量，不能修改其值
  ```java
  public class FinalPrimitiveType {

    public static void main(String[] args) {
      // TODO Auto-generated method stub
      final int a = 5;
      a=10; // error The final local vaiable a cannot be assigned. It must be blank and not using a compound assignment
    }
  }
  ```
  * 如果是实例对象，那么不能修改其指针（但是可以修改对象内的值）
  ```Java
  class FinalObject
  {
    int a = 10;
  }

  public class FinalObjectTest {

    public static void main(String[] args) {
      final FinalObject obj1 = new FinalObject();
      System.out.println(obj1.a);
      obj1.a = 20;
      System.out.println(obj1.a);
      
      obj1 = new FinalObject();
      //final对象不能变更指针
    }

  }
  ```

## 4 常量设计和常量池

