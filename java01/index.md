# Java基本语句和结构


Java 核心技术读书笔记——基本语句和结构

<!--more-->


{{% admonition note "Java简介" %}}
Java 是由Sun Microsystems公司于1995年5月推出的Java面向对象程序设计语言和Java平台的总称。由James Gosling和同事们共同研发，并在1995年正式推出。

Java 分为三个体系：

* JavaSE(Standard Edition，java平台标准版) 面向PC级应用开发
* JavaEE(Enterprise Edition，java平台企业版) 面向企业级应用开发
* JavaME(Micro Edition，java平台微型版) 面向嵌入式应用开发
2005年6月，JavaOne大会召开，SUN公司公开Java SE 6。此时，Java的各种版本已经更名以取消其中的数字"2"：J2EE更名为Java EE, J2SE更名为Java SE，J2ME更名为Java ME。
{{% /admonition %}}


## 1 主要特性

### 1.1 简单性
* `Java` 的语法与 `C语言` 和 `C++` 语言很接近，很容易学习和使用。
* `Java` 丢弃了 `C++` 中很少使用的、很难理解的、令人迷惑的特性。
* `Java` 不使用指针，而是引用。并提供了自动分配和回收内存空间，使得程序员不必为内存管理而担忧。
### 1.2 面向对象
* `Java`提供类、接口和继承等面向对象的特性，为了简单起见，<span style="background: #fffaa5">支持**类**之间的**单继承**，支持**接口**之间的**多继承**，</span>并支持类与接口之间的实现机制（关键字为 `implements`）。
* `Java`全面支持动态绑定，而 `C++`只对虚函数使用动态绑定。
### 1.3 分布式
`Java`支持 `Internet` 应用的开发，在基本的 `Java` 应用编程接口中有一个网络应用编程接口（`java net`），它提供了用于网络应用编程的类库，包括 `URL`、`URLConnection`、`Socket`、`ServerSocket` 等。`Java` 的 `RMI（远程方法激活）`机制也是开发分布式应用的重要手段。
### 1.4 健壮性
`Java` 的强类型机制、异常处理、垃圾的自动收集等是 `Java` 程序健壮性的重要保证。对指针的丢弃是 `Java` 的明智选择。`Java` 的安全检查机制使得 `Java` 更具健壮性。
### 1.5 安全性
`Java` 通常被用在网络环境中，为此，`Java` 提供了一个安全机制以防恶意代码的攻击。除了`Java` 具有的许多安全特性以外，`Java` 对通过网络下载的类具有一个安全防范机制（`类 ClassLoader`），如分配不同的名字空间以防替代本地的同名类、字节代码检查，并提供安全管理机制（`类 SecurityManager`）让 `Java` 应用设置安全哨兵。
### 1.6 体系结构中立
`Java` 程序（后缀为 `java` 的文件）在 `Java` 平台上被编译为体系结构中立的字节码格式（后缀为 `class`的文件），然后可以在实现这个 `Java` 平台的任何系统中运行。这种途径适合于异构的网络环境和软件的分发。
### 1.7 可移植性
这种可移植性来源于体系结构中立性，另外，`Java` 还严格规定了各个基本数据类型的长度。`Java` 系统本身也具有很强的可移植性，`Java` 编译器是用 `Java` 实现的，`Java` 的运行环境是用 `ANSI C` 实现的。
### 1.8 解释型
如前所述，`Java` 程序在 `Java` 平台上被编译为字节码格式，然后可以在实现这个`Java`平台的任何系统中运行。在运行时，`Java` 平台中的 `Java` 解释器对这些字节码进行解释执行，执行过程中需要的类在联接阶段被载入到运行环境中。
### 1.9 高性能
与那些解释型的高级脚本语言相比，`Java` 的确是高性能的。事实上，`Java` 的运行速度随着 `JIT(Just-In-Time）`编译器技术的发展越来越接近于 `C++`。
### 1.10 多线程
在 `Java` 语言中，线程是一种特殊的对象，它必须由 `Thread` 类或其子（孙）类来创建。通常有两种方法来创建线程：其一，使用型构为 `Thread(Runnable) `的构造子类将一个实现了 `Runnable` 接口的对象包装成一个线程，其二，从 `Thread` 类派生出子类并重写 `run` 方法，使用该子类创建的对象即为线程。值得注意的是 `Thread` 类已经实现了 `Runnable` 接口，因此，任何一个线程均有它的 `run` 方法，而 `run` 方法中包含了线程所要运行的代码。线程的活动由一组方法来控制。`Java` 支持多个线程的同时执行，并提供多线程之间的同步机制（关键字为 `synchronized`）。
### 1.11 动态性
`Java` 语言的设计目标之一是适应于动态变化的环境。`Java` 程序需要的类能够动态地被载入到运行环境，也可以通过网络来载入所需要的类。这也有利于软件的升级。另外，`Java `中的类有一个运行时刻的表示，能进行运行时刻的类型检查。


## 2 Java语法规范

`Java` 是面向对象的编程语言，一个 `Java` 程序可以认为是一系列对象的集合，而这些对象通过调用彼此的方法来协同工作。下面简要介绍下类、对象、方法和实例变量的概念。

* 对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
* 类：类是一个模板，它描述一类对象的行为和状态，是`Java`程序最小独立单元，包括成员变量和成员方法。
* 方法：方法就是行为，一个类可以有很多方法。逻辑运算、数据修改以及所有动作都是在方法中完成的。
* 实例变量：每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定。

### 2.1 基本语法

