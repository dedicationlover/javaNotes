# Java基础知识

## 1. Java语言介绍

### 1.1 java的核心优势：跨平台

跨平台的原因：解释性语言。（Java是既编译又解释的）

**编译型语言**：把做好的源程序全部编译成二进制代码的可运行程序，然后，可直接运行这个程序。

**解释型语言**：把做好的源程序翻译一句，然后执行一句，直至结束。

| 语言类型   | 效率               | 跨平台                 |
| ---------- | ------------------ | ---------------------- |
| 编译型语言 | 执行速度快、效率高 | 依靠编译器、跨平台性差 |
| 解释型语言 | 执行速度慢、效率低 | 依靠解释器、跨平台性好 |

**为什么解释性语言跨平台性好？**

​	解释性语言依赖于解释器，没有解释器就无法执行，而解释器是存在于各自平台上的，比如说JavaScript是依赖于浏览器的解释执行，Java依赖于各个平台上的JVM。而编译型语言之所以跨平台性差，是因为源码通过编译器编译，将内存地址等已经关联，但是不同平台下的寻址方式可能不同，所以在Windows下编译好的二进制文件或.exe文件在Linux下就不能正常运行。

​	Java需要编译成.class文件（class文件需满足JVM的规定），但不能直接执行，解释运行在JVM上。

### 1.2 write once, run anywhere(WORA)的理解？

​	焦点在源文件上，原本C语言是直接调用操作系统API，而不同操作系统上的API是不同的，为了C源文件能够在不同的平台上运行，所以需要对源文件修改。而Java将一切和操作系统有关的调用都封装到JVM中，所以根据不同的操作系统写了不同的JVM，Java源文件就不需要修改。

​	JVM这种机制也是抽象和封装的体现，从机器码01到汇编，从汇编到c，从c到Java，层次不断提高，越来越像人类语言，封装进的东西也越来越多，硬件、内存、操作系统，但是效率存在一定的损失。

### 1.3 Java的编译与解释

​	Java的编译过程：Java源代码 --> .class字节码（第一次编译）-->  目标机器代码（JVM执行第二次编译或解释）

​	.class文件经JVM解析或编译运行：

  - 解析：.class文件经过JVM内嵌的解析器解析执行
  - 编译：存在JIT编译器（just in time compile,即时编译器）把经常运行的代码作为“热点代码”编译与本地平台相关的机器码，并进行各种层次的优化
  - AOT编译器（Ahead-of-Time Compilation, 一种新的编译方式，直接将字节码编译成机器代码，避免了JIT预热等各方面的开销）

**优化**：若JVM加载字节码后，完全进行解释执行，这势必会影响执行效率。所以，对于这个运行环节，JVM会进行一些优化处理，如JIT技术，将某些运行特别频繁的代码编译成机器码。**AOT**技术，是在运行前，通过工具直接将字节码转换为机器码。

**JIT**：解释执行速度相对慢，有些方法和代码块是高频调用的（小部分的热点耗费了大多数资源），既热点代码，所以引入JIT技术，提前将这类字节码直接编译成本地机器码（运行时编译）。类似于缓存技术，运行时再遇到这类代码直接就可以执行，而不是先解释后执行。**JIT是运行时才做的，需要预热才知道哪些是热点；AOT是编译器，静态的，直接编译成类似类库的东西。**

**区别：**写个程序直接执行字节码就是解释执行；写个程序运行时把字节码动态翻译成机器码就是JIT；写个程序把Java源代码直接翻译成机器码就是AOT；造个CPU直接执行字节码，字节码就是机器码。

​	在运行时，JVM会通过类加载器加载字节码，解释或者编译执行。主流Java版本中，如JDK8实际是解释和编译混合的一种模式，即混合模式。

​	JVM启动时，可指定不同参数对运行模式进行选择：

​	**-Xint**：只进行解释执行，不对代码进行编译（抛弃了JIT可能带来的性能优势）

​	**-Xcomp**：JVM关闭解释器，不进行解释执行，叫做最大优化级别，但未必最高效（JVM启动变慢非常多，同时有些JIT编译器优化方式不能进行有效优化）

​	**-Xmixed**：混合模式

JVM作为一个强大的平台，不仅仅只有Java语言可以运行在JVM上，本质上合规的字节码都可以运行，类似Clojure, Scala, Groovy, JRuby, Jython等。

### 1.4 J2EE, J2SE, J2ME

