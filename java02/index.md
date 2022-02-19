# Java面向对象


Java 核心技术读书笔记——Java面向对象

<!--more-->

## 1 面向对象思想

{{% admonition note "笔记" %}}
* 对像是一个变量；
* 类就是类型（是规范，是定义），从万千对象中抽取共性；
* 类规定了对象应该有的属性内容和方法；
* 对象是类的具体实现，是活生生的；
{{% /admonition %}}

## 2 类

类（ `class`） 是构造对象的模板或蓝图。由类构造（`construct`）对象的过程称为创建类的实例 （`instance`）。

```Java
// 最简单的类
class A{

}
```
## 3 对象

### 3.1 对象的三个主要特性
* **对象的行为**（`behavior`）：可以对对象施加哪些操作，或可以对对象施加哪些方法？
* **对象的状态**（`state`）：当施加那些方法时，对象如何响应？
* **对象标识**（`identity`）：如何辨别具有相同行为与状态的不同对象？ 

{{% admonition note "笔记" %}}
```Java
A obj1 = new A();
A obj2 = new A();
```
以上两个对象，它们的类型都是A，但是这是两个不同的对象，在内存中有不同的存放地址。因此，没有两个对象是完全一样的。

{{% /admonition %}}

### 3.2 对象的赋值方式
**对象赋值是`Reference`赋值，基本类型是直接值拷贝**
```Java
//MyNumber.java

class MyNumber
{
  int num = 5; 
}

//ReferenceTest.java
public class ReferenceTest {

  public static void main(String[] args) {
    
    // 基本类型的变量值小，可直接拷贝
    int num1 = 5;
    int num2 = num1;
    System.out.println("num1: " + num1 + ", num2: " + num2); // num1: 5, num2: 5
    num2 = 10;
    System.out.println("num1: " + num1 + ", num2: " + num2); // num1: 5, num2: 10
    
    
    MyNumber obj1 = new MyNumber();
    MyNumber obj2 = new MyNumber();
    System.out.println(obj1.num); // 5
    System.out.println(obj2.num); // 5
    System.out.println("======接下来输出新值=====");
    
    obj2 = obj1;

    /**
     *   5         5
     *   ⇑    ⇖
     *  obj1      obj2
     * 
     * 对象包含多个值，不容易复制，赋值采用共享同一块内存区域；
     *  obj1 和 obj2 指向了同一个 5 
     * 
     */

    obj2.num = 10;
    /**
     *   10   5
     *   ⇑    ⇖
     *  obj1      obj2
     * 
     */
    
    System.out.println(obj1.num); // 10
    System.out.println(obj2.num); // 10

  }

}


//ArgumentPassingTest
public class ArgumentPassingTest {

  public static void main(String[] args) {
    int a = 1, b = 2;
    swap(a,b);
    
    /**
     *  1  2  1  2
     *  ⇑  ⇑  ⇑  ⇑
     *  a  b  m  n
     *  
     *  swap的形参为int型，int直接赋值，
     *  调用后内存中出现 a b m n
     *  m n 的交换并不影响 a b 的交换
     *  
     */

    System.out.println("a is " + a + ", b is " + b);  //a=1, b=2  没有交换
    
    
    MyNumber obj1 = new MyNumber();
    MyNumber obj2 = new MyNumber();
    obj2.num = 10;

    swap(obj1, obj2);
    /**
     *      5            10
     *   ⇗    ⇖       ⇗    ⇖
     *  obj1  obj3   obj2  obj4  
     * 
     *  对象包含多个值，不容易复制，赋值采用共享同一块内存区域；
     *  obj1 和 obj3 指向同一块内存；
     *  obj2 和 obj4 指向同一块内存；
     */

    System.out.println("obj1 is " + obj1.num + ", obj2 is " + obj2.num);  //  obj1  10,   obj2  5  交换成功

  }

  public static void swap(int m, int n) //swap的形参为int型，int直接赋值
  {
    int s = m;
    m = n;
    n = s;
  }

  public static void swap(MyNumber obj3, MyNumber obj4)
  {
    int s = obj3.num;
    obj3.num = obj4.num;
    obj4.num = s;
  }

}

```

## 4 构造函数
* `Java`构造函数的名称必须**和类名一样**，且**没有返回值**
  ```Java
  public class A{
    int id;
    public A(int id2){
      id = id2;
    }
  }
  ```
* `Java`有构造函数，但是没有析构函数
  * 构造函数是制造对象的过程（在内存中开辟一个空间存储数据）
  * 析构函数是清楚对象的过程（将一个数据对象清空）
* 每个变量都是**有生命周期**的，它只能存储在离它最近的一对`{}`中
  ```Java
  public class LifeCycleTest {

    public static void main(String[] args) {
      int a=0;  // a 在main函数中都是active
      
      //i只能活在for循环中
      for(int i=0; i<5; i++) {
        int k = 0;
        k++;
      }
      
      if(a>0) {
        Object obj1 = new Object();  //obj1 只能在if分支中
        System.out.println("a is positive");
      } else {
        Object obj2 = new Object();  //obj2只能在else分支中
        System.out.println("a is non-positive");
      }
    }
  }
  ```