* 大小写敏感
* 类名：<span style="background: #fffaa5">是以**大写字母**开头的名词。如果名字由多个单词组成，每个单词的第一个字母都应该大写</span>称为**骆驼命名法**。
* 方法名：<span style="background: #fffaa5">所有的方法名都应该以**小写字母**开头。如果方法名含有若干单词，则后面的每个单词首字母大写。</span>
* 源文件名：源代码的文件名必须与公共类的名字相同，并用 `.java` 作为扩展名。
* 主方法： `public static void main(String[] args)` 运行已编译的程序时，Java 虚拟机将从指定类中的 `main`方法开始执行，因此为了代码能够执行，在类的源文件中必须包含一个 `main` 方法。
   * `main`函数是程序启动的总入口。
   * `main`函数的形参`args`是外界提供给`main`函数的参数，可以在`main`函数中使用。

### 2.2 Java 标识符

`Java` 所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。

* <span style="background: #fffaa5">所有的标识符都应该以字母（`A-Z` 或者 `a-z`）、美元符（`$`）、或者下划线（`_`）开始</span>
* 首字符之后可以是字母（`A-Z` 或者 `a-z`）,美元符（`$`）、下划线（`_`）或数字的任何字符组合
* <span style="background: #fffaa5">关键字不能用作标识符</span>
* 标识符**大小写敏感**
* 合法标识符举例：`age`、`$salary`、`_value`、`__1_value`
* 非法标识符举例：`123abc`、`-salary`

### 2.3 Java 修饰符

`Java` 可以使用修饰符来修饰类中方法和属性。主要有两类修饰符：

* 访问控制修饰符 : `default`, `public` , `protected`, `private`
* 非访问控制修饰符 : `final`, `abstract`, `static`, `synchronized`

### 2.4 Java 变量

`Java` 中主要有如下几种类型的变量

* 局部变量
* 类变量（静态变量）
* 成员变量（非静态变量）

### 2.5 Java 关键字

<table>
   <tr>
      <th>类别</th>
      <th>关键字</th>
      <th>说明</th>
   </tr>
   <tr>
      <td rowspan="3">访问控制</td>
      <td>private</td>
      <td>私有的</td>
   </tr>
   <tr>
      <td>protected</td>
      <td>受保护的</td>
   </tr>
   <tr>
      <td>public</td>
      <td>公共的</td>
   </tr>
   <tr>
      <td rowspan="13">类、方法和变量修饰符</td>
      <td>abstract</td>
      <td>声明抽象</td>
   </tr>
   <tr>
      <td>class</td>
      <td>类</td>
   </tr>
   <tr>
      <td>extends</td>
      <td>扩充,继承</td>
   </tr>
   <tr>
      <td>final</td>
      <td>最终值,不可改变的</td>
   </tr>
   <tr>
      <td>implements</td>
      <td>实现（接口）</td>
   </tr>
   <tr>
      <td>interface</td>
      <td>接口</td>
   </tr>
   <tr>
      <td>native</td>
      <td>本地，原生方法（非 Java 实现）</td>
   </tr>
   <tr>
      <td>new</td>
      <td>新,创建</td>
   </tr>
   <tr>
      <td>static</td>
      <td>静态</td>
   </tr>
   <tr>
      <td>strictfp</td>
      <td>严格,精准</td>
   </tr>
   <tr>
      <td>synchronized</td>
      <td>线程,同步</td>
   </tr>
   <tr>
      <td>transient</td>
      <td>短暂</td>
   </tr>
   <tr>
      <td>volatile</td>
      <td>易失</td>
   </tr>
   <tr>
      <td rowspan="12">程序控制语句</td>
      <td>break</td>
      <td>跳出循环</td>
   </tr>
   <tr>
      <td>case</td>
      <td>定义一个值以供 switch 选择</td>
   </tr>
   <tr>
      <td>continue</td>
      <td>继续</td>
   </tr>
   <tr>
      <td>default</td>
      <td>默认</td>
   </tr>
   <tr>
      <td>do</td>
      <td>运行</td>
   </tr>
   <tr>
      <td>else</td>
      <td>否则</td>
   </tr>
   <tr>
      <td>for</td>
      <td>循环</td>
   </tr>
   <tr>
      <td>if</td>
      <td>如果</td>
   </tr>
   <tr>
      <td>instanceof</td>
      <td>实例</td>
   </tr>
   <tr>
      <td>return</td>
      <td>返回</td>
   </tr>
   <tr>
      <td>switch</td>
      <td>根据值选择执行</td>
   </tr>
   <tr>
      <td>while</td>
      <td>循环</td>
   </tr>
   <tr>
      <td rowspan="6">错误处理</td>
      <td>assert</td>
      <td>断言表达式是否为真</td>
   </tr>
   <tr>
      <td>catch</td>
      <td>捕捉异常</td>
   </tr>
   <tr>
      <td>finally</td>
      <td>有没有异常都执行</td>
   </tr>
   <tr>
      <td>throw</td>
      <td>抛出一个异常对象</td>
   </tr>
   <tr>
      <td>throws</td>
      <td>声明一个异常可能被抛出</td>
   </tr>
   <tr>
      <td>try</td>
      <td>捕获异常</td>
   </tr>
   <tr>
      <td rowspan="2">包相关</td>
      <td>import</td>
      <td>引入</td>
   </tr>
   <tr>
      <td>package</td>
      <td>包</td>
   </tr>
   <tr>
      <td rowspan="8">基本类型</td>
      <td>boolean</td>
      <td>布尔型</td>
   </tr>
   <tr>
      <td>byte</td>
      <td>字节型</td>
   </tr>
   <tr>
      <td>char</td>
      <td>字符型</td>
   </tr>
   <tr>
      <td>double</td>
      <td>双精度浮点</td>
   </tr>
   <tr>
      <td>float</td>
      <td>单精度浮点</td>
   </tr>
   <tr>
      <td>int</td>
      <td>整型</td>
   </tr>
   <tr>
      <td>long</td>
      <td>长整型</td>
   </tr>
   <tr>
      <td>short</td>
      <td>短整型</td>
   </tr>
   <tr>
      <td rowspan="3">变量引用</td>
      <td>super</td>
      <td>父类,超类</td>
   </tr>
   <tr>
      <td>this</td>
      <td>本类</td>
   </tr>
   <tr>
      <td>void</td>
      <td>无返回值</td>
   </tr>
   <tr>
      <td rowspan="3">保留关键字</td>
      <td>goto</td>
      <td>是关键字，但不能使用</td>
   </tr>
   <tr>
      <td>const</td>
      <td>是关键字，但不能使用</td>
   </tr>
   <tr>
      <td>null</td>
      <td>空</td>
   </tr>
