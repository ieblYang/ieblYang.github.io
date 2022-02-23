# [Java核心技术] 继承、接口和抽象类


Java 核心技术读书笔记——Java 继承、接口和抽象类

<!--more-->

## 1 继承

* 面向对象编程语言和面向过程的编程语言最突出的特点就是**变量类型的继承**
* 面向过程编程语言没有继承，出现很多类型重复定义
* 对象可以分成若干类别，类别内的对象属性和方法具有一定的共同点
* 将共同点提取出来，及形成了父类/基类/超类
* 其他类则自动成为子类/派生类

{{% admonition note "笔记" %}}
```Java
public class Human{
  int height;
  int weight;
  public void eat(){}
}

public class Man{
  int height;
  int weight;
  public void eat(){};
  public void plough(){}; //耕田
}

public class Woman{
  int height;
  int weight;
  public void eat(){};
  public void weave(){}; //织布
}


public class Man extends Human{
  public void plough(){}; //耕田
}

public class Woman extends Human{
  public void weave(){}; //织布
}
```
* 子类继承父类所有的属性和方法（但不能直接访问private成员）
* 根据信息隐藏原则：子类会继承父类所有方法。可以直接使用。
* 子类也会继承父类的父类的所有属性和方法（但不能直接访问private成员）
* 在同样方法名和参数情况下，本类的方法会比父类的方法优先级高。
{{% /admonition %}}

* 单根继承原则：每个类都只能继承一个类
* 如果不写`extends`，`Java`类都默认继承`java.lang.Object`类
* `Java`**所有类**从`java.lang.Object`开始，构建出一个类型继承树
* `Object`类里面默认就有`clone`、`equals`、`finalize`、`getClass`、`hashCode`、`toString`等方法

## 2 抽象类和接口

### 2.1 抽象类
* 类：属性（0或多个）+ 方法（0或多个）
* 一个完整（健康）的类：所有的方法都有实现（方法体）
* 类可以没有方法，但是有方法就肯定要实现，这才是个完整的类
* 一个完整（健康）的类才可以被实例化，被`new`出来
* 如果一个类暂时有方法未实现，需要被定义为**抽象类**
* 抽象类关键字`abstract`声明
* 抽象类组成：
  * 成员变量，个数不限
  * 具体方法，方法有实现，个数不限
  * 抽象方法，加`abstract`关键字，个数不限
  ```Java
  public abstract class Shape{
    int area;
    // 当图形未知时，无法给胡calArea的具体实现，因此此方法被定义为abstract
    // calArea() 是一个不完整/不健康的方法，因为它没有方法体。
    public abstract void calArea(); 
  }

  ```
* 抽象类也是类。一个类继承于抽象类，就不能继承于其他的（抽象）类。
* 子类可以继承于抽象类，但是一定要实现父类们所有`abstract`方法。如果不能完全实现，那么子类也必须被定义为抽象类。
* 只有实现父类（们）的所有抽象方法，才能变成完整类。

### 2.2 接口

* 如果类的所有方法都没有实现，那么这个类就算是接口（`interface`）
  ```Java
  public interface Animal{
    public void eat();
    public void move();
  }
  ``` 
* 类只可以继承（`extends`）一个类（可以是抽象类也可以是普通类），但是可以实现（`implements`）多个接口，继承和实现可以同时发生。
  * 继承抽象类，必须实现所有`abstract`的方法；
  * 实现多个接口，必须实现接口中所定义的所有方法；
  * 一个类的方法，只会在当前类或者父类中定义，肯定不会在所实现的父类接口中定义
* 接口不算类，或者说是“特殊”的类
* 接口可以继承多个接口，没有实现的方法将会叠加
* 类实现接口，就必须实现所有未实现的方法，如果没有全部实现，那么只能成为一个抽象类
* 接口里可以定义变量，但是一般是常量

* `extends` 必须写在 `implements` 前面 
  ```Java
  //Animal.java
  public interface Animal {
    public void eat();
    public void move();
  }
  
  //LandAnimal.java
  public abstract class LandAnimal implements Animal {

    public abstract void eat() ;

    public void move() {
      System.out.println("I can walk by feet");
    }
  }

  //ClimbTree.java
  public interface ClimbTree {
    public void climb();
  }

  //Rabbit.java
  public class Rabbit extends LandAnimal implements ClimbTree {

    public void climb() {
      System.out.println("Rabbit: I can climb");    
    }

    public void eat() {
      System.out.println("Rabbit: I can eat");    
    }
  }

  ```
* 抽象类和接口的相同点：两者都不能被实例化，不能new操作
* 抽象类和接口的不同点：
  * 抽象类`abstradt`，接口`interface`
  * 抽象类可以有部分方法实现，接口所有方法不能有实现
  * 一个类只能继承（`extends`）一个（抽象）类，实现（`implements`）多个接口
  * 接口可以继承（extends）多个接口
  * 抽象类有构造函数，接口没有构造函数
  * 抽象类可以有`main`，也能运行，接口没有main函数
  * 抽象类方法可以有`private`/`protected`，接口方法都是`public`