| 名称 | 解释       | 定位           |
| ---- | ---------- | -------------- |
| J2EE | Java企业版 | 服务器端       |
| J2SE | Java标准版 | 个人计算机     |
| J2ME | Java微型版 | 消费性电子产品 |

### 1.5 JDK, JRE, JVM

| 缩写 | 全拼                                     | 解释                                                         |
| ---- | ---------------------------------------- | ------------------------------------------------------------ |
| JDK  | java development kit，java开发工具包     | 整个Java的核心，JRE+Java的开发工具                           |
| JRE  | java runtime environment，Java运行时环境 | JAVA程序所必须的环境的集合，JVM+Java语言的核心类库           |
| JVM  | java virtual machine，Java虚拟机         | java实现跨平台的最核心的部分，将字节码文件转成具体系统平台的机器指令 |

三者关系

![查看源图像](https://img.javatt.com/0d/0d3a8b77d989255be50aa3f55c1051dc.png)

### 1.6 系统不同，JVM版本不同--跨平台

**JVM是一种规范**。就是一个虚拟的用于执行字节码的计算机。可用软件实现（IBM, SUN, BEA等），也可用硬件实现（sun/intel研发的Java芯片）。

JVM是一个抽象的计算机，和实际的计算机一样，它具有指令集并使用不同的存储区域。它负责执行指令，还要管理数据、内存和寄存器。SUN公司指定的JVM规范：①指令集 ②寄存器 ③ 类文件的格式（.class）④ 栈 ⑤垃圾回收堆  ⑥ 存储区

补充《core java》第13章 部署Java程序

​	JAR文件是压缩的，使用了zip压缩格式。

pack200 是一种较通常的 ZIP 压缩算法更加有效的压缩类文件的方式。Oracle 声 称， 对类文件的压缩率接近 90%。有关更加详细的信息请参看网站 http://doc.oracle.com/javase/1.5.0/docs/guide/deployment/deployment-guide/pack200.htmlo

## 2. 基本使用

### 2.1 Java文件名为什么要和public类名一致？

Java文件名必须和public类名一致，一个文件可以有多个class类，但只能有一个public class，编译会生成多个.class文件，并注意代码格式。若没有public类则文件名和类名无所谓。

**规定：编译器决定**

> 当编写一个java源代码文件时，此文件通常被称为编译单元（有时也被称为转译单元），每个编译单元都必须有一个后缀名.java，而在编译单元内则可以有一个public类，该类的名称必须与文件的名称相同（包括大小写，但不包括文件的后缀名.java）。每个编译单元只能有一个public类，否则编译器就不会接受。如果在该编译单元之中还有额外的类的话，那么在包之外的世界是无法看见这些类的，这是因为它们不是public类，而且它们主要用来为主public类提供支持。
>
> 每个编译单元（文件）只能有一个public类，意思是，每个编译单元只能有一个公开接口，而这个接口就由其public类来表示。（软件架构设计和安全性设计上得出的结论，或者说Java的设计者们从这方面的考虑，或者是一个规范？）
>
> [参考](www.cnblogs.com/fuhongliang/p/4304477.html)

**优点**

- 防止*.class*文件覆盖，*A.java*下*public class B*，若不记住这种关系，则可能再写出*B.java*下*public class B*，编译后只会留有一份*B.class*
- 被项目中其他任何类引用时，只需在使用它前*import*所对应的class文件即可，将类名与文件名一一对应就可以方便虚拟机在相应的路径（包）中找到相应的类的信息，否则很难找到需要的类，开销也很大。

### 2.2 第一个Java程序的总结和疑问

**总结**

- Java对大小写敏感
- main方法为应用程序的入口方法
- 一个源文件可以有多个.class文件，一个类对应于一个.class文件，但只能写一个public类，并且文件名与其一致
- 缩进，编程风格

**疑问**

① 为什么有的语言大小写敏感而有的语言大小写不敏感？

*语言诞生时的主观因素？*

② 为什么大多数语言将main方法作为应用程序入口方法，如c,c++,java?

*约定俗成？设计习惯？c,c++影响？*

### 2.3 Java的目标代码

Java的目标代码是字节码，不是位码，在执行之前，需要将字节码转换为本机代码。为了实现语言的动态性与安全性，Java编译器没有将变量和方法的引用直接编译为内存的引用，也没有确定程序运行中的内存布局，而是将符号引用信息保留在字节码中，这样类的装载和符号引用的消解都要在运行时进行。

**编译型语言的目标代码都是位码吗？位码是什么？**

​	目标代码是由编译器或汇编器处理后的代码，一般由机器代码或接近于机器语言的代码组成。目标文件（object file）即存放目标代码的计算机文件，它常被称为二进制文件（binaries）。目标文件包含着机器代码（可直接被计算机中央处理器执行）以及代码在运行时使用的数据，如重定位信息，如用于链接或调试的程序符号（变量和函数的名字），此外还包括其他调试信息。目标文件是从源代码文件产生程序文件这一过程的中间产物，链接器正是通过把目标文件链接在一起来生成可执行文件或库文件。目标文件中唯一的要素是机器代码。目标代码通常采用三种形式：机器语言（全是01）、汇编语言、待装配机器语言模块。

### 2.4 变量和数据类型

**标识符**：

​    Java采用Unicode国际标准字符集，每个字母16位表示，范围是`\u0000`到`\uFFFF`，共65535个字符：其中前256个（`\u0000`-`\u00FF`）字符和ASCII码中的字符完全重合。

**判断**：

​    使用Character类中的`isJavaIdentifierStart(char ch)`和`isJavaIdentifierPart(char ch)`测试*ch*中的Unicode字符是否可以作为标识符的开始字符或后继字符。

**标识符风格约定**：

- 以字母、下划线`_`、美元符`$`开头；
- 变量名和方法名：`_`和`$`不作为标识符的第一个字符，首字母小写，如*anyVariableWord*；
- 类名和接口名首字母大写，如 *HelloWorld*;
- 常量名完全大写，下划线`_`作为单词分隔符，如 *MAXIMUM_SIZE*;
- 方法名用动词，类名与接口名用名词
- 变量名应该能够表示一定的含义，尽量不要使用单个字符作为变量名；
- 不可以是Java的关键字（Java关键字全为小写），可以用汉字命名，但不推荐。

**基本数据类型（内置类型）**：4类8种

- 逻辑型：boolean（1 bit，默认初始值为false）

​     【与其他高级语言不同，Java种的布尔值和数字之间不能来回转换，即false和true不对应于任何零或非零的整数值】

- 文本型：char（单引号，初始值默认为`\u0000`）

  【unicode，总数达34168个字符；转义字符反斜杠`\`】

  【直接以八进制或十六进制代表字符的表示方法：在反斜杠后跟3位八进制数字或在反斜杠后跟`u`,后面再跟4位十六进制数字都可代表一个字符常量，如`\141`和`\u0061`都表示`a`】

- 整型：byte, short, int, long
- 浮点型：double, float

### 2.5 整数类型进制转换

| 类型  | 占用存储空间 |                        表示数的范围                        |
| :---: | :----------: | :--------------------------------------------------------: |
| byte  |    1字节     |                          -128~127                          |
| short |    2字节     |      -2<sup>15</sup>~2<sup>15</sup>-1  (-32768~32767)      |
|  int  |    4字节     | -2<sup>31</sup>~2<sup>31</sup>-1  (-2147483648~2147483647) |
| long  |    8字节     |              -2<sup>63</sup>~2<sup>63</sup>-1              |

注：Java中所有的整数类型都是有符号的整数类型，Java没有无符号整数类型。

**byte**：由于不同机器对于多字节数据的存储方式不同，可能是大端存储，也可能是小端存储。这样，再分析网络协议或文件格式时，为了解决不同机器上的字节存储顺序问题，用byte类型来表示数据是比较合适的。而通常情况下，由于其表示的数据范围很小，容易造成溢出，因此要尽量少用。

【大端存储】：数据的低字节存储在高地址单元中

【小端存储】：数据的低字节存储在低地址单元中

**大整数和大浮点数**：Java中有两个类`BigInteger`和`BigDecimal`分别表示大整数类和大浮点数类，至于两个类的对象能表示最大范围不清楚，理论上能够表示无限大的数，只要计算机内存足够大。

**进制转换**：

10, 2, 8, 16进制转换函数（默认全部为10进制数）

Interger.toBinaryString(a); //2进制（0b开头，以四个字节输出）

Interger.toOctalString(a); //8进制（0开头）

Interger.toHexString(a); //16进制（0x开头）

JDK7：二进制数可以用`_`分割，如：0b0000_000_00_0（可以随意分割）

#### 问题：为什么byte范围是 -128~127，即10000000表示 -128？

[时钟角度](https://www.zhihu.com/question/20159860/answer/21113783)

同余角度：

>很多人并不理解补码。**补码就是同余啊。**1000000是正128你知道吧，正负128模256是同余的。加减乘可以直接算也是同余的定理决定的，而不是凑出来的巧合，哪可能凑出这种东西？
>
>8位只能表示256个数，0到255，但我还想表示一些负数怎么办呢？就用与该负数同余的正数来表示呗。-1=255，-2=254，等等。
>
>建议脱离算数的思维方式，**这其实就是一个环**。模任何一个正整数（如256），可以把所有整数分类，比如模256可分256类，0,256,-256...是一类（余0类），1,257,-255...是一类（余1类），等等，这256类可看作环的元素，你看-128和128是同一个类里的（余128类)，用一个代表另一个罢了。补码和普通的unsigned integers都是在每类中选一个数，unsigned integers选0到255，补码表示的有符号整数选 -128到127，都是一个数恰好对应一个类。
>
>当你明白这一切后，补码就是顺理成章的事。

### 2.6 浮点数误差问题

​    浮点数默认类型为`double`，默认值为`0.0`。浮点数在运算过程中不会因溢出而导致异常处理。若出现下溢，则结果为`0.0`，若上溢，则结果为正或负无穷大`infinity`，非法数表示为`NaN`。

**infinity**：正数除以0（5.0/0）

**-infinity**：负数除以0（-5.0/0）

**NaN**：0.0除以0.0，或 对一个负数开方

【所有正无穷大数值都相等，所有负无穷大数值都相等，NaN不与任何数值相等，甚至和NaN都不相等】

float：小数点后7位

double：小数点后16位

**float和double都采用IEEE754标准**

### 2.7 char-字符串

​    char类型用来表示在unicode编码表中的字符。char类型用单引号。char可以直接当做整数用。默认初始值为`\u0000`.

​    Java里面的字符串，是定义成：`String`类

**String**

​    String不是基本类型而是一个类。字符串在Java中是对象，在Java中有三个类可以表示字符串：`String`、`StringBuffer`和`StringBuilder`。一个String的对象表示一个字符串，字符串放在双引号中，字符串中的字符也是unicode字符，与C/C++不同，Java中的字符串不以`\0`为结束符。String对象表示的字符串是不能修改的。如果需要对字符串修改，应该使用StringBuffer类。

**Java中String的结束符是什么？**

> Java里面一切都是对象,是对象的话,字符串肯定就有长度,即然有长度,编译器就可以确定要输出的字符个数,当然也就没有必要去浪费那1字节的空间用以标明字符串的结束了。比如,数组对象里有一个属性length,就是数组的长度,String类里面有方法length()可以确定字符串的长度,因此对于输出函数来说,有直接的大小可以判断字符串的边界,编译器就没必要再去浪费一个空间标识字符串的结束。

### 2.8 基本数据类型-类型转换

**1、自动类型转换**：容量小的数据类型可以自动转换为容量大的数据类型

**特例**：可以将整型常量直接赋给byte,short, char等类型变量，而不需要进行强制类型转换，只要不超出其表数范围。

![image-20220514113944306](C:\Users\wangk\AppData\Roaming\Typora\typora-user-images\image-20220514113944306.png)

int，char，short，byte也可互转，只要表示的数值没有超过数值范围。

**为什么int转float会有精度丢失，而转double不会？**

>java 的浮点类型都依据 [IEEE](https://www.baidu.com/s?wd=IEEE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) [754](https://www.baidu.com/s?wd=754&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 标准。[IEEE](https://www.baidu.com/s?wd=IEEE&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) [754](https://www.baidu.com/s?wd=754&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao) 定义了32 位和 64 位双精度两种浮点二进制小数标准。
>
>对于32 位浮点数float用第1位表示数字的符号，用第2至9位来表示指数，用最后23 位来表示尾数，即小数部分。
>
>对于64 位[双精度浮点数](https://www.baidu.com/s?wd=双精度浮点数&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，用第1位表示数字的符号，用11位表示指数，52位表示尾数。
>
>即如果int的值在23位内表示，则float可以精确表示，超过23位则float的23位不够表示，但是double拥有52位可以精确表示整数，完全包含了32位的int。

**2、强制类型转换**

- 强制类型转换，又被称为造型，用于显示的转换一个数值的类型，在有可能丢失信息的情况下进行的转换是通过造型来完成的，但可能造成精度降低或溢出。

- 当将一种类型强制转换为另一种类型，而又超出了目标类型的表示范围，就会被截断成为一个完全不同的值。

```java
double x = 3.14;
int xn = (int)x; // 3

int a = 300;
byte ab = (byte)a; // 44
```

**3、运算时类型提升**

- 建议类型转换放在第一个，防止计算中溢出

```java
// long total = 70 * 60 * 24 *365 * 70; // -1719527296
long total = 70L * 60 * 24 * 365 * 70; // 2575440000
```

- 整数运算

  a. 如果两个操作数有一个为long，则结果也为long

  b. 没有long时，结果为int，即使操作数全为short, byte，结果也是int

- 浮点运算

  a. 如果两个操作数有一个为double，则结果为double

  b. 只有两个操作数都是float，则结果才为float

#### 问题：为什么两个short类型相加会自动提升为int?

```java
public static void main(String[] args){
    short s1 = 1;
    short s2 = 5;
    s1 = s1 + s2;  // Type mismatch: cannot convert from int to short
    short s3 = s2 + 1; // Type mismatch: cannot convert from int to short
    s3 = 4 + 45;
}
```

s1 + s2系统会自动将它们提升为int再运算，结果为int类型，赋给short类型，编译报错；s3 = s2 + 1也是同样的原因；s3 = 4 + 45,系统先计算4+45=50，也就是变为s3 = 50，50在short表示的范围内，自动转型为short。但是为什么java在两个short型运算时自动提升为int，即使它们没有超过表示范围？

> why was it defined this way? A major reason is that at the time of design of the Java language and java virtual machine, 32-bit was the standard word size of computers, where there is no performance advantage to doing basic arithmetic with smaller types. The Java virtual machine was designed to take advantage of this, by using 32-bit as the int size, and then providing dedicated instructions in the Java bytecode for arithmetic with ints, longs, floats, and doubles, but not with any of the smaller numeric types(byte, short and char). Eliminating the smaller types makes the bytecode simpler, and lets the complete instruction set, with room for future expansion, still fit the opcode in a single byte. Similarly, the JVM was designed with a bias towards easy implementation on 32-bit systems, in the layout of data in classes and in the stack, where 64-bit types(doubles and longs) take two slots and all other types(32-bit or smaller) take one slot.
>
> So, the smaller types were generally treated as second-class citizens in the design of Java, converted to ints at various steps, because that simplified some things. The smaller types are still important because they take less memory when packed together(e.g., in arrays), but they do not help when evaluating expressions.
>
> stack overflow

使用较小类型运算没有性能优势，消除较小的类型使得字节码更简单，并且使得具有未来扩展空间的完整指令集仍然适合单个字节中的操作码。因此，较小的类型通常被视为Java设计中的二等公民，在各个步骤转换为int，因此这简化了一些事情。

**Why does the java api use int instead of short or byte?**

>Some of the reasons have already been pointed out. For example, the fact that [*"...(Almost) All operations on byte, short will promote these primitives to int"*](https://stackoverflow.com/a/27122853/3182664). However, the obvious next question would be: **WHY** are these types promoted to `int`?
>
>So to go one level deeper: The answer may simply be related to the Java Virtual Machine Instruction Set. As summarized in the [Table in the Java Virtual Machine Specification](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.11.1), **all** integral arithmetic operations, like adding, dividing and others, are only available for the type `int` and the type `long`, and **not** for the smaller types.
>
>(An aside: The smaller types (`byte` and `short`) are basically only intended for *arrays*. An *array* like `new byte[1000]` will take 1000 bytes, and an array like `new int[1000]` will take 4000 bytes)
>
>Now, of course, one could say that *"...the obvious next question would be: **WHY** are these instructions only offered for `int` (and `long`)?"*.
>
>One reason is mentioned in the JVM Spec mentioned above:
>
>> If each typed instruction supported all of the Java Virtual Machine's run-time data types, there would be more instructions than could be represented in a byte
>
>Additionally, the Java Virtual Machine can be considered as an abstraction of a real processor. And introducing dedicated [Arithmetic Logic Unit](http://en.wikipedia.org/wiki/Arithmetic_logic_unit) for smaller types would not be worth the effort: It would need additional transistors, but it still could only execute one addition in one clock cycle. The dominant architecture when the JVM was designed was 32bits, just right for a 32bit `int`. (The operations that involve a 64bit `long` value are implemented as a special case).
>
>(Note: The last paragraph is a bit oversimplified, considering possible vectorization etc., but should give the basic idea without diving too deep into processor design topics)
>
>------
>
>EDIT: A short addendum, focussing on the example from the question, but in an more general sense: One could also ask whether it would not be beneficial to store *fields* using the smaller types. For example, one might think that memory could be saved by storing `Calendar.DAY_OF_WEEK` as a `byte`. But here, the Java Class File Format comes into play: All the [Fields in a Class File](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-4.5) occupy at least one "slot", which has the size of one `int` (32 bits). (The "wide" fields, `double` and `long`, occupy two slots). So explicitly declaring a field as `short` or `byte` would not save any memory either.
>
>https://stackoverflow.com/questions/27122610/why-does-the-java-api-use-int-instead-of-short-or-byte

答案可能只与Java虚拟机指令集有关。较小的类型（byte，short）基本上只用于数组。

>If each typed instruction supported all of the Java Virtual Machine's run-time data types, there would be more instructions than could be represented in a byte。

为较小的类型引入专用的算术逻辑单元不值得付出努力：它需要额外的晶体管，但它仍然只能在一个时钟周期内执行一次加法。JVM设计时的主流架构是32位，适合32位int。

### 2.9 基本类型变量与引用类型变量

#### 1、复合数据类型

​    一般将用户定义的新类型称为复合数据类型。java基于面向对象的概念，以类和接口的形式定义新类型。因此在java中类和接口是两种用户定义的复合数据类型。

​    将日期的三个变量进行封装，用class创造一个日期类，创造一种新的数据类型，定义如下：(只有变量没有方法的类)

```java
class mydate{
	int day;
    int month;
	int year;
}
```

#### 2、基本类型变量和引用类型变量

java中数据类型分为两大类：基本数据类型和复合类型。变量也有两种类型：基本类型与引用类型。java的8种基本类型的变量称为基本类型变量，而类、接口和数组变量是引用类型变量。两种类型变量的结构和含义不同，系统对它们的处理也不同。

**（1）基本类型与引用类型变量**

- 基本类型（primitive type）

  基本类型：简单数据类型是不能简化的、内置的数据类型、由编程语言本身定义，它表示了真实的数字、字符和整数。

  基本数据类型的变量包含了单个值，这个值的长度和格式符合变量所属数据类型的要求，可以是一个数字、一个字符或一个布尔值。例如一个整型值是32位的二进制补码格式的数据。

- 引用类型（reference type）

  引用数据类型：Java语言本身不支持C++中的结构（struct）或联合（union）数据类型，它的复合数据类型一般都是通过类或接口进行构造，类提供了捆绑数据和方法的方式，同时可以针对程序外部进行信息隐藏。

  值是指向内存空间的引用（地址）。是指向的内存中保存着变量所表示的一个值或一组值。引用在其他语言中称为指针或内存地址。java不支持显示使用内存地址，必须通过变量名对某个内存地址进行访问。

**（2）两种类型变量的不同处理**

- 基本类型变量声明时，系统直接给该变量分配空间，可以直接操作。
- 引用类型（引用型）变量声明时，只是给该变量分配引用空间，数据空间未分配，声明后不能直接使用。引用型变量在声明后必须通过实例化开辟数据空间，才能对变量所指向的对象进行访问。
- 引用型变量的赋值（引用赋值，赋值的是地址）
- 引用类型包括类、接口和数组类型，还有一种特殊的null类型。

**（3）数据类型在内存中的存储**

- 基本数据类型的存储原理：所有的简单数据类型不存在“引用”的概念，基本数据类型都是直接存储在内存中的内存栈上的，数据本身的值就是存储在栈空间里面，而Java语言里面八种数据类型是这种存储模型；
- 引用类型的存储原理：引用类型继承于Object类（也是引用类型）都是按照Java里面存储对象的内存模型来进行数据存储的，使用Java内存堆和内存栈来进行这种类型的数据存储，简单地讲，“引用”是存储在有序的内存栈上的，而对象本身的值存储在内存堆上的；

**区别**: **基本数据类型和引用类型的区别主要在于基本数据类型是分配在栈上的，而引用类型是分配在堆上的（需要java中的栈、堆概念）**

基本类型和引用类型的内存模型本质上是不一样的

### 2.10 变量

**（1）命名规范**

- 所有变量、方法、类名：见名知意
- 变量、方法名：首字母小写和驼峰原则：如 run(), runRun(), age, ageNew, monthSalary
- 常量：大写字母和下划线 ，如 MAX_VALUE
- 类名： 首字母大写和驼峰原则，如 Man, GoodMan

**（2）变量作用域**

-  局部变量（方法或{}）
-  类成员变量（方法外进行声明且属于一个类的定义体的变量，static声明：类在就在。没有static为实例变量，调用构造方法创建实例对象时创建，只有有引用指向变量所属的对象，该变量就存在。）
- 方法参数（方法调用时传递的参数）
- 异常处理器（catch语句块）参数（catch语句块的入口参数，作用域为后{}）

```java
class MyClass { // 类成员变量作用域 start
    ...
	member variable declarations
    ...
    public void aMethod(method parameters){ // 方法参数作用域 start
        ...
        local variable declarations // 局部变量作用域 start
        ...
        catch(exception handler parameters){  // 异常处理器参数作用域 start
            ...
        } // 异常处理器参数作用域 end
        ... // 局部变量作用域 end
    } // 方法参数作用域 end
    ...
} // 类成员变量作用域 end
```

**将变量提升为成员变量时，有两个缺点**

- 增大了变量的生存时间，这将导致更大的系统开销；
- 扩大了变量的作用域，这不利于提高程序的内聚性

**成员变量**

- 需要定义的变量用于描述某个类或某个对象固有的信息；
- 某个类中需要以一个变量来保存该类或者实例运行时的状态信息；
- 某个信息需要在某个类的多个方法之间进行共享

**局部变量**

- 尽可能的缩小作用域。局部变量的作用范围越小，它在内存中存在的时间越短，程序运行性能越好。

### 2.11 运算符

Java支持的运算符如下：

算术运算符：+，-，*，/，%，++，--

赋值运算符：=

关系运算符：>，<，>=，<=，==，!=，instanceof

逻辑运算符：&&，||，!

位运算符：&，|，^，~，>>，<<，>>>

条件运算符：? :

扩展赋值运算符：+=，-=，*=，/=

**（1）算术运算符**

**/**：除法运算符的两个运算数都是整数类型，则计算结果也是整数，就是将自然除法的结果截断取整。

**%**：两个操作数中至少一个是浮点数，允许第二个操作数是0或0.0，但是求余运算的结果是NaN。0或0.0对零以外的任何数求余都将得到0或0.0

```java
public static void main(String[] args) {
		System.out.println(5%0);  // java.lang.ArithmeticException: / by zero
		System.out.println(5.0%0);  // NaN
		System.out.println(5%0.0);  // NaN
		System.out.println(0.0%0);  // NaN
		System.out.println(0%0.0);  // NaN
		System.out.println(0%0);  // java.lang.ArithmeticException: / by zero
		System.out.println(0%5);  // 0
		System.out.println(0.0%5); // 0.0
}
```

**（2）赋值**

​	赋值表达式是有值的，赋值表达式的值就是右边被赋的值。如 *String str2 = str* 表达式的值就是*str*。连续赋值成立。

**（3）比较运算符**

**==（!=）**：只有当两个引用变量引用的相同类的实例时才可以比较，而且必须这两个引用指向同一个对象才会返回true（!=：两个引用变量均为同一类的实例才可比较）

```java
public static void main(String[] args) {
		String str = new String("254");
		Integer i = new Integer("254");
		// 编译报错Incompatible(矛盾、不匹配) operand types String and Integer
		System.out.println(str == i); 
}
```

**引用类型的缓存问题**

**包装类型**

```java
public static void main(String[] args){
    Integer a = 2;
    Integer b = 2;
    System.out.println(a == b); //true
    Integer c = 128;
    Integer d = 128;
    System.out.println(c == d);  // false
}
```

这与Java的Integer类的设计有关，java的java.lang.Integer类的源码如下，其会对-128~127做缓存。

```java
Integer
```

**String类型的缓存**

```java
String str1 = new String("string");
String str2 = new String("string");
str1 == str2;  // false
String str3 = "hello";
String str4 = "hello";
str3 == str4;  // true
```

因为执行 `String str3 = "hello"`时，系统会创建一个内容为`hello`的`String`实例，并将这个实例缓存起来，当第二次执行`String str4 = "hello"`时，系统会先检查缓存中是否有一个`String`实例的内容与这个`hello`直接量（字面量）的字符序列相同，如果从缓存中找到这样一个`String`实例，系统会直接让这个引用指向这个`String`实例。

**缓存的意义**：缓存是一种非常优秀的设计模式，Java将一些创建成本大，需要频繁使用的对象进行缓存，从而提高程序的运行性能。

**（4）位运算**

```java
System.out.println(128 << 1);    // 256
System.out.println(128 >> 1);    // 64
System.out.println(128 >>> 1);   // 64  高位补0
System.out.println(-128 << 1);   // -256
System.out.println(-128 >> 1);   // -64
System.out.println(-128 >>> 1);  // 2147483584  高位补0
```

**移位运算规则**：

- 对于低于`int`类型（如`byte`, `short`, `char`）的操作数总是先自动类型转换为int类型后再移位；

- 对于`int/long`类型的整数移位 `a >> b`，当`b > 32(64)`时，先对`b`做32（64）求余，再移位。

**（5）其他运算**

**幂运算**

`java.lang.Math`类的`pow()`方法

```java
public static double pow(double a, double b);  //a^b
```

**instanceof**

在运行时指出对象是否是特定类的一个实例。返回一个布尔值。

**字符串连接符**：+

**（6）优先级**

&  >  ^  >  |  >  &&  > ||  > ? :

优先使用括号

**（7）扩展的赋值运算符**

```java
byte a = 5; a = a + 5; // 报错，将int型的5赋值给byte型
byte a = 5; a += 5;  // 不报错
```

`b += 1`等价于 `b=(byte)(b+1)`，底层会对这个结果进行强制转换的，所以它编译的时候没事，如果`b=127`，那么加1后变成`128`了，成了`int`类型了，超过了`byte`类型的最大范围了，系统会强转，把`int`类型的前面三个高位丢弃，丢弃后，如果剩下的一位中，最高位为1，则取反加1，成为负数，如果最高位为0，直接把值赋给`b`。

若可以使用扩展运算符，推荐总使用这种扩展后的运算符。

## 3. 逻辑控制

#### 3.1 选择

`switch`表达式可以是`int`（或能自动转为int）、枚举类型、字符串。

```java
public static void main(String[] args){
    String a = "陕西";
    switch(a){
        case "陕西": System.out.println("输入为陕西"); break;
        case "安徽": System.out.println("输入为安徽"); break;
        default: System.out.println("无结果"); break;
    }
}
```

注意：总是优先将包含范围小的条件放在前面处理。

`switch`：当找到相等的`case`后，程序开始执行这个`case`后的代码，不再判断后面的`case`，无论相不相等都会执行，除非遇到`break`。

**花括号的省略**：为什么在if, else, for, while中只有一条语句时可以省略{}？因为单行语句本身就是一个整体，就无需再使用花括号将其视为一个整体。花括号括起来表示一个整体 ---- 代码块，与单行语句含义相同，只是语句块的大小不同。

#### 3.2 循环

`do while`循环的循环条件必须有一个分号，这个分号表示循环结束。

`for`若需要在*for*之外使用循环变量 *i*,*j*,*k* 这类的变量，推荐在*for*循环之前定义一个变量*temp*来保存这个循环变量的值，如下：

```java
int temp = 0;
for (int i = 0; i < 10; i++){
    temp = i;
}
```

如果将`i`放在`for`循环之前定义，则变量`i`的作用域被扩大了，功能也被扩大了，作用域扩大的后果是：如果该方法还有另一个循环也需要定义循环变量，则不能再次使用`i`作为循环变量。

#### 3.3 方法

`return`语句的作用：返回值；方法结束

方法只有值传递，没有引用传递（值传递：将实际参数值的副本（复制品）传入方法内，而参数本身不会受到任何影响）

```java
public class TestValue{
    public static void main(String[] args){
        int a = 4;
        add(a);
        System.out.println(a);  // 4
    }
    public static void add(int a){
        a++;
    }
}
```

在*main*方法中调用*add()*方法时，*main*方法还未结束。因此，系统分别为*main*和*add*方法分配两块栈区（方法栈），分别用于保存*main*方法和*add*方法的局部变量。*main*方法中的 *a* 变量作为参数传入*add*方法中，实际上是在*add*方法栈中重新产生了一个变量 *a*，并将*main*方法栈区中 *a* 变量的值赋给*add*方法中 *a*变量（*add*方法中的变量*a*形参进行了初始化），此时，系统存在两个*a*变量，只是存在于不同的方法栈区中而已。

**值传递**是将变量的一个副本传递到方法中，方法中如何操作该变量副本，都不会改变原变量的值。

**引用传递**将变量的内存地址传递给方法，方法操作变量时会找到保存在该地址的变量，对其进行操作，会对原变量造成影响。

保持方法的原子性，一个方法实现一个功能。

#### 3.4 递归

递归结构包括两个部分：

定义递归头：什么时候不调用自身方法。如果没有头，将陷入死循环

递归体：什么时候需要调用自身方法

一个方法体内调用它自身，被称为方法的递归。方法递归包含了一种隐式的循环，它会重复执行某段代码，但这种重复执行无须循环控制。递归一定要向已知方向递归。

## 4. API文档

（26）

## 5. 键盘输入

（27）

## 6. 面向对象

（28）-（51）

## 7. 数组及常用类

（52）-（71）

## 8. 异常机制

（72）-完



