</table>

### 2.6 Java 注释
Java 中的注释不会出现在可执行程序中。因此，可以在源程序中根据需要添加任意多的注释，而不必担心可执行代码会膨胀。

* 单行注释 `//`
* 长段注释 `/*` `*/`
* 多行注释 `/**` `*/`

```Java
/**
 * 这是一个多行注释示例
 * 
 * ©author ieblYang
 */
public class FirstSample
{

	/*这是一个长段注释示例*/
	public static void main(String[] args)
	{
		//这是一个单行注释示例
		System.out.println("We will not use 'Hello, World!'");
	}
}
```

## 3 数据类型

在 `Java` 中，一共有 8 种基本类型：`byte`（字节）、`short`（短整数）、 `int`（整数）、 `long`（长整数）、 `float、double`（浮点数）、 `char`（字符）、`boolean`（布尔） 。

### 3.1 整型

整型用于表示没有小数部分的数值，可正可负。Java 提供了 4 种整型。如下表所示：

|类型|存储需求|取值范围|默认值|
|----|----|----|----|
|`byte`|1字节|-128 ～ 127|0|
|`short`|2字节|-32768 ～ 32767|0|
|`int`|4字节|-2147483648 ～ 2147483647|0|
|`long`|8字节|-9223372036854775808 ～ 9223372036854775807|0L|

{{% admonition note "笔记" %}}
**byte**
* 1 byte = 8 bits
* 存储有符号的、以二进制补码表示的整数。
* 用在大型数组中可以显著节约空间，主要代替小整数，因为`byte`变量占用的空间只有`int`的 1/4。
* 在二进制文件读写中使用较多。
{{% /admonition %}}

```Java
public class IntegerTest {
   public static void main(String[] args){
      short a1 = 32767;
      System.out.println(a1);
      //short a2 = 32768; error 越界

      int b1 = 2147483647;
      System.out.println(b1);
      //int b2 = 32768; error 越界

      long c1 = 1000000000000L;
      System.out.println(c1);

      long c2 = 2147483647; //隐式做了从int变成long的操作
      System.out.println(c2);

      long c3 = 2147483648L; //去掉L将报错
      System.out.println(c3);
   }
}

```


### 3.2 浮点类型

浮点类型用于表示有小数部分的数值。在 `Java` 中有两种浮点类型，`float`（单精度）、`double`（双精度）。