## 3 转型、多态和契约设计

### 3.1 类转型

* 变量支持相互转化，比如 `int a = (int)3.5`;
* 类型可以相互转型，但是只限制于有继承关系的类。
  * 子类可以转换成父类（向上转型），而父类不可以转换为子类（向下转型）。
    ```Java
    Human obj1 = new Man(); // Ok, Man extends Human
    Man obj2 = new Human(); // illegal, Man is a derived class Human
    ```
  * 父类转子类，有一种特殊情况例外，即父类本身就是从子类转化而来的。
    ```Java
    Human obj1 = new Man(); // Ok, Man extends Human
    Man obj2 = (Man) obj1; // Ok, because obj1 is born from Man class
    ```

### 3.2 多态

* 类型转换，带来的作用就是**多态**。
* 子类继承父类的所有方法，但子类可以定义一个名字、参数和父类一样的方法，这种行为就是**重写**`（覆盖，覆写）`
* 子类的方法优先级**高于**父类。
  ```Java
  // Human.java
  public class Human {
  int height;    
    int weight;
    
    public void eat()  {
      System.out.println("I can eat!");
    }
  }

  // Man.java
  public class Man extends Human {
    public void eat() {
      System.out.println("I can eat more");
    }
    
    public void plough() { }

    public static void main(String[] a) {
      Man obj1 = new Man();
      obj1.eat();   // call Man.eat()
      Human obj2 =  (Human) obj1;  // 类转型 obj1 -> obj2
      obj2.eat();   // call Man.eat()
      Man obj3 = (Man) obj2;
      obj3.eat();   // call Man.eat()
    }
  }
  ```
* 多态的作用
  * 以统一的接口来操纵某一类中不同对象的动态行为
  * 对象之间的解耦

  ```Java
  // Animal.java
  public interface Animal {
    public void eat();
    public void move();
  }

  // Cat.java
  public class Cat implements Animal
  {
    public void eat() {
      System.out.println("Cat: I can eat");
    }
    
    public void move(){
      System.out.println("Cat: I can move");
    }
  }

  // Dog.java
  public class Dog implements Animal
  {
    public void eat() {
      System.out.println("Dog: I can eat");
    }
    
    public void move() {
      System.out.println("Dog: I can move");
    }
  }

  // AnimalTest.java
  public class AnimalTest {
    
    public static void haveLunch(Animal a)  {
      a.eat();
    }
    
    public static void main(String[] args) {
      Animal[] as = new Animal[4];
      as[0] = new Cat();
      as[1] = new Dog();
      as[2] = new Cat();
      as[3] = new Dog();
      
      for(int i=0;i<as.length;i++) {
        as[i].move();  //调用每个元素的自身的move方法
      }
      for(int i=0;i<as.length;i++) {
        haveLunch(as[i]);
      }
      
      haveLunch(new Cat());  //Animal  a = new Cat();  haveLunch(a);
      haveLunch(new Dog());
      haveLunch(
          new Animal()
          {
            public void eat() {
              System.out.println("I can eat from an anonymous class");            
            }
            public void move() {
              System.out.println("I can move from an anonymous class");
            }
            
          });
    }

    public static void huveLunch(Animal a){
      a.eat();
    }

  }

  /**
   * 
   * 运行结果：
   * Dog: I can move
   * Cat: I can move
   * Dog: I can move
   * Cat: I can eat
   * Dog: I can eat
   * Cat: I can eat
   * Dog: I can eat
   * Cat: I can eat
   * Dog: I can eat
   * I can eat from an anonymous class
   * 
   */
  ```

### 3.3 契约设计

* 契约：规定规范了对象应该包含的行为方法
* 接口定义了方法的名称、参数和返回值，规范了派生类的行为
* 基于接口，利用转型和多态，不影响真正方法的调用，成功地将调用类和被调用类解耦
* 被调用类（`haveLunch`只和`Animal`有联系）
  ```Java
  // Animal 是一个接口，里面所有的方法都是空的，没有方法体。
  public static void huveLunch(Animal a){
        a.eat();
      }
  ```
* 调用类
  ```Java
  haveLunch(new Cat());  //Animal  a = new Cat();  haveLunch(a);
  haveLunch(new Dog());
  haveLunch(
      // 匿名类只在此句用一次就结束了，
      // 只需要传进来一个实现Animal接口的对象，就可以运行haveLunch方法。
      new Animal()
      {
        public void eat() {
          System.out.println("I can eat from an anonymous class");            
        }
        public void move() {
          System.out.println("I can move from an anonymous class");
        }
        
      });

  ```

