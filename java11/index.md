# Java 面向对象有哪些特征？


Java 面向对象有哪些特征？

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
* 接口可以继承多个多个接口，没有实现的方法将会叠加
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
* 
## 3 转型、多态和契约设计