|类型|存储需求|取值范围|表示|默认值|
|----|----|----|----|----|
|`float`|4字节|大约 ± 3.40282347E+38F (有效位数为 6 ~ 7 位）|后缀 F 或 f|0.0f|
|`double`|8字节|大约 ± 1.79769313486231570E+308 (有效位数为15位）|无后缀或添加后缀 D 或 d|0.0d|

{{% admonition note "笔记" %}}
* `float`、`double`均符合 `IEEE 754` 标准。
* `float`、`double`均不能用来表示很精确的数字。
{{% /admonition %}}

```Java
public class Floatingtest{
   public static void main(String[] args){
      float f1 = 1.23f;
      //float f2 = 1.23;  error, float赋值必须带f
      double d1 = 4.56d;
      double d2 = 4.56; //double 可以省略末尾d

      System.out.println(f1);
      System.out.println((double) f1);
      System.out.println(d1);
      System.out.println((float) d2);


      System.out.println(f1 == 1.229999999f); //true
      System.out.println(f1 - 1.229999999f); //0.0
      System.out.println(d2 == 4.559999999999999999d);//true
      System.out.println(d2 - 4.559999999999999999d);//0.0
   }

```
### 3.3 char 型

`char`类型是一个单一的 16 位 `Unicode`字符,最小值是 `\u0000`（即为 0）,最大值是 `\uffff`（即为65535）,`char` 数据类型可以储存任何字符。

```Java
public class Floatingtest{
   public static void main(String[] args){
      char a = 'a';
      char b = 97;  //根据 ascii 码转化为 a
      char c = '我';
      char d = '\u4e00'; //“一”字  \u4e00 ~ \u9fa5 两万多汉字
      System.out.println(a);
      System.out.println(b);
      System.out.println(c);
      System.out.println(d);
```
### 3.4 boolean 类型

* `boolean` (布尔）类型有两个值：`false`（默认值） 和 `true`, 用来判定逻辑条件。
* 整型值和布尔值之间不能进行相互转换。

## 4 变量

在 Java 中，每个变量都有一个类型（ type)。在声明变量时，变量的类型位于变量名之前。

Java语言支持的变量类型有：

* 类变量：独立于方法之外的变量，用 `static` 修饰。
* 实例变量：独立于方法之外的变量，不过没有 `static` 修饰。
* 局部变量：类的方法中的变量。

### 4.1 变量初始化

声明一个变量之后，必须用赋值语句对变量进行显式初始化， 千万不要使用未初始化的变量。

在 Java 中， 变量的声明尽可能地靠近变量第一次使用的地方， 这是一种良好的程序编写风格。

### 4.2 常量

在 Java 中， 利用关键字 `final` 指示常量。
关键字 `final` 表示这个变量只能被赋值一次。一旦被赋值之后，就不能够再更改了。习惯上,常量名使用**全大写**。

## 5 运算符

* 算术运算符 `+`、 `-`、 `*`、 `/`、`%`
* 逻辑运算符 `&&`、 `||` 、`！`
* 比较运算符 `!=`、 `>`、 `>=`、`<`、`<=`、`==`
* 移位运算符 `>>`、 `<<`

{{% admonition warning "警告" %}}
* 当参与 `/` 运算的两个操作数都是整数时，表示整数除法；否则，表示浮点除法。 
* 整数被 `0` 除将会产生一个异常， 而浮点数被 `0` 除将会得到无穷大或 `NaN` 结果。
{{% /admonition %}}

### 5.1 数值类型之间的转换


### 5.2 强制类型转换

强制类型转换的语法格式是在圆括号中给出想要转换的目标类型，后面紧跟待转换的变量名。例如：
```Java
/*直接进行强制类型转换*/
double x * 9.997;
int nx = (int) x;	//变量 nx 的值为 9。强制类型转换通过截断小数部分将浮点值转换为整型

/*使用 Math_ round 方法对浮点数进行舍入运算，得到最接近的整数*/
double x z 9.997;
int nx = (int) Math.round(x);	//变量 nx 的值为 9。当调用 round 的时候返回的结果为 long 型,需要使用强制类型转换。

```

{{% admonition warning "警告" %}}
如果试图将一个数值从一种类型强制转换为另一种类型， 而又超出了目标类型的表示范围，结果就会截断成一个完全不同的值。例如，`(byte)300` 的实际值为 44。
{{% /admonition %}}

### 5.3 结合赋值和运算符

可以在赋值中使用二元运算符。

```Java
x += 4;
//等价于
x = x + 4;
```

### 5.4 自增与自减运算符

* 自增运算符：`++`
* 自减运算符：`--`
* 前缀自增自减法(++a,--a): 先进行自增或者自减运算，再进行表达式运算。
* 后缀自增自减法(a++,a--): 先进行表达式运算，再进行自增或者自减运算。
```Java
int m = 7;
int n = 7;
int a = 2 * ++m; // now a is 16, m is 8
int b = 2 * n++; // now b is 14, n is 8
/* 建议不要在表达式中使用 ++, 因为这样的代码很容易让人闲惑，而且会带来烦人的 bug。 */
```

### 5.5 关系和 boolean 运算符

* 相等：`==`
* 不等：`!=`
* 小于：`<`
* 大于：`>`
* 小于等于：`<=`
* 大于等于：`>=`
* 逻辑“与”：`&&`
* 逻辑“或”：`||`
* 逻辑“非”：`!`
* 三元运算符：`?:`

```Java
/* && 和 || 运算符是按照“短路”方式来求值的：如果第一个操作数已经能够确定表达式的值，第二个操作数就不必计算了。*/
x != 0 && 1 / x > x + y // no division by 0 ，如果 x 等于 0, 那么第二部分就不会计算。

x < y ? x : y //会返回 x 和 y 中较小的一个
```

### 5.6 位运算符

Java 定义了位运算符，应用于整数类型(int)，长整型(long)，短整型(short)，字符型(char)，和字节型(byte)等类型。位运算符作用在所有的位上，并且按位运算。
下表列出了位运算符的基本运算，假设整数变量 A 的值为 60 和变量 B 的值为 13：

|操作符|描述|例子|
|----|----|----|
|`＆` |如果相对应位都是1，则结果为1，否则为0	|`(A＆B)`，得到12，即 0000 1100|
|&#124;|如果相对应位都是 0，则结果为 0，否则为 1|	`（A | B）`得到61，即 0011 1101|
|`^`	|如果相对应位值相同，则结果为0，否则为1	|`（A ^ B）`得到49，即 0011 0001|
|`〜`	|按位取反运算符翻转操作数的每一位，即0变成1，1变成0。|	`(〜A)`得到-61，即 1100 0011|
|`<<` |按位左移运算符。左操作数按位左移右操作数指定的位数。|	`A << 2`得到240，即 1111 0000|
|`>>` |按位右移运算符。左操作数按位右移右操作数指定的位数。|	`A >> 2`得到15，即 1111|
|`>>>`|按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。|	`A>>>2`得到15，即 0000 1111|

### 5.7 括号与运算符级别

如果不使用圆括号，就按照给出的运算符优先级次序进 行计算。同一个级别的运算符按照从左到右的次序进行计算 （除了表中给出的右结合运算符外）。

|运算符|结合性|
|----|----|
|`[].()`(方法调用)|从左向右|
|`!` `~` `++` `--` `+`(一元运算) `-`(一元运算) `()`(强制类型转换) `new`|从右向左|
|`*` `/` `%`|从左向右|
|`+` `-`|从左向右|
|`<<` `>>` `>>>`|从左向右|
|`<` `<=` `>` `>=` `instanccof`| 从左向右|
| `==` `!=` |从左向右|
| `&` |从左向右|
| `^` |从左向右|
| &#124; | 从左向右|
| `&&` |从左向右|
| &#124;&#124; | 从左向右|
| `?:`| 从右向左|
| `=` `+=` `-=` `*=` `/=` `%=` `&=` `|=` `^=` `<<=` `>>=` `>>>=`|从右向左|

### 5.8 枚举类型

（后面补充）

## 6 字符串

Java 没有内置的字符串类型，而是在标准 Java 类库中提供了一个预定义类，很自然地叫做 String。 每个用双引号括起来的字符串都是 String 类的一个实例：

```Java
String e = ""; // an empty string 
String greeting = "Hello";
```

### 6.1 子串

String 类的 substring 方法可以从一个较大的字符串提取出一个子串。 例如：

```Java
String s = greeting.substring(0, 3); 
//第一个参数是开始位置，第二个参数是不想复制的第一个位置
```
创建了一个由字符“ Hel ” 组成的字符串。

### 6.2 拼接

Java 语言使用 `+` 号连接（拼接）两个字符串。

```Java
String expletive = "Expletive";
String PC13 = "deleted";
String message = expletive + PC13;
```

当将一个字符串与一个非字符串的值进行拼接时，后者被转换成字符串。

```Java
int age = 13;
String rating = "PC" + age;
```

如果需要把多个字符串放在一起， 用一个定界符分隔，可以使用静态 join 方法：

```Java
String all = String.join(" / ", "S", "M","L", "XL");
// all is the string "S / H / L / XL"
```

### 6.3 不可变字符串

### 6.4 检测字符串是否相等

使用equals方法检测两个字符串是否相等。

```Java
s.euqals(t)  //相等返回true,否则返回false
"Hello".equals(greeting);
```

使用equalsIgnoreCore方法检测不区分大小写的情况下，字符串是否相等。

```Java
"Hello".equalsIgnoreCase("hel1o")
```
{{% admonition warning "警告" %}}
一定不要使用`==`运算符检测两个字符串是否相等！ 这个运算符只能够确定两个字串是否放置在同一个位置上。当然，如果字符串放置在同一个位置上， 它们必然相等。但是，完全有可能将内容相同的多个字符串的拷贝放置在不同的位置上。

如果虚拟机始终将相同的字符串共享， 就可以使用=运算符检测是否相等。但实际上只有字符串常量是共享的，而 + 或 substring 等操作产生的结果并不是共享的。
```Java
String greeting = "Hello"; //initialize greeting to a string
if (greeting == "Hello"){
	...
}
// probably true
if (greeting.substring(0, 3) == "HeV"){
	...
}
// probably false
```
{{% /admonition %}}

### 6.5 空串与 NUll 串

空串 `""` 是一个 Java 对象，有自己的串长度（0）和内容（空）。可以调用以下代码检查一个字符串是否为空：

```Java
if (str.lengthQ = 0){
	...
}

if (str.equals("")){
	...
}
```
String 变量还可以存放一个特殊的值，名为 `null`, 这表示目前没有任何对象与该变量关联。要检查一个字符串是否为 `null`, 要使用以下条件：

```Java
if (str == null){
	...
}
```

有时要检查一个字符串既不是 `null` 也不为空串，这种情况下就需要使用以下条件：

```Java
if (str != null && str.length() != 0){
	...
}
```

### 6.6 码点与代码单元

Java 字符串由 `char` 值序列组成。 `char` 数据类型是一个采用 `UTF-16 编码`表示 `Unicode 码点`的代码单元。大多数的常用 `Unicode` 字符使用一个代码单元就可以表示，而辅助字符需要一对代码单元表示。
`length` 方法将返回采用 UTF-16 编码表示的给定字符串所需要的代码单元数量。例如：

```Java
String greeting = "Hello";
int n = greeting.length。; // is 5 .
```

要想得到实际的长度，即码点数量，可以调用：

```Java
int cpCount = greeting.codePointCount(0, greeting.lengthQ);
```
调用 `s.charAt(n)` 将返回位置 `n` 的代码单元，`n` 介于 `0 ~ s.length()-l` 之间。例如：

```Java
char first = greeting.charAtO); // first is 'H'
char last = greeting.charAt(4); // last is 'o'
```

要想得到第 i 个码点，应该使用下列语句

```Java
int index = greeting.offsetByCodePoints(0, i);
int cp = greeting.codePointAt(index);
```

### 6.7 String API

方法摘要：
|类型|方法|描述|
|----|------|----|
|char|`charAt (int index)`|返回指定索引处的 `char` 值|
|int |`codePointAt(int Index)`|返回从给定位置开始的码点|
|int| `offsetByCodePoints(int startlndex, int cpCount)`|返回从 `startlndex` 代码点开始，位移 `cpCount` 后的码点索引|
|int| `compareTo(String other)`|按照字典顺序，如果字符串位于 `other` 之前， 返回一个负数；如果字符串位于 `other` 之后，返回一个正数；如果两个字符串相等，返回 `0`|
|IntStream|` codePoints()`|将这个字符串的码点作为一个流返回。调用 `toArray` 将它们放在一个数组中|
|boolean| `equals(0bject other)`|如果字符串与 `other` 相等， 返回 `true`|
|boolean| `equalsIgnoreCase(String other)`|如果字符串与 `other` 相等（忽略大小写）返回 `true`|
|boolean| `startsWith(String prefix)`|如果字符串以 `prefix` 开头，则返回 `true`|
boolean| `endsWith(String suffix)`|如果字符串以 `suffix` 结尾，则返回 `true`|
|int| `indexOf(String str)`|返回与字符串`str`  匹配的第一个子串的开始位置。|
|int| `indexOf(String str, int fromIndex )`|返回与字符串 `str`  匹配的第一个指定字符的开始位置，从 `fromIndex` 开始搜索，如果在原始串中不存在`str`，返回 `-1`|
|int|`indexOf(int cp)`|返回与代码点 `cp` 匹配的第一个子串的开始位置|
|int|`indexOf(int cp, int fromIndex)`|返回此字符串 匹配的第一个子串的开始位置，从 `fromIndex`开始搜索，如果在原始串中不存在 `str`，返回 `-1`|
| int |`lastIndexOf(String str)`|返回与字符串 `str`匹配的最后一个子串的开始位置，从原始串尾端开始计算|
| int |`lastIndexOf(String str, int fromIndex )`|返回与字符串 `str` 匹配的最后一个子串的开始位置，从 `fromIndex` 开始计算|
| int |`lastindexOf(int cp)`|返回与代码点 `cp` 匹配的最后一个子串的开始位置，从原始串尾端开始计算|
| int |`lastindexOf(int cp, int fromIndex )`|返回与代码点 `cp` 匹配的最后一个子串的开始位置，从 `fromIndex` 开始计算|
|int| `length()`|返回字符串的长度|
|int| `codePointCount(int startlndex , int endlndex)`|返回 `startlndex` 和 `endludex-1`之间的代码点数量。没有配成对的代用字符将计入代码点|
|String| `replace( CharSequence oldString,CharSequence newString)`|返回一个新字符串。这个字符串用 `newString` 代替原始字符串中所有的 `oldString`。可以用 `String` 或 `StringBuilder` 对象作为 `CharSequence` 参数|
|String| `substring(int beginlndex)`|返回一个新字符串。这个字符串包含原始字符串中从 `beginlndex`到串尾的所有代码单元|
|String| `substring(int beginlndex, int endlndex)`|返回一个新字符串。这个字符串包含原始字符串中从 `beginlndex` 到 `endlndex-l`的所有代码单元|
|String|`toLowerCase( )`|返回一个新字符串。这个字符串将原始字符串中的大写字母改为小写|
|String|`toUpperCase( )`|返回一个新字符串。这个字符串将原始字符串中的小写字母改为大写|
|String| `trim( )`|返回一个新字符串。这个字符串将删除了原始字符串头部和尾部的空格|
|String|`join(CharSequence delimiter, CharSequence ... elements)`|返回一个新字符串，用给定的定界符连接所有元素|


### 6.8 构建字符串

如果需要用许多小段的字符串构建一个字符串， 那么应该按照下列步骤进行。 首先，构建一个空的字符串构建器：

```Java
StringBuilder builder = new StringBuilder();
```
当每次需要添加一部分内容时，就调用 append 方法。

```Java
builder.append(ch); // appends a single character
bui1der.append(str); // appends a string
```
在需要构建字符串时就凋用 toString 方法，将可以得到一个 String 对象，其中包含了构建器中的字符序列。

```Java
String completedString = builder.toString();
```
StringBuilder 类中的重要方法：
|方法|描述|
|----|----|
|`StringBuilder()`|构造一个空的字符串构建器|
|`int length()`|返回构建器或缓冲器中的代码单元数量|
|`StringBuilder append(String str)`|追加一个字符串并返回 `this`|
|`StringBuilder append(char c)`|追加一个代码单元并返回 `this`|
|`StringBuilder appendCodePoint(int cp)`|追加一个代码点，并将其转换为一个或两个代码单元并返回 `this`|
|`void setCharAt(int i ,char c)`|将第 `i` 个代码单元设置为 `c`|
|`StringBuilder insert(int offset,String str)`|在 `offset` 位置插入一个字符串并返回 `this`|
|`StringBuilder insert(int offset,char c)`|在 `offset` 位置插入一个代码单元并返回 `this`|
|`StringBuilder delete(int startindex,int endlndex)`|删除偏移量从 `startindex` 到 `-endlndex-1` 的代码单元并返回 `this`|
|`String toString()`|返回一个与构建器或缓冲器内容相同的字符串|

## 7 输入输出

### 7.1 读取输入

通过控制台进行输人，首先需要构造一个 Scanner 对象，并与“标准输人流” `System.in` 关联。`Scanner`类定义在`java.util` 包中，使用时需要先通过`import`引入`java.util`包。 

```Java
import java.util.*; 

public class InputTest
{
	public static void main(String[] args)
	{
		Scanner in = new Scanner(Systera.in);

		// nextLine 输入一行
		System.out.print("What is your name? ");
		String name = in.nextLine();

		// nextInt 读取一个整数
		System.out.print("How old are you? ")；
		int age = in.nextInt();

		// 控制台输出
		System.out.println("Hello," + name + ".Next year, you'll be " + (age + 1));
	}
}
```
#### java.util.Scanner 5.0

|类型|方法|描述|
|----|----|----|
||`Scanner (InputStream in)`|用给定的输入流创建一个 Scanner 对象|
|String| `nextLine( )`|读取输入的下一行内容|
|String| `next( )`|读取输入的下一个单词（以空格作为分隔符）|
|int| `nextInt( )`|读取并转换下一个表示整数的字符序列|
|double| `nextDouble( )`|读取并转换下一个表示浮点数的字符序列|
|boolean| `hasNext( )`|检测输入中是否还有其他单词|
|boolean| `hasNextInt( )`|检测是否还有表示整数的下一个字符序列|
|boolean| `hasNextDouble( )`|检测是否还有表示整数或浮点数的下一个字符序列|

#### java.lang.System 1.0

* `static Console console()`


	如果有可能进行交互操作，就通过控制台窗口为交互的用户返回一个 Console 对象，否则返回 null。对于任何一个通过控制台窗口启动的程序，都可使用 Console 对象。否则，其可用性将与所使用的系统有关。

#### java.io.Console

* `static char[] readPassword(String prompt, Object...args)`
* `static String readLine(String prompt, Object...args)`


	显示字符串 `prompt` 并且读取用户输入，直到输入行结束。args 参数可以用来提供输人格式。

### 7.2 格式化输出

Java SE 5.0 沿用了 C语言库函数中的 printf 方法。

```Java
double x = 10000.0/3.0;
System.out.printf("%8.2f", x);//用 8 个字符的宽度和小数点后两个字符的精度打印
//输出结果3333.33
```

每一个以 ％ 字符开始的格式说明符都用相应的参数替换。 格式说明符尾部的转换符将指示被格式化的数值类型。
|转换符|类型|举例|
|----|----|----|
|d|十进制整数|159|
|x|十六进制整数|9f|
|o|八进制数|237|
|f|定点浮点数|15.9|
|e|指数浮点数|1.59e+01|
|g|通用浮点数|——|
|a|十六进制浮点数|0x1.fccdp3|
|s|字符串|Hello|
|c|字符|H|
|b|布尔|True|
|h|散列码|4268b2|
|tx 或 Tx|日期时间（T强制大写）|已经过时，应当改为使用 javaJime 类|
|%|百分号|%|
|n|与平台有关的行分隔符|一|

还可以给出控制格式化输出的各种标志，如下表所示：
|标志|目的|举例|
|----|----|----|
|+|打印正数和负数的符号| +3333.33|
|空格|在正数之前添加空格| 3333.33| 
|0|数字前面补 0| 003333.33|
|-|左对齐 |13333.33|
|(|将负数括在括号内|( 3333.33 )|
|,|添加分组分隔符| 3,333.33 
|#（对于 f 格式）|包含小数点| 3,333. 
|#（对于 x 或 0 格式）| 添加前缀 0x 或 0| 0xcafe
|$| 给定被格式化的参数索引。例如，`％1$d`， `％1$x` 将以十进制和十六进制格式打印第一个参数|159 9F|
|< |格式化前面说明的数值。 例如，`％d%<X` 以十进制和十六进制打印同一个数值|159 9F|

### 7.3 文件的输入输出

对文件进行读取需要用File对像构造一个Scanner对象，如下所示：

```Java
Scanner in = new Scanner(paths.get("myfile.txt")，"UTF-8");
```
{{% admonition warning "警告" %}}
可以构造一个带有字符串参数的 Scanner, 但这个 Scanner 将字符串解释为数据，而不是文件名。例如，如果调用:
```Java
Scanner in = new Scanner("myfile.txt"); // ERROR?
```
这个 scanner 会将参数作为包含 10 个字符的数据：‘ m ’,‘ y ’,‘ f ’ 等。
{{% /admonition %}}

如果文件名中包含反斜杠符号，就要记住在每个反斜杠之前再加一个额外的反斜杠："`c:\\mydirectory\\myfile.txt`" 。

对文件进行写入就需要构造一个PrintWriter对象。在构造器中，只需要提供文件名，如果文件不存在，创建该文件。

```Java
PrintWriter out = new PrintWriter("myfile.txt","UTF-8");  
```

如果用一个不存在的文件构造一个 Scanner, 或者用一个不能被创建的文件名构造一个 PrintWriter,那么就会发生异常。Java 编译器认为这些异常比“被零除” 异常更严重。应该告知编译器： 已经知道有可能出现“ 输入 / 输出” 异常。这需要在 main 方法中用 throws 子句标记，如下所示：

```Java
public static void main(String[] args) throws IOException
{
	Scanner in = new Scanner(Paths.get("myfi1e.txt"), "UTF-8");
}
```
#### java.util.Scanner 5.0
* `Scanner(File f)`


	构造一个从给定文件读取数据的 Scanner。
* `Scanner(String data)`


	构造一个从给定字符串读取数据的 Scanner。

#### java.io.PrintWriter 1.1

* `PrintWriter(String fileName)`
	

	构造一个将数据写入文件的 PrintWriter。文件名由参数指定。

#### java.nio.file.Paths 7
* `static Path get(String pathname)`
	

	根据给定的路径名构造一个 Path。

## 8 控制流程

### 8.1 条件语句

#### if 语句

```Java
if(布尔表达式)
{
   //如果布尔表达式为true将执行的语句
}
```

#### if...else

```Java
if(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false
}
```

#### if...else if...else

```Java
if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
}else if(布尔表达式 2){
   //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
   //如果布尔表达式 3的值为true执行代码
}else {
   //如果以上布尔表达式都不为true执行代码
}
```

#### 嵌套的 if...else

```Java
if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
   if(布尔表达式 2){
      //如果(布尔表达式 1的值为true)&&(布尔表达式 2的值为true)执行代码
   }
}
```

### 8.3 循环

#### while 循环

```Java
//不满足条件，则不能进入循环
while( 布尔表达式 ) {
  //循环内容
}
```

#### do...while 循环

```Java
//不满足条件，也至少执行一次
do {
    //代码语句
}while(布尔表达式);
```

#### for 循环

```Java
for(初始化; 布尔表达式; 更新) {
    //代码语句
}
```

#### 增强 for 循环

Java5 引入了一种主要用于数组的增强型 for 循环。
* 声明语句：声明新的局部变量，该变量的类型必须和数组元素的类型匹配。其作用域限定在循环语句块，其值与此时数组元素的值相等。

* 表达式：表达式是要访问的数组名，或者是返回值为数组的方法。

```Java
for(声明语句 : 表达式)
{
   //代码句子
}

/**
 * Test.java
 */

public class Test {
   public static void main(String args[]){
      int [] numbers = {10, 20, 30, 40, 50};
 
      for(int x : numbers ){
         System.out.print( x );
         System.out.print(",");
      }
      System.out.print("\n");
      String [] names ={"James", "Larry", "Tom", "Lacy"};
      for( String name : names ) {
         System.out.print( name );
         System.out.print(",");
      }
   }
}

/**
 *运行结果
 *10,20,30,40,50,
 *James,Larry,Tom,Lacy,
 */
```

#### switch case 语句

* switch 语句中的变量类型可以是： byte、short、int 或者 char。从 Java SE 7 开始，switch 支持字符串 String 类型，同时 case 标签必须为字符串常量或字面量。

* switch 语句可以拥有多个 case 语句。每个 case 后面跟一个要比较的值和冒号。

* case 语句中的值的数据类型必须与变量的数据类型相同，而且只能是常量或者字面常量。

* 当变量的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句。

* 当遇到 break 语句时，switch 语句终止。程序跳转到 switch 语句后面的语句执行。case 语句不必须要包含 break 语句。如果没有 break 语句出现，程序会继续执行下一条 case 语句，直到出现 break 语句。

* switch 语句可以包含一个 default 分支，该分支一般是 switch 语句的最后一个分支（可以在任何位置，但建议在最后一个）。default 在没有 case 语句的值和变量值相等的时候执行。default 分支不需要 break 语句。

```Java
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
```

### 8.4 中断控制流程语句

#### `break` 关键字

* `break` 主要用在循环语句或者 `switch` 语句中，用来跳出整个语句块。

* `break` 跳出最里层的循环，并且继续执行该循环下面的语句。

#### `continue` 关键字
* `continue` 适用于任何循环控制结构中。让程序立刻跳转到下一次循环的迭代。

* 在 `for` 循环中，`continue` 语句使程序跳过本次循环。

* 在 `while` 或者 `do…while` 循环中，程序立即跳转到布尔表达式的判断语句。

## 9 大数值

如果基本的整数和浮点数精度不能够满足需求， 那么可以使用java.math 包中的两个
很有用的类：`Biglnteger` 和 `BigDecimal` 这两个类可以处理包含任意长度数字序列的数值。
`Biglnteger` 类实现了任意精度的整数运算，`BigDecimal` 实现了任意精度的浮点数运算。
使用静态的 `valueOf` 方法可以将普通的数值转换为大数值：

```Java
BigInteger a = BigInteger.valueOf(100);
```

处理大数值不能使用算术运算符（如`：`+ 和 `*`），需要使用大数值类中的 add 和 multiply 方法。

```Java
BigInteger c = a.add(b); // c = a + b
BigInteger d = c.multiply(b.add(BigInteger.valueOf(2))); // d = c * (b + 2)
```

## 10 数组

数组是一种数据结构，用来存储同一类型值的集合。通过一个整型下标可以访问数组中的每一个值。例如，如果 a 是一个整型数组， a[i] 就是数组中下标为 i 的整数。

### 10.1 声明数组

在声明数组变量时，需要指出数组类型 （ 数据元素类型紧跟 []）和数组变量的名字。下面是声明数组变量的语法：

```Java
dataType[] arrayRefVar;   // 首选的方法


dataType arrayRefVar[];  // 效果相同，但不是首选方法
```

### 10.2 创建数组

Java语言使用new操作符来创建数组，语法如下：

```Java
/**
 * 使用 dataType[arraySize] 创建了一个数组。
 * 把新创建的数组的引用赋值给变量 arrayRefVar。
 */
arrayRefVar = new dataType[arraySize];
```

数组变量的声明，和创建数组可以用一条语句完成，如下所示：

```Java
dataType[] arrayRefVar = new dataType[arraySize];

dataType[] arrayRefVar = {value0, value1, ..., valuek};
```
数组的元素是通过索引访问的。数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1。

### 10.3 处理数组

数组的元素类型和数组的大小都是确定的，所以当处理数组元素时候，我们通常使用基本循环或者 For-Each 循环。

```Java
public class TestArray {
   public static void main(String[] args) {
      double[] myList = {1.9, 2.9, 3.4, 3.5};
 
      // 打印所有数组元素
      for (int i = 0; i < myList.length; i++) {
         System.out.println(myList[i] + " ");
      }
      // 计算所有元素的总和
      double total = 0;
      for (int i = 0; i < myList.length; i++) {
         total += myList[i];
      }
      System.out.println("Total is " + total);
      // 查找最大元素
      double max = myList[0];
      for (int i = 1; i < myList.length; i++) {
         if (myList[i] > max) max = myList[i];
      }
      System.out.println("Max is " + max);
   }
}

//运行结果如下：
1.9
2.9
3.4
3.5
Total is 11.7
Max is 3.5
```

JDK 1.5 引进了一种新的循环类型，被称为 For-Each 循环或者加强型循环，它能在不使用下标的情况下遍历数组。

语法格式如下：
```Java
for(type element: array)
{
    System.out.println(element);
}

public class TestArray {
   public static void main(String[] args) {
      double[] myList = {1.9, 2.9, 3.4, 3.5};
 
      // 打印所有数组元素
      for (double element: myList) {
         System.out.println(element);
      }
   }
}

//运行结果如下：
1.9
2.9
3.4
3.5

```

### 10.4 多维数组

多维数组可以看成是数组的数组，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组，例如：

```Java
String str[][] = new String[3][4];
```
#### 10.4.1 多维数组的动态初始化（以二维数组为例）
* 直接为每一维分配空间，格式如下：

```Java
type[][] typeName = new type[typeLength1][typeLength2];
```

type 可以为基本数据类型和复合数据类型，arraylength1 和 arraylength2 必须为正整数，arraylength1 为行数，arraylength2 为列数。
例如：

```Java
int a[][] = new int[2][3];
```

解析：

二维数组 a 可以看成一个两行三列的数组。

* 从最高维开始，分别为每一维分配空间，例如：

```Java
String s[][] = new String[2][];
s[0] = new String[2];
s[1] = new String[3];
s[0][0] = new String("Good");
s[0][1] = new String("Luck");
s[1][0] = new String("to");
s[1][1] = new String("you");
s[1][2] = new String("!");
```
解析：

`s[0]=new String[2]` 和 `s[1]=new String[3]` 是为最高维分配引用空间，也就是为最高维限制其能保存数据的最长的长度，然后再为其每个数组元素单独分配空间 `s0=new String("Good")` 等操作。

#### 10.4.2 多维数组的引用（以二维数组为例）

对二维数组中的每个元素，引用方式为 arrayName[index1][index2]，例如：

```Java
num[1][0];
```

### 10.5 Arrays 类

java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。

具有以下功能：

* 给数组赋值：通过 fill 方法。
* 对数组排序：通过 sort 方法,按升序。
* 比较数组：通过 equals 方法比较数组中元素值是否相等。
* 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

具体说明请查看下表：

|序号|方法|说明|
|----|----|----|
|1| `public static int binarySearch(Object[] a, Object key)`|用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1) |
|2| `public static boolean equals(long[] a, long[] a2)`|如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）|
|3| `public static void fill(int[] a, int val)`|将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）|
|4| `public static void sort(Object[] a)`|对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）|