* 当变量被**创建时**，变量将**占据内存**，当**变量消亡**时，系统将**收回内存**
* `Java` 具有**内存自动回收机制**，当变量退出其生命周期后，`JVM`会自动回收所分配的内存对象的内存。所以不需要析构函数来释放内存。
* 对象回收效率依赖于**垃圾回收器**`GC（Garbage Collector）`，其回收算法关系到性能好坏。
* 每个`Java`类都**必须有构造函数**
* 如果没有显式定义构造函数，Java编译器**自动**为该类产生一个**空的无形参**构造函数。
* 每个子类的构造函数第一句话，**都默认调用父类的无形参构造函数`super()`**，除非子类的构造函数第一句话是`super`，而且`super`语句必须放在第一条。
* 一个类可以有**多个构造函数**，只要**形参列表不相同**即可。
* 在new对象的时候，根据实参的不同，自动挑选相应的构造函数。如果实参形参匹配不上，将会报错。
  ```Java
  class MyPairNumber
  {
    int m;
    int n;
    public MyPairNumber()
    {
      m = 0;
      n = 0;
    }
    public MyPairNumber(int a)
    {
      m = a;
      n = 0;
    }
    public MyPairNumber(int a, int b)
    {
      m = a;
      n = b;
    } 
  }
  public class ConstructorTest {

    public static void main(String[] args) {
      MyPairNumber obj1 = new MyPairNumber();
      MyPairNumber obj2 = new MyPairNumber(5);
      MyPairNumber obj3 = new MyPairNumber(10,20);
      System.out.println("obj1 has " + obj1.m + "," + obj1.n); //obj1 has 0,0
      System.out.println("obj2 has " + obj2.m + "," + obj2.n); //obj2 has 5,0
      System.out.println("obj3 has " + obj3.m + "," + obj3.n); //obj3 has 10,20

      // A a = new A();  //error, A类中没有无参数的构造函数
    }
  }
  ```
## 5 信息隐藏和`this`
### 5.1 信息隐藏
* 面向对象有一个法则：**信息隐藏**
  * 类的成员属性，是私有的`private`;
  * 类的方法是共有`public`的，通过方法修改成员属性的值。
  * 通过类的方法来简介访问类的属性，而不是直接访问类的属性。
  * `get`和`set`方法是共有`public`的，统称`getter`和`setter`
  * 外界对类成员的操作只能通过`get`和`set`方法
  ```Java
  //InfoHiding.java
  class InfoHiding {
    private int id;

    public InfoHiding(int id2) {
      id = id2;
    }
    public int getId() {
      return id;
    }
    public void setId(int id2) {
      id = id2;
    }
  }
  //InfoHidingTest.java
  public class InfoHidingTest {
    public static void main(String[] args) {
      InfoHiding obj = new InfoHiding(100);
      obj.setId(200);
      System.out.println(obj.getId());
    }
  }
  ```
### 5.2 `this`
* `this` 负责指向本类中的成员变量
```Java
//InfoHiding.java
class InfoHiding {
  private int id;

  public InfoHiding(int id2) {
    id = id2;
  }
  public int getId() {
    return id;
  }
  public void setId(int id2) {
    id = id2;
  }
}
//InfoHiding2.java
public class InfoHiding2 {
  private int id;
  
  public InfoHiding2(int id)  //在构造函数中，形参的优先级更高。
  {
    this.id = id;             //this 相当于 InfoHiding2，指向本类中的成员变量
  }

  public int getId() {
    return id;
  }

  public void setId(int id) {
    this.id = id;
  }
}
```

* `this` 负责指向本类中的成员方法

```Java
 this.add(5,3); //调用本类的add方法，this可忽略
```

* `this` 可以代替本类中的构造函数
```Java
//MyPairNumber.java
public class MyPairNumber {
  private int m;
  private int n;
  public int getM() {
    return m;
  }
  public void setM(int m) {
    this.m = m;
  }
  public int getN() {
    return n;
  }
  public void setN(int n) {
    this.n = n;
  } 
  public MyPairNumber() {
    this(0, 0);
  } 
  public MyPairNumber(int m)  {
    //this 当作构造函数使用
    this(m, 0);
  } 
  public MyPairNumber(int m, int n) {
    // this 指向本类中的成员变量
    this.m = m;  // m = 5  
    this.n= n;   // n = 0 
  } 
  public int sum() {
    // this 指向本类中的成员方法
    return this.add(m,n);  //return add(m,n); 也可以
  }
  public int add(int a, int b){
    return a+b;
  } 
}


//ThisTest.java
public class ThisTest {

  public static void main(String[] args) {
    
    MyPairNumber obj = new MyPairNumber(5);
        
    System.out.println(obj.sum());  // 5
  }

}

```
