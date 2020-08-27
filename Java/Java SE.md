# Java SE

## 程序种类

1. Application：Java应用程序，是可以由Java解释器直接运行的程序。

2. Applet：即Java小应用程序，是可随网页下载到客户端由浏览器解释执行的Java程序。

3. Servlet：Java服务器端小程序，由Web服务器(容器)中配置运行的Java程序。

## 编程思想

​		POP和OOP均可实现代码的复用和模块化编程

### POP

​		面向解决问题的过程逐个步骤进行编程，自顶向下，步骤可细化成若干子步骤。开发过程中只针对问题本身的实现，程序的主体是函数，一个函数即一个封装起来的模块，可实现一定功能。优缺点是：

1. 逻辑较简单、程序规模较小时，可更快速的实现，流程清楚，可快速找到相应节点进行分析。
2. 没有很好的模块化的设计，复用性低，扩展性差，维护难度大。

### OOP

​		针对实现功能的实体。抽提出参与的实体，创建对象，为实体添加属性和方法，通过调用实体的方法解决问题，即事件需由实体触发。多采用子模块化的设计，每个模块单独存在，可被重复利用。优缺点是：

1. 封装性更强，模块化程度更高，数据更安全+封闭。

2. 复用性高，可继承，可覆盖，代码间低耦合，低耦合系统可降低后期维护的工作量，利于长期维护和拓展。

3. 性能低，逻辑抽象度高，需在性能上作出牺牲，计算所需时间和存储空间开销较大，增加运行开销。

    例如：修改对象内部时，由于对象的私有属性不允许外部直接存取，需增加许多只负责读写的行为，使得程序臃肿，增加运行开销。

## Java基础

### 字符编码

#### ASCII码/ANSI码

```
Q. 关于ASCII码和ANSI码，以下说法不正确的是？（D）
A：标准ASCII只使用7个bit
B：在简体中文的Windows系统中，ANSI就是GB2312
C：ASCII码是ANSI码的子集
D：ASCII码都是可打印字符

A. 不同的国家和地区制定了不同的标准，由此产生了 GB2312、GBK、Big5、Shift_JIS 等各自的编码标准。这些使用 1 至 4 个字节来代表一个字符的各种汉字延伸编码方式，称为 ANSI 编码。在简体中文Windows操作系统中，ANSI 编码代表 GBK 编码；在日文Windows操作系统中，ANSI 编码代表 Shift_JIS 编码。 不同 ANSI 编码之间互不兼容，当信息在国际间交流时，无法将属于两种语言的文字，存储在同一段 ANSI 编码的文本中。 当然对于ANSI编码而言，0x00~0x7F之间的字符，依旧是1个字节代表1个字符。这一点是ANSI编码与Unicode编码之间最大也最明显的区别。ASCII码包含一些特殊空字符，所以不能打印。
```

### 数据类型

#### 基本数据类型

|                     | 位数 | 默认值        | min            | max             | 备注         |
| ------------------- | ---- | ------------- | -------------- | --------------- | ------------ |
| byte 字节型         | 8    | 0             | -2^7           | 2^7-1           | 补码有符号位 |
| short 短整型        | 16   | 0             | -2^15          | 2^15-1          | 补码有符号位 |
| int 整型            | 32   | 0             | -2^31          | 2^31-1          | 补码有符号位 |
| long 长整型         | 64   | 0L            | -2^63          | 2^63-1          |              |
| float 浮点型        | 32   | 0.0f          | 1.401298e-45   | 3.402823e+38    | 单精度       |
| double 双精度浮点型 | 64   | 0.0d          | 4.9000000e-324 | 1.797693e+308   | 双精度       |
| boolean 布尔型      | 1    | false         |                |                 |              |
| char 字符型         | 16   | ’\u0000’ 空格 | ’\u0000’ 空格  | ’\uffff’ 65,535 | Unicode字符  |

##### 运算

**优先级问题**：强制类型转换的优先级高于+ - * /

**i++问题**：i++不是原子性操作，i++执行了多部操作，这多个阶段中间都可以被中断分离开：

1. 内存到寄存器：从变量i中读取i的值，开辟一个临时存储区，将i的值复制到临时存储区。
2. 寄存器自增：值+1
3. 写回内存：将+1后的值写回i中，临时存储区的值等待被调用（参与运算、赋值、输出等）。

```
Q. 以下代码执行的结果是多少？（0）
int count=0;
int num=0;
for(int i=0;i<=100;i++){
	num=num+i;
	count=count++;
}
System.out.println(num*count);
	
A. count=count++原理是temp=count；count=count+1；count=temp；因此count始终是0,这仅限于java。
```

```
Q. 以下代码执行的结果是多少？（编译错误）
int i=-5;
i=++(i++);
System.out.println(i);

A. 先执行括号中的i++，在执行i++的时候，Java会将i先存放到一个临时变量中去，并返回该临时变量的值，假设为temp。这句可以拆成temp=i（值为-5）并返回temp的值，然后i自加1，此时i的值为-4，但是之后执行就会出现问题，由于返回了temp的值，继续执行的表达式为i=++(-5)，单目运算符无法后跟一个字面量，所以在IDEA编辑器中提示Variable expected，表示此处应为变量。
```

```
Q. 下面的程序的结果是什么?（1）
int i = 0;
i = i++ + i;
  
A. i++先参与运算再自身自增，i++是0，i变为1，i++ + i=0+1=1。
```

**原子性问题**：

1. Java提供了java.util.concurrent.atomic包来提供线程安全的基本类型包装类，int用AtomicInteger，这些包装类都是是用CAS来实现。
2. ReentrantLock的lock()
3. synchronized (SynchronizedTest.class) 

**精度问题**：

​		进制的小数无法精确的表达 10 进制小数，计算机在计算 10 进制小数的过程中要先转换为 2进制进行计算，这个过程中出现了误差。如`4.0-3.6=0.40000001`。

**类型转换问题**：

1. 所有的byte、short、char型的值将被提升为int型；
2. 如果有一个操作数是long型，计算结果是long型；
3. 如果有一个操作数是float型，计算结果是float型；
4. 如果有一个操作数是double型，计算结果是double型；
5. 被fianl修饰的变量不会自动改变类型，当2个final修饰相操作时，结果会根据等式左边变量的类型而转化。
6. long→float，无须强制转换。float占4个字节为什么比long占8个字节大呢，因为底层的实现方式不同。浮点数的32位并不是简单直接表示大小，而是按照一定标准分配的。第1位，符号位，即S；接下来8位，指数域，即E；剩下23位，小数域，即M，取值范围为[1 ,2 ) 或[0 , 1)；然后按照公式： V=(-1)^s * M * 2^E。也就是说浮点数在内存中的32位不是简单地转换为十进制，而是通过公式来计算而来，通过这个公式虽然，只有4个字节，但浮点数最大值要比长整型的范围要大。

```
Q. 以下代码执行结果为（语句：b3=b1+b2编译出错）
byte b1=1,b2=2,b3,b6; 
final byte b4=4,b5=6; 
b6=b4+b5; 
b3=(b1+b2); 
System.out.println(b3+b6);

A. 被final修饰的变量是常量，这里的b6=b4+b5可以看成是b6=10,在编译时就已经变为b6=10了。而b1和b2是byte类型，java中进行计算时候将他们提升为int类型，再进行计算，b1+b2计算后已经是int类型，赋值给b3，b3是byte类型，类型不匹配，编译不会通过，需要进行强制转换。Java中的byte，short，char进行计算时都会提升为int类型。没有final修饰的变量相加后会被自动提升为int型，与目标类型byte不相容，需要强制转换（向下转型）。
```

```
Q. byte a=3; byte b=2; b=a+b; System.out.println(b);（×）
   byte a=127; byte b=126; b=a+b; System.out.println(b);（×）
   byte a=3; byte b=2; a+=b; System.out.println(b);（√）
   byte a=127; byte b=127; a+=b; System.out.println(b);（√）
   
A. byte类型的变量在做运算时被会转换为int类型的值，故前两句左为byte，右为int，会报错；而后两句中用的是a+=b的语句，此语句会将被赋值的变量自动强制转化为相对应的类型。
```

```
Q. 针对以下代码，x==f1[0]的结果为（true）
class CompareReference{
   public static void main(String [] args){
   float f=42.0f;
   float f1[]=new float[2];
   float f2[]=new float[2];
   float[] f3=f1;
   long x=42;
   f1[0]=42.0f;
  }
}

A. 两个数值进行二元操作时，会有如下的转换操作：
   如果两个操作数其中有一个是double类型，另一个操作就会转换为double类型。
   否则，如果其中一个操作数是float类型，另一个将会转换为float类型。
   否则，如果其中一个操作数是long类型，另一个会转换为long类型。
   否则，两个操作数都转换为int类型。
   故，x==f1[0]中，x将会转换为float类型。
```

**取整问题**：

floor：求小于参数的最大整数，返回double类型。

ceil：求大于参数的最小整数，返回double类型。源码注释：如果参数是-1.0到0.0之间的数，结果是-0.0。

round：对小数进行四舍五入，返回int类型。

```
Math.floor(-4.2) //-5.0
Math.ceil(5.6) //6.0
Math.ceil(-0.5) //-0.0
Math.round(-4.6) //-5
Math.round(-4.5) //-4
```

**位运算问题**：

```
Q. What results from the following code fragment?（-6）
int i = 5;
int j = 10;
System.out.println(i + ~j);

A. 计算机本身储存的就是补码，补码只有转换为原码原码才可以被正常人类识别为对应为正常的二进制数。
   10的补码就是人类识别的10的原码：0000 0000 0000 1010——补码
   ~10的补码就是：1111 1111 1111 0101
   ~10的反码就是：1111 1111 1111 0100——补码减1
   ~10的原码就是：1000 0000 0000 1011——反码取反，换算为整数为-11
```

**函数问题**：

​		Math.cos()中参数的单位是弧度。

​		Math.toRadians()：把角度转化成弧度

​		Math.toDegrees()：把弧度转化成角度

**三元运算符问题**：

​		三元运算符会对2个结果的数据类型进行自动的类型提升，若两个操作数都是直接量数字，则返回值类型为范围较大者。

```
Q. Object o1 = true ? new Integer(1) : new Double(2.0); //1.0
```

#### 引用数据类型

​		每一个实体都有地址值，引用类型默认值为null，每一个实体内的数据都有默认值。

##### 引用传递

​		一块堆内存空间可同时被多个栈内存空间共同指向，每个栈内存都可修改同一个堆内存空间的属性值。

##### 包装数据类型

​		将基本类型封装成对象，在对象中定义方法，通过方法操作数据。常用操作：基本数据类型与字符串间的转换。包装类型变量可以直接接收它所包括的类型变量。

​		Integer 是 int 的包装类，必须实例化为才能使用，Integer 变量实际是对象的引用，指向对象，默认值为 null。int 是基本数据类型，不必实例化就能用，它直接存数据值，默认是 0。

###### 自动装/拆箱

1. Integer 变量和 int 变量比较时，只要两个变量的值相等，则结果为 true，因为自动拆箱。
2. 非 new 生成的 Integer 变量和 new Integer()生成的变量比较时，结果为 false。
3. 对于两个非 new 生成的 Integer 对象，进行比较时，如果两个变量的值在-128~127，即byte的取值范围，在该取值范围内，自动装箱就不会创建新的对象，而是从常量池中获取。超过了byte取值范围就会再创建新对象。
4. collection类型的集合（ArrayList,LinkedList）只能装入对象类型的数据，但是JDK5以后提供了自动装箱与自动拆箱，所以int类型自动装箱变为了Integer类型，ArrayList.add(0)编译能够正常通过。

```
Integer i1 = 97;
Integer i2 = 97;
Integer i3 = 197;
Integer i4 = 197;
i1==i2;		//true
i3==i4;		//false
```

```
Q. Double d = 100;（编译错误）

A. 对于Integer和Double装箱只能装对应的数据类型，不对应就会报错，100默认是int型，而不是byte、short、long，100.0默认是double，而不是float。

Integer i=100;//√
Integer i2=100.0;//×，因为默认是double
Integer i3=(byte)100;//×
Short (byte)100; //√，说明上面的规律对Short不适用
Double d=100; //×
Double d=100.0;//√
Double d=100.0f;//×
double d=100;//√，100是int类型，自动转换为double.
Double d=Double.valueOf("100"); //√
Double d=Double.valueOf(100);//√
Double d=new Double(100);//√
```

```
Q. 对于下列代码，s.equals(9)返回的是？（true）
Integer s=new Integer(9);
Integer t=new Integer(9);
Long u=new Long(9);

A. (s.equals(9))，在进行equals比较前，会对9调用Integer.valueOf方法，进行自动装箱, 由于IntegerCache中已经存在9，所以直接返回其引用，引用相同，equals就自然相同了，所以结果为真。
```

##### String

###### 不可变性

​		字符串一旦定义不可改变，final修饰的类是不被能继承的，所以final修饰的类是不能被篡改的。在进行String类对象内容修改时，实际上原始的字符串没有发生变化，改变的是String类对象的引用关系，最终没有引用的堆内存空间将成为垃圾空间。

​		字符串常量在堆中的常量池，字符串每进行一次拼接，即在堆中创建了一个新对象。若要经常修改字符串的内容，请尽量少用String，因为字符串的指向“断开-连接”会大大降低性能。对于要经常修改内容的情况，建议使用StringBuilder、StringBuffer。

*Q. String a = b+c 是原子性操作吗？分情况讨论。*

```
这是因为字符串字面量拼接操作是在Java编译器编译期间就执行了，也就是说编译器编译时，直接把"java"、"and"和"python"这三个字面量进行"+"操作得到一个"javaandpython" 常量，并且直接将这个常量放入字符串池中，这样做实际上是一种优化，将3个字面量合成一个，避免了创建多余的字符串对象（只有一个对象"javaandpython"，在字符串常量池中）。而字符串引用的"+"运算是在Java运行期间执行的，即str1 + str2 + str3在程序执行期间才会进行计算，它会在堆内存中重新创建一个拼接后的字符串对象。且在字符串常量池中也会有str1,str2与str3，这里创建多少个新的对象与原来字符串常量池中有没有str1\str2\str3有关，如果之前存在就不会创建新的对象。
总结来说就是：字面量"+"拼接是在编译期间进行的，拼接后的字符串存放在字符串池中；而字符串引用的"+"拼接运算实在运行时进行的，新创建的字符串存放在堆中。
那么再来看这题，很明显只在编译期间在字符串常量池中创建了"welcometo360"一个字符串
```

```
Q. 执行结果为？
public class SystemUtil{
    public static boolean isAdmin(String userId){
        return userId.toLowerCase()=="admin";
    }
    public static void main(String[] args){
        System.out.println(isAdmin("Admin"));//false
        System.out.println(isAdmin("admin"));//true
    }
}

A. toLowerCase()参数中有字母不是小写时，源码会new String();
```

###### 不可变好处

​		不可变的理由：安全，共享，效率。

**安全**：

1. java.lang.String 引用的对象一定是 java.lang.String的对象，不是其它子类的对象，这样才能保证它的安全和效率。
2. String设成final禁止继承，避免被其他人继承后破坏其内部的array。
3. 如果字符串是可变的，那么会引起很严重的安全问题。譬如，数据库的用户名、密码都是以字符串的形式传入来获得数据库的连接，或者在socket编程中，主机名和端口都是以字符串的形式传入。因为字符串是不可变的，所以它的值是不可改变的，否则黑客们可以钻到空子，改变字符串指向的对象的值，造成安全漏洞。
4. 类加载器要用到字符串，不可变性提供了安全性，以便正确的类被加载。譬如你想加载java.sql.Connection类，而这个值被改成了myhacked.Connection，那么会对你的数据库造成不可知的破坏。

**共享**：

1. 只有当字符串是不可变的，字符串池才有可能实现。字符串池的实现可以在运行时节约很多heap空间，因为不同的字符串变量都指向池中的同一个字符串。但如果字符串是可变的，那么String interning将不能实现（String interning是指对不同的字符串仅仅只保存一个，即不会保存多个相同的字符串），因为这样的话，如果变量改变了它的值，那么其它指向这个值的变量的值也会一起改变。
2. 因为字符串是不可变的，所以是多线程安全的，同一个字符串实例可以被多个线程共享。这样便不用因为线程安全问题而使用同步，字符串自己便是线程安全的。

**效率**：

1. 因为字符串是不可变的，所以在它创建的时候hashcode就被缓存了，不需要重新计算。这就使得字符串很适合作为Map中的键，字符串的处理速度要快过其它的键对象，这就是HashMap中的键往往都使用字符串。

###### 常量池

​		直接赋值产生的String类对象，实际上作为String类的匿名对象的形式存入对象池，该匿名对象由系统自动生成。若用直接赋值实例化String类对象，若设置的内容相同，则这些对象会使用已有对象的引用分配，其栈内存均会自动指向同一块堆内存空间；若是新内容，则会自动开辟新的内存空间。

```
String s1="a";
String s2="b";
String s3="a"+"b";
String s4=s1+s2;
String s5=s1+"b";
String s6="ab";
         
System.out.println(s6==s3); //常量拼接与常量比较 true
System.out.println(s6==s5); //常量与变量拼接与常量比较 false
System.out.println(s6==s4); //变量拼接与常量比较 false
         
字符串的拼接与常量比较的问题，若拼接字符串的+两边存在变量，则会在堆上new一个新的对象，此时与常量的==结果为false。若拼接字符串的+两边均是常量，由于java的常量优化机制，拼接的结果是指向常量池的，与常量==的结果是true。
```

```
String str1 = "hello";
这里的str1指的是方法区中的字符串常量池中的“hello”，编译时期就知道的；
 
String str2 = "he" + new String("llo");
这里的str2必须在运行时才知道str2是什么，所以它是指向的是堆里定义的字符串“hello”。

String s = "a"+"b"+"c";
语句中，“a”,"b", "c"都是常量，编译时就直接存储他们的字面值，而不是他们的引用，在编译时就直接将它们连接的结果提取出来变成"abc"了。
```

**证明**：equals()方法是String类定义的，类中的方法只有实例化对象才可调用，直接利用字符串"hello"调用equals()方法，证明字符串常量是String类的匿名对象。

```
String str=new String(“hello”);
System.out.print("hello".equals(str)); 
```

​		每个字符串都是一个String类的匿名对象，所以首先会在堆空间中开辟一块空间保存字符串”hello”。然后使用关键字new，new永远表示开辟新的堆内存空间，所以其内容不会保存在对象池中，开辟另一块内存空间，真正使用的是关键字new开辟的堆内存，之前定义的字符串常量的常量池内存空间将不会有任何的栈内存指向，将成为垃圾，等待被GC回收。所以，使用构造方法的方式开辟的字符串对象，实际上会开辟两块空间，其中有一块空间将成为垃圾，造成内存的浪费。解决办法是手工入池：String stra=new String(“hello”).intern();。

##### 数组

1. 数组是一个对象，不同类型的数组具有不同的类。没有重写equals()。
2. 定义一维数组时，必须显式指明数组的长度；
3. 定义数组时，其一维数组的长度必须首先指明，其他维数组长度可以稍后指定；
4. 采用给定值初始化数组时，不必指明长度；
5. []是数组运算符，在声明一个数组时，数组运算符可以放在数据类型与变量之间，也可以放在变量之后。

```
Q. Which statement declares a variable a which is suitable for referring to an array of 50 string objects?（Java）（BCF）
A.char a[][];
B.String a[]
C.String[] a;
D.Object a[50];
E.String a[50];
F.Object a[];

A. 题目问哪个语句声明了一个适合于创建50个字符串对象数组的变量。
A：char[][] 定义了二位字符数组。在Java中，使用字符串对char数组赋值，必须使用toCharArray()方法进行转换。所以A错误。
B、C：在Java中定义String数组，有两种定义方式：String a[]和String[] a。所以B、C正确。
D、E：数组是一个引用类型变量 ，因此使用它定义一个变量时，仅仅定义了一个变量 ，这个引用变量还未指向任何有效的内存 ，因此定义数组不能指定数组的长度。所以D、E错误。
F：Object类是所有类的父类。子类其实是一种特殊的父类，因此子类对象可以直接赋值给父类引用变量，无须强制转换，这也被称为向上转型。这体现了多态的思想。所以F正确。
```

### 关键字

​		true false 是boolean的变量值，null是Java中的空值，是编译器赋予特定含义的，但并不是关键字。void是关键字。

#### 修饰符

|            | 修饰符                                                    |
| ---------- | --------------------------------------------------------- |
| 类         | public、默认、abstract、final                             |
| 成员变量   | public、默认、protected、private、static、final           |
| 构造方法   | public、默认、protected、private                          |
| 成员方法   | public、默认、protected、private、static、abstract、final |
| 抽象方法   | public、protected                                         |
| 接口方法   | public、abstract                                          |
| 接口成员   | public+static+final   均可省略                            |
| 成员内部类 | public、默认、protected、private                          |
| 局部内部类 | final、abstract                                           |
| 匿名内部类 |                                                           |

##### final

​		final作为对象成员存在时，在使用之前必须初始化。如果不在声明时赋值，则需在构造函数中初始化。

#### 访问修饰符

| 访问修饰符 |                            | 可见范围                     |
| ---------- | -------------------------- | ---------------------------- |
| public     | 共有的                     | 对所有类可见                 |
| protected  | 受保护的                   | 对同一包内的类和所有子类可见 |
| default    | 默认的（不使用任何修饰符） | 在同一包内可见               |
| privarte   | 私有的                     | 在同一类内可见               |

#### 断言

​		assert，程序执行到assert处，结果一定是预期结果。Java默认不开启断言。此时若断言错误，运行时不会出现错误。需在运行时增加-ea参数开启断言，若产生错误，输出：后的异常信息。

```
运行命令：java -ea Demo
assert num==20 : "num的内容不是20";  //若num!=20，产生错误，输出异常信息：num的内容不是20
```

### 类

#### 初始化过程

​		分层执行，先父后子。

1. 加载父类名.class到内存
2. 加载子类名.class到内存
3. 在栈上为对象名开辟内存空间
4. 在堆上为对象申请内存空间
5. 按位置的先后初始化父类的静态成员变量和静态代码块
6. 按位置的先后初始化子类的静态成员变量和静态代码块
7. 为父类的成员变量进行默认初始化、显式初始化、执行构造代码块、执行构造方法
8. 为子类的成员变量进行默认初始化、显式初始化、执行构造代码块、执行构造方法
9. 将对象的地址赋值给变量

```
Q. 以下代码执行后输出结果为（blockAblockBblockA）
public class Test{
    public static Test t1 = new Test();
    {
         System.out.println("blockA");
    }
    static{
        System.out.println("blockB");
    }
    public static void main(String[] args){
        Test t2 = new Test();
    }
}

A. 静态块：用static申明，JVM加载类时执行，仅执行一次
   构造块：类中直接用{}定义，每一次创建对象时执行
   执行顺序优先级：静态块/静态变量>main()>构造块>构造方法
   静态块和静态变量按照申明顺序执行
   先初始化静态成员变量，执行Test t1 = new Test();所以先输出blockA构造块，
   然后执行静态块，输出blockB；
   最后执行main方法中的Test t2 = new Test();输出构造块blockA。
```

```
Q. 运行代码，输出的结果是（P is init）
public class P {
	public static int abc = 123;
	static{
		System.out.println("P is init");
	}
}
public class S extends P {
	static{
		System.out.println("S is init");
	}
}
public class Test {
	public static void main(String[] args) {
		System.out.println(S.abc);
	}
}

A. 子类引用父类的静态字段，只会触发子类的加载、父类的初始化，不会导致子类初始化。而静态代码块在类初始化的时候执行。
```

```
Q. 下面代码的输出是什么？(null)
public class Base{
    private String baseName = "base";
    public Base(){
        callName();
    }
 
    public void callName(){
        System. out. println(baseName);
    }
 
    static class Sub extends Base{
        private String baseName = "sub";
        public void callName(){
            System. out. println (baseName) ;
        }
    }
    public static void main(String[] args){
        Base b = new Sub();
    }
}

A. new Sub();在创造派生类的过程中首先创建基类对象，然后才能创建派生类。创建基类即默认调用Base()方法，在方法中调用callName()方法，由于派生类中存在此方法，则被调用的callName（）方法是派生类中的方法，此时派生类还未构造，所以变量baseName的值为null。
```

#### 抽象类和接口的区别

1. 接口允许多继承，抽象类不能多继承。

2. 接口里定义的变量只能是公共的静态的常量，public static final这三个关键字可以省略；抽象类中的变量一般是普通变量。

3. 接口没有成员变量，抽象类可以继承成员变量。

5. JDK1.8以前，抽象类的方法默认访问权限为protected

    JDK1.8时，抽象类的方法默认访问权限变为default

6. 接口的修饰符abstract可省略。

    接口中的方法默认是public abstract。

    JDK1.8及以前，接口中的方法必须是public的。
    
    JDK1.8，接口中可以有default方法和static方法，使得接口更加接近抽象类。
    
    JDK1.9时，接口中可以有private方法和private static方法。

#### 构造方法

​		构造方法不使用void来声明，因为程序的编译依靠定义结构解析，以防构造方法定义的结构与普通方法完全一样。

​		构造完成之前，属性都为其对应数据类型的默认值。只有在整个构造都完成之后，才会将内容赋给属性。

​		构造方法不能递归使用，可以重载。一个简单的Java类一定要保留一个无参构造方法，即使没有定义任何构造方法，程序编译之后也会自动的为类增加一个没有参数、没有方法名称、类名称相同、没有返回值的构造方法，若定义了有参构造方法，则不会再自动生成默认的构造方法。可在构造函数内用this();显示调用本类的其他构造方法，java默认的在调用子类构造方法前默认先调用父类的构造方法，因为子类对象中有父类中定义的属性，这些属性应由父类进行初始化。所以应默认调用父类的构造方法对这些属性进行初始化。如果一个类中有多个构造方法之间使用this()相互调用，那么至少要保留一个构造方法作为出口先去调用父类的构造方法，如果没有指定调用父类的哪个构造方法，那么java默认调用父类无参数的构造方法，因为系统默认提供一个无参的构造方法，若父类没有无参构造方法，可用super(参数)显示调用父类的有参构造方法，通常在子类构造方法的第一句加super();，若不在第一句，会使父类数据多次初始化。this()和super()不能同时出现。

#### 重载

​		方法重载的参数传递可能会发生隐式转换，若没有找到对应的方法，就会找到最近的兼容方法。所以对于方法体一样的方法，只需定义一个最广范围的数据类型作参数。

#### 变量

​		申请内存来存储值，内存管理系统根据变量的类型为变量分配存储空间。

#### 内部类

​		内部类的实例对象是外部类的对象的一个成员，若没有外部对象，内部对象也是不存在的，何谈静态成员。

##### 成员内部类

​		可以直接调用外部类的所有资源（包括静态），但自身需要依靠外部类来实例化，所以成员内部类不能有静态属性。

##### 局部内部类

​		局部内部类在局部方法中，局部方法执行结束就没用了，添加权限修饰符没有意义。

​		局部内部类若使用abstract修饰，可以在该方法中直接继承使用。

​		局部内部类若使用final修饰，则方法中自己不能继承，其他不影响。

​		局部内部类可访问final或effctively final标记的局部变量，但不能改变变量的值。局部变量的生命周期与局部内部类的对象的生命周期的不一致。栈中的局部变量不需要专门的垃圾回收，方法入栈，局部变量创建，方法出栈，局部变量清除。而堆中有专门的垃圾回收机制，局部内部类虽然在方法中，但是它的内存是分配在堆中的，而方法在栈中出栈时，在堆中内部类对象并不会立刻被回收，如果内部类可以修改方法中的局部变量的话，一旦方法已经出栈，而内部类的对象仍然存在被其它引用或者在堆中没有被垃圾回收，局部内部类对象访问一个已不存在的局部变量，这个时候就会出现矛盾。所以这就规定内部类只能访问方法中final修饰的局部变量，不能修改局部变量，访问其实也是局部变量的副本，编译器会将外部的final变量在编译阶段就作为内部类的成员变量写入内部类中，所以访问外部的final变量，在编译器就放入内部类了，与外部无关，使得访问安全。

#### 枚举类

1. 枚举类在后台实现时，实际上是转化为一个继承了java.lang.Enum类的实体类，原先的枚举类型变成对应的实体类型，enum A变成了个class  A。若原来有构造函数，则在此基础上添加String s和int i两个参数，生成新的构造函数。

2. 类中会添加若干字段来代表具体的枚举类型。 枚举类中所有的枚举值都是类的静态常量。

    ```
    public static final A VALUEA;
    public static final A VALUEB;
    ```

3. 类中添加一段static代码段，来初始化枚举中的每个具体类型，初始化时会对所有的枚举值对象进行第一次初始化，即分别调用构造方法。并将所有具体类型放到一个$VALUE数组中，以便用序号访问具体类型。

    ```
    static{
    	VALUEA = new A("VALUEA",0);
    	VALUEB = new A("VALUEB",0);
    	$VALUES = new A[]{VALUEA, VALUEB};
    }
    ```

```
Q. 执行结果为（Compiles fine and output is prints”It is a account type”thrice followed by”FIXED”）
enum AccountType{
    SAVING, FIXED, CURRENT;
    private AccountType(){
        System.out.println(“It is a account type”);
    }
}
class EnumOne{
    public static void main(String[]args){
        System.out.println(AccountType.FIXED);
    }
}
```

#### 继承/实现

​		子类继承父类全部的成员（属性和方法）。static修饰的方法也可被继承，可以在子类中直接访问，但不具备多态的性质。接口中的static方法，实现该接口的子类中不会有该静态方法，必须用接口名调用。私有成员会被隐式继承，但不能被访问。

​		实际子类对象同时存在子、父两变量内存空间，分别用this、super标识。super标识的成员仍是在父类中定义的。

```
Q. 运行结果为（Car）
class Car extends Vehicle{
    public static void main (String[] args{
        new  Car(). run();
    }
    private final void run(){
        System. out. println ("Car");
    }
}
class Vehicle{
    private final void run(){
        System. out. println("Vehicle");
    }
}

A. 父类的private方法子类不可见，所以虽然用final修饰，但子类里的该方法不算重写，只是简单的一个本类私有方法，所以访问的还是子类的run()，不会产生编译错误。
```

#### 多态

​		不同类的对象对同一消息作出不同的响应叫做多态。同一消息可以根据发送对象的不同而采用多种不同的行为方式。可以用于消除类型之间的耦合关系，Spring 的核心就是多态和面向接口编程。

##### 分类

​		编译时多态：方法的重载。运行时多态：方法的覆盖。

##### 存在的条件

​		存在继承关系、子类重写父类的方法、父类引用指向子类。

##### 实现原理

​		多态是指一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。Java 多态允许父类引用变量指向子类对象，引用变量发出的方法调用到底是哪个对象实现的方法，必须在由程序运行期间才能决定。

##### 方法绑定

​		java 中的方法调用有静态绑定和动态绑定之分，静态绑定指的是我们在编译期就已经确定了会执行那个方法的字节码，而动态绑定只有在运行时才能知晓。

​		Java 中的静态方法、私有方法以及 final 修饰的方法的调用，都属于静态绑定，对于重载的实例方法的调用，也是采用静态绑定。方法调用动作会被编译成静态调用指令，该指令对应常量池中方法的符号引用。运用的是静态多分派，即根据静态类型确定对象，因此不是根据new的类型确定调用的方法，依据类而直接使用。

```
在多态情况下，静态函数调用时编译和运行看左边，所以子父类存在同名静态函数访问的是父类，子类并不能覆盖父类的方法，所以子类不一定能够继承和覆盖Parent类的static方法。
```

​		Java 对于普通方法运用的是动态单分配，是根据new的类型确定对象，从而确定调用的方法。调用动态绑定的实现主要依赖于JVM的方法表，记录当前类以及所有父类的可见方法字节码在内存中的直接地址。但通过类引用调用和接口引用调用的实现则有所不同。总体而言，当某个方法被调用时，JVM 首先要查找相应的常量池，得到方法的符号引用，并查找调用类的方法表以确定该方法的直接引用，最后才真正调用该方法。

```
Q. 判断对错。在java的多态调用中，new的是哪一个类就是调用的哪个类的方法。（×）
```

#### 静态导入

​		JDK5+后，static import 包名.类名.方法名;，导入到方法那一级，就好像该静态方法定义在使用的类中一样。调用时不需再用类名.方法名()，可直接用方法名()。

​		普通的导包只可以导到当前层，不可以再导入包里面的包中的类。

```
Q. 要导入java/awt/event下面的所有类，叙述正确的是？（C）
A：import java.awt.*和import java.awt.event.*都可以
B：只能是import java.awt.*
C：只能是import java.awt.event.*
D：import java.awt.*和import java.awt.event.*都不可以
```

### 对象

#### 创建对象的方式

1. 用 new 关键字创建
2. 调用对象的 clone 方法
3. 利用反射，调用 Class 类的或者是 Constructor 类的 newInstance()方法。
4. 用反序列化，调用 ObjectInputStream 类的 readObject()方法。

```
只有new和反射会调用构造方法。构造方法的作用是完成对象的初始化。当程序执行到new操作符时，首先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化。而clone和反序列化，对象的初始化并不是通过构造函数完成的，而是读取别的内存区域中的对象的各个域来完成。
```

#### 创建对象的步骤

1. 类加载检查，先检查对象所属类是否已被加载、解析、初始化过，如果没有，先进行类加载过程。
2.  分配内存，为对象分配内存，分配方式有指针碰撞（标记整理）和空闲列表（标记清除）。
3. 初始化为 0 值，将内存空间除了对象头都初始化为 0 值。
4. 设置对象头，类的元数据信息，对象哈希码，对象年龄等。
5. 执行 init 方法，对对象真正初始化。

#### 对象的内存布局

1. 对象头：
    1. 运行时数据（哈希码，GC 年龄，锁状态等）。
    2. 类型指针，指向类元数据。

2. 实例数据：实例数据是对象真正的有效信息，对其填充部分起占位作用。
3. 对象对齐：

#### 对象访问形式

1. 使用句柄：在内存中开辟句柄池，栈中的引用变量，指向句柄池中的句柄地址，指向堆中的实例对象数据，和方法区的对象类型数据。
2. 直接使用指针：栈中的引用对象变量，直接指向堆中的对象，其中堆中的对象头又指向方法区中的对象类型数据。

### 泛型

​		泛型虽有擦除机制，但仍可在运行时用反射动态获取泛型实际类型。

### 异常

​		一个异常将终止整个程序。一条语句可以同时处理多个不是子父类关系的异常，用 | 隔开异常类型。若在主方法上使用throws，表示主方法里不需进行异常处理，异常交由JVM进行默认处理，此时程序中断执行。开发中一般不在主方法上用throws，因为如果程序出现了错误，也希望可以正常结束调用。

​		catch语句块中的return语句，只是把结果写入一片内存空间，方法还未执行完毕，所以会继续执行finally，若finally中没有return语句，则没有改变内存空间里的结果，返回值仍是catch中的，若finally中有return语句，则重新将结果保存到了内存空间并返回。

​		若运行中出现异常，由JVM自动根据异常的类型实例化一个与之类型匹配的异常类对象。若当前语句无异常处理，则交由JVM进行默认异常处理，输出异常信息，结束程序的执行。若当前语句存在异常的捕获操作，由try语句捕获产生的异常类实例化对象，将异常对象按顺序与每一个catch进行匹配，若匹配成功，则使用当前catch语句进行异常处理，并执行finally之后的语句；若匹配失败，则此异常交由JVM进行默认异常处理，输出异常信息结束程序的执行。

```
Q. 下面代码运行结果是（finally语句块 和是：43）
public class Test{ 
    public int add(int a,int b){   
         try {
             return a+b;      
         } 
        catch (Exception e) {  
            System.out.println("catch语句块");
         }
         finally{ 
             System.out.println("finally语句块");
         }
         return 0;
    } 
     public static void main(String argv[]){ 
         Test test =new Test(); 
         System.out.println("和是："+test.add(9, 34)); 
     }
}

A. return有两个作用，1是返回值，2是结束运行，就是不再执行后面的代码，所以不执行return 0。
方法内部的return 0没有在编译期间抛出不可达的语句，是因为第一个return是写在try块中的，因此编译器推断这句可能得不到执行，所以最后的return 0是可能达到的，不会出现编译错误。
```

```
Q. 下面代码的输出结果是什么（12）
try{
	int i = 100 / 0;
	System.out.print(i);
}catch(Exception e){
	System.out.print(1);
	throw new RuntimeException();
}finally{
	System.out.print(2);
}
System.out.print(3);

A. try catch是直接处理，处理完成之后程序继续往下执行，throw则是将异常抛给它的上一级处理，程序便不往下执行了。本题的catch语句块里面，打印完1之后，又抛出了一个RuntimeException，程序并没有处理它，而是直接抛出，因此执行完finally语句块之后，程序终止了。
```

### I/O

#### 继承关系

![image-20200419155013267](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200419155013267.png)

![image-20200419154958610](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200419154958610.png)

#### transient关键字

​		用transient修饰的属性不会写入磁盘中，作用是赋默认值。

#### 序列化

​		java 序列化是指把 java 对象转换为字节序列的过程，而 java 反序列化是指把字节序列恢复为 java对象的过程。

​		序列化是把对象转换成有序字节流，以便在网络上传输或者保存在本地文件中。

​		反序列化是客户端从文件中或网络上获得序列化后的对象字节流后，根据字节流中所保存的对象状态及描述信息，通过反序列化重建对象。利用序列化实现远程通信，可以在网络上传送对象的字节序列。在进程间传递对象，永久性保存对象。

​		JDK 类库中序列化的步骤 ：只有实现了 Serializable 或 Externalizable 接口的对象才能被序列化，否则抛出异常。如果类 a 仅仅实现了 Serializable 接口，则ObjectOutputStream 采用默认的序列化方式，对 a 对象的非 transient 实例变量进行序列化；ObjectInputStream 采用默认的反序列化方式，对 a 对象的非 transient 实例变量进行反序列化。没有添加 serialVersionUID 可能会导致反序列化失败。继承了一个已经实现序列化接口的父类，并且与父类有重复的属性，在反序列化的时候就会导致重复的属性数据丢失。

#### 节点流/处理流

​		按照流是否直接与特定的地方（如磁盘、内存、设备等）相连，分为节点流和处理流两类。

##### 节点流

​		可以从或向一个特定的地方（节点）读写数据，常用的节点流：

1. 对文件进行处理的节点流：FileInputStream 、FileOutputStrean、FileReader、FileWriter。
2. 对字符串进行处理的节点流：StringReader、StringWriter。
3. 对内存中数组进行处理的节点流：ByteArrayInputStream、ByteArrayOutputStream、CharArrayReader、CharArrayWriter。
4. 对管道进行处理的节点流：PipedInputStream、PipedOutputStream、PipedReader、PipedWriter。

##### 处理流

​		是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写，如BufferedReader。处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。

1. 缓冲流：BufferedInputStrean、BufferedOutputStream、BufferedReader、BufferedWriter。增加缓冲功能，避免频繁读写硬盘。
2. 转换流：InputStreamReader、OutputStreamReader。实现字节流和字符流之间的转换。
3. 数据流：DataInputStream、DataOutputStream。提供将基础数据类型写入到文件中，或者读取出来。

##### RandomAccessFile

​		可以附加或更新文件

##### 流的关闭顺序

1. 一般情况下是：先打开的后关闭，后打开的先关闭。
2. 另一种情况：看依赖关系，如果流a依赖流b，应该先关闭流a，再关闭流b。例如，处理流a依赖节点流b，应该先关闭处理流a，再关闭节点流b。
3. 可以只关闭处理流，不用关闭节点流。处理流关闭的时候，会调用其处理的节点流的关闭方法。

## JDK1.8新特性

### 接口

#### 默认方法

​		位于接口内，非抽象方法。接口声明默认方法时，需提供该方法的具体实现。子类可重写或不重写默认方法，若子类实现了接口中的默认方法，则调用时使用子类中的具体实现，否则使用接口中定义的默认实现。

##### 作用

​		解决接口的修改与现有的实现不兼容的问题

##### 冲突

若实现的多个接口中有相同的默认方法，解决方法有二：

1. 重写接口的默认方法

2. super调用指定接口的默认方法，接口名.super.默认方法名()

#### 静态方法

​		接口可以声明静态方法，必须提供其实现。

##### 作用

​		可以把常用的工具方法直接写在接口上。不需要实例化，便于直接使用，节省内存空间。

#### 接口与抽象类的区别

​		Java8中的接口相比于以前的版本更加接近抽象类。接口里定义的变量只能是公共的静态的常量，抽象类中的变量一般是普通变量。接口允许多继承，却没有成员变量。抽象类可以继承成员变量，却不能多继承。

### Stream

#### Stream API

​		Stream，流，是一个来自数据源的元素队列，以一种声明的方式处理数据，元素是特定类型的对象，将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。 Java中的Stream并不会存储元素，而是按需计算。

#### 数据源

​		流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。

#### 聚合操作

​		类似SQL语句一样的操作， 比如filter、map、reduce、find、match、sorted、sum等。和以前的Collection操作不同， Stream操作还有两个基础的特征：

1. **Pipelining**：中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
2. **内部迭代**：以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

#### 生成流

stream()：为集合创建串行流。

parallelStream()：为集合创建并行流。

```
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl"); 
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());

List<Integer> transactionsIds = widgets.stream()
             .filter(b -> b.getColor() == RED)
             .sorted((x,y) -> x.getWeight() - y.getWeight())
             .mapToInt(Widget::getWeight)
             .sum();
```

### Lambda表达式

#### 函数式接口（SAM接口）

​		Single Abstract Method interfaces。接口内只能有一个抽象方法，接口内可有多个默认方法，默认方法不是抽象方法，有一个默认的实现。接口声明上可以添加@FunctionalInterface注解，加上后，自动进行编译级错误检查，当接口不符合函数式接口的定义时将会报错。函数可以作为变量、参数、返回值和数据类型。

 		函数式接口默认继承Object类，包含了来自java.lang.Object里对public抽象方法的实现。这些Object类中的抽象方法在函数式接口中均有@Override注解声明，不被当作抽象方法。

​		可使用Lambda表达式来表示该接口的一个实现，本质上是将函数的实现直接转换为了一个声明语句的定义，极大简化了原有的实现方式。Java8之前一般用匿名类。

##### 实现

```
//定义函数式接口
@FunctionalInterface
interface GreetingService {
       String sayMessage(String message);
}
//接口实现
GreetingService greetService = message -> "Hello " + message;
//调用
System.out.println(greetService.sayMessage("world"));
```

```
//作为变量
public class FunctionTest implements Function<String,String>{
	@Override
	public String apply(String t){
		return t+"ok";
	}
	public void test(){
		String res=andThen(str->str.toUpperCase()).apply("test");//TESTOK
	}
}
```

##### 常见函数式接口

1. java.lang.Runnable,
2. java.util.Comparator,
3. java.util.concurrent.Callable
4. java.util.function包下的接口，如Consumer、Predicate、Supplier、Function等。

#### 应用场景

##### 代替Runnable

```
//使用Lambda表达式创建线程
Runnable r = () -> {
    //to do something
};
Thread t = new Thread(r);
t.start();
	
//或者使用更加紧凑的形式
Thread t = new Thread(() -> {
    //to do something
});
t.start;
```

##### 列表迭代

```
//使用Lambda表达式对一个列表的每一个元素进行操作
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.forEach(x -> System.out.println(x));

//如果只需要调用单个函数对列表元素进行处理，那么可以使用更加简洁的方法引用代替Lambda 表达式
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.forEach(System.out::println);
```

##### 事件监听

```
//ActionListener接口
public void actionPerformed(ActionEvent e) {
	//handle the event
}

//使用Lambda表达式后
button.addActionListener(e -> {
    //handle the event
});
```

##### Map映射

```
//使用Stream对象的map方法将原来的列表经由Lambda表达式映射为另一个列表，并通过collect方法转换回List类型
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> mapped = numbers.stream().map(x -> x * 2).collect(Collectors.toList());
mapped.forEach(System.out::println);
```

##### Reduce聚合

​		reduce 操作，就是通过二元运算对所有元素进行聚合，最终得到一个结果。例如使用加法对列表进行聚合，就是将列表中所有元素累加，得到总和。因此，我们可以为 reduce 提供一个接收两个参数的 Lambda 表达式，该表达式就相当于一个二元运算。

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream().reduce((x, y) -> x + y).get();
System.out.println(sum);//15
```

##### Predicate接口

​		java.util.function 包中的 Predicate 接口可以很方便地用于过滤。若需要对多个集合对象采用不同的过滤条件进行过滤，并执行相同的处理逻辑，那么可以将这些相同的操作封装到filter之类的自定义方法中，接收的参数为集合+Predicate类型的过滤条件，filter方法中使用Predicate条件对象.test(集合元素)进行过滤。由调用者提供过滤条件，分别调用filter方法，以lambda表达式的形式传入集合+过滤条件的二元组，以便重复使用。

```
//不使用Predicate接口，对于每一个集合对象，都需要编写过滤条件和处理逻辑
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<String> words = Arrays.asList("a", "ab", "abc");
	numbers.forEach(x -> {
    if (x % 2 == 0) {
        //process logic
    }
})
words.forEach(x -> {
    if (x.length() > 1) {
        //process logic
    }
})

//使用Predicate接口，将相同的处理逻辑封装到filter方法中，重复调用。
public static void main(String[] args) {
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    List<String> words = Arrays.asList("a", "ab", "abc");
	filter(numbers, x -> (int)x % 2 == 0);
    filter(words, x -> ((String)x).length() > 1);
}
public static void filter(List list, Predicate condition) {
    list.forEach(x -> {
        if (condition.test(x)) {
            //process logic
        }
    })
}

//filter 方法也可写成：
public static void filter(List list, Predicate condition) {
    list.stream().filter(x -> condition.test(x)).forEach(x -> {
        //process logic
    })
}
```

## 数据结构

### 树

1. 若一棵树有n个节点，则有n-1条边。
2. 树的高度=树的深度
3. 节点的深度：该节点到根节点唯一路径的长
4. 节点的高度：该节点到叶子节点最长路径的长

#### 满二叉树

​		只有度为0和2的节点

#### 完全二叉树

​		从上到下，从左到右，依次排列。具有n个节点的完全二叉树的深度为log2(n)+1，向下取整。

#### 完美二叉树

​		每一层节点数达到最大值

#### 二叉排序树

​		BinarySearchTree，左子树中所有节点的关键字都比根节点小，右子树中所有节点的关键字都比根节点大。每棵子树都是二叉排序树，节点的关键字不能相等。

#### 平衡二叉树

​		左子树和右子树的高度相差最多为1，左右子树均是平衡二叉树。

#### 字典树

​		根节点不包含字符，除根节点外每一个节点都只包含一个字符。从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。每个节点的所有子节点包含的字符都不相同。

#### 红黑树

1. 每个节点或者是黑色，或者是红色。
2. 根节点是黑色。
3. 每个空的叶子节点是黑色。
4. 如果一个节点是红色的，则它的子节点必须是黑色的。
5. 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点，确保没有一条路径会比其他路径长出一倍，因此红黑树是相对接近平衡的二叉树。它的时间复杂度是 O(logN)，一棵含有 n 个节点的红黑树的高度至多为 2log(n+1)。

### 队列

#### 应用场景

​		缓存、广度优先搜索

#### 阻塞队列

​		enqueue：若队满，就阻塞，直到队不满，再添加元素，然后返回。

​		dequeue：若队空，就阻塞，直到队不空，再删除元素，然后返回。

##### BlockingQueue 

​		无法向BlockingQueue中插入null，BlockingQueue将抛出NullPointerException。具有4组不同的方法用于插入、移除以及对队列中的元素进行检查。如果请求的操作不能得到立即执行的话，每个方法的表现也不同。这些方法如下：

|      | 抛异常     | 特定值   | 阻塞    | 超时                      |
| ---- | ---------- | -------- | ------- | ------------------------- |
| 插入 | add(o)     | offer(o) | put(o)  | offer(p,timeout,timeunit) |
| 移除 | remove(o)  | poll(o)  | take(o) | poll(timeout,timeunit)    |
| 检查 | element(o) | peek(o)  |         |                           |

抛异常：如果试图的操作无法立即执行，抛一个异常。

特定值：如果试图的操作无法立即执行，返回一个特定的值(常常是 true / false)。

阻塞：如果试图的操作无法立即执行，该方法调用将会发生阻塞，直到能够执行。

超时：如果试图的操作无法立即执行，该方法调用将会发生阻塞，直到能够执行，但等待时间不会超过给定值。返回一个特定值以告知该操作是否成功(典型的是true / false)。

###### 阻塞同步

使用lock锁的多条件（condition）阻塞控制。使用BlockingQueue封装了根据条件阻塞线程的过程，而我们就不用关心繁琐的await/signal操作了。

###### Condition

对于notEmpty条件对象则是用于存放等待或唤醒调用take方法的线程，告诉他们队列已有元素，可以执行获取操作。同理notFull条件对象是用于等待或唤醒调用put方法的线程，告诉它们，队列未满，可以执行添加元素的操作。takeIndex代表的是下一个方法(take，poll，peek，remove)被调用时获取数组元素的索引，putIndex则代表下一个方法（put, offer, or add）被调用时元素添加到数组中的索引。

##### ArrayBlockingQueue

​		put()方法将指定的元素插入此队列的尾部，如果该队列已满，则等待可用的空间。

###### 数据结构

​		有界的阻塞队列，其内部实现是将对象放到一个数组里。容量设定好后一旦初始化，大小就无法修改。

###### 锁

​		ArrayBlockingQueue内部的阻塞队列是通过重入锁ReenterLock和Condition条件队列实现的，所以ArrayBlockingQueue中的元素存在公平访问与非公平访问的区别，对于公平访问队列，被阻塞的线程可以按照阻塞的先后顺序访问队列，即先阻塞的线程先访问队列。而非公平队列，当队列可用时，阻塞的线程将进入争夺访问资源的竞争中，也就是说谁先抢到谁就执行，没有固定的先后顺序。

######  手写ArrayBlockingQueue

```
//属性
public class ArrayBlockingQueue<E> extends AbstractQueue<E> implements BlockingQueue<E>, java.io.Serializable {
	private static final long serialVersionUID = -817911632652898426L;
    //底层是一个Object数组
    final Object[] items;
    //表示下一个要获取的元素的下标
    int takeIndex;
    //表示下一个要放入队列的元素下标
    int putIndex;
    //存放队列中元素的个数
    int count;
   	//保证并发访问安全
    final ReentrantLock lock;
    private final Condition notEmpty;
    private final Condition notFull;
    //迭代器
    transient Itrs itrs = null;

//构造方法
public ArrayBlockingQueue(int capacity,boolean fair){
	if (capacity <= 0){
		throw new IllegalArgumentException();
	}
	//创建数组  
	this.items = new Object[capacity];
	//创建锁和阻塞条件
	lock = new ReentrantLock(fair);  
	notEmpty = lock.newCondition();
	notFull = lock.newCondition();
}

//添加元素的方法，put方法，阻塞时可中断
public void put(E e) throws InterruptedException {
	checkNotNull(e);
	final ReentrantLock lock = this.lock;
	lock.lockInterruptibly();//该方法可中断
	try {
		//当队列元素个数与数组长度相等时，无法添加元素
		while (count == items.length)
			//将当前调用线程挂起，添加到notFull条件队列中等待唤醒
			notFull.await();
		enqueue(e);//如果队列没有满直接添加。。
	}finally{
		lock.unlock();
	}
}

//移除元素的方法
public E take() throws InterruptedException{
	final ReentrantLock lock = this.lock;
	lock.lockInterruptibly();
	try {
		while (count == 0)
			notEmpty.await();
		return dequeue();
	}finally{
		lock.unlock();
	}
}

//出队的方法
private E dequeue() {
	final Object[] items = this.items;
	@SuppressWarnings("unchecked")
	E x = (E) items[takeIndex];
	items[takeIndex] = null;
	if (++takeIndex == items.length){
		takeIndex = 0;
	}
	count--;
	if (itrs != null){
		//同时更新迭代器中的元素数据
		itrs.elementDequeued();
	}
	notFull.signal();
	return x;
}

//add方法实现，间接调用了offer(e)
public boolean add(E e){
	if (offer(e)){
		return true;
	}
	else{
		throw new IllegalStateException("Queue full");
	}
}

//offer方法
public boolean offer(E e){
	//检查元素是否为null
	checkNotNull(e);
	final ReentrantLock lock = this.lock;
	//加锁
	lock.lock();
	try {
		//判断队列是否满
		if (count == items.length){		
			return false;
		}
		else {
			//添加元素到队列
			enqueue(e);
			return true;
		}
	}
	finally{
		lock.unlock();
	}
}

//入队操作
private void enqueue(E x){
	//获取当前数组
	final Object[] items = this.items;
	//通过putIndex索引对数组进行赋值
	items[putIndex] = x;
	//索引自增，如果已是最后一个位置，重新设置 putIndex = 0;
	if (++putIndex == items.length)
	putIndex = 0;
	count++;//队列中元素数量加1
	//唤醒调用take()方法的线程，执行元素获取操作。
	notEmpty.signal();
}
```

##### LinkedBlockingQueue

###### 数据结构

​		线程安全的阻塞队列，实现了先进先出等特性。内部以一个链式结构(链接节点)对其元素进行存储。这一链式结构可以选择一个上限。如果没有定义上限，将使用 Integer.MAX_VALUE 作为上限。每个添加到LinkedBlockingQueue队列中的数据都将被封装成Node节点，添加的链表队列中，其中head和last分别指向队列的头结点和尾结点。额外的Node对象，可能在长时间 内需要高效并发地处理大批量数据时，对于GC可能存在较大影响。如果存在添加速度大于删除速度时候，有可能会内存溢出，这点在使用前希望慎重考虑。

###### 锁

​		ArrayBlockingQueue实现的队列中的锁是没有分离的，即添加操作和移除操作采用的同一个ReentrentLock锁，LinkedBlockingQueue实现的队列中的锁是分离的，其添加采用的是putLock，移除采用的则是takeLock，添加和删除操作并不是互斥操作，可以同时进行，这样能大大提高队列的吞吐量，也意味着在高并发的情况下生产者和消费者可以并行地操作队列中的数据，以此来提高整个队列的并发性能。

### 栈

#### 应用场景

​		深度优先搜索、逆序、括号匹配、表达式求值、浏览器的前进和后退

### 链表

#### 应用场景

##### 缓存淘汰策略

FIFO：First In First Out，删除最先载入的元素。

LFU：Least Frequently Used，删除使用频率最少的元素。一般不使用该方法，因为原来经常使用的元素，频率很高，而现在要使用的元素，频率却低。可以使用HashMap+双向链表实现，现成的有LinkedHashMap。

```java
public class LRUCache {
    class DqueueNode {
        int key;
        int value;
        DqueueNode prev;
        DqueueNode next;

        public DqueueNode() {
        }

        public DqueueNode(int k, int v) {
            key = k;
            value = v;
        }
    }

    private Map<Integer, LRUCache.DqueueNode> cache;
    private DqueueNode head;
    private DqueueNode tail;
    private int capacity;
    private int size;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        cache = new HashMap<Integer, LRUCache.DqueueNode>();
        head = new DqueueNode();
        tail = new DqueueNode();
        head.next = tail;
        tail.prev = head;
    }

    public void put(int key, int value) {
        DqueueNode node = cache.get(key);
        //元素已存在，更新值
        if (node!=null) {
            node.value = value;
            moveToTail(node);
        }
        //新元素，判断容量，添加到map和队尾，更新size
        else {
            //容量已满，需淘汰位于队首的最久未使用的缓存
            if (size>=capacity) {
                cache.remove(head.next.key);
                removeHead();
            }
            //新元素添加到队尾
            node=new DqueueNode(key,value);
            addToTail(node);
            cache.put(key,node);
            size++;
        }
    }

    public int get(int key){
        DqueueNode node = cache.get(key);
        //元素已存在，新使用，移到队尾
        if (node!=null) {
            moveToTail(node);
            return node.value;
        }
        return -1;
    }

    private void addToTail(DqueueNode node) {
        node.prev=tail.prev;
        node.next=tail;
        tail.prev.next=node;
        tail.prev=node;
    }

    private void moveToTail(DqueueNode node) {
        node.prev.next=node.next;
        node.next.prev=node.prev;
        addToTail(node);
    }

    private  void removeHead() {
        head.next.next.prev=head;
        head.next=head.next.next;
    }
}
```

LRU：Least Recently Used，删除最近最少使用的元素。LRU可用链表实现，在尾结点添加元素，说明离尾结点越远的元素，使用时间越久远。可以使用小根堆priorityQueue或嵌套双向链表实现。

1. 小根堆：给每个元素加个时间戳，每次将访问频数最少里时间戳最早的放到小根堆堆顶，有现成的PriorityQueue默认小根堆。
2. 嵌套双向链表：给每一个频数建一条双向链表，链表里按添加顺序排列。将频数链表按频数大小以双向链表的形式串联。

#### 单链表

​		用时间换空间

#### 双链表

​		用空间换时间

#### 循环链表

## 类集

### 类集

#### StringBuffer

​		默认初始容量16，线程安全。

​		String、StringBuffer、StringBuilder都是final修饰的。当final修饰的为引用变量时，在赋值后其指向地址无法改变，但对象内容可以改变。final修饰类只是限定类不可被继承，而非限定了其对象是否可变，所以StringBuffer虽是final，但值却可以改变。

#### StringBuilder

​		线程不安全，在不需考虑线程安全的情况下，性能优于StringBuffer。

#### Object

​		Object类可接受全部引用数据类型的数据（数组、接口、类），所有对象（包括数组）都实现这个类的方法。接口不会继承任何类，自然不会继承Object，但接口属于引用数据类型，所以可以用Object接收。

#### Arrays

​		复制的效率System.arraycopy>clone>Arrays.copyOf>for循环。

1. System类源码中给出了arraycopy方法，是native本地方法，肯定是最快的。
2. Arrays.copyOf源码中调用了System.arraycopy，多了一个步骤，肯定就不是最快的。

#### Properties

​		持久的属性集，底层数据结构为Hashtable，线程安全，可把数据写入到流中，可从流中加载数据。键和值需是String类型，键和值不能为null，重写了toString。

#### Iterator

在用迭代器遍历一个集合对象时，如果遍历过程中对集合对象的内容进行了修改（增加、删除、修改），则会抛出 Concurrent Modification Exception。迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 modCount 变量。集合在被遍历期间如果内容发生变化，就会改变 modCount 的值。每当迭代器使用 hashNext()/next()遍历下一个元素之前，都会检测 modCount 变量是否为 expectedmodCount 值，是的话就返回遍历；否则抛出异常，终止遍历。

##### 快速失败

​		当多个线程对集合进行结构上的改变的操作时，有可能会产生 fail-fast 机制。这个机制是防止多线程并发访问 list 可能带来错误，所以抛出异常提醒一下。
​		Fail-Fast机制，List 维护一个 modCount 变量，add、remove、clear 等涉及了改变 list 元素的个数的方 法 都 会 导 致 modCount 的 改 变 。 如 果 判 断 出 modCount != expectedModCount, 抛 出ConcurrentModificationException 异常，从而产生 fail-fast 机制。

​		HashMap的迭代器(Iterator)是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。

```
enumeration线程安全
```

##### 安全失败

​		fail—safe，采用安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。java.util.concurrent 包下的容器都是安全失败，可以在多线程下并发使用，并发修改。fail-safe 机制有两个问题：

1. 需要复制集合，产生大量的无效对象，开销大。
2. 无法保证读取的数据是目前原始数据结构中的数据。

### Collection/Collections

​		Collection是集合类的一个顶级接口，其直接继承接口有List、Set、Queue。

​		Collections则是集合类的一个工具类/帮助类，其中提供了一系列在collection上进行操作或返回collection的静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作，返回由指定collection支持的新collection，以及少数其他内容。如果为此类的方法所提供的collection或类对象为null，则这些方法都会抛出 NullPointerException。

![Collection  List  Set  Queue  ArrayList  Vector  EnumSet  SortedSet  HashSet  Deque  Stack  T reset  LinkedHashSet  LinkedList  PriorityQueue ](data:image/png;base64,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)

|                   | 线程安全       | null key | null 值 | 数据结构         |
| ----------------- | -------------- | -------- | ------- | ---------------- |
| ArrayList         | ×              |          | √       | 动态数组         |
| LinkedList        | ×              |          | √       | 双向链表         |
| Vector            | √ synchronized |          | √       | 数组             |
| HashSet           | ×              |          | √ 一个  | HashMap          |
| LinkedHashSet     | ×              |          | √ 一个  | HashMap+双向链表 |
| TreeSet           | ×              |          | ×       | TreeMap          |
| HashMap           | ×              | √ 一个   | √       |                  |
| HashTable         | √ synchronized | ×        | ×       |                  |
| LinkedHashMap     | ×              | √ 一个   | √       | HashMap+双向链表 |
| ConcurrentHashMap | √              | ×        | ×       |                  |
| TreeMap           | ×              | ×        | √       | 红黑树           |

#### List

​		有序的集合，有索引。

##### SubList()方法

```
List list = new ArrayList();
list.add("hello");
list.add("world");
list.add("java");
List subList = list.subList(1, 3);
System.out.println(subList);    //[world, java]
subList.set(1, "javaSE");
System.out.println(subList);    //[world, javaSE]
System.out.println(list);    //[hello, world, javaSE]
//list中的元素也被替换成了javaSE，说明subList和原集合的数据是共用的，即视图技术，返回的List和传入的list数据共享。
```

##### CopyOnWriteArrayList

​		相当于线程安全的 ArrayList，它实现了 List 接口，支持高并发。线程安全机制是通过 volatile 和互斥锁实现的。增删改时，先获取互斥锁，然后创建新数组，复制到 volatile 数组中，释放锁，在copy的新数组上进行修改，不会影响 COWIterator 中的数据，修改完成之后改变原有数据的引用指向修改好的容器即可。保证了两个容器可以同时读，只有查找效率高，用于读多写少的并发场景，代价是产生大量的对象。

##### ArrayList

​		底层实现是Object 数组，默认长度是10，扩容创建新的数组，长度为原来的1.5倍，增删会造成数组元素的移动，支持随机访问。

###### 手写ArrayList

```
//顺序映像实现的线性表

public class MyArrayList<E> implements MyList<E> {
    private static final int DEFAULT_CAPACITY = 10;
    private static final int MAX_CAPACITY = Integer.MAX_VALUE - 8;
    private E[] elements;
    private int size;
    private int modCount; // 统计集合结构修改的次数

    @SuppressWarnings("unchecked")
    public MyArrayList() {
        elements = (E[]) new Object[DEFAULT_CAPACITY];
    }

    @SuppressWarnings("unchecked")
    public MyArrayList(int initialCapacity) {
        if (initialCapacity < 0 || initialCapacity > MAX_CAPACITY) {
            throw new IllegalArgumentException("initialCapacity=" + initialCapacity);
        }
        elements = (E[]) new Object[initialCapacity];
    }

     //在末尾添加元素
    @Override
    public boolean add(E e) {
        add(size, e);
        return true;
    }

     //在指定索引位置添加元素，如果该位置有元素，那么元素往后移。
    @Override
    public void add(int index, E element) {
        checkIndexForAdd(index);
        if (size == elements.length) {
            // 扩容
            int minCapacity = size + 1;
            int newLength = calculateCapacity(minCapacity);
            grow(newLength);
        }
        //添加元素，逐个后移。
        for (int i = size; i > index; i--) {
            elements[i] = elements[i - 1];
        }
        elements[index] = element;
        size++;
        modCount++;
    }

    @SuppressWarnings("unchecked")
    private void grow(int newLength) {
        E[] newArr = (E[]) new Object[newLength];
        for (int i = 0; i < size; i++) {
            newArr[i] = elements[i];
        }
        elements = newArr;
    }

    private int calculateCapacity(int minCapacity) {
        if (minCapacity > MAX_CAPACITY || minCapacity < 0) {
            throw new ArrayStoreException("elements too many");
        }
        // 一定能存下minCapacity的数据
        int newLength = elements.length + (elements.length >> 1);
        //newLength<0代表newLength超过了Integer_MaxSize
        if (newLength > MAX_CAPACITY || newLength < 0) {    
            newLength = MAX_CAPACITY;
        }
        // newLength可能小于minCapacity
        return newLength > minCapacity ? newLength : minCapacity;
    }

    private void checkIndexForAdd(int index) {
        if (index < 0 || index > size()) {
            throw new IndexOutOfBoundsException("index=" + index + ", size=" + size);
        }
    }

    //清除集合中所有的元素
    @Override
    public void clear() {
        for (int i = 0; i < size(); i++) {
            elements[i] = null;
        }
        size = 0;
        modCount++;
    }

    //判断集合中是否有与对象o相等的元素
    @Override
    public boolean contains(Object o) {
        return indexOf(o) != -1;
    }

    //获取指定索引位置的元素
    @Override
    public E get(int index) {
        checkIndex(index);
        return elements[index];
    }

    private void checkIndex(int index) {
        if (index < 0 || index >= size()) {
            throw new IndexOutOfBoundsException("index=" + index + ", size=" + size);
        }
    }

    //返回第一个与指定对象相等的元素的索引
    @Override
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++) {
            //允许存储null元素，但null不可用equals方法，所以需分情况。
                if (elements[i] == null) return i;    
            }
        } 
        else {
            for (int i = 0; i < size; i++) {
                if (o.equals(elements[i])) return i;
            }
        }
        return -1;
    }

    //判断集合中是否有元素
    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    @Override
    public MyIterator<E> iterator() {
        return new Itr();
    }

    @Override
    public MyIterator<E> iterator(int index) {
        checkIndexForAdd(index);
        return new Itr(index);
    }

    //返回最后一个与指定对象相等的元素的索引
    @Override
    public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size - 1; i >= 0; i--) {
                if (elements[i] == null) return i;
            }
        } 
        else {
            for (int i = size - 1; i >= 0; i--) {
                if (o.equals(elements[i])) return i;
            }
        }
        return -1;
    }

    //删除指定索引位置的元素
    @Override
    public E remove(int index) {
        checkIndex(index);
        E oldValue = elements[index];
        for (int i = index; i < size - 1; i++) {
            elements[i] = elements[i + 1];
        }
        size--;
        modCount++;
        return oldValue;
    }

    //删除第一个与指定对象相等的元素
    @Override
    public boolean remove(Object o) {
        int index = indexOf(o);
        if (index == -1) return false;
        remove(index);
        return true;
    }

    //设置指定索引位置的元素
    @Override
    public E set(int index, E element) {
        checkIndex(index);
        E oldValue = elements[index];
        elements[index] = element;
        return oldValue;
    }

    //求集合中元素的个数
    @Override
    public int size() {
        return size;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < size; i++) {
            if (i != 0) {
                sb.append(", ");
            }
            sb.append(elements[i]);
        }
        return sb.append("]").toString();
    }

    private class Itr implements MyIterator<E> {
        // 属性
        private int cursor; // 下一个元素的索引位置
        private int expModCount = modCount;    //使用迭代器中集合被修改的次数，将其初始化为集合操作中集合被修改的次数。
        private int lastRet = -1; // 最近一次返回元素的索引，如果没有返回元素lastRet=-1
        //当调用Iterator进行遍历操作时，如果有其他线程修改list会出现modCount!=expectedModCount的情况，就会报并发修改异常java.util.ConcurrentModificationException。

        // 构造方法
        public Itr() {
        }

        public Itr(int index) {
            cursor = index;
        }

        //在cursor索引位置添加元素，如果该位置有元素，则该元素往后移
        @Override
        public void add(E e) {
            checkConModification();
            MyArrayList.this.add(cursor, e);
            expModCount = modCount;
            cursor++;
            lastRet = -1;
        }

        private void checkConModification() {
            if (expModCount != modCount) {
                throw new ConcurrentModificationException();
            }
        }

        @Override
        public boolean hasNext() {
            return cursor != size; 
        }

        @Override
        public boolean hasPrevious() {
            return cursor != 0; 
        }

        //将cusor往后移动一个位置, 并且返回越过的元素
        @Override
        public E next() {
            // 判断迭代器是否有效
            checkConModification();
            if(!hasNext()) {
                throw new NoSuchElementException();
            }
            lastRet = cursor;
            return elements[cursor++];
        }

        @Override
        public int nextIndex() {
            return cursor;
        }

        @Override
        public E previous() {
            checkConModification();
            if(!hasPrevious()) {
                throw new NoSuchElementException();
            }
            lastRet = --cursor;
            return elements[cursor];
        }

        @Override
        public int previousIndex() {
            return cursor - 1;
        }

        //删除索引为lastRet的元素，即最近一次next遍历返回的元素。
        @Override
        public void remove() {
            checkConModification();
            if(lastRet == -1) {
                throw new IllegalStateException();
            }
            MyArrayList.this.remove(lastRet);
            expModCount = modCount;
            cursor = lastRet; 
            lastRet = -1;
        }

        //将索引为lastRet的元素更新成新的元素e
        @Override
        public void set(E e) {
            checkConModification();
            if(lastRet == -1) {
                throw new IllegalStateException();
            }
            elements[lastRet] = e;
            lastRet = -1;
        }
    }
}
```

##### LinkedList

​		底层双向链表，内部类Node(val,next,pre)是链表的节点，也可以被当作堆栈、队列或双端队列进行操作，不支持随机访问，增删元素时间不受元素位置影响，时间复杂度O(1)。

##### Vector

​		底层实现为Object 数组，默认长度是10，若设置了增长系数capacityIncrement ，每次扩容增加这个数，否则扩容为 2 倍，支持随机访问。线程安全。

##### Stack

​		Stack继承了Vector，线程安全。

#### Set

##### HashSet

​		继承AbstractSet类，同时也实现set接口。

###### 唯一性

​		当把对象加入HashSet时，HashSet会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入的对象的hashcode值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同hashcode值的对象，这时会调用equals()方法来检查hashcode相等的对象是否真的相同。如果两者相同，HashSet就不会让加入操作成功。

###### hashCode()与equals()

​		hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。equals方法被覆盖过，则hashCode方法也必须被覆盖。规定如下：

1. 如果两个对象相等，则hashcode一定也是相同的
2. 两个对象相等，对两个equals方法返回true
3. 两个对象有相同的hashcode值，它们也不一定是相等的

##### TreeSet

###### 有序性

​		根据创建set时提供的Comparator进行排序，若没提供Comparator，则需元素实现Comparable接口，类里重写compareTo(元素类型 元素名)方法，默认情况下按照元素的自然顺序进行排序。

###### 唯一性

​		支持排序的类集，利用Comparable接口实现重复元素的判断，需要比较所有属性。

​		非排序集合消除重复元素，依靠重写Object类的hashCode()和equals()。

#### Queue

​		模拟队列的特性，不允许随机访问，窄化了对LinkedList的方法的访问权限。

### Map

​		Map集合的数据结构只针对键有效，跟值无关。

#### 符号表

​		将键映射到值的对象，根据索引（键）查找值，索引可以是任意数据类型。一个映射不能包含重复的键，每个键最多只能映射到一个值。

#### WeakHashMap

​		继承AbstractMap，也实现了Map接口。跟hashmap的区别是，WeakHashMap的key是弱引用的，当某个键不再正常使用时，会从WeakHashMap中自动移除。

#### HashMap

​		无序的符号表。

##### 底层数据结构

​		数组+链表。

​		数组长度为2^n。为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。Hash 值的范围值-2147483648到2147483647，前后加起来大概40亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个40亿长度的数组，内存是放不下的，所以这个散列值是不能直接拿来用的。用之前还要先对数组的长度进行取模运算，得到的余数才能是用来存放的位置也，即对应的数组下标。取余(%)操作中如果除数是2的幂次则等价于被除数与其除数减一的与(&)操作，也就是说 hash%length==hash&(length-1)的前提是 length 是2的 n 次方，采用二进制位操作 &，相对于%能够提高运算效率。

```
//下面这个方法保证了HashMap总是使用2的幂作为哈希表的大小
static final int tableSizeFor(int cap) {
	int n = cap - 1;
	n |= n >>> 1;
	n |= n >>> 2;
	n |= n >>> 4;
	n |= n >>> 8;
    n |= n >>> 16;
    return(n<0)?1:(n>=MAXIMUM_CAPACITY)?MAXIMUM_CAPACITY:n+1;
}
```

##### hash算法

​		理论上，若数组长度为素数，则元素更易平均分布。实际上，数组的长度为素数，对hash表的性能影响并不大。通常数组的长度不用素数，而是2^n。

##### hash冲突

###### 开放地址法

​		当冲突发生时，使用某种探测技术，如线性探测、随机探测、不同的关键字使用不同的探测距离，在散列表中形成一个探测序列。沿此序列逐个单元地查找，直到找到给定的关键字，或者找到空。ThreadLocalMap使用开放地址法解决哈希冲突。

1. 线性探测法
2. 平方探测法

###### 拉链法

​		JDK1.8之后在解决哈希冲突时有了较大的变化，当链表长度大于等于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。当长度较小时，则会将红黑树退化成链表。

##### 查询优化

​		在平常我们用HashMap的时候，HashMap里面存储的key是具有良好的hash算法的key（比如String、Integer等包装类），冲突几率自然微乎其微，此时链表几乎不会转化为红黑树，但是当key为我们自定义的对象时，我们可能采用了不好的hash算法，使HashMap中key的冲突率极高，但是这时HashMap为了保证高速的查找效率，就引入了红黑树来优化查询了。

​		红黑树是为了解决二叉查找树的缺陷，二叉查找树在某些情况下会退化成线性结构。最坏时间复杂度为O(N)。而平衡二叉树AVL的查找时间复杂度维持在O(logN)，但执行一次插入或删除之类的会改变树的平衡性的操作时最多需要O(logN)次旋转，使得插入和删除操作的时间复杂度为O(2logN)。红黑树RBT不是高度平衡的，因此插入和删除操作改变树的平衡性的概率要远小于AVL，一旦需要旋转，插入一个结点最多只需要旋转2次，删除最多3次，虽然变色的时间复杂度为O(logN)，但实际上变色操作由于简单，所需要的代价很小。

###### 临界值8

​		通过源码我们得知HashMap源码作者通过泊松分布算出，当桶中结点个数为8时，出现的几率是亿分之6的，因此常见的情况是桶中个数小于8的情况，此时链表的查询性能和红黑树相差不多，因为转化为树还需要时间和空间，所以此时没有转化成树的必要。

​		亿分之6这个几乎不可能的概率是建立在什么情况下的 答案是：建立在良好的hash算法情况下，例如String，Integer等包装类的hash算法、如果一旦发生桶中元素大于8，说明是不正常情况，可能采用了冲突较大的hash算法，此时桶中个数出现超过8的概率是非常大的，可能有n个key冲突在同一个桶中，此时再看链表的平均查询复杂度和红黑树的时间复杂度，就知道为什么要引入红黑树了。例如hash算法写的不好，一个桶中冲突1024个key，使用链表平均需要查询512次，但是红黑树仅仅10次，红黑树的引入保证了在大量hash冲突的情况下，HashMap还具有良好的查询性能。

##### 唯一性

###### put()

1. 判断table是否为空。
2. 若table为空，开辟空间创建数组，addEntry()，return null。
3. 若table非空，根据key算hash，调用key对象的hash方法。
4. 根据hash和table.length算index
5. 若该位置没有元素则直接插入`key-value`。
6. 若该位置有元素，逐个遍历。如果该位置存了一个`TreeNode`，说明是一个红黑树，执行树的插入操作。如果是一个链表，则遍历链表。
7. 判断hash值是否相等，若遍历结束hash仍未相等，则addEntry()，return null。若hash相等则判断是否是同个对象，若是同个对象则替换并返回旧值。若不是同个对象则调用equals()。
8. 插入之后size ++，如果`size > threshold`，进行扩容（具体扩容逻辑后面会讲）。如果当前链表长度大于`TREEIFY_THRESHOLD`，转换成红黑树结构。

###### get()

​		获取对象时，我们将key传递给get，他调用hashcode计算hash从而得到bucket位置，并进一步调用equals()方法确认键值对。

##### 扩容

​		HashMap会根据当前存储空间bucket的占用情况自动调整容量。Node[] table的初始化长度length默认值是16，Load factor负载因子默认值是0.75，threshold是HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。结合负载因子的定义公式可知，threshold就是在此Load factor和length(数组长度)对应下允许的最大元素数目，为了防止链表太长影响效率，超过这个数目就重新resize(扩容)，扩容后的HashMap容量是之前容量的两倍，rehash操作是重建内部数据结构，当底层的数组进行扩容后要重新计算索引位置，元素原有的顺序则会被完全打乱，所以不能保证映射的顺序不变。

##### 负载因子

​		元素个数/数组长度，即链表的平均长度。默认的负载因子0.75是对空间和时间效率的一个平衡选择，建议大家不要修改，除非在时间和空间比较特殊的情况下，如果内存空间很多而又对时间效率要求很高，可以降低负载因子Load factor的值；相反，如果内存空间紧张而对时间效率要求不高，可以增加负载因子loadFactor的值，这个值可以大于1。加载因子过高虽然减少了空间开销，但同时也增加了查询成本（在大多数 HashMap 类的操作中，包括 get 和 put 操作，都反映了这一点）。在设置初始容量时应该考虑到映射中所需的条目数及其加载因子，以便最大限度地减少 rehash 操作次数。如果初始容量大于最大条目数除以加载因子，则不会发生 rehash 操作。如果很多映射关系要存储在 HashMap 实例中，则相对于按需执行自动的 rehash 操作以增大表的容量来说，使用足够大的初始容量创建它将使得映射关系能更有效地存储。

 		size这是HashMap中实际存在的键值对数量。注意和table的长度length、容纳最大键值对数量threshold的区别。而modCount字段主要用来记录HashMap内部结构发生变化的次数，主要用于迭代的快速失败。强调一点，内部结构发生变化指的是结构发生变化，例如put新键值对，但是某个key对应的value值被覆盖不属于结构变化。

##### 线程不安全原因

###### JDK1.7 

​		Hashmap在插入元素过多的时候需要进行Resize扩容，Resize的条件是 HashMap.Size >= Capacity * LoadFactor。并发下，执行扩容时transfer()方法 可能会造成元素之间会形成一个循环链表。链表头插法的会颠倒原来一个散列桶里面链表的顺序。在并发的时候原来的顺序被另外一个线程a颠倒了，而被挂起线程b恢复后拿扩容前的节点和顺序继续完成第一次循环后，又遵循a线程扩容后的链表顺序重新排列链表中的顺序，最终形成了环。这样当获取一个不存在的key时，计算出的index正好是环形链表的下标时就会出现死循环。

######  JDK1.8

​		在发生hash碰撞，不再采用头插法方式，而是直接插入链表尾部，因此不会出现环形链表的情况，但是在多线程的情况下仍然不安全。判断是否出现hash碰撞时，若没发生碰撞则会直接插入元素，假设两个线程A、B都在进行put操作，并且hash函数计算出的插入下标是相同的，当线程A执行完后由于时间片耗尽导致被挂起，而线程B得到时间片后在该下标处插入了元素，完成了正常的插入，然后线程A获得时间片，由于之前已经进行了hash碰撞的判断，所有此时不会再进行判断，而是直接进行插入，这就导致了线程B插入的数据被线程A覆盖了，从而线程不安全。

​		除此之外，++size时，A、B这两个线程同时进行put操作时，假设当前HashMap的zise大小为10，当线程A从主内存中获得size的值为10后准备进行+1操作，但是由于时间片耗尽只好让出CPU，线程B拿到CPU还是从主内存中拿到size的值10进行+1操作，完成了put操作并将size=11写回主内存，然后线程A再次拿到CPU并继续执行(此时size的值仍为10)，当执行完put操作后，还是将size=11写回内存，此时，线程A、B都执行了一次put操作，但是size的值只增加了1，所有说还是由于数据覆盖又导致了线程不安全。并发环境下推荐使用 ConcurrentHashMap 。

##### HashMap/HashTable

###### null支持

​		hashmap的key可以有一个null，value可以为Null，而hashtable不可以存储null。HashMap在put的时候会调用hash()方法来计算key的hashcode值，可以从hash算法中看出当key==null时返回的值为0。因此key为null时，hash算法返回值为0，不会调用key的hashcode方法。

​		HashTable存入的value为null时，抛出NullPointerException异常。如果value不为null，而key为空，在执行到int hash=key.hashCode()时同样会抛出NullPointerException异常。

###### 初始容量/扩容大小

​		创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。

​		创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小（HashMap 中的tableSizeFor()方法保证）。也就是说 HashMap 总是使用2的幂作为哈希表的大小。

###### 底层数据结构

​		JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

###### 线程安全

​		HashTable 线程安全，方法都是 sychronized 修饰，锁住整个 hash 表，并发效率低下。hashMap 线程不安全。

#### TreeMap

​		有序的符号表，底层是红黑树（广义上的平衡二叉树，即保证树高/添加/删除元素的时间复杂度为log(n)即可。），可以保证键的排序和唯一性。元素需可比较，即实现Comparable接口的元素，或者传入一个Comparator比较器，具体取决于使用的构造方法。

### AQS

​		AQS，AbstactQueuedSynchronizer，同步队列器，是一个构建锁和同步器的框架，使用 AQS 能够简单有效的构造出大量应用广泛的同步器，从而控制多线程有序的访问共享资源。如 ReentrantLock独占锁、ReentrantReadWriteLock读写锁、Semaphore信号量（共享锁）等的基础类都是AbstractQueuedSynchronizer。

#### CLH

​		Craig, Landin, and Hagersten，同步FIFO队列。虚拟的双向队列，即不存在队列实例，通过一个个的Node组成，靠节点与节点之间的 pre 和 next 关系维护。 每个Node都是对没有获取到锁的线程的封装，这个Node是AbstractQueuedSynchronizer里面的内部类。当线程没有成功获取锁的时候，会将这个线程加入到队列中，然后当线程释放锁后，会从队列中唤醒一个线程重新占有锁。

#### 原理

​		如果被请求的共享资源空闲，则将当前请求线程设为有效的工作线程，并且将共享资源设置为锁定状态。如果请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 锁队列实现的。AQS 将每条请求共享资源的线程封装成一个 CLH 锁队列的一个节点来实现锁的分配，将暂时获取不到的线程放入队列中，线程通过CAS操作来改变state的值，如果成功说明这个线程拿到锁了，如果不成功就放入队列中挂起，等待它的前一个线程来唤醒它重新对state进行CAS操作来获取锁。

#### Node

##### 属性

​		等待队列中每个线程被封装为一个 Node 实例。

1. volatile Thread thread：当前Node所代表的线程

2. volatile int watiStatus：节点所处的同步状态。将线程是否成功通过CAS操作改变state的值，作为这个线程是否获取到锁的依据。一般都是默认state等于0的时候，这个时候锁是没有被其他线程获取的，通过CAS操作改变了state的值，如果改变成功说明获取到了锁，如果改变不成功，说明没有获取到锁。

    ```
    //表示
    static final int CANCELLED =  1;
    //表示后继者的线程需要被唤醒
    static final int SIGNAL    = -1;
    //表示线程正在等待条件
    static final int CONDITION = -2;
    //表示下一个acquireShared应无条件传播
    static final int PROPAGATE = -3;
    ```

3. volatile Node prev：节点的前驱节点

4. volatile Node next：节点的后继节点

5. Node nextWaiter：标记当前CLH队列是独占锁队列还是共享锁队列。Node类型的常量SHARED、EXCLUSIVE用于设置nextWaiter，用于表示当前节点是共享的，还是互斥的，分别用于共享锁和独占锁。

    ```
    //标记表示节点正在共享模式中等待
    static final Node SHARED = new Node();
    //标记表示节点正在独占模式下等待
    static final Node EXCLUSIVE = null;
    
    //若nextWaiter=SHARED，则CLH队列是“共享锁”队列；
    //若nextWaiter=EXCLUSIVE，(即nextWaiter=null)，则CLH队列是“独占锁”队列。
    Node nextWaiter;
    ```

##### 方法

```
final boolean isShared() {
	return nextWaiter == SHARED;
}
```

```
final Node predecessor(){
	Node p = prev;
	if (p == null) throw new NullPointerException();
	else return p;
}
```

```
// Used to establish initial head or SHARED marker
Node() {
}

// Used by addWaiter
Node(Thread thread, Node mode) {   
	this.nextWaiter = mode;
	this.thread = thread;
}

// Used by Condition
Node(Thread thread, int waitStatus) {
	this.waitStatus = waitStatus;
	this.thread = thread;
}
```

### JUC包

#### 原子类

​		在 32 位操作系统中，64 位 的 long 和 double 变量由于会被 JVM 当作两个分离的 32 位来进行操作，所以不具有原子性。原子类基本通过自旋 CAS 来实现，期望的值和现在的值是否一致，如果一致就更新。主要利用 CAS+volatile + native 方法来保证操作的原子性，从而避免同步方法的高开销。

```
 public final boolean compareAndSet(long expect, long update) {
	 return unsafe.compareAndSwapLong(this, valueOffset, expect, update);
 }
```

#### ConcurrentHashMap

##### 底层数据结构

​		数组（Segment） + 数组（HashEntry） + 链表（HashEntry节点）。底层一个Segments数组，存储一个Segments对象，一个Segments中储存一个Entry数组，存储的每个Entry对象又是一个链表头结点。

![在这里插入图片描述](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAo4AAAGkCAYAAAC/0aQtAAAgAElEQVR4AezBCXzT9cH48c8vSdP7Ckg5ErFNAjjLWZDhSbECgilSAZnOMWb7RzdvkSKCbFCfWQUvZIj4oM4XzAGtswEVrEUUOQQUERWpbYUARTApvdvk98v3T9D4dCgKSqGl3/cbcULlYslYBCAAAYicjeKM2TALwawN4idtzBGQIZa4hChfmiEylpaLgA2zEIAABCAAAQjGLhHlognXEpEBAnLEBvHTNsxCQIZY4hL/pXxphoAMscQl/kv50gzB2CWiXPy38qUZAhCAAAQgAAEIQACCsUtEufhW+dIMkbG0XJw01xKRAQIQjF0iysUJbMwRgIAcsUEEbRA5ZIglLnFUuVgyNkMscYnvbBA5IAABCGZtEAHlSzNExtJy8QMbcwRjl4hyUS6WjEUAAhDM2iBOaGOOgByxQfyUcrFkbIZY4hI/b2OOYOwSUS6E2DArQyxxiW9tzBGAAAQgAAEIQAACEIBg1gbxa5UvzRAZS8vFtzaIHHLEBvFD5UszRMbSciHEBpEDAhCAAAQgAAEIQAACEICAHLFBnFj50gzBrA3i55WLJWMRORvFCWwQOWSIJS5xYhtzBOSIDeLkuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNuE224TbbBNus024zTbhNtuE22wTbrNNuM024TbbhNtsE26zTbjNNtFalC/NEIxdIsrFjykXS8ZmiCUu8b0NsxA5G8Ux5UszBCAAAQhmbRDf25gjGLtElIsfs0HkgAAEIDKWloug8qUZglkbRFPlSzMEszYIITaIHHLEBnEyNogccsQGcbxysWQsImej+F750gzBrA3ieOVLMwQgAAEIZm0QkpBE66GIo5BomQ6ydNxf4PE8bjQj0RZs5GGliCHiQQbxczbysFLEEPEgg2h9PBY7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LkIpRSyEQiFatmxJ165dWbhwISkpKSilWLVqFTNnziQ3NxdN07C988473Hfffbz55pu8/fbbXHfdddg0TcM0TUzTpMqiRYuYNm0avXv3ZsqUKbz44ou0bNkSW8+ePWncuDGLFi0iOzuba665hn/9618sXbqUhx9+mLfffpvPP/8cwzAQCGLPRCCIucrKSlwuF5qmIajRCgsLMU0Tj8fDmSg7O5srr7wSt9tNJBJB13VuvvlmHA4HPXr0wO/306dPH/bs2cOwYcOIRCJ899139O/fH4/Hg+28886jT58+KKV47733yM3NpUmTJuzatYvLLrsMl8vFddddx5QpU7ANHz6c9PR0OnbsyPr167ntttvwer00adKERo0aoWkaV155JX379iUQCHDPPfdw4MABtmzZwrBhw9B1nVPVokULTpWmaWiahqZpxJKu68SSpmlomkYsRKNRAoEAFRUVPP/887z11lsUFxfzS2zcuJENGzZgGAaVlZUkJSVha9y4MXFxcQQCARo1akReXh7t27enqKiI0tJSLrzwQjRNwzZ8+HC6du1KUVERGzdupMrrr7/O22+/TXx8PHPmzCEzM5OEhAT69OlDWloaTZs25csvv2TQoEFYlkXz5s2Jj4/Hdvnll9O1a1fS09O5+OKLyc3NZdWqVUyePBlN0xAIYs9EIIg5v9+PZVlomoagxopGoxQVFeFwOPB4PJyJGjVqxIIFC+jQoQNFRUU88MAD2HJycvjb3/5GSkoKW7ZsITU1laSkJLKysnjppZd49913mT17Njav14tpmoTDYTRNIxKJYGvVqhVTp07lnHPOweVyUSU+Ph6bw+HA7/dj0zSNH2rcuDE2l8vFhAkTWLx4MUophgwZwum4++67OVWapuFyudA0jVjRdR3Lsogl0zRxOBycjkAgwMcff8zWrVs5cuQIjRo1YsCAAZx77rnMnz+fX6J3795MmzaNOnXq8MUXX/DYY4+hlOKDDz5g+/btuN1uvv32W/r27UuzZs148MEHefXVV3nppZdYtGgRtsTERGymaRIOh6nyu9/9jpkzZ+JwOHC73dgMw8DtdmMzTZNQKMS/k5SUhM3j8dC3b19WrVqF3++nZ8+eCATVw0QgiLnKykosy0LTNAQ1lt/vJxgMkpCQgGEYnIl0XScuLo66desSjUZxOBzYNm/eTEFBAVOnTmXTpk3Y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Mjt27ODCCy/k8ssv58knn+TCCy/EMAz2huM47Mr999+Pwh4FAgFGjx5NLBZj6dKlvPvuuxx55JH8+Mc/5l9x++23U1VVhe/LL7/k4osvxnf22WcTDod56623uOuuu7jgggvYtGkTRx99NNOnT0dKic80Tb797W8zcuRIpk6dyl//+ldOPPFEfDfffDPz5s3jkEMO4Z577sHXt29ffvCDH/DHP/6RpUuXcsIJJ3DMMccQDAa56qqrePvttwmFQlx11VWEw2Fmz57NZ599RmVlJclkkvLychQU2paBgkK7MgwDy7KIx+O0h02bNrF8+XIGDhyIwj9pampi27Zt1NTU8O/wPI9UKkUgEMCyLA40Rx11FDNnziQ/P5/Vq1czf/58fBs3bmThwoWsW7eO6upqjjvuOA499FBmzpzJxRdfzFlnncUVV1yBb8CAARiGQV5eHk1NTTQbM2YM11xzDYFAgFgshs+yLLp27Yqu6+Tl5ZFIJNiVQYMGYZomvXr1YtCgQbzxxhusWbOGuXPnsrds22ZXZs2ahcJecRyHTCZDNBpl6dKlrFmzhkwmw7667LLLmDJlCr558+bh8zyPZ599lg8++IANGzaQyWSIRCLMmjWL2bNn88QTT3DnnXcybNgwTNOkrKyMQCBAt27dqK+vp9nMmTOZOHEipmnSrKKiAiEE4XCYxsZGdqWiooJYLIZpmowYMYLFixczceJEunTpQklJCQoKbctAQaFdGYaBZVkkEgna07p161D4J3/6058499xz+Xe5rksqlaKgoIADkWVZdO/enYKCArZs2YKu6ziOw6JFixg6dCg33XQTl19+Ob54PM5pp53G8OHDGT9+POeffz4+IQS7EgwGKSwsxDRNNE1jTyzLorGxkWZCCJqdeOKJPPzww0yaNAnTNNlbgUCAXXn22WdR2KN0Os27777Lhx9+yKpVq+jVqxcnnngi559/PieeeCL7qqCggB49euCLxWL4NmzYwE033cTHH3/Mq6++yty5c3Echy5duvDII4/w+OOP8+STTzJkyBB8Qgh2JRaLUVRUREtCCFrTdZ1UKkUzIQTNRo8ezfz580mlUpxyyilomoaCQtsyUFBoV4ZhYFkW8XgchYOS4zgkk0kKCgo4WAghKC0tpaqqitdff51wOIzvb3/7G3PnzsXzPE4++WRisRh78tprr7Fy5Uo0TePyyy9n8uTJ7M5ZZ53Fz3/+c3Zl8ODBNDQ0MGnSJPaFruvsyqRJk1DYpVwux+uvv05VVRUfffQRoVCIG264gW9/+9v069cPy7JoKwUFBVRUVPD973+ffv36IYQgmUzy29/+lpUrV2KaJtOmTUPXdfZkzpw5zJs3D13Xeemll9idyspKHnroIaZPn855551HS127duXII4/k3XffZc6cOSgotD0DBYV2pWkalmWRy+XIZrMEg0EUDiq2bROPxykpKeFAc9hhh/HrX/+aZsOHD+fZZ5/Fd9FFF3HRRRfR2qhRo2jpyiuvpNn48eMZP348vuOPP57ly5fT2vvvv0+zhx9+mGYzZ85k5syZ+Kqrq2nJNE2OOuoo+vTpg0K7cV2X2bNnc++99+K6LjNmzODmm2+mZ8+e/DseffRRWrr22mtptmTJElq77rrraO3vf/87zaqqqmi2ePFiWrvmmmtoNnHiRCZOnIhv4MCBLFq0iGannXYaLRUXFzNx4kRisRgKCm3PQEGh3YVCIXRdJ5FI0LVrVxQOKrZt09DQQPfu3VHYZ++//z7vv/8+FRUVFBcXo9BuNE1jxIgRXHHFFVx66aUccsghdAaJRII333yTt99+m/vvvx8FhfZhoKDQ7sLhMJqmEY/H6dq1KwoHFdu2aWhooLKyEoV9lsvlGDhwICNHjsQ0TRTajRCCU045hVNOOQXTNOkspJQIIZg9eza9evVCQaF9GCgotLtoNIphGNTW1tKvXz8UDiq5XI6amhpKS0tR2Gfjxo1DocOYpklnk5+fz5QpU1BQaF8aCgrtLhqNYhgGtbW1KBx0mpqa2Lp1K4ceeigKCgoKCp2ShoJCu4vFYui6Tk1NDQoHnc8//xxN0+jVqxcKCgoKCp2ShoJCu4tGoxiGQW1tLQoHndWrV9OjRw+i0SgKCgoKCp2SgYJCu4tGo+i6Tk1NDfubbdts27aNRCKBZVn07NmTcDjM/mLbNq7rYlkWUkqqq6spLi7GdV22b99OLpcjPz+f7t27k0wmSSQSlJWV0ZFef/11+vbtS0FBAQoKCgoKnZKGgkK7i8ViGIZBbW0t+9uSJUu46aabePzxx3niiSeorq5mf1q2bBnvvPMOvs2bN3P11VdTW1vLsmXLWLBgAX/4wx+YM2cOK1as4IsvvuAHP/gB27Zto6Ns2bKFZcuWMWTIEAoKClBQUFBQ6JQMFBTaXTQaxTAMampq2J88z2PlypUcf/zxnH/++di2jWEY+P76179yxx13EIlEuO222ygtLeWjjz7i9ttvp6amBtd1+cUvfkE4HOadd95hw4YNJJNJpkyZwpIlS0gkElx22WWMGDGCf/zjH8yePRshBLfffjvl5eXceeeddOvWjVdffZXS0lJuuOEGcrkcd911F5s2beLpp59m1KhRHHHEEZSVldGlSxeOPvpowuEwd999N3/729+YMWMGAwcO5I033mD69Ol0hMceewzDMBg5ciSmaaKgoKCg0CkZKCi0u1gshq7r1NbWsj9pmsagQYN49NFHOeKII6isrMSyLBzH4dprr+XGG2+kurqa733vezz77LNce+21zJs3j7Vr1/Lxxx8zfvx4XnnlFd566y0WLVrEa6+9xuzZs3nxxRfZuXMnjz76KIcddhjf/e53+elPf4qUkqlTp7J8+XK++OILVq1axe9//3tuueUW3nzzTU4//XROPPFEPM/jkksu4frrr2fixIn4IpEI9fX1rF+/njVr1vDDH/4QIQQjRozggw8+4Oyzz8Y0TdrTjh07+N3vfkfv3r0ZM2YMHSmZTNLQ0EBn1tTUhEK7yeVyJBIJTNOks2pqasLzPBQU9o6BgkK7i0QiBAIBamtrkVIihGB/mTBhAsFgkAcffJBAIMDll19ONptl8+bNfPbZZziOw6effsrWrVvxPI/CwkKi0Si2bdPsW9/6FoFAgN69ezN69Gjy8/Pxua7LihUr+PTTT9m5cydSStatW8fOnTvxnXPOOZimyYABA1ixYgWnn346zWzbJpPJUFBQgM91XV544QU++OAD+vXrRywWw1deXs7LL79MJpPBNE3aSyaT4YEHHmDHjh3cfPPNFBQU0JF++ctf8rvf/Y7OrK6uDoV28+qrr7J582Y0TaOzisfjNDQ0oKCwdwwUFNqdYRjk5eVRV1dHOp0mHA6zv5imyfjx4xk9ejR33XUX7733HiNGjMCyLMrLywkGgyxYsIDS0lKmTZvG+eefT2VlJeeeey7NgsEgQgiEEAQCAYQQ+KSUCCEwDIPevXsTCoV48cUXycvLwxeNRvFpmoZt2/iEEDQzTRPDMPAZhsGMGTO44IIL+MMf/sALL7xAv379ME0T0zRpb3/+85958MEHOeOMM5g2bRodpW/fvvznf/4n6XQaBQKBADz5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3HGsWbOG1atXI6Vk0KBBmKbJnqxfv57f//736LrOiBEjGDRoELtz9NFH8+yzz1JcXExrw4cP5/7772f27Nnsix49esDWOlq76KKL+KbK5XLYtk1r6XQaz/NoyXEccrkcLUkpyWazpFIpPvzwQ9577z1eeeUVotEo48aN45xzzuGDDz7gxRdfZF906dKFnj174ovFYvg2bNjArFmzWLNmDa+//jr33XcfjuNQVFTE448/zmOPPcbChQu57bbb8Akh2JW8vDyKi4tpSQhBa7quk06naSaEoNno0aN56KGHSKVSTJw4EU3TUFBoWwYKCvvVkCFDuPfee/E8D4UDUiwWI5vN4jgOB5revXszY8YMIpEIvr59+/KTn/wE0zS54IILWLlyJaZpcvPNN1NYWIimaWzduhXP87j33nuJRqOceuqpBINBfIcddhjFxcX4DjvsMC666CIymQxCCAKBAL5bb72VoqIifJdffjk9e/bEd/3117NixQoMw+DBBx+ktLSUZrFYjCFDhjB48GAU9igQCBAIBGgtEomwN3K5HG+//TZVVVUsXboUXde57LLLOO200xgyZAiRSIS//OUvtIVYLEbv3r2ZOXMmvXr1QghBMpnkoYce4uOPP0YIweTJk9E0jT355S9/yYIFC9A0jeeee47dqaio4JFHHuHSSy/l9NNPp6Vu3boxdOhQ3n//fX72s5+hoND2hPzfUFA4oNWV9ae1wur1KOy1urL+tFZYvZ69cdNNN/HSSy+xaNEi+vTpQ1uoK+vP7gzxUmzZsoVvktdee40333yT2bNnY1kWe+Oss87CcRweWb6W1gqr16OwS88++ywzZswgmUxy4YUXcvPNN1NeXo4QgmazZ8/mT3/6E8888wz9+vXj67iuixACTdPweZ6HT9M0HMdBSommaUgpMQwD13XxPA8hBLquI4TAcRx0XUcIgeu6CCHQNA3HcZBS0sw0TTzPw6dpGlJKPM9D13WklLiui5QSXdfxaZpGs1//+tfU1dUxZ84c9tb48eOprq7mo48+okuXLigo7J6BgoKCwh7k5eWRyWRwHAeFfXbLLbewbds2fvSjH2FZFgrt6sgjj+SWW25h8uTJ9OvXj7ag6zotaZpGM8MwaE3XdXRdpyXDMGim6zrNDMOgNU3TaCaEQNd1fEIIDMOgtaamJpYsWcLrr7/OvHnzUFBoHwYK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)

##### 扩容

###### JDK1.7

```java
//put()方法
1. 确定段的位置，调用Segment中的put()方法
2. 加锁
3. 检查当前Segment数组中包含的HashEntry节点的个数，如果超过阈值就重新hash
4. 再次hash确定放的链表。
5. 扩容时put()只对 Segments对象中的Hashentry数组进行重哈希
6. 在对应的链表中查找是否相同节点，如果有直接覆盖，如果没有将其放置链表尾部
```

​		先判断table的存储元素的数量是否超过当前的threshold=table.length*loadFactor（默认0.75），如果超过就先扩容。先将该Segment段对应的HashEntry数组长度增加一倍，然后遍历原来的旧的table数组，把每一个数组元素也就是Node链表迁移到新的数组里面，迁移完毕后，把新数组的引用直接替换旧的。在迁移链表时用了两个for循环来优化。

​		最大并发个数就是Segment的个数，默认值是16，可以通过构造函数改变，一经创建不可更改，这个值就是并发的粒度，每一个segment下面管理一个table数组，加锁的时候其实锁住的是整个segment，这样设计的好处在于数组的扩容是不会影响其他的segment的，简化了并发设计，不足之处在于并发的粒度稍粗。

###### JDK1.8

​		插入数据时，插入后才判断下一次++size的大小是否会超过当前的阈值，如果超过就扩容。 

​		JDK8的锁粒度更细，将锁的级别控制在了更细粒度的table元素级别，只需锁住这个链表的head节点，不影响其他的table元素的读写。好处在于并发的粒度更细，影响更小，从而并发效率更好，理想情况下talbe数组元素的大小就是其支持并发的最大个数。不足之处在于并发扩容的时候，由于操作的table都是同一个，不像JDK7中分段控制，所以这里需要等扩容完之后，所有的读写操作才能进行，所以扩容的效率就成为了整个并发的一个瓶颈点。本来在一个线程扩容的时候，如果影响了其他线程的数据，那么其他的线程的读写操作都应该阻塞，但JDK8对扩容做了优化，源码里引入了一个ForwardingNode类，在一个线程发起扩容时，会改变sizeCtl的值， 扩容时会判断这个值，如果超过阈值就要扩容，扩容完毕后更新sizeCtl为新数组大小的0.75倍 。在此期间如果其他线程的有读写操作都会判断head节点是否为forwardNode节点，如果是就帮助扩容。 

```java
//sizeCtl：默认为0，用来控制table的初始化和扩容操作
-1：代表table正在初始化  
-N：表示有N-1个线程正在进行扩容操作  
其余情况：
1. 如果table未初始化，表示table需要初始化的大小。  
2. 如果table初始化完成，表示table的容量，默认是table大小的0.75倍  。
```

##### 线程安全底层实现方式

###### JDK1.7

​		ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。一个 ConcurrentHashMap里包含一个 Segment 数组。Segment 的结构和HashMap类似，是一种数组和链表结构，一个 Segment包含一个HashEntry 数组，每个HashEntry是一个链表结构的元素，用于存储键值对数据。Segment继承了ReentrantLock，所以Segment是一种可重入锁，扮演锁的角色。HashEntry首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。每个 Segment 守护着一个HashEntry数组里的元素，当对HashEntry数组的数据进行修改时，必须首先获得对应的 Segment的锁。

###### JDK1.8 

​		ConcurrentHashMap取消了Segment分段锁，采用CAS和synchronized来保证并发安全。数据结构跟HashMap1.8的结构类似，数组+链表/红黑二叉树。synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。Java 8在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为O(N)）转换为红黑树（寻址时间复杂度为O(log(N))）。

##### ConcurrentHashMap/Hashtable

###### 底层数据结构

​		JDK1.7的ConcurrentHashMap底层采用分段的数组+链表实现，JDK1.8 采用的数据结构跟HashMap1.8的结构一样，数组+链表/红黑二叉树。

​		Hashtable和JDK1.8之前的HashMap的底层数据结构类似都是采用数组+链表的形式，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的.

###### 实现线程安全的方式

​		在JDK1.7的时候，ConcurrenHashMap（分段锁） 对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。 到了JDK1.8 的时候已经摒弃了Segment的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 synchronized和CAS来操作。JDK1.6以后 对 synchronized锁做了很多优化，ConcurrenHashMap整个看起来就像是优化过且线程安全的 HashMap，虽然在JDK1.8中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本。

​		Hashtable，使用同一把锁，使用synchronized来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用put添加元素，另一个线程不能使用put添加元素，也不能使用get，竞争会越来越激烈效率越低。

#### ArrayBlockingQueue

#### LinkedBlockingQueue

#### Semaphore

​		通过共享锁实现。Semaphore内部维护了一组虚拟的许可，许可的数量可以通过构造函数的参数指定。访问特定资源前，必须使用acquire方法获得许可，如果许可数量为0，该线程则一直阻塞，直到有可用许可。访问资源后，使用release释放许可。

##### 应用场景

​		可以用来控制同时访问特定资源的线程数量，通过协调各个线程，以保证合理的使用资源。 Semaphore可以用来做流量分流，特别是对公共资源有限的场景，比如数据库连接。假设有个需求要读取几万个文件的数据到数据库中，由于文件读取是IO密集型任务，可以启动几十个线程并发读取，但是数据库连接数只有10个，这时就必须控制最多只有10个线程能够拿到数据库连接进行操作。这个时候，就可以使用Semaphore做流量控制。

#### CountDownLatch

​		通过共享锁实现。CountDownLatch 对象维护一个 count，执行一次 doneSignal.countDown()时，count 减，一直到count 为 0 时， doneSignal.await()的等待线程才能运行。 CountDownLatch 的作用是允许 1 或 N 个线程等待其他线程执行完成后才运行。计数器无法被重置。

```
允许一个线程或多个线程等待特定情况，同步完成线程中其他任务。
例如：百米赛跑，就绪运动员等待发令枪发动才能起步。
```

#### CyclicBarrie

​		通过共享锁实现。调用线程创建n个齐头并进的CyclicBarrier 对象。每个线程执行 cb.await()时，参与者数量加一，当参与者数量达到 SIZE，阻塞的参与线程继续运行。CyclicBarrier允许 N 个线程相互等待，直到到达某个公共屏障点。计数器可以被重置后使用，因此它被称为是循环的 barrier。

```
让指定数量的线程等待期他所有的线程都满足某些条件之后才继续执行。
例如：排队上摩天轮时，每到齐四个人，就可以上同一个车厢。
```

## 反射

### 类加载

#### 类加载过程

**加载**：将.class字节码文件加载到内存，创建一个与之对应的字节码文件对象（Class对象）作为方法区类数据的访问入口。

**链接**：

1. 验证：确保被加载类的正确性，字节码文件不一定由JVM生成，也可由自己编写，这样就不会自动检查编译是否正确，需要验证其正确性。
2. 准备：为静态数据分配内存空间并进行初始化
3. 解析：将类中的符号引用替换成直接引用

**初始化**：静态数据进行显示初始化，执行静态代码块。

#### 加载时机

​		懒加载，需要时才加载进内存。

1. 创建类的对象
2. 访问类的静态变量
3. 调用类的静态方法
4. 初始化子类的时候，父类和子类均被加载。
5. 直接用java命令运行某个主类
6. 使用反射方式来强制创建某个类或接口对应的字节码文件对象

#### 类加载器

​		将.class文件加载到内存中，为之生成对应的Class对象。若加载类的类加载器不同，则这两个类必不相等。从Java虚拟机的角度来说，只存在两种不同的类加载器：一种是启动类加载器（Bootstrap ClassLoader），这个类加载器使用C++语言实现（HotSpot虚拟机中），是虚拟机自身的一部分；另一种就是所有其他的类加载器，这些类加载器都由Java语言实现，独立于虚拟机外部，并且全部继承自java.lang.ClassLoader。

1. Bootstrap ClassLoader：根类加载器
2. Extension ClassLoader：扩展类加载器
3. System/Application ClassLoader：系统（应用）类加载器，负责加载用户类路径ClassPath上所指定的类。

##### 自定义类加载器

1. 继承 ClassLoader。loadClass()在父加载器无法加载类时，会调用自定义类加载器中的findeClass()。
2. 重写 findClass()方法。该方法需实现将指定类名称转换为Class对象。
3. 调用 defineClass()方法。该方法可将字节数组转为Class对象。

#### 双亲委派机制

​		Java程序的类加载器采用双亲委派模型，实现双亲委派的代码集中在java.lang.ClassLoader的loadClass()方法中。一个类加载器收到类加载请求之后，首先判断当前类是否被加载过。已经被加载的类会直接返回，如果没有被加载，首先将类加载请求转发给父类加载器，若父类加载器为空，则一直转发到启动类加载器，只有当父类加载器无法完成时才尝试自己加载，加载失败抛出ClassNotFoundException异常。重载 loadClass()方法会破坏该机制。

**优点**：

1. 可以避免类的重复加载。(相同的类被不同的类加载器加载会产生不同的类)，双亲委派保证了 java 程序的稳定运行。
2. 保证核心 API 不被修改。 

​		例如类java.lang.Object，它存在在rt.jar中，无论哪一个类加载器要加载这个类，最终都是委派给处于模型最顶端的Bootstrap ClassLoader进行加载，因此Object类在程序的各种类加载器环境中都是同一个类。相反，如果没有双亲委派模型而是由各个类加载器自行加载的话，如果用户编写了一个java.lang.Object的同名类并放在ClassPath中，那系统中将会出现多个不同的Object类，程序将混乱。因此，如果开发者尝试编写一个与rt.jar类库中重名的Java类，可以正常编译，但是永远无法被加载运行。

### 反射

#### 反射机制

​		在Java中的反射机制是指在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法。对于任意一个对象，都能够调用它的任意一个方法，这种动态获取信息以及动态调用对象方法的功能称为Java语言的反射机制。

**优点**：

1. 能够运行时动态获取类的实例，大大提高系统的灵活性和扩展性。 
2. 与Java动态编译相结合，可以实现无比强大的功能 

**缺点**：

1. 使用反射的性能较低 
2. 使用反射相对来说不安全 
3. 破坏了类的封装性，可以通过反射获取这个类的私有方法和属性。反射能够改变单例模式，只需要获取私有的构造方法，然后 setAccessible(true),然后通过构造方法创建对象就行。封装的含义和单例的含义不一样，封装是将一组程序封装起来，对外提供接口， 让外部调用人员不必在意里面的实现环节，直接调用即可。而外部人员及时通过反射获取到了里面的一些私有，仍然是不能窥探其实现逻辑的，单纯的调用其私有方法， 没有任何的意义。

#### 原理

​		Java的反射就是利用类加载时加载到jvm中的.class文件来进行各种操作。

#### 作用

1. 在运行时判断任意一个对象所属的类。
2. 在运行时判断任意一个类所具有的成员变量和方法。
3. 在运行时任意调用一个对象的方法
4. 在运行时构造任意一个类的对象
5. 可以通过配置文件来动态配置和加载类，以实现软件工程理论里所提及的类与类，模块与模块之间的解耦。反射最经典的应用是spring框架。

#### 应用场景

1. JDBC：我们在使用 JDBC 连接数据库时使用 Class.forName()通过反射加载数据库的驱动程序，Class.forName('com.mysql.jdbc.Driver.class').newInstance();生成驱动对象实例。
2. Spring：Spring框架也用到很多反射机制，最经典的就是Spring 通过XML配置模式装载Bean的过程（注入属性 调用方法）：
    1. 将程序内所有 XML 或 Properties 配置文件加载入内存中;
    2. Java 类里面解析 xml 或 properties 里面的内容，得到对应实体类的字节码字符串以及相关的属性信息; 
    3. 使用反射机制，根据这个字符串获得某个类的 Class 实例; 
    4. 动态配置实例的属性

3. Web服务器中利用反射调用了Sevlet的服务方法。
4. Eclispe等开发工具利用反射动态刨析对象的类型与结构，动态提示对象的属性和方法。
5. 生成动态代理

6. 编译时的类型由声明该对象时使用的类型决定，运行时的类型由实际赋给对象的类型决定，如：Person p =new Student();，编译时类型为Person，而运行时为Student。程序在运行时还可能接收到外部传入的一个对象，该对象的编译时类型为Object，但程序又需要调用该对象运行时类型的方法。为了这些问题程序需要在运行时发现对象和类的真实信息。然而，如果编译时根本无法预知该对象和类可能属于哪些类，程序只依靠运行时信息来发现该对象和类的真实信息，此时就必须使用反射。

### 注解

#### 元注解

​		解释说明注解的注解，如Target标识注解可以放置的位置，Retenion注解信息保留的最后期限。

#### 注解原理

​		注解本质是一个继承了Annotation接口的特殊接口，其具体实现类是 Java 运行时生成的动态代理类。我们通过反射获取注解时，返回的是 Java 运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用 AnnotationInvocationHandler 的 invoke 方法。该方法会从 memberValues 这个Map 中索引出对应的值。而 memberValues 的来源是Java常量池。

#### 自定义注解

​		自定义注解不能使用和继承其他注解

```
public @interface 注解名{
	...	 //配置项返回值类型只能是基本数据类型、String、Emun、Class、数组。
}
```

## 设计模式

### SOLID原则

S 单一原则： 每个类的职责应该单一，不要在一个类中引入过多的接口。

O 开闭原则：对新增开放，对修改封闭。

L 里氏替代原则：用父类接口去接收实际创建的子类或实现类

I 接口隔离原则

D 依赖倒置原则

### 创建型模式

​		用于描述怎样创建对象，特点是将对象的创建与使用分离。单例、原型、工厂方法、抽象工厂、建造者等。

#### 单例模式

​		Singleton。某个类只能生成一个实例，提供静态方法供他人调用获得实例，由自己维护实例。关键在于构造方法私有化，开放一个静态方法，返回实例对象。适合当一个类只要求生成一个对象，对象需要被共享，类对象需要频繁实例化又被频繁销毁的场景。若未保证安全，多线程时，由于线程切换，可能会造成出现两个实例。

​		优点：在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例，如首页页面缓存，可以省略创建对象所花费的时间，new 操作的次数减少，这将减轻 GC 压力，缩短 GC 停顿时间。避免对资源的多重占用，如写文件操作。

​		缺点：没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

​		线程安全的单例：饿汉式、懒汉式、枚举、静态内部类、双检锁模式

##### 应用

1. Spring中配置文件中定义的bean作用域默认为单例模式。Spring 通 过 ConcurrentHashMap 实现单例注册表的特殊方式实现单例模式。
2. Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。
3. 一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。
4. 创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。

##### 手写单例模式

懒加载，调用时才开始创建实例，启动快，占用资源少。

立即加载，启动时就创建实例，启动慢，可能造成资源浪费。天生线程安全，使用没有延迟。

```
//线程不安全+懒加载。
public class MySingleton1 {
	static MySingleton1 singleton;
	private MySingleton1() {
	}
	public static MySingleton1 getInstance(){
		if (singleton == null){
			singleton = new MySingleton1();
		}
		return singleton;
	}
}
```

```
//线程安全+懒加载 加锁 双重检查锁定 效率低
public class MySingleton2 {
	private volatile static MySingleton2 singleton;
	private MySingleton2() {
	}
	public static MySingleton2 getInstance(){
		if (singleton == null){
			synchronized(MySingleton2.class){
				if (singleton == null){
					singleton = new MySingleton1();
				}
			}
		}
		return singleton;
	}
}
```

```
//线程安全+懒加载 静态内部类实现
public class MySingleton5 {
	private MySingleton5() {
	}
	public static MySingleton5 getInstance(){
		return SingletonHolder.getOutterInstance();
	}
	static class SingletonHolder{
		static MySingleton5 mySingleton5;
		static {
			mySingleton5 = new MySingleton5();
		}
		public static MySingleton5 getOutterInstance(){
			return mySingleton5;
		}
	}
}
```

```
//线程安全+立即加载。
public class MySingleton4 {
	private static MySingleton4 singleton;
	static {
		singleton = new MySingleton4();
	}
	private MySingleton4() {
	}
	public static MySingleton4 getInstance(){
		return singleton;
	}
}
```

##### 手写DCL单例模式

​		Double Checked Locking，双重检查锁定。在懒汉单例模式的基础上加入并发控制，保证在多线程环境下，对外只存在一个对象。

```
public class DclDemo{
    //提供私有的静态属性(存储对象的地址)
    //加入volatile防止其他线程可能访问一个为null的对象
    private volatile static DclDemo instance;

    //构造器私有化(避免外部new构造器)
    private DclDemo() {
    }

    public static DclDemo getInstance() {
        //double检测
        //如果已经有对象，防止再次创建
        if (null != instance) {
        	return instance;
        }
        //有两个线程A B，A进来发现无对象，创建对象
        //此时B也进来，发现无对象，也创建对象，这样就有两个对象
        synchronized(DclDemo.class) {
            if (instance == null) {
                //1.为对象分配内存空间    2.初始化对象    3.返回对象的引用
                //但由于jvm编译器的优化产生的重排序缘故，步骤2、3可能会发生重排序
                //利用volatile的特性即可阻止重排序和解决可见性
                instance = new TestDcl();
            }
        }
        return instance;
    }
}
```

#### 工厂模式

​		首先需要定义一个基类，该类的子类通过不同的方法实现了基类中的方法。然后需要定义一个创建产品对象的工厂类，工厂类可以根据传入的参数动态决定生成不同的子类实例，当得到子类的实例后，开发人员可以调用基类中的方法而不必考虑到底返回的是哪一个子类的实例，实际调用的是实现类，并以父类形式返回。

​		客户端不负责对象的创建，而是由专门的工厂类完成。创建对象时不会对客户端暴露创建逻辑，通过使用一个共同的接口来指向新创建的对象。客户端只负责对象的调用，实现了创建和调用的分离，降低了客户端代码的难度。

​		缺点是，如果增加和减少产品子类，需要修改简单工厂类，违背了开闭原则。如果产品子类过多，会导致工厂类非常的庞大，违反了高内聚原则，不利于后期维护。

​		抽象工厂模式是工厂模式方法的升级版本，工厂方法模式只能产生一种产品，而抽象工厂模式可以生产多个等级的产品。

##### 应用

1. Spring中通过 BeanFactory 或 ApplicationContext 创建 bean 对象实例。BeanFactory 采用了工厂设计模式，负责读取 bean 配置文档，管理 bean 的加载，实例化，维护bean 之间的依赖关系，负责 bean 的生命周期。BeanFactroy 采用的是延迟加载形式来注入 Bean 的，即只有在使用到某个 Bean 时(调用 getBean())，才对该 Bean 进行加载实例化。ApplicationContext 除了提供上述 BeanFactory 所能提供的功能之外，还提供了更完整的框架功能：国际化支持、资源访问，比如访问 URL 和文件、事件机制，同时加载多个配置文件等。ApplicationContext 在解析配置文件时对配置文件中的所有对象都初始化了，getBean() 方法只是获取对象的过程。

#### 建造者模式

​		可链式编程，自由选择所需的建造部分

##### 应用

1. 初始化Cache类的实例时。

#### 原型模式

​		Prototype。用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或相似的新对象。用 Clone()方法直接赋值对象。

### 结构型模式

​		用于描述如何将类或对象按某种布局组成更大的结构。代理、适配器、桥接、装饰、外观、享元、组合。

#### 代理模式

​		Proxy。为某对象提供一种代理以控制对该对象的访问。即客户端通过代理间接地访问该对象，从而限制、增强或修改该对象的一些特性。一定程度了降级了系统的耦合度。使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了 AOP 。

##### 静态代理

```
public class HouseOwner{
	public boolean rentHouse(int money){
		System.out.println("房东收到的钱为："+money);
		if(money>=2000){
			return true;
		}
		return false;
	}
}
public class HouseProxy entends HouseOwner{
	@Override
	public boolean rentHouse(int money){
		System.out.println("中介收到的钱为："+money);
		return super.rentHouse(money-500);
	}
}
```

##### 动态代理

###### JDK动态代理

​		代理对象会对被代理对象的所有方法都增强。可以用.equals(method.getName())选择性的增强指定的方法。需要有接口的实现。通过动态代理获得代理对象的实例Proxy.newProxyInstance。利用拦截器（实现InvocationHandler）加上反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理。

**手写例子**：

```
public void test1(){
	//初始化一个被代理的对象
	RentHouse houseOwner = new HouseOwner();
		
	//通过JDK的动态代理获得一个代理对象的实例
	//Proxy.newProxyInstance返回值为代理对象
		
	//houseOwner.getClass().getClassLoader：委托类类加载器
	//houseOwner.getClass.getInterfaces()：委托类的接口
	//new InvocationHandler()：响应处理器
	RentHouse houseProxy = (RentHouse)Proxy.newProxyInstance(houseOwner.getClass().getClassLoader, houseOwner.getClass.getInterfaces(), new InvocationHandler()){
		@Override
		//代理对象proxy
		//代理对象正在执行的方法
		//代理对象正在执行的方法所对应的参数
		public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
			if("rentHouse".equals(method.getName())){
				args[0]=(int)args[0]-500;
			}
			return method.invoke(houseOwner, args);
			//返回代理对象执行完方法的返回值
		}
	});
	//代理对象去执行方法
	boolean x = houseProxy.rentHouse(2500);
}
```

###### CGLib动态代理

​		基于继承去依赖，可以不实现接口。利用ASM开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。需实现MethodInterceptor接口，重写Intercept()拦截方法。

**手写例子**：

```
//Cglib动态代理，实现MethodInterceptor接口
public class CglibProxy implements MethodInterceptor {
    private Object target;//需要代理的目标对象
    
    //重写拦截方法
    @Override
    public Object intercept(Object obj, Method method, Object[] arr, MethodProxy proxy) throws Throwable {
        System.out.println("Cglib动态代理，监听开始！");
        //方法执行，参数：target目标对象 arr参数数组
        Object invoke = method.invoke(target, arr);
        System.out.println("Cglib动态代理，监听结束！");
        return invoke;
    }
    //定义获取代理对象方法
    public Object getCglibProxy(Object objectTarget){
        //为目标对象target赋值
        this.target = objectTarget;
        Enhancer enhancer = new Enhancer();
        //设置父类,因为Cglib是针对指定的类生成一个子类，所以需要指定父类
        enhancer.setSuperclass(objectTarget.getClass());
        enhancer.setCallback(this);// 设置回调 
        Object result = enhancer.create();//创建并返回代理对象
        return result;
    }
    
    public static void main(String[] args) {
        CglibProxy cglib = new CglibProxy();//实例化CglibProxy对象
        UserManager user =  (UserManager) cglib.getCglibProxy(new UserManagerImpl());//获取代理对象
        user.delUser("admin");//执行删除方法
    }
}
```

##### 应用

1. Spring中的AOP
2. Servlet
3. ServletContext

#### 适配器模式

​		Adapter。将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。主要角色有：

1. 目标接口 Target：当前系统业务所期待的接口，它可以是抽象类或接口。
2. 适配者类 Adaptee：它是被访问和适配的现存组件库中的组件接口。
3. 适配器类 Adapter：它是一个转换器，通过继承或引用适配者的对象，把适配者接口转换成目标接口，让客户按目标接口的格式访问适配者。

##### 应用

1. Spring AOP 的增强或通知(Advice)使用到了适配器模式，与之相关的接口是 AdvisorAdapter。通知有很多类型：前置通知、后置通知、环绕通知、返回通知、异常通知。每种通知都有相对应的拦截器。Spring 预定义的通知要通过对应的适配器，适配成 MethodInterceptor 接口(方法拦截器)类型的对象（如：MethodBeforeAdviceInterceptor 负责适配 MethodBeforeAdvice）。
2. SpringMVC的handlerAdapter。在 Spring MVC 中，DispatcherServlet 根据请求信息调用 HandlerMapping，解析请求对应的 Handler。解析到对应的 Handler（也就是我们平常说的 Controller 控制器）后，开始由 HandlerAdapter 适配器处理。HandlerAdapter 作为期望接口，具体的适配器实现类用于对目标类进行适配，Controller 作为需要适配的类。

#### 装饰器模式

​		Decorator design pattern。作用于对象层，在不改变现有对象结构的情况下，动态的给对象增加一些职责，增强单个对象的能力。相比于使用继承，装饰者模式更加灵活。当我们需要修改原有的功能，又不愿直接去修改原有的代码时，设计一个 Decorator 套在原有代码外面。主要角色：

1. 抽象构件 Component：定义一个抽象接口以规范准备接收附加责任的对象。
2. 具体构件 Concrete Component：实现抽象构件，通过装饰角色为其添加一些职责。
3. 抽象装饰 Decorator：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。
4. 具体装饰 ConcreteDecorator：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。

#####  应用

1. Java IO类中多数都使用了装饰器模式，如bufferedReader和BufferedWriter，增强了Reader和Writer对象，以实现提升性能的Buffer层次的读取和写入。

2. InputStream 家族，InputStream 类下有 FileInputStream，读取文件、BufferedInputStream ，增加缓存，使读取文件速度大大提升，等子类都在不修改 InputStream 代码的情况下扩展了它的功能。

3. Spring 中配置 DataSource 的时候，DataSource 可能是不同的数据库和数据源。可以根据客户的需求在少修改原有类的代码下动态切换不同的数据源？这个时候就要用到装饰者模式(这一点我自己还没

    太理解具体原理)。

### 行为型模式

​		用于描述类或对象之间怎样相互协作共同完成单个对象都无法单独完成的任务，以及怎样分配职责。模板方法、策略、命令、职责链、状态、观察者、中介者、迭代器、访问者、备忘录、解释器。

#### 观察者模式

​		Observer pattern。多个对象间存在一对多关系，当一个对象发生改变时，把这种改变通知给其他多个对象，从而影响其他对象的行为。

##### 应用

1. 很多事件监听器使用了观察者模式，多个对象依赖一个对象，当这个对象发生修改时，多个依赖的对象监听到变化便会修改自身。

2. Spring 事件驱动模型就。比如我们每次添加商品的时候都需要重新更新商品索引，这个时候就可以利用观察者模式来解决这个问题。Spring 事件驱动模型中的三种角色：事件角色、事件监听者角色、事件发布角色。

    1. ApplicationEvent (org.springframework.context 包下)充当事件的角色，这是一个抽象类。
    2. ApplicationListener 充当了事件监听者角色，它是一个接口，里面只定义了一个 onApplicationEvent（ApplicationEvent E）方法来处理 ApplicationEvent。
    3. ApplicationEventPublisher 充当了事件的发布者，它也是一个接口。

    流程：

1. 定义一个事件: 实现一个继承自 ApplicationEv ent，并且写相应的构造函数。
2. 定义一个事件监听者：实现 ApplicationListener 接口，重写 onApplicationEvent() 方法。
3. 使用事件发布者发布消息: 可以通过 ApplicationEventPublisher 的 publishEvent() 方法发布消息。

#### 模板方法模式

​		一种行为设计模式，它定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。 模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤的实现方式。

##### 应用

1. Spring中的jdbcTemplate。一般情况下，我们都是使用继承的方式来实现模板模式，但是 Spring 并没有使用这种方式，而是使用 Callback 模式与模板方法模式配合，既达到了代码复用的效果，同时增加了灵活性。

#### 迭代器模式

​		Iterator。提供一种方法来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。

#### 职责链模式

​		Chain of Responsibility。把请求从链中的一个对象传到下一个对象，直到请求被响应为止。通过这种方式去除对象之间的耦合。

#### 策略模式

​		Strategy。定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的改变不会影响使用算法的客户。

## 锁

​		锁使用不当会出现系统性能问题

### 死锁

​		多线程中的最差情况，多个线程相互占用对方资源的锁，又相互等待对方先释放锁，若无外力干预则这些线程一直处于阻塞的假死状态，形成死锁。

#### 出现原因

1. 互斥
2. 占有资源并且等待其他资源 
3. 不可抢占
4. 相互等待

#### 解决方案

1. 一次性申请所有的资源，不能只占有部分资源。
2. 加锁顺序，线程按照一定的顺序加锁，按照顺序申请资源。
3. 加锁时限，线程尝试获取锁的时候加上一定的时限，超过时限则放弃对该锁的请求，并释放自己占有的锁。
4. 死锁检测

### 活锁

​		多个线程拿到资源却又相互主动将资源释放给别的线程使用，导致资源在多个线程之间跳动却得不到执行，形成活锁。

### 饥饿

​		优先级高的线程一直抢占优先级低的线程的资源，或一个线程一直占用着一个资源而不释放，导致其他线程得不到执行，称为饥饿。

**饥饿与死锁的区别：**饥饿一段时间后线程能够得到执行，如占用资源的线程结束并释放了资源。

### 无锁

​		没有对资源进行锁定，所有的线程均可访问并修改同一个资源，但同时只能有一个线程能修改成功。典型特点是一个修改操作在一个循环内进行，若没有产生冲突，则修改成功并退出，否则继续下一次循环，不断尝试修改共享资源。当多个线程修改同一个值时必定有一个线程能够修改成功，其他失败线程则不断重试直到修改成功。

​		无锁不会出现线程出现的跳跃性问题，在某些场合下非常高效。

#### 应用

​		JDK的CAS原理及应用是无锁的实现。

### 可重入锁

​		Synchronized和ReentrantLock都是可重入锁，锁的可重入性标明了锁是针对线程分配方式而不是针对方法。例如调用Synchronized方法A中可以调用Synchronized方法B，而不需要重新申请锁。

### 乐观锁

​		对于并发间操作产生的线程安全问题持乐观状态，乐观锁认为竞争不总是会发生，因此它不需要持有锁，将比较-替换这两个动作作为 一个原子操作尝试去修改内存中的变量，如果失败则表示发生冲突，那么就应该有 相应的重试逻辑。同一个线程每进入一次，锁的计数器都自增1，所以要等到锁的计数器下降为0时才能释放锁。

### 悲观锁

​		对于并发间操作产生的线程安全问题持悲观状态， 悲观锁认为竞争总是会发生，因此每次对某资源进行操作时，都会持有一个独占的 锁，就像synchronized，不管三七二十一，直接上了锁就操作资源了。

### 读写锁

​		按照数据库事务隔离特性的类比读写锁，在访问统一个资源（一个文件）的时候，使用读锁来保证多线程可以同步读取资源。ReadWriteLock是一个读写锁，通过readLock()获取读锁，通过writeLock()获取写锁。

### 可中断锁

​		可中断是指锁是可以被中断的，Synchronized内置锁是不可中断锁，ReentrantLock可以通过lockInterruptibly方法中断显性锁。例如线程B在等待等待线程A释放锁，但是线程B由于等待时间太久，可以主动中断等待锁。

### 公平锁

​		公平锁尽量以线程的等待时间先后顺序获取锁，等待时间最久的线程优先获取锁。要维护一个队列，后来的线程要加锁，即使锁空闲，也要先检查有没有其他线程在wait，如果有自己要挂起，加到队列后面，然后唤醒队列最前面的线程。这种情况下相比较非公平锁多了一次挂起和唤醒。
所以线程切换的开销，非公平锁效率高于公平锁的原因，因为非公平锁减少了线程挂起的几率，后来的线程有一定几率逃离被挂起的开销。

​		synchronized隐性锁是非公平锁，它无法保证等待的线程获取锁的顺序，ReentrantLook可以自己控制是否公平锁。

### 细粒度锁

​		用多个锁将不同的操作分别锁。

### 分布式锁

### volatile

1. 禁止指令重排，volatile变量之前的变量执行先于volatile变量执行，volatile之后的变量执行在volatile变量之后。
2. volatile为所修饰的共享变量提供了一种免锁机制，并没有为共享变量上锁。
3. 使用volatile相当于告诉JVM某个共享变量随时可能被其他线程修改，需要线程到内存中去读取这个共享变量的值，而不是从缓存中读取。Volatile 变量在汇编阶段，会多出一条 lock 前缀指令，这会导致当前处理器缓存的数据写回到系统内存中，且让其他改数据的处理器缓存失效。保证可见性，即当一个线程修改volatile变量的值时，会被立即更新到内存中，另一个线程就可以实时看到此变量的更新值。
4. 不保证原子性，volatile 的一个重要作用就是和CAS结合 ， 保证了原子性。
5. 修饰共享变量实现伪同步。volatile保证了可见性，相对于没有用volatile修饰的共享变量，大大提高了线程读取到最新数据的可能性，但是volatile不保证原子性，说明线程读到的最新数据可能并不是真正意义上的最新版。
6. volatile不能修饰final变量，volatile修饰的变量随时可能会被修改，而final修饰的变量是不会被修改，因此volatile不能修饰final变量。

### CAS算法

​		Compare and Swap，即比较-替换。假设有三个操作数：内存值 V、 旧的预期值 A、要修改的值 B，当且仅当预期值 A 和内存值 V 相同时，才会将内存值修改为 B 并返回 true，否则什么都不做并返回 false。

​		CAS 一定要 volatile 变量配合，这样才能保证每次拿到的变量是主内存中最新的那个值，否则旧的预期值A对某条线程来说，永远是一个不会变的值 A，只要某次 CAS 操作失败，永远 都不可能成功。

​		加锁或使用 synchronized 关键字带来的性能损耗较大，而用 CAS 可以实现乐观锁，它实际上是直接利用了 CPU 层面的指令，所以性能很高。

​		一个无限循环中，执行一个 CAS 操作，当操作成功，返回 true 时，循环结束；当返回 false 时，接着执行循环，继续尝试 CAS 操作，直到返回 true。

​		尽管CAS机制使得我们可以不依赖同步，不影响和挂起其他线程实现原子性操作，能大大提升运行时的性能，但是会导致一个ABA的问题。如线程一和线程二都取出了主存中的数据为A，这时候线程二将数据修改为B，然后又修改为A，此时线程一再次去进行compareAndSet的时候仍然能够匹配成功，而实际对的数据已经发生了变更了，只不过发生了2次变化将对应的值修改为原始的数据了，并不代表实际数据没有发生变化。这时候前面提到的原子操作AtomicStampedReference/AtomicMarkableReference就很有用了。这允许一对变化的元素进行原子操作。

#### 应用

​		java.util.concurrent包下的Semaphore等、java.util.concurrent.atomic包下面的 Atom类都有CAS算法的应用。

### Synchronized

1. 关键字，可修饰类、方法、代码块，还可作用于静态方法、类或某个实例，但都对程序的效率有很大的影响，因此应该尽量缩小同步范围。 通常没必要同步整个方法，只需要同步关键代码块（操作了共享变量的区域）。
2. Java通过synchronized进行访问的同步，synchronized修饰非静态成员方法和静态成员方法上同步的目标是不同的。非静态方法锁的是实例的对象，多个非静态方法同时加了synchronized，互不影响 。静态方法锁的是类，多个静态方法加了synchronized，如果其中一个方法被锁了，其他的静态方法是拿不到锁的，没法执行，只有等该方法执行结束，其它静态方法才能拿到锁。
3. 保证可见性、原子性、排序性。
4. Java每个对象都有一个对象锁与之相关联，该锁表明对象在任何时候只允许被一个线程锁拥有，当一个线程调用对象的一段synchronized代码时，需要先获取这个锁，然后去执行相应的代码，执行结束之后，释放锁。
5. 在synchronized代码被执行期间，线程可以调用对象的wait()方法，释放对象锁，进入等待状态，并且可以调用notify()方法或者notifyAll()方法通知正在等待的其他线程。notify()方法仅唤醒一个线程（即等待队列中的第一个线程），并允许它去获得锁，notifyAll()方法唤醒所有等待这个对象的线程并允许他们去获得锁，但并不是让所有唤醒线程都去获取到锁，而是让他们去竞争。

#### 同步实现

​		Synchronized 同步的实现，是基于进入退出监视器 Monitor 对象实现的，无论是同步代码块还是同步方法，都是如此；同步代码块，是根据 monitorenter 和 monitorexit 指令实现的，同步方法，是通过设置方法的 ACC_SYNCHRONIZED 访问标志;监视器 Monitor 对象存在于每个对象的对象头中。

#### 锁升级

##### 偏向锁

​		初次执行到synchronized代码块的时候，锁对象变成偏向锁（通过CAS修改对象头里的锁标志位），字面意思是“偏向于第一个获得它的线程”的锁。执行完同步代码块后，线程并不会主动释放偏向锁。当第二次到达同步代码块时，线程会判断此时持有锁的线程是否就是自己（持有锁的线程ID也在对象头里），如果是则正常往下执行。由于之前没有释放锁，这里也就不需要重新加锁。如果自始至终使用锁的线程只有一个，很明显偏向锁几乎没有额外开销，性能极高。

##### 轻量级锁

​		一旦有第二个线程加入锁竞争，偏向锁就升级为轻量级锁（自旋锁）。这里要明确一下什么是锁竞争：如果多个线程轮流获取一个锁，但是每次获取锁的时候都很顺利，没有发生阻塞，那么就不存在锁竞争。只有当某线程尝试获取锁的时候，发现该锁已经被占用，只能等待其释放，这才发生了锁竞争。

​		自旋锁是采用让当前线程不停地的在循环体内执行实现的，当循环的条件被其他线 程改变时才能进入临界区。

​		在轻量级锁状态下继续锁竞争，没有抢到锁的线程将自旋，即不停地循环判断锁是否能够被成功获取。获取锁的操作，其实就是通过CAS修改对象头里的锁标志位。先比较当前锁标志位是否为“释放”，如果是则将其设置为“锁定”，比较并设置是原子性发生的。这就算抢到锁了，然后线程将当前锁的持有者信息修改为自己。

​		长时间的自旋操作是非常消耗资源的，一个线程持有锁，其他线程就只能在原地空耗CPU，执行不了任何有效的任务，这种现象叫做忙等（busy-waiting）。如果多个线程用一个锁，但是没有发生锁竞争，或者发生了很轻微的锁竞争，那么synchronized就用轻量级锁，允许短时间的忙等现象。这是一种折衷的想法，短时间的忙等，换取线程在用户态和内核态之间切换的开销。

##### 重量级锁

​		忙等是有限度的，有个计数器记录自旋次数，默认允许循环10次，可以通过虚拟机参数更改。如果锁竞争情况严重，某个达到最大自旋次数的线程，会将轻量级锁升级为重量级锁（依然是CAS修改锁标志位，但不修改持有锁的线程ID）。当后续线程尝试获取锁时，发现被占用的锁是重量级锁，则直接将自己挂起（而不是忙等），等待将来被唤醒。

```
在读JDK源码时发现ConcurrentHashMap在JDK1.7及以前使用的是ReentranLock，而JDK1.8改用Synchronized，说明现在Synchronized的性能要优些，说明做了大量的优化。

在JDK1.6之前，synchronized直接加重量级锁，很明显现在由于锁升级等得到了很好的优化。其实synchronized的优化我感觉就借鉴了ReenTrantLock中的CAS技术。都是试图在用户态就把加锁问题解决，避免进入内核态的线程阻塞。
```

### Lock接口

#### 成员方法

**void lock()**：以阻塞的方式获取锁，获取成功则立即返回，若别的线程持有锁，当前线程就等待，直到获取到锁之后返回。会忽略interrupt()方法。lock()方法会对Lock实例对象进行加锁，因此所有对该对象调用lock()方法的线程都会被阻塞，直到该Lock对象的unlock()方法被调用。

**void unlock()**：

**tryLock()**：以非阻塞的方式来获取锁，若获取到了锁，立即返回true，否则立即返回false。

**tryLock(long timeout,TimeUnit unit)**：若获取到锁，立即返回true，否则会等待参数给定的时间单元。在等待的过程中，如果获取到了锁，就返回true，如果等待超时则返回false。

**lockInterruptibly()**：若获取了锁，立即返回。若没有获取到锁，当前线程处于休眠状态，直到获取到锁，或者当前线程被别的线程中断，会受到InterruptedException异常。

#### ReentrantLock

​		悲观锁思想实现的，可重入锁。ReentrantLock实现了Lock接口，并且内部有三个内部类Sync、NonFairSyn、FairSync。Sync继承了AQS，有两个子类，公平锁FairSync和非公平锁NonfairSync。这里运用了模板模式，在AbstractQueuedSynchronizer里面实现了加锁和释放锁的抽象操作，具体的实现例如tryAcquire，tryRelease这些方法需靠具体的实现类来实现。

![image-20200502131807606](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200502131807606.png)

![这里写图片描述](https://img-blog.csdn.net/20180728235957195?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTIwMjM1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 构造方法

1. ReentrantLock默认使用非公平锁是基于性能考虑，公平锁为了保证线程规规矩矩地排队，需要增加阻塞和唤醒的时间开销。如果直接插队获取非公平锁，跳过了对队列的处理，速度会更快。

2. 可在申明ReentrantLock时，向构造器里面传入一个true，那ReentrantLock就是一个公平锁。

    ```
    public class ReentrantLock implements Lock, java.io.Serializable {
        private static final long serialVersionUID = 7373984872572414699L;
       // 同步器的引用
        private final Sync sync;
       //非公平锁的构造函数
        public ReentrantLock() {
            sync = new NonfairSync();
        }
        // 公平锁的构造函数
    	public ReentrantLock(boolean fair) {
    	// 三目运算符，如果为true 则为公平锁，反之为非公平锁
            sync = fair ? new FairSync() : new NonfairSync();
        }
    ```

##### 非公平锁和公平锁的区别

1. 非公平锁加锁的时候是直接先去利用CAS操作更改锁的状态值来判断有没有拿到锁，如果没有拿到，再去判断锁的状态值是否等于0，如果等于0再去尝试获取锁。
2. 公平锁加锁的时候会先去获取锁的状态值是否等于0，如果等于0，还要去判断当前队列中是否有排在自己前面的节点，如果没有才去获取锁。

##### 加锁

###### 非公平锁

```
1.直接利用CAS更改同步状态值，如果成功，就将锁的独占线程设置成当前线程，说明获取到锁。
2.更改状态值失败，在tryAcquire方法里面获取状态值，如果状态值等于0，就利用CAS操作更改状态值，成功说明获取到锁。如果状态值不等于0，判断拥有锁的线程是不是当前线程，如果是的，就对状态值加一操作，拿到锁。
3.上述过程中没有拿到锁，就将当前线程封装成Node节点，并快速插入到队尾，如果快速插入失败的话，要是队头为null的话，先初始化一个Node节点作为队尾，再然后将Node节点作为队尾插入。
4.插入队尾完成后，利用无限for循环，先判断当前Node节点的前驱节点是否是队头，如果是的，尝试获取锁，如果获取成功就直接返回，如果失败或者前驱节点不是队头，判断自己的前驱节点的状态是否是SIGNAL，SIGNAL代表可以唤醒后驱节点，如果是的直接返回true，如果不是的，就找到最近的一个状态是SIGNAL的节点作为前驱节点返回true，再将自己挂起，等待唤醒。
5.挂起后，如果被唤醒，判断自己是否被中断了，如果被中断就自己尝试中断自己。
```

1. lock方法实际调用的是NonFairSync里面的lock方法，先直接利用CAS操作尝试去改变state的值，判断state是否是0（表示当前锁未被占用），如果是0则把它置为1，并且设置当前线程为该锁的独占线程，表示获取锁成功。当多个线程同时尝试占用同一个锁时，CAS操作只能保证一个线程操作成功，如果没有成功再调用acquire，公平锁的lock方法和这里不太一样，公平锁的lock不会直接去尝试改变state的值，而是直接调用acquire方法。

2. “非公平”即体现在这里，如果占用锁的线程刚释放锁，state置为0，而排队等待锁的线程还未唤醒时，新来的线程就直接抢占了该锁，那么就“插队”了。

    ```
    //ReentrantLock
    public void lock() {
    	sync.lock();
    }
    ```

    ```
    //NonFairSync
    final void lock() {
    	//尝试直接改变state的值，如果成功说明获取到锁
    	if (compareAndSetState(0, 1))
    		//将锁的独占线程设置为当前线程
    		setExclusiveOwnerThread(Thread.currentThread());
    	else
    		//如果直接尝试改值失败，就进行下面的操作
    		acquire(1);
    }
    ```

3. 我们往下看acquire。

    ```
    //AbstractQueuedSynchronizer
    public final void acquire(int arg) {
    	if (!tryAcquire(arg) && acquireQueued(addWaiter
    	(AbstractQueuedSynchronizer.Node.EXCLUSIVE), arg))
    		//如果没有获取到锁，但是有中断操作，就自己中断
                selfInterrupt();
    ```

4. tryAcquire调用的是NonFairSync的nonfairTryAcquire方法。尝试通过CAS操作改变state值来拿到锁。先判断state是否等于0，等于0说明当前锁没有被占用，利用CAS操作更改state的值，要是更改成功说明获取锁成功。如果state不等于0，说明这个锁已经有线程占有了，就判断占有锁的线程是不是当前线程，要是是的，就对state进行加一操作，这里也体现了ReentrantLock是可重入锁。

    ```
    //AbstractQueuedSynchronizer
    protected final boolean tryAcquire(int acquires) {
    	return nonfairTryAcquire(acquires);
    }
    ```

    ```
    //NonFairSync
    final boolean nonfairTryAcquire(int acquires) {
    	final Thread current = Thread.currentThread();
    	//获取当前锁的状态值，如果是等于0，说明没有线程占有这个锁
    	int c = getState();
    	if (c == 0) {
    		//通过cas改变状态值，如果更改成功，说明取得了锁
    		if (compareAndSetState(0, acquires)) {
    			//将拥有锁的线程替换成当前线程
    			setExclusiveOwnerThread(current);
    			return true;
            }
    	}
    	//如果拥有锁的线程是当前线程，则将状态值加一
    	else if (current == getExclusiveOwnerThread()) {
    		int nextc = c + acquires;
    		if (nextc < 0) // overflow
    			throw new Error("Maximum lock count exceeded");
    		setState(nextc);
    		return true;
    	}
    	return false;
    }
    ```

5. addWaiter方法，由于cas改变state失败，将当前线程封装成Node，并快速添加到队尾，如果快速添加失败，就通过for的无限循环插入到队尾。

    ```
    //AbstractQueuedSynchronizer
    private Node addWaiter(Node mode) {
    	//先将当前线程封装成Node，由于mode是null，所以是独占模式
    	Node node = new Node(Thread.currentThread(), mode);
    	//Try the fast path of enq; backup to full enq on failure
        //获取队尾，通过cas快速替换当前tail节点为node节点
        Node pred = tail;  
        if (pred != null) {
        	node.prev = pred;
        	if (compareAndSetTail(pred, node)) {
    			pred.next = node;
    			return node;
            }
    	}
    	//如果快速插入失败，则利用无限for循环进行插入操作
    	enq(node);
    	return node;
    }
    ```

6. enq方法，常见的CAS操作，就是无限for循环，直到CAS操作成功才跳出for循环。

    ```
    //AbstractQueuedSynchronizer
    private Node enq(final Node node) {
    	for (;;) {
    		Node t = tail;
    		//如果队尾是空的，就先初始化一个节点，并设置为头节点，队尾也指向头节点
    		//然后执行完后再循环一次，队尾就不会为空了
    		if (t == null) { // Must initialize
    			if (compareAndSetHead(new Node()))
    				tail = head;
    		} else {
    			//将node设置为队尾
     			node.prev = t;
    			if (compareAndSetTail(t, node)) {
    				t.next = node;
    				return t;
                }
            }
    	}
    }
    ```

7. acquireQueued方法，成功将Node插入到队尾后，判断这个队尾的前一个Node是不是队头，如果是队头的话尝试获取锁，如果它前面的节点不是队头的话就挂起，等待被唤醒后再次执行判断它前面的Node是不是队头，如果是队头就尝试获取锁，如果获取成功，就将这个节点设置成头节点。

    ```java
    //AbstractQueuedSynchronizer
    final boolean acquireQueued(final Node node, int arg) {
    	boolean failed = true;
        try {
    		boolean interrupted = false;
            // 死循环，直到满足条件退出
            // 首先将当前线程进行阻塞，等待其他线程进行唤醒
            // 其他线程进行唤醒以后，判断当前是否获取资源。此时，可能有其他线程的加入，导致获取失败
            for (;;) { 
            // 获取当前节点的前驱节点
         	    final Node p = node.predecessor();
    			// 如果当前节点的前驱节点是head节点，且尝试获取锁成功
    			if (p == head && tryAcquire(arg)) {
    				// 设置当前的node节点为head节点
    				setHead(node);
    				p.next = null; // help GC
    				failed = false;
    				return interrupted;
    			}
    			// 判断当前线程是否应该阻塞
    			if (shouldParkAfterFailedAcquire(p, node) && parkAndCheckInterrupt())
    				// 如果当前线程被中断，则 parkAndCheckInterrupt()返回为true。
    				interrupted = true;
    		}
    	} finally {
    		if (failed)	
                cancelAcquire(node);
    	}
    }
    ```

8. shouldParkAfterFailedAcquire方法。如果Node的前节点的waitStatus是等于1的，那么就直接可以确认将Node挂起的。如果waitStatus是大于0的，则代表线程被取消，重新设置Node的前驱节点。找到第一个ws<=0的节点，将这个节点设置为Node的前驱节点。默认的Node的ws是null的，所以会将ws设置为SIGNAL，也就是-1。
    ```
    //AbstractQueuedSynchronizer
    private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) {
    	int ws = pred.waitStatus;
    	if (ws == Node.SIGNAL)
    		return true;
    	if (ws > 0) {
    		do {
    			node.prev = pred = pred.prev;
    		} while (pred.waitStatus > 0);
    		pred.next = node;
    	} else {
    		compareAndSetWaitStatus(pred, ws, Node.SIGNAL);
        }
    	return false;
    }
    ```

 9. parkAndCheckInterrupt方法，将当前线程挂起，挂起后如果被唤醒，就判断当前线程是否被中断了。

    ```
    //AbstractQueuedSynchronizer
    private final boolean parkAndCheckInterrupt() {
    	LockSupport.park(this);
    	return Thread.interrupted();
    }
    ```

###### 公平锁

1. lock方法，直接调用acquire方法。

    ```
    //ReentrantLock
    final void lock() {
    	acquire(1);
    }
    ```

2. acquire方法，调用tryAcquire方法。

    ```
    //ReentrantLock
    public final void acquire(int arg) {
    	if (!tryAcquire(arg) &&
    		acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
    		selfInterrupt();
    }
    ```

3. tryAcquire方法调用的是FairSync中重写的tryAcquire方法。

    ```
    //FairSync
    protected final boolean tryAcquire(int acquires) {
    	final Thread current = Thread.currentThread();
    	int c = getState();
    	if (c == 0) {
    		if (!hasQueuedPredecessors() &&compareAndSetState(0, acquires)) {
    			setExclusiveOwnerThread(current);
    			return true;
            }
    	}
    	else if (current == getExclusiveOwnerThread()) {
    		int nextc = c + acquires;
    		if (nextc < 0)
    			throw new Error("Maximum lock count exceeded");
    		setState(nextc);
    		return true;
    	}
    	return false;
    }
    ```

4. 获取锁的前面加了一个hasQueuedPredecessors的判断，这个判断是判断队列中有没有比自己排在更前面的节点，也就是头节点的下一个节点是否存在。

    ```
    //FairSync
    public final boolean hasQueuedPredecessors() {
    	Node t = tail; // Read fields in reverse initialization order
    	Node h = head;
    	Node s;
    	return h != t&&((s = h.next) == null || s.thread != Thread.currentThread());
    }
    ```

##### 解锁

```
1.更改状态值也就是减一操作，判断当前线程是否是持有锁的线程，如果是的，则判断释放后的状态值是否为0，如果是的说明锁被完全释放，返回true，如果锁没有被完全释放，返回false
2.如果锁被完全释放，就唤醒队头的后驱节点，唤醒的前提是后驱节点的waitstatus是小于等于0的，如果不是的，找到离队头最近的waitstatus是小于等于0的节点进行唤醒，唤醒调用LockSupport.unpark(s.thread);
```

1. unlock()方法，调用的AbstractQueuedSynchronizer的release方法。

    ```
    //ReentrantLock
    public void unlock() {
    	sync.release(1);
    }
    ```

2. release()方法，如果释放资源成功，并且头节点不为空，并且头节点的状态值不等于0，就唤醒下一个Node节点来获取锁。

    ```
    //AbstractQueuedSynchronizer
    public final boolean release(int arg) {
    	if (tryRelease(arg)) {
    		Node h = head;
    		if (h != null && h.waitStatus != 0)
    			unparkSuccessor(h);
    		return true;
        }
    	return false;
    }
    ```

3. tryRelease方法，锁的状态值进行减一操作，如果当前线程不是获取到锁的线程，就抛出IllegaMonitorStateException异常。如果状态值在减一后变成0，那么说明这个锁被完全释放了，将占有锁的线程设置成空。

    ```
    //AbstractQueuedSynchronizer
    protected final boolean tryRelease(int releases) {
    	int c = getState() - releases;
    	if (Thread.currentThread() != getExclusiveOwnerThread())
    		throw new IllegalMonitorStateException();
    	boolean free = false;
    	if (c == 0) {
    		free = true;
    		setExclusiveOwnerThread(null);
    	}
    	setState(c);
    	return free;
    }
    ```

4. unparkSuccessor方法，释放完锁后就需要唤醒下一个节点来获取锁。获取Node的后驱元素，如果后驱节点是空，或者状态是取消，要找到离队头最近的状态是小于等于0的节点，然后就将这个节点唤醒。

    ```
    //AbstractQueuedSynchronizer
    private void unparkSuccessor(AbstractQueuedSynchronizer.Node node) {
    	//把状态值设为0
    	int ws = node.waitStatus;
        if (ws < 0)
            compareAndSetWaitStatus(node, ws, 0);
    	Node s = node.next;
    	if (s == null || s.waitStatus > 0) {
    		s = null;
    		for (Node t = tail; t != null && t != node; t = t.prev){
    			if (t.waitStatus <= 0)
    				s = t;
    		}
    	}
    	if (s != null)
    		LockSupport.unpark(s.thread);	
    }
    ```

##### Synchronized和ReenTrantLock 

1. Synchronized是关键字，依赖于JVM的特性实现；Lock是接口，基于 JDK 实现。
2. Synchronized在线程执行完毕后或发生异常时会自动释放锁，因此不会发生异常死锁；Lock执行完毕后或发生异常时不会自动释放锁，所以需要在finally中调用unlock()方法实现释放锁。
3. Synchronized是非中断锁，必须等待线程执行完成释放锁；Lock是可以中断锁。ReentrantLock在加锁的时候选择tryLock加锁，可以在没有获取到锁后直接返回，不用阻塞，或者lockInterruptibly方法加锁的话，可以在阻塞的过程中被中断。
4. Lock可以使用读锁提高多线程读效率。
5. synchronized 只能是是非公平锁；ReenTrantLock 默认实现非公平锁，也支持公平锁(先等先得)。
6. synchronized 经过一系列的优化，性能已得到大大的提升，和ReenTrantLock 相差无几。
7. Synchronized 与 wait 、notify 和 notifyAll 方法结合使用可以实现等待通知机制，但不够灵活，notify 被通知的线程由JVM选择，notifyAll 会通知所有等待状态的线程，存在效率问题。而 ReentrantLock 通过与 Condition 接口和 newCondition 方法结合使用，使线程对象可以注册在指定的Condition中，从而可以有选择性的进行线程通知，在线程调度上更加灵活。

#### ReentrantReadWriteLock

​		是ReadWriteLock读写锁接口的一个具体实现，实现了读写的分离，读锁是共享的，写锁是独占的，读和读之间不会互斥，读和写、写和读、写和写之间才会互斥，提升了读写的性能。

## 线程

### 操作系统

#### 调度模型

分时调度模型：平均分配。

抢占式调度模型：优先级。

操作系统是分时操作系统，时间片为毫秒级别，可同时运行多个程序。

Java是抢占式调度模型，一个线程用完CPU后，操作系统会根据线程优先级、线程饥饿情况等数据算出一个总优先级，并分配下一个时间片给某个线程执行。

#### 速率差异

CPU和内存速率差异：高速缓存

内存和IO速率差异：缓存

CPU和IO速率差异：中断

#### 并发

​		一个时间段内发生多件事情，可能同时发生，也可能不同时发生。多个CPU通过调度和控制可实现并发，进程和线程为实现并发的基本单位。

#### 并行

​		同一时刻同时发生多件事情。

#### 存储管理方式

##### 传统存储管理方式

​		作业必须一次全部加载到内存中，方可运行。当作业很大，就无法运行。而且多道作业运行时，内存不足容纳所有作业，导致多道程序性能下降。操作系统引入了虚拟内存的概念，利用计算机的空间局部性和时间局部性原理，将程序分的一部分装入内存运行，其余部分留在外存，等需要的时候再讲外存的程序装入内存继续运行。虚拟内存好像给用户提供了一个比实际内存大得多的存储器，叫虚拟存储器，大小由计算机地址结构决定。

##### 虚拟内存实现方式

###### 请求分页存储管理

​		将虚拟地址内存空间划分为大小相等的页块，同时内存地址空间，也划分位等大小的页块。系统维持一个页表，存储这虚拟页号到物理块块号的映射。程序中的逻辑地址由两部分组成：页号 P 和页内位移量W。相邻的页面在内存中不一定相邻，即分配给程序的内存块之间不一定连续。逻辑地址转化为物理地址时，根据页表将页号转化为块号，块号*页大小加上页内偏移得到物理地址。如果程序执行时，调用到不在内存中的虚拟页面时，发生缺页中断，将页由外村调入内存。如果内存已满，采用页面置换算法将老的淘汰，载入新的。页面置换算法常见的有 FIFO,LRU。
**优点**：没有外碎片，每个内碎片不超过页的大小。
**缺点**：程序全部装入内存，要求有相应的硬件支持，如地址变换机构缺页中断的产生和选择淘汰页面等都要求有相应的硬件支持。增加了机器成本和系统开销。

###### 请求分段存储管理

​		将用户程序地址空间分成若干个大小不等的段，每段能够定义一组相对完整的逻辑信息。存储分配时，以段为单位，段内地址连续，段间不连续。虚拟地址由段号和段内地址组成，虚拟地址到实存地址的变换通过段表来实现。分页对程序猿而言是不可见的。而分段通常对程序猿而言是可见的，因而分段为组织程序和数据提供了方便。
**优点**：可以分别编写和编译，可以针对不同类型的段采取不同的保护，可以按段为单位来进行共享，包括通过动态链接进行代码共享。
**缺点**：会产生碎片

###### 请求段页式存储管理

​		用分段方法来分配和管理虚拟存储器。程序的地址空间按逻辑单位分成基本独立的段，而每一段有自己的段名，再把每段分成固定大小的若干页。用分页方法来分配和管理实存。
**优点**：段页式管理是段式管理和页式管理相结合而成，具有两者的优点。
**缺点**：由于管理软件的增加，复杂性和开销也增加。另外需要的硬件以及占用的内存也有所增加，使得执行速度下降。

##### 内存换页

###### 过程

​		操作系统虚拟内存换页的过程在进程开始运行之前，不是全部装入页面，而是装入一个或者零个页面，之后根据进程运行的需要，动态装入其他页面；当内存已满，而又需要装入新的页面时，则根据某种算法淘汰某个页面，以便装入新的页面。
​		在使用虚拟页式存储管理时需要在页表中增加一些内容：页号、驻留位（中断位）、内存块号、外存地址、访问号、修改位：

1. 驻留位：表示该页在外存还是内存；
2. 访问位：表示该页在内存期间是否被访问过，又称 R 位；
3. 修改位：表示该页在内存中是否被修改过，又称 M 位；

​		缺页本身是一种中断，与一般的中断一样，需要经过 4 个处理步骤：

1. 保护 CPU 现场
2. 分析中断原因
3. 转入缺页中断处理程序进行处理
4. 恢复 CPU 现场，继续执行

###### 算法

1. 最优页面置换算法
2. 先进先出页面置换算法（FIFO）及其改进：实现简单，但没有考虑局部性原理，性能可能会很差，还会发生 Belady 异常现象。使用 FIFO 算法时，四个页框时缺页次数比三个页框时多。这种奇怪的情况称为 Belady 异常现象。
3. 最近最少使用页面置换算法（LRU）
4. 时钟页面置换算法（clock）
5. 最近未使用页面置换算法（NRU）

### 进程

1. 正在运行的程序，系统进行资源分配和调用的独立单位，每一个进程都有它自己的内存空间和系统资源，各进程地址空间独立，当一个进程崩溃后，在保护模式下不会对其他进程产生影响。
2. 程序较健壮，启动耗时较长，进程切换消耗资源大，效率较差。

#### 多进程

​		同一个时间段内执行多个任务，提高CPU的使用率，非提高执行速度。

#### 调度算法

​		先来先服务调度算法、短作业(进程)优先调度算法、优先权调度算法、高响应比优先调度算法、时间片轮转法、多级反馈队列调度算法。

#### 进程间通信

​		IPC，InterProcess Communication，指在不同进程之间传播或交换信息。进程间通信方式：

1. 管道：包括无名管道和命名管道，是半双工的，数据只能单向流动，具有固定的读端和写端。管道是一个固定大小的缓冲区，从管道读数据是一次性操作，数据一旦被读，它就从管道中被抛弃。
    1. 无名管道pipe，只能用于父子进程和兄弟进程。
    2. 有名管道FIFO，是一种文件类型，通信方式类似于在进程中使用文件来传输数据，只不过 FIFO 类型文件同时具有管道的特性，在数据读出时，FIFO管道中同时清除数据，可用于任何进程，允许无亲缘关系进程间通信。
2. 消息队列：是由消息组成的链表，存放在内核中。一个消息队列由一个标识符（队列 ID）来标识。消息队列独立于发送与接收进程，克服了信号传递信息少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。消息队列可以实现消息的随机查询，消息不一定要以先进先出的次序读取，也可以按消息的类型读取。
3. 信号量：semaphore，是一个计数器。信号量用于实现进程间的互斥与同步，控制多个进程对共享资源的访问，而不是用于存储进程间通信数据。信号量基于操作系统的 PV 操作，程序对信号量的操作都是原子操作。每次对信号量的 PV 操作不仅限于对信号量值加 1 或减 1，而且可以加减任意正整数。
4. 共享存储：Shared Memory，指两个或多个进程共享一个给定的存储区。共享内存是最快的一种IPC，因为进程是直接对内存进行存取。因为多个进程可以同时操作，所以需要进行同步。信号量与共享内存通常结合在一起使用，信号量用来同步对共享内存的访问。
5. 套接字：Socket，一种通信机制，凭借这种机制，客户/服务器（即要进行通信的进程）系统的开发工作既可以在本地单机上进行，也可以跨网络进行。也就是说它可以让不在同一台计算机
    但通过网络连接计算机上的进程进行通信。也因为这样，套接字明确地将客户端和服务器区分开来。
6. Streams：

#### 进程空间

​		内核态内存空间、用户态的堆栈（一般 8M，从高地址向低地址增长）、数据段、进程代码段。

​		线程共享的有：进程代码段、进程共有数据、文件描述符、信号处理器、进程当前目录、进程用户 ID、进程组 ID。
​		线程私有的：线程 ID、寄存器的值、线程的栈、线程优先级、错误返回码、线程信号屏蔽码。

###### 线程上下文切换开销

​		上下文切换的开销包括直接开销和间接开销。

**直接开销**：

1. 操作系统保存恢复上下文(CPU 寄存器值，程序计数器值等)所需的开销。
2. 线程调度器调度线程的开销。

**间接开销**：

1. 处理器高速缓存重新加载的开销。
2. 上下文切换可能导致整个一级高速缓存中的内容被冲刷，即被写入到下一级高速缓存或主存。

#### 用户态/内核态

​		防止用户进程访问对操作系统的稳定运行造成破坏。对一些资源的访问进行了等级划分。内核态
和用户态是操作系统的两种运行级别，内核态权限高，用户态权限低。
​		操作系统的很多操作会消耗系统的物理资源，例如创建一个新进程时，要做很多底层的细致工作，如分配物理内存，从父进程拷贝相关信息，拷贝设置页目录、页表等，这些操作显然不能随便让任何程序都可以做，于是就产生了特权级别的概念，与系统相关的一些特别关键性的操作必须由高级别的程序来完成，这样可以做到集中管理，减少有限资源的访问和使用冲突。

##### 用户态

​		当一个进程在执行用户自己的代码时处于用户运行态（用户态），此时特权级最低，为 3 级，是普通的用户进程运行的特权级，大部分用户直接面对的程序都是运行在用户态。Ring3 状态不能访问Ring0 的内核地址空间，包括代码和数据。

##### 内核态

​		当一个进程因为系统调用陷入内核代码中执行时处于内核运行态（内核态），此时特权级最高，为 0 级。执行的内核代码会使用当前进程的内核栈，每个进程都有自己的内核栈。

##### 切换

​		进程大部分时间是运行在用户态下的，在其需要操作系统帮助完成一些用户态自己没有特权和能力完成的操作时就会切换到内核态。切换到内核的方式有：系统调用、发生异常、外围设备的中断。

#### 内核态

### 线程

1. 线程是进程内部执行任务的基本单位，程序中单个顺序的控制流，程序使用CPU的基本单位，一个进程中的不同执行路径，多个线程之间没有独立的地址空间，有各自的堆栈和局部变量。一个线程死掉则整个进程死掉。要求同时进行且要共享某些变量的并发操作，只能用线程。
2. 创建和销毁线程很耗资源
3. 单线程程序：一个进程只有一条执行路径 
4. 多线程程序：一个进程有多条执行路径
5.  JVM是一个多线程的进程，至少有主线程+垃圾回收线程，JVM会启动一个主线程去执行main()。

#### 多线程

​		多个线程共享同一个进程的资源（堆内存和方法区），一个线程一个栈，有自己的局部变量。一个时间点只能执行一个线程，线程运行随机。线程过多会造成栈溢出、堆异常。多线程让程序分配更多的时间片，提高CPU利用率，让程序占用计算机更多的资源。更多作用：

1. 发挥多核CPU的优势：单核 CPU 上所谓的"多线程"那是 假的多线程，同一时间处理器只会处理一段逻辑，只不过线程之间切换得比较快， 看着像多个线程"同时"运行罢了。多核 CPU 上的多线程才是真正的多线程，它能 让你的多段逻辑同时工作，多线程，可以真正发挥出多核CPU 的优势来，达到充 分利用CPU 的目的。
2. 防止阻塞：从程序运行效率的角度来看，单核 CPU 不但不会发挥出多线程的优势，反而会因 为在单核CPU 上运行多线程导致线程上下文的切换，而降低程序整体的效率。但 是单核 CPU 我们还是要应用多线程，就是为了防止阻塞。试想，如果单核 CPU 使 用单线程，那么只要这个线程阻塞了，比方说远程读取某个数据吧，对端迟迟未返 回又没有设置超时时间，那么你的整个程序在数据返回回来之前就停止运行了。多 线程可以防止这个问题，多条线程同时运行，哪怕一条线程的代码执行读取数据阻 塞，也不会影响其它任务的执行。
3. 便于建模：这是另外一个没有这么明显的优点了。假设有一个大的任务 A，单线程编程，那么 就要考虑很多，建立整个程序模型比较麻烦。但是如果把这个大的任务 A 分解成 几个小任务，任务B、任务 C、任务 D，分别建立程序模型，并通过多线程分别运 行这几个任务，那就简单很多了。

#### Thread类

##### 成员方法

setPriority(int newPriority)：设置线程的优先级，默认5，最小1，最大10。

sleep(long millis)：当前线程休眠，让出CPU的时间片，等待long毫秒后才可抢CPU时间片。Thread.sleep(0)的作用是，Java 采用抢占式的线程调度算法，因此可能会出现某条线程常常获取到 CPU 控制权的情况，为了让某些优先级比较低的线程也能获取到 CPU 控制权，可以使用Thread.sleep(0)手动触发一次操作系统分配时间片的操作，这也是平衡CPU控制权的一种操作。sleep()是线程本身的静态方法，谁调用谁休眠，就算a线程调用b线程的sleep也是a线程休眠。

join()：线程加入，等待该线程终止。join(long millis)为等待该线程终止的时间最长为millis毫秒。

start()：启动一个线程，调用该Runnable对象的run()。start()创建的线程交由JVM管理，和操作系统交互，只有调用了start()方法，才会表现出多线程的特性，不同线程的 run()方法里面的代码交替执行。一个线程不能多次启动，Thread实例只能产生一个线程，一旦调用start()方法，该实例的started标记则标记为true，无论该线程最后有没被执行到底，调用一次start()便不能再运行。若对已启动的线程对象再次调用start()，将产生IllegalThreadStateException异常。

run()：run()中的代码即为线程该执行的任务，可以重复调用，相当于同步的方式，如果只是调用 run()方法，那么代码还是同步执行的，必须等待一个线程的 run()方法里面的代码全部执行完毕之后，另外一个线程才可以执行其 run() 方法里面的代码，不会启动线程。

yield()：线程礼让，当前线程放弃时间片，重新加入抢夺CPU时间片的行列中。只保证当前线程放弃 CPU 占用而不能保证使其它线程一定 能占用 CPU，执行yield()的线程有可能在进入到暂停状态后马上又被执行。yield()不会释放锁，只是通知调度器自己可以让出cpu时间片，但只是建议，调度器也不一定采纳。

setDaemon(boolean on)：后台线程，守护当前线程。当还有非守护线程存在，主线程不会退出。等所有的非守护线程执行完毕后，主线程才会退出。须在线程启动之前调用，当线程正在运行时调用会产生异常。

interrupt()：中断线程

void stop()：中断线程

suspend()：阻塞一个线程，不会释放锁，直到其他的线程调用resume方法，才能继续向下执行。被打断不会导致InterruptedException。容易造成死锁，已被标记过时。

#### 多线程实现

1. 继承Thread类实现多线程。Thread是Runnable接口的子类，任何类继承Thread类便成为一个线程的主类，使用继承Thread类的方式会带来单继承局限。线程启动的主方法需复写Thread类中的run()方法实现，调用public void start()启动多线程，start()和操作系统交互，启动一个线程，该线程执行run()中的代码。

    ```
    //不同步
    class MyThread extends Thread{
    	private int ticket=5;
    	@Override
    	public void run(){
    		for(int x=0;x<50;x++){
    			if(this.ticket>0){
    				System.out.println("卖票, ticket="+this.ticket--);
    			}
    		}
    	}
    }
    public class Demo{
    	public static void main(String[] args) throws Exception{
    		MyThread mt1 = new MyThread();
    		MyThread mt2 = new MyThread();
    		mt1.start();
    		mt2.start();
    	}
    }
    ```

    ```
    //同步
    class MyThread extends Thread{
    	...同上...
    }
    public class Demo{
    	public static void main(String[] args) throws Exception{
    		MyThread mt = new MyThread();    //创建一个线程对象
    		new Thread(mt).start();    //启动线程
    		new Thread(mt).start();    //启动线程
    	}
    }
    ```

    ```
    //使用Lambda表达式直接定义线程主体并调用Thread类的start()方式启动多线程
    public class Demo{
    	public static void main(String[] args){
    		String name="线程1";
    		new Thread(()->{
    			for(int i=0;i<100;i++){
    			System.out.println(this.name+"-->"+i);
    		}).start();
    	}
    }
    ```

2. 实现Runnable接口实现多线程。Runnable接口声明处有@FunctionalInterface注解，Runnable为函数式接口，可以使用Lambda表达式来表示该接口的一个实现。使用Runnable接口可避免单继承局限。线程的主类需复写Runnable接口中的run()方法，该方法不能返回操作结果，Runnable接口没有提供可被继承的start()方法去启动多线程，需用Thread类有参构造：Thread(Runnable target)，接收一个Runnable接口对象。

    ```
    //不同步
    class MyThread implements Runnable{    //定义线程的主体类
    	private String name;
    	public MyThread(String name){
    		this.name=name;
    	}
    	@Override
    	public void run(){    //线程的主类需复写Runnable接口中的run()方法
    		for(int i=0;i<100;i++){
    			System.out.println(this.name+"-->"+i);
    		}
    	}
    }
    public class Demo{
    	public static void main(String[] args){
    		MyThread mt1 = new MyThread("线程1");   
    		MyThread mt2 = new MyThread("线程2"); 
    		new Thread(mt1).start();   
    		new Thread(mt2).start();  
    	}
    }
    ```

    ```
    //同步
    	class MyThread implements Runnable{
    		...同上...
    	}
    	public class Demo{
    		public static void main(String[] args) throws Exception{
    			MyThread mt = new MyThread();    //创建一个线程对象
    			new Thread(mt).start();    //启动线程
    			new Thread(mt).start();    //启动线程
    		}
    	}
    
    ```

3. 实现Callable接口实现多线程。call()方法上可以实现线程操作数据的返回，返回的数据类型由Callable接口上的泛型类型动态决定。

    ```
    @FunctionalInterface
    public interface Callable<V>{
    	public V call() throws Exception;
    }
    ```

#### 生命周期

**新建 new**：创建线程对象，没有执行资格。

**就绪 runnable**：start() 之后、sleep()睡醒之后、yield()之后。线程已启动，有执行资格，等待cpu调度，没有执行权。

**运行**：有执行资格，取得执行权，开始执行。

**阻塞 blocked**：处于运行状态中的线程由于某种原因，暂时放弃对CPU的使用权，停止执行，此时进入阻塞状态，无执行资格，无执行权，其他线程就可以获得执行的机会。被阻塞的线程会在合适的时候重新进入就绪状态，等待线程调度器再次调度它。根据阻塞产生的原因不同，阻塞状态可分为三种：

1. 等待阻塞：运行状态中的线程执行wait()方法，使本线程进入到等待阻塞状态，等待某个通知（notify），收到其他线程的通知后进入runnable状态。

2. 同步阻塞：线程在获取synchronized同步锁时，由于锁被其它线程所占用，导致获取锁失败，则会进入同步阻塞状态，等待获取锁。一旦获取到锁就进入runnable状态继续运行。

3. 其他阻塞：
    1. 通过调用线程的sleep()方法主动放弃所占用的处理器资源，当sleep()状态超时线程重新转入就绪状态。
    2. join()，当join()等待的线程终止或者超时，线程重新转入就绪状态。
    3. 线程发出了一个阻塞式I/O请求时，线程会进入到阻塞状态，I/O处理完毕时，线程重新转入就绪状态。
    4. 程序调用了线程的suspend()方法将该线程挂起。但这个方法容易导致死锁，所以应该尽量避免使用该方法。线程被调用了resdme()恢复方法后进入runnable状态。

**等待 waiting**：线程处于无限制等待状态，等待一个特殊的事件来重新唤醒。一旦通过相关事件唤醒线程，线程就进入了runnable状态继续运行。

1. 通过wait()方法进行等待的线程等待一个notify()或者notifyAll()方法。
2. 通过join()方法进行等待的线程等待目标线程运行结束而唤醒。

**timed_waiting**：线程进入一个有时限的等待，如sleep(3000)，等待3s后线程重新进入runnable状态。

**死亡**：执行完毕，没有执行资格，等待垃圾回收。

```
Q. Which method you define as the starting point of new thread in a class from which n thread can be execution? (B)
A：public void start()
B：public void run()
C：public void int()
D：public static void main(String args[])
E：public void runnable()

A. 题目的意思是，下列哪一个方法你认为是新线程开始执行的点，也就是从该点开始线程n被执行。
   start()方法是启动一个线程，此时的线程处于就绪状态，但并不一定就会执行，还需要等待CPU的调度。
   run()方法才是线程获得CPU时间，开始执行的点。
```

#### 线程间通信方式

​		锁机制、信号量机制、信号机制。线程间通信的主要目的是用于线程同步，所以线程没有像进程通信中用于数据交换的通信机制。

##### 唤醒阻塞线程

1. 如果线程是因为调用了 wait()、sleep()或者 join()方法而导致的阻塞，可以中断线程， 并且通过抛出 InterruptedException 来唤醒它，属于checked exception。
2. 如果线程遇到了 IO 阻塞，无能为力， 因为 IO是操作系统实现的，Java 代码并没有办法直接接触到操作系统。

##### 等待唤醒机制

​		如果需要在线程间相互唤醒的话就需要借助Object.wait(), Object.nofity() 。Obj.wait()与Obj.notify()必须要与synchronized(Obj)一起使用，也就是wait,与notify是针对已经获取了Obj锁进行操作，从语法角度来说就是Obj.wait(),Obj.notify必须在synchronized(Obj){...}语句块内。之所以定义在Object类中，因为线程之间通信的媒介是锁，锁可以是任意对象。

**wait()**：获取对象锁的线程主动释放对象锁。无限制的休眠线程，进入阻塞状态，直到有其它线程调用对象的notify()唤醒该线程，才能继续获取对象锁，并继续执行。Thread.sleep()与Object.wait()二者都可以暂停当前线程，释放CPU控制权，主要的区别在于Object.wait()在释放CPU同时，释放了对象锁的控制。

​		所谓的释放锁资源实际是通知对象内置的monitor对象进行释放，而只有所有对象都有内置的monitor对象才能实现任何对象的锁资源都可以释放。又因为所有类都继承自Object，所以wait(）就成了Object方法，也就是通过wait()来通知对象内置的monitor对象释放，而且事实上因为这涉及对硬件底层的操作，所以wait()方法是native方法，底层是用C写的。其他都是Thread所有，所以其他3个是没有资格释放资源的。而join()有资格释放资源其实是通过调用wait()来实现的。

​		void wait(long timeout)：若在指定的时间内没被唤醒，则自动苏醒。

​		void wait(long timeout, int nanos)：精确到纳秒

**notify()**：随机唤醒一个等待该对象（该锁）的线程，即因释放obj的锁而休眠的线程。注意：notify()调用后，并不是马上就释放对象锁的，而是在相应的synchronized(){}语句块执行结束，自动释放锁后，JVM会在wait()对象锁的线程中随机选取一线程，赋予其对象锁，唤醒线程，继续执行。这样就提供了在线程间同步、唤醒的操作。

```
Q. 执行结果为（Thread 2 sent notify. Thread 1 wake up）
public static void main(String[]args)throws Exception {
    final Object obj = new Object();
    Thread t1 = new Thread() {
        public void run() {
            synchronized (obj) {
                try {
                    obj.wait();
                    System.out.println("Thread 1 wake up.");
                } catch (InterruptedException e) {
                }
            }
        }
    };
    t1.start();
    Thread.sleep(1000);
    Thread t2 = new Thread() {
        public void run() {
            synchronized (obj) {
                obj.notifyAll();
                System.out.println("Thread 2 sent notify.");
            }
        }
    };
    t2.start();
}
```

**notifyAll()**：唤醒所有等待该对象（该锁）的线程。

wait()和sleep()方法的区别：

1. 均放弃CPU一定的时间。
2. wait是Object类的方法，sleep是Thread类的方法。
3. 若线程持有某个对象的监视器，sleep方法不会放弃这个对象的监视器，wait方法会放弃这个对象的监视器。
4. wait要靠别人叫醒或打断，sleep自己醒或被打断。

###### Fork/Join框架

​		任务自动分散小任务，并发执行，合并小任务结果。如果任务拆解的很深，系统内的线程数量堆积，导致系统性能性能严重下降。如果函数的调用栈很深，会导致栈内存溢出。

##### 线程之间传递数据

1. 共享对象
2. 阻塞队列BlockingQueue就是为线程之间共享数据而设计。阻塞式方法是指程序会一直等待该方法调用完成返回结果，期间线程被挂起，不做其他事情，直到得到结果之后才会返回。ServerSocket 的accept()方法就是一直等待客户端连接。此外，还有异步和非阻塞式方法在任务完成前就返回。

#### 异常

​		一个线程运行时发生异常，如果异常没有被捕获该线程将会停止执行。Thread.UncaughtExceptionHandler 是用 于处理未捕获异常造成线程突然中断情况的一个内嵌接口。当一个未捕获异常将造 成线程中断的时候JVM会使用Thread.getUncaughtExceptionHandler()来查询线程的UncaughtExceptionHandler并将线程和异常作为参数传递给handler的uncaughtException()方法进行处理。

#### 线程安全问题

##### 前提

​		多线程环境，多个线程对共享数据进行修改操作。

##### 原因

###### 可见性问题

​		可见性是指某个线程修改了某一个共享变量的值，而其他线程是否可以看见该共享变量修改后的值。单线程读到的肯定都是最新的值，在多线程编程中每个线程都有自己的工作内存，线程先把共享 变量的值从主内存读到工作内存，形成一个副本，当计算完后再把副本的值刷回主内存。从读取到最后刷回主内存这是一个过程，当还没刷回主内存时对其他线程是不可见的，所以其他线程从主内存读到的值是修改之前的旧值。像 CPU 的缓存优化、硬件优化、指令重排及对 JVM 编译器的优化，都会出现可见性的问题。

###### 原子性问题

​		操作共享数据并非原子操作，即一个操作或者一系列操作不能被分割执行。

###### 指令重排序

​		为了优化程序执行和提高 CPU 的处理性能，更加充分的利用CPU缓存，JVM 和操作系统会对没有依赖关系的指令进行重排序，执行顺序不确定，但保证当前线程执行结果一致，但指令重排后势必会影响多线程的执行结果。

##### ThreadLocal

1. ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。

2. 多线程编程中，变量是多个线程共享的。在同步机制中，通过对象的锁机制保证同一时间只有一个线程访问变量，使用同步机制要求程序慎密地分析什么时候对变量进行读写，什么时候需要锁定某个对象，什么时候释放对象锁等繁杂的问题，程序设计和编写难度相对较大。

3. 而ThreadLocal则从另一个角度来解决多线程的并发访问。对象都在堆里创建，为了提升效率，ThreadLocal会从堆中弄一个缓存到自己的栈，为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。如果多个线程使用该变量就可能引发问题，这时volatile 变量就可以发挥作用了，它要求线程从主存中读取变量的值。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。可用来解决数据库连接、Session 管理等。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

4. 由于ThreadLocal中可以持有任何类型的对象，低版本JDK所提供的get()返回的是Object对象，需要强制类型转换。但JDK5.0通过泛型很好的解决了这个问题，在一定程度地简化ThreadLocal的使用。

5. 对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。

6. 每个线程对象都有自己的ThreadLocalMap变量，key是自己维持的 ThreadLocal 对象，值为 ThreadLocal 的值。使用开放地址法来处理哈希冲突，在ThreadLocalMap中的散列值分散得十分均匀，很少会出现冲突。并且ThreadLocalMap经常需要清除无用的对象，所以使用纯数组更加方便。因为线程必然要访问threadLocal 变量，然后调用 threadLocal.get()方法，这个方法的实现就是：先获取当前线程，然后获取当前线程的 ThreadLocalMap 对象，map 对象的 key 就是维持的 ThreadLocal对象，value 就是变量的值。
   
```
    public T get() {
    	Thread t = Thread.currentThread();
    	ThreadLocalMap map = getMap(t);
    	if (map != null) {
    		ThreadLocalMap.Entry e = map.getEntry(this);
     		if (e != null) {
     			@SuppressWarnings("unchecked")
     			T result = (T)e.value;
     			return result;
     		}
     	}
     	return setInitialValue();
    }
```

7. ThreadLocal 实例通常来说都是 private static 类型，用于关联线程。static 的 ThreadLocal 变量是一个与线程相关的静态变量，即一个线程内，static 变量是被各个实例共同引用的，但是不同线程内，static 变量是隔开的。
   
8. 使用方法：一个 class 中定义了一个 ThreadLocal 变量。通过重写方法 initialValue()初始化值。通过threadLocal对象调用get()方法threadLocal.get()获取值，set()方法threadLoca.set(threadLocal.get()+10)设置值。

##### 排查

​		JDK中用jstack命令排查多线程问题，用于打印出给定的java进程ID或core file或远程调试服务的Java堆栈信息。如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。另外，jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息，如果现在运行的java程序呈现hung状态，jstack是非常有用的。

##### 解决方案

1. 同步代码块	
2. 同步方法
3. 锁机制

#### 线程池

​		每个线程都要通过 new Thread(xxRunnable).start()的方式来创建并运行一个线程，线程少的话这不会是问题，而真实环境可能会开启多个线程让系统和程序达到最佳效率，当线程数达到一定数量就会耗尽系统的 CPU 和 内存资源，也会造成 GC频繁收集和停顿，因为每次创建和销毁一个线程都比较消耗系统资源，如果为每个任务都创建线程这无疑是一个很大的性能瓶颈。希望线程得到重复利用，线程池为有限资源池，线程代码结束后，线程不会死亡，线程池中的线程复用极大节省了系统资源，当线程一段时间不再有任务处理时它也会自动销毁，而不会长驻内存。

##### 底层执行原理

​		由于创建线程时需获取全局锁，这将严重影响性能。因此ThreadPoolExecutor的处理流程中引入工作队列，以便在执行execute()方法时尽量少直接创建线程。

1. 执行execute()提交任务，判断核心线程池是否已满。
2. 若核心线程池未满，创建线程执行任务；若核心线程池已满，判断工作队列是否已满。
3. 若工作队列未满，将任务存储在队列里；若工作队列已满，判断线程池最大线程数是否已满。
4. 若未达最大线程数，则创建线程执行任务；若已达最大线程数，则按照饱和策略处理无法执行的任务。

##### 参数

**corePoolSize**：核心线程池数量。在线程数少于核心数量时，有新任务进来就新建一个线程，即使有的线程没事干，等超出核心数量后，就不会新建线程了，空闲的线程就得去任务队列里取任务执行了。

**maximumPoolSize**：最大线程数量。包括核心线程池数量+核心以外的数量。如果任务队列满了，并且池中线程数小于最大线程数，会再创建新的线程执行任务。

**keepAliveTime**：核心池以外的线程存活时间，即没有任务的外包的存活时间。核心线程会一直存活，即使没有任务需要执行，如果给线程池设置 allowCoreThreadTimeOut(true)，则核心线程在空闲时头上也会响起死亡的倒计时。如果任务是多而容易执行的，可以调大这个参数，那样线程就可以在存活的时间里有更大可能接受新任务。

**threadFactory**：线程工厂，用来创建线程。可以给线程起个好听的名字，设置个优先级啥的。

**handler**：饱和策略，当线程均处于忙时：

1. CallerRunsPolicy：只要线程池没关闭，就直接用调用者所在线程来运行任务。
2. AbortPolicy：直接抛出RejectedExecutionException异常。
3. DiscardPolicy：悄悄把任务放生，不做了。
4. DiscardOldestPolicy：把队列里待最久的那个任务扔了，然后再调用execute()试试看能行不。
5. 我们也可以实现自己的RejectedExecutionHandler接口自定义策略，比如如记录日志什么的。

**workQueue**：保存待执行任务的阻塞队列。队列分为有界队列和无界队列。有界队列的长度有上限，当线程池中的核心线程数已满时，新任务就要保存到队列中了，当达到上限，若没有核心线程去即时取走处理，则会创建临时线程。无界队列长度没有上限，当没有核心线程空闲的时候，新来的任务可以无止境的向队列中添加，而永远也不会创建临时线程。线程池中使用的队列是BlockingQueue接口，常用的实现有如下几种：

1. ArrayBlockingQueue：基于数组、有界，按FIFO原则对元素进行排序。

2. LinkedBlockingQueue：基于链表，按FIFO排序元素。吞吐量通常要高于ArrayBlockingQueue。Executors.newFixedThreadPool()使用了这个队列。
3. SynchronousQueue：不存储元素的阻塞队列。每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态。吞吐量通常要高于LinkedBlockingQueue。Executors.newCachedThreadPool使用了这个队列。
4. PriorityBlockingQueue：具有优先级的、无限阻塞队列

##### Excutors类

​		静态工厂类，通过静态工厂方法返回ExecutorService、

ScheduledExecutorService、ThreadFactory 和 Callable 等类的对象。

**newFixedThreadPool**：

1. 返回ThreadPoolExecutor实例。
2. 核心线程数和最大线程数都是指定值。当线程池中的线程数超过核心线程数后，任务都会被放到阻塞队列中。
3. keepAliveTime为0，不限时，单位为TimeUnit.MILLISECONDS，即多余的空余线程会被立即终止，由于这里没有多余线程，这个参数也没什么意义了。
4. 工作队列为LinkedBlockingQueue，无界阻塞队列，默认容量Integer.MAX_VALUE，相当于没有上限。
5. 执行任务的流程：
    1. 有新任务时，若线程数少于核心线程数，则创建线程执行任务。
    2. 线程数等于核心线程数后，将任务加入阻塞队列，由于队列容量非常大，可以一直加加加。执行完任务的线程反复去队列中取任务执行。
6. 适用于负载比较重的服务器，为了资源的合理利用，需要限制当前线程数量。	

**newSingleThreadExecutor**：

1. 返回FinalizableDelegatedExecutorService包装的ThreadPoolExecutor实例。
2. 核心线程数和最大线程数都是1，相当于特殊的 FixedThreadPool。
3. keepAliveTime为0，不限时，单位为TimeUnit.MILLISECONDS，多余的空余线程会被立即终止，由于这里没有多余线程，这个参数也没什么意义了。
4. 阻塞队列为LinkedBlockingQueue，无界阻塞队列，使用的是默认容量Integer.MAX_VALUE，相当于没有上限。
5. 执行任务的流程：
    1. 线程池中没有线程时，新建一个线程执行任务。
    2. 有一个线程以后，将任务加入阻塞队列，不停加加加。唯一的这一个线程不停地去队列里取任务执行。

5. 适用于串行执行任务的场景，每个任务必须按顺序执行，不需要并发执行。	

 **newCachedThreadPool**：

1. 返回ThreadPoolExecutor实例。
2. 核心线程数为0，最大线程数Integer.MAX_VALUE无上限。
3. keepAliveTime为60s。
4. 工作队列为SynchronousQueue，同步队列，这个队列的作用就是传递任务，并不会保存。当提交任务的速度大于处理任务的速度时，每次提交一个任务，就会创建一个线程。极端情况下会创建过多的线程，耗尽CPU和内存资源。
5. 执行任务的流程：
    1. 没有核心线程，直接向SynchronousQueue中提交任务。
    2. 如果有空闲线程，就去取出任务执行。
    3. 如果没有空闲线程，就新建一个。
    4. 执行完任务的线程有 60 秒生存时间，如果在这个时间内可以接到新任务，就可以继续活下去，否则就拜拜。

5. 适用于并发执行大量短期的小任务，或者是负载较轻的服务器。极端情况下会创建过多的线程，耗尽CPU和内存资源。由于空闲 60 秒的线程会被终止，长期处于空闲状态则不会占用任何资源。

**newScheduledThreadPool**：

1. 返回ScheduledThreadPoolExecutor实例。
2. 核心线程数为给定值，最大线程数Integer.MAX_VALUE无上限。
3. 存活时间不限，非核心线程空闲超过10s就会被回收。
4. 工作队列为DelayedWorkQueue，按照超时时间升序排序。
5. 任务执行流程：
    1. 若线程均处于繁忙状态，新任务进入DelayedWorkQueue。
    2. 添加任务提供了另外两个方法：
        1. scheduleAtFixedRate()：按某种速率周期执行。
        2. scheduleWithFixedDelay()：在某个延迟后执行。
6. 适用于需要多个后台线程定时或周期性执行任务的场景

**execute(xxRunnble)** ：没有返回值，如果不需要知道线程的结果就使用execute方法，性能会好很多。

**submit(xxRunnble)**：返回一个Future对象，如果想知道线程结果就使用submit提交，而且它能在主线程中通过Future的get方法捕获线程中的异常。

**shutdown()**：不再接受新的任务，之前提交的任务等执行结束再关闭线程池。 

**shutdownNow()**：不再接受新的任务，通过调用每个线程的 interrupt()方法尝试中断池中正在处理的任务，再关闭线程池，返回所有未处理的线程list列表。

##### 手写可复用的线程池

```
public class ThreadPoolManager {
	private final String TAG = this.getClass().getSimpleName();

	//定义线程池的几个关键属性的值
	private static final int CORE_POOL_SIZE = Runtime.getRuntime().availableProcessors() * 2;	//核心线程数为CPU数*2。一般是4、8，好点的16个线程。
	private static final int MAXIMUM_POOL_SIZE = 64;	//最大线程数设置为64
	private static final int KEEP_ALIVE_TIME = 1;    //空闲线程存活时间1秒
	
	//根据处理的任务类型选择不同的阻塞队列。如果是要求高吞吐量的，可以使用SynchronousQueue队列；如果对执行顺序有要求，可以使用PriorityBlockingQueue；如果最大积攒的待做任务有上限，可以使用LinkedBlockingQueue。
	private final BlockingQueue<Runnable> mWorkQueue = new LinkedBlockingQueue<>(128);	
	
	//创建自己的ThreadFactory。在其中为每个线程设置个名称。
	private final ThreadFactory DEFAULT_THREAD_FACTORY = new ThreadFactory() {
		private final AtomicInteger mCount = new AtomicInteger(1);
		public Thread newThread(Runnable r) {
			Thread thread = new Thread(r, TAG + " #" + mCount.getAndIncrement());
			thread.setPriority(Thread.NORM_PRIORITY);
			return thread;
		}
	};
	
	//创建线程池。选择的饱和策略为DiscardOldestPolicy。
	private ThreadPoolExecutor mExecutor = new ThreadPoolExecutor(
		CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE_TIME,
		TimeUnit.SECONDS, mWorkQueue, DEFAULT_THREAD_FACTORY,
	    new ThreadPoolExecutor.DiscardOldestPolicy());
	    	
	private static volatile ThreadPoolManager mInstance = new ThreadPoolManager();
	
	public static ThreadPoolManager getInstance() {
		return mInstance;
	}
	
	public void addTask(Runnable runnable) {
		mExecutor.execute(runnable);
	}
	
	public void shutdownNow() {
		mExecutor.shutdownNow();
	}
}
```

##### 线程池数量配置

1. 自定义线程池时，如果任务是 CPU 密集型（需要进行大量计算、处理），则应该配置尽量少的线程，比如 CPU 个数 + 1，这样可以避免出现每个线程都需要使用很长时间但是有太多线程争抢资源的情况。
2. 如果任务是 IO密集型（主要时间都在 I/O，CPU 空闲时间比较多），则应该配置多一些线程，比如 CPU 数的两倍，这样可以更高地压榨 CPU。
3. 为了错误避免创建过多线程导致系统奔溃，建议使用有界队列。因为它在无法添加更多任务时会拒绝任务，这样可以提前预警，避免影响整个系统。
4. 执行时间、顺序有要求的话可以选择优先级队列，同时也要保证低优先级的任务有机会被执行。
5. 线程数=CPU核心数/(1-阻塞系数)，阻塞系数为0~1。
6. maxsize 过大会占用更多资源，cpu 会频繁地进行上下文切换，会导致 cpu 缓存的数据失效和重新加载，结果就是上下文切换和 reload 的时间变多，也就是说 cpu 将更多的时间花费到对线程的管理上去了，这时候更多的线程反而更慢。

#### 守护线程

1. Daemon程序是一直运行的服务端程序，又称为守护进程。通常在系统后台运行，没有控制终端不与前台交互，Daemon程序一般作为系统服务使用。Daemon是长时间运行的进程，通常在系统启动后就运行，在系统关闭时才结束。一般说Daemon程序在后台运行，是因为它没有控制终端，无法和前台的用户交互。Daemon程序一般都作为服务程序使用，等待客户端程序与它通信。我们也把运行的Daemon程序称作守护进程。
2. 与守护线程相对应的就是用户线程，守护线程就是守护用户线程，当用户线程全部执行完结束之后，守护线程才会跟着结束。守护线程必须伴随着用户线程，如果一个应用内只存在一个守护线程，没有用户线程，守护线程自然会退出。
3. 程序的主线程Main（比方java程序一开始启动时创建的那个线程）不会是守护线程 
4. Daemon thread在Java里面的定义是，如果虚拟机中只有Daemon thread在运行，则虚拟机退出。 虚拟机中可能会同时有很多个线程在运行，只有当所有的非守护线程都结束的时候，虚拟机的进程才会结束，不管在运行的线程是不是main()线程。
5. Main主线程结束了（Non-daemon thread），如果此时正在运行的其他threads是daemonthreads，JVM会使得这个threads停止，JVM也停下。如果此时正在运行的其他threads有Non-daemonthreads,那么必须等所有的Non daemon线程结束了，JVM才会停下来。
6. 必须等所有的Non-daemon线程都运行结束了，只剩下daemon的时候，JVM才会停下来，注意Main主程序是Non-daemon 线程。默认产生的线程全部是Non-daemon线程。
7. JVM的资源回收线程就是这类线程。
8. 在该类线程中产生的其他线程不用设置，默认都是守护线程。

##### 创建

​		Thread.setDaemon(Boolean b)，setDaemon()方法需要在start方法调用之前使用，如果创建的线程没有显示调用此方法，这默认为用户线程。

```
import java.io.IOException; 
public class DaemonThreadTest extends Thread{ 
    publicDaemonThreadTest() { 
	} 
	public voidrun(){ 
		for(int i = 1; i <= 100;i++){ 
			try{ 
				Thread.sleep(100); 
			} catch (InterruptedException ex){ 
				ex.printStackTrace(); 
			} 
			System.out.println(i); 
		} 
	} 
	publicstatic void main(String [] args){ 
		DaemonThreadTest test = newDaemonThreadTest(); 
		// 如果不设置daemon，那么线程将输出100后才结束 
		test.setDaemon(true); 
		//在test未结束前，执行下面的输入操作，则test终止执行，因为jvm中只剩下守护线程时会终止
		test.start(); 
		System.out.println("isDaemon = " +test.isDaemon()); 
		try { 
			//接受输入，使程序在此停顿，一旦接收到用户输入，main线程结束，守护线程自动结束 
			System.in.read(); 
		} catch (IOException ex) { 
			ex.printStackTrace(); 
		}
		System.out.print(Thread.currentThread().getName()+"结束");
	} 
} 
```

### 协程

​		协程是一种用户态的轻量级线程，协程的调度完全由用户控制。协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。协程则是异步。协程不需要多线程的锁机制。

#### 线程和进程的区别

​		线程和进程是不同的操作系统资源管理方式。

1. 地址空间：线程是进程内的一个执行单元，进程内至少有一个线程，它们共享进程的地址空间，而进程有自己独立的地址空间。
2. 资源拥有：进程是资源分配和拥有的单位,同一个进程内的线程共享进程的资源。
3. 线程是处理器调度的基本单位,但进程不是。
4. 二者均可并发执行。
5. 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口，但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

#### 协程与线程的区别

1. 一个线程可以多个协程，一个进程也可以单独拥有多个协程，这样python中则能使用多核CPU。

2. 线程进程都是同步机制，而协程则是异步。

3. 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态。

## JVM/GC

### JVM

#### 作用

​		Java 程序通过javac编译成 .class文件后，由JVM负责调用系统函数执行程序，操作系统并不认识.class 文件。当有个 JVM 这个抽象层，就可以实现跨平台了。JVM 只需要正确执行 .class 文件，就可以运行在 Linux、Windos、MacOS 等平台了。Java 跨平台的意义在于一次编译，处处运行，这里 JVM 功不可没。比如在 Maven 仓库下载的 jar 包就可以到处运行，不需要在每个平台上再编译一次。

​		Java 是一门抽象的语言，并且有自动内存管理机制。而操作系统无法去进行自动垃圾回收等操作，所以就有了虚拟机。虚拟机可以对字节码加载、自动垃圾回收、并发等。而 JVM 只是一个规范，定义了 .class 文件的结构、加载机制、数据存储、运行时栈等诸多内容，最常用的 JVM 实现就是 Hotspot。

​		了解 JVM，主要用在调优以及故障排查上面，你会对运行中的各种资源分配，有一个比较全面的掌控。

​		运行java编译出来的目标文件，要注意jvm的版本问题，低版本的jvm没有办法运行高版本的java代码。

#### 代码运行原理

​		Java 程序通过 javac 编译成 .class 文件，然后虚拟机将其加载到元数据区，执行引擎将会通过混合模式执行这些字节码。执行时，会翻译成操作系统相关的函数。Java 文件->编译器->字节码->JVM->机器码。Java虚拟机栈采用基于栈的架构，有比较丰富的 opcode。这些字节码可以解释执行，也可以编译成机器码，运行在底层硬件上，可以说 JVM 是一种混合执行的策略。JVM 的生命周期是和Java程序的运行一样，当程序运行结束，JVM实例也就跟着消失了。

#### 内存分配

​		对数据分类管理，提高程序运行效率。

##### 共享内存区

​		所有线程共享。

###### 堆 Heap

1. 堆是最常处理的区域，它存储JVM启动时创建的数组和对象实例，包括其属性和方法。堆内存需要关键字new才可开辟。
2. 堆内存又分为：新生代和老年代。默认情况下，年轻代占堆空间的三分之一，新生代中又分为1个Eden区和2个Surivor区，由于新生代中大部分对象（98%）都是朝生夕死的，所以两块内存的比例不是 1:1，大概是 8:1。其中eden区占8/10，SurvivorFrom和SurvivorTo各占1/10。默认情况下，老年代占堆空间的三分之二。大对象直接进入老年代，比如很长的字符串以及数组。
3. JVM垃圾收集也主要是在堆上面工作，如果实际所需的堆超过了自动内存管理系统能提供的最大容量时抛出OutOfMemoryError 异常。

###### 方法区 MethodArea

1. 方法区是可供各条线程共享的运行时内存区域，方法区是个概念。

2. 存储了每一个类的结构信息，例如运行时常量池（Runtime Constant Pool）、字段和方法数据、构造函数和普通方法的字节码内容、还包括一些在类、实例、接口初始化时用到的特殊方法。当创建类和接口时，如果构造运行时常量池所需的内存空间超过了方法区所能提供的最大内存空间后就会抛出OutOfMemoryError。

3. **运行时常量池**：RuntimeConstantPool，是方法区的一部分，每一个运行时常量池都分配在JVM的方法区中， 在类和接口被加载到 JVM 后，每个类对应的运行时常量池就被创建。运行时常量池是每 一个类或接口的常量池（Constant_Pool）的运行时表现形式，它包括了若干种常量： 编译器可知的数值字面量到必须运行期解析后才能获得的方法或字段的引用。如果 方法区的内存空间不能满足内存分配请求，那Java虚拟机将抛出一个OutOfMemoryError异常。

4. **字符串常量池**：

    1. JDK1.7之前，字符串常量池在永久代实现的方法区中。

    2. JDK1.7，字符串常量池被单独从方法区拿到了堆中，运行时常量池剩下的东西还在永久代实现的方法区中。
    3. JDK1.8，hotspot移除了永久代用元空间(Metaspace)取而代之，这时字符串常量池还在堆，运行时常量池还在元空间实现的方法区中。

**方法区的实现**：

**永久代**：perm generation，JDK1.7及之前，方法区的实现为永久代，指内存的永久保存区域，主要存放Class和Meta（元数据）的信息，Class在被加载的时候被放入永久区域，它和存放实例的区域不同，GC不会在主程序运行期对永久区域进行清理。所以这也导致了永久代的区域会随着加载的Class的增多而胀满，最终抛出Out of Memory异常。

**元空间**：Metaspace，Java8中，永久代已经被移除，方法区的实现为元数据区（元空间）。类加载器存储的位置在元空间，每一个类加载器的存储区域都称作一个元空间，所有的元空间合在一起就是我们说的元空间。当一个类加载器被垃圾回收器标记为不再存活，其对应的元空间会被回收。元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限制。采用元空间而不用永久代的几点原因：

1. 为了解决永久代的Out of Memory问题，元数据和class对象存在永久代中，容易出现性能问题和内存溢出。
2. 类及方法的信息等比较难确定其大小，因此对于永久代的大小指定比较困难，太小容易出现永久代溢出，太大则容易导致老年代溢出（因为堆空间有限，此消彼长）。类的元数据放入native memory，字符串池和类的静态变量放入java堆中，这样加载多少类的元数据就不再由MaxPermSize控制，而由系统的实际可用空间来控制。
3. 永久代会为 GC 带来不必要的复杂度，并且回收效率偏低。

##### 私有内存区

​		线程隔离的，每个线程互不共享，在新线程创建时才创建的。

###### 程序计数器 ProgramCounterRerister

1. 程序计数器区域一块内存较小的区域，它用于存储线程的每个执行指令，每个线程都有自己的程序计数器，此区域不会有内存溢出的情况。
2. 指示当前程序执行到了哪一行，执行Java方法时记录正在执行的虚拟机字节码指令地址，执行本地方法时，计数器值为null。
3. 由于程序计数器（PC寄存器）中存储的数据所占空间不会随着程序执行而发生改变，因此，程序计数器（PC寄存器）不会发生内存溢出现象。

###### 虚拟机栈 VMStack

1. 虚拟机栈描述的是 Java 方法执行的内存模型，每个方法被执行的时候都会同时创建一个栈帧StackFrame，用于存储局部变量表、操作数栈、动态链接、方法出口、常量池引用等信息，每一块栈帧只能保存一块堆内存地址。每一个方法被调用直至执行完成的过程就对应着一个栈帧在虚拟机栈中从 入栈到出栈的过程。
2. 当线程请求分配的栈容量超过 JVM 允许的最大容量时抛出StackOverflowError 异常。

###### 本地方法栈 NativeMethodStack

1. 存储执行本地方法的局部变量，支持本地方法，即native标识的非Java语言实现的方法。
2. 当线程请求分配的栈容量超过 JVM 允许的最大容量时抛出StackOverflowError 异常。 

##### 调优

1. 参数设置：-Xms20M -Xmx20M -Xmn10M -XX:SurvivorRatio=8，表示堆空间最大20M，最小20M，其中新生代10M(那么老年代就是20-10=10M)，新生代中Eden区与一个Survivor区比例为8:1。
2. 在使用Serial或者ParNew垃圾回收器的情况下，可以设置参数-XX:PretenureSizeThreshold的值指定大于该值的对象直接在老年代分配。
3. 对象晋升老年代的年龄阈值可以通过参数-XX:MaxTenuringThreshold设置。

### 内存泄漏

​		内存泄露，memory leak，由于疏忽或错误造成程序未能释放已经不再使用的内存的情况。内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，失去了对该段内存的控制，因而造成了内存的浪费。 memory leak会最终会导致out of memory。

#### 常发性内存泄漏

​		发生内存泄漏的代码会被多次执行到，每次被执行的时候都会导致一块内存泄漏。 

#### 偶发性内存泄漏

​		发生内存泄漏的代码只有在某些特定环境或操作过程下才会发生。常发性和偶发性是相对的。对于特定的环境，偶发性的也许就变成了常发性的。所以测试环境和测试方法对检测内存泄漏至关重要。 

#### 一次性内存泄漏

​		发生内存泄漏的代码只会被执行一次，或者由于算法上的缺陷，导致总会有一块仅且一块内存发生泄漏。比如，在类的构造函数中分配内存，在析构函数中却没有释放该内存，所以内存泄漏只会发生一次。 

#### 隐式内存泄漏

​		程序在运行过程中不停的分配内存，但是直到结束的时候才释放内存。严格的说这里并没有发生内存泄漏，因为最终程序释放了所有申请的内存。但是对于一个服务器程序，需要运行几天，几周甚至几个月，不及时释放内存也可能导致最终耗尽系统的所有内存。所以，我们称这类内存泄漏为隐式内存泄漏。 

#### Java产生内存泄露的情况

##### 静态集合类

1. 像HashMap、Vector等的使用最容易出现内存泄露，这些静态变量的生命周期和应用程序一致，他们所引用的所有的对象Object也不能被释放，因为他们也将一直被Vector等引用着。
2. 当集合里面的对象属性被修改后，再调用remove()方法时不起作用。

##### 监听器

​		在java 编程中，我们都需要和监听器打交道，通常一个应用当中会用到很多监听器，我们会调用一个控件的诸如addXXXListener()等方法来增加监听器，但往往在释放对象的时候却没有记住去删除这些监听器，从而增加了内存泄漏的机会。

##### 各种连接

​		比如数据库连接（dataSourse.getConnection()），网络连接(socket)和io连接，除非其显式的调用了其close（）方法将其连接关闭，否则是不会自动被GC 回收的。对于Resultset 和Statement 对象可以不进行显式回收，但Connection 一定要显式回收，因为Connection 在任何时候都无法自动回收，而Connection一旦回收，Resultset 和Statement 对象就会立即为NULL。但是如果使用连接池，情况就不一样了，除了要显式地关闭连接，还必须显式地关闭Resultset Statement 对象（关闭其中一个，另外一个也会关闭），否则就会造成大量的Statement 对象无法释放，从而引起内存泄漏。这种情况下一般都会在try里面去取得连接，在finally里面释放连接。

##### 单例模式

​		不正确使用单例模式是引起内存泄露的一个常见问题，单例对象在被初始化后将在JVM的整个生命周期中存在（以静态变量的方式），如果单例对象持有外部对象的引用，那么这个外部对象将不能被jvm正常回收，导致内存泄露。

### GC

#### 垃圾回收机制

​		所有对象的回收都是由Java虚拟机通过垃圾回收机制完成的。GC为了能够正确释放对象，会监控每个对象的运行状况，对他们的申请、引用、被引用、赋值等状况进行监控，Java会使用有向图的方法进行管理内存，实时监控对象是否可以达到。当GC检测到堆中的某个对象不再被栈所引用，会不定期对该堆中保存的对象进行回收，回收后会释放其所占用的空间，帮助防止内存泄漏。

#### 主要任务

1. 分配内存
2. 确保被引用对象的内存不被错误的回收
3. 回收不再被引用的对象的内存空间

#### 垃圾

1. 一块没有任何栈内存指向的堆空间将成为垃圾。
2. 全局变量始终会有一个Class对象的句柄指向它，除非这个Class对象要被回收了，否则静态引用的对象不会被垃圾回收，只要静态变量没有被销毁也没有置null，其对象一直被保持引用，也即引用计数不可能是0，因此不会被垃圾回收。因此，单例对象在运行时不会被回收。

3. 废弃常量：没有引用的常量
4. 无用类：无用类必须同时满足3个条件：
    1. 该类所有的实例都已经被回收
    2. 加载该类的ClassLoader已经被回收
    3. 该类对应的java.lang.Class对象没有在任务地方被引用，无法在任何地方通过反射访问该类的方法。

##### 引用类型

​		JDK1.2 之前，一个对象只有“已被引用”和"未被引用"两种状态，这将无法描述某些特殊情况下的对象，比如，当内存充足时需要保留，而内存紧张时才需要被抛弃的一类对象。所以在 JDK.1.2 之后，Java 对引用的概念进行了扩充，将引用分为了：强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）、虚引用（Phantom Reference）4 种，这 4 种引用的强度依次减弱。

###### 强引用

​		Object obj = new Object(); ，只要obj还指向Object对象，Object对象就不会被回收 obj = null; //手动置null
只要强引用存在，垃圾回收器将永远不会回收被引用的对象，哪怕内存不足时，JVM也会直接抛出OutOfMemoryError，不会去回收。如果想中断强引用与对象之间的联系，可以显示的将强引用赋值为null，这样一来，JVM就可以适时的回收对象了。

###### 软引用

​		软引用是用来描述一些非必需但仍有用的对象。在内存足够的时候，软引用对象不会被回收，只有在内存不足时，系统则会回收软引用对象，如果回收了软引用对象之后仍然没有足够的内存，才会抛出内存溢出异常。这种特性常常被用来实现缓存技术，比如网页缓存，图片缓存等。
在 JDK1.2 之后，用java.lang.ref.SoftReference类来表示软引用。

###### 弱引用

​		弱引用的引用强度比软引用要更弱一些，无论内存是否足够，只要 JVM 开始进行垃圾回收，那些被弱引用关联的对象都会被回收。在 JDK1.2 之后，用 java.lang.ref.WeakReference 来表示弱引用。

###### 虚引用

​		虚引用是最弱的一种引用关系，如果一个对象仅持有虚引用，那么它就和没有任何引用一样，它随时可能会被回收，在 JDK1.2 之后，用 PhantomReference 类来表示，通过查看这个类的源码，发现它只有一个构造函数和一个 get() 方法，而且它的 get() 方法仅仅是返回一个null，也就是说将永远无法通过虚引用来获取对象，虚引用必须要和 ReferenceQueue 引用队列一起使用。

#### 垃圾回收器

**serial**：串行收集器。单线程，垃圾回收的时候，必须暂停其他工作。使用复制算法，新生代收集器。虚拟机运行在Client模式时的默认新生代收集器。 
**ParNew收集器**：serial 的多线程版本。虚拟机运行在Server模式的默认新生代收集器，在单CPU的环境中，ParNew收集器并不会比Serial收集器有更好的效果。 
**Parallel Scavenge收集器**：复制算法的多线程收集器。注重吞吐量，cpu 运行代码时间/cpu 耗时总时间。使用复制算法，新生代收集器。
**Serial Old 收集器**：单线程收集器，使用标记－整理算法，是老年代的收集器。
**Parallel Old 收集器**：Parallel 老年代版本。
**CMS 收集器**： 标记清除算法，并发收集器。垃圾收集线程与用户线程（基本上）同时工作。注重最短时间停顿，使用CMS并不能达到GC效率最高（总体GC时间最小），但它能尽可能降低GC时服务的停顿时间，这一点对于实时或 者高交互性应用（譬如证券交易）来说至关重要。

```
CMS的GC过程：
1.初始标记:暂停用户线程，对引用进行遍历并标记；
2. 并发标记：在并发情况下，遍历除1中标记过的线程并标记；
3. 并发预清理：并发情况下对以上的标记进行清理；
4. 重标记：由于3过程是并发的，可能会产生一些引用，所以需要暂停用户线程重新标记；
5. 并发清理：清理4过程产生的标记；
6. 并发重置：做一些收尾工作。
```

#### 垃圾搜索算法

​		垃圾收集器会对堆进行回收前，确定对象中哪些是“存活”，哪些是”死亡“（不可能再被任何途径使用的对象）。

##### 引用计数算法

​		每当一个地方引用它时，计数器+1；引用失效时，计数器-1；计数值=0——不可能再被引用。在引用计数变化为0时即刻发生，而且只针对某一个对象以及它所依赖的其它对象。一般也称引用计数垃圾收集为直接的垃圾收集机制。垃圾收集的开销被分摊到整个应用程序的运行当中了，而不是在进行垃圾收集时，要挂起整个应用的运行，直到对堆中所有对象的处理都结束。

​		引用计数器实现简单，效率高；但是不能解决循环引用问问题（A 对象引用 B 对象，B 对象又引用 A 对象，并没有因为两个对象互相引用就没有回收，同时每次计数器的增加和减少都带来了很多额外的开销，内存占用大，效率慢，会引起内存泄漏。如处理环形数据时，有两个数组相互引用，将引用这两对象的引用置为null后，这两个对象依然不能被回收，它们的引用计数始终为1。

![image-20200430000748585](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200430000748585.png)

![image-20200430000809333](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200430000809333.png)

##### 可达性分析算法

​		向图，树图，把一系列“GC Roots”作为起始点，从节点向下搜索，路径称为引用链，当一个对象到GC Roots没有任何引用链相连，即不可达时，则证明此对象是不可用的。例如一颗树有很多丫枝，其中一个分支断了，跟树上没有任何联系，那就说明这个分支没有用了，就可以当垃圾回收去烧了。在Java中可作为GCRoots的对象：

1. 虚拟机栈（栈帧中的本地变量表）中引用的对象；
2. 方法区中类静态属性引用的对象；
3. 方法区中常量引用的对象；
4. 本地方法栈中JNI（Java Native Interface  Java本地接口）引用的对象	

#### 触发时机

​		即使是被判断不可达的对象，也要再进行筛选，当对象没有覆盖finalize()方法，或者finalize方法已经被虚拟机调用过，则没有必要执行。如果有必要执行——放置在F-Queue的队列中——Finalizer线程执行。对象可以在被GC时可以自我拯救(this)，机会只有一次，因为任何一个对象的finalize()方法都只会被系统自动调用一次。并不建议使用，应该避免。使用try_finaly或者其他方式。

​		回收对象的两次标记过程：可达性分析的不可达对象，并不马上回收。真正死亡至少经过两次标记：

1. 对可达性分析不可达的对象进行第一次标记，并进行筛选。筛选条件是重写 finalize 方法且没有调用的对象，将其放入队列中。
2. 进行第二次标记，在执行 finalize 方法时，该对象依然没有被引用，才会被真正回收掉。finalize 方法只能被调用一次。

#### 垃圾回收算法

​		垃圾算法的实现涉及大量的程序细节，而且不同的虚拟机平台实现的方法也各不相同。垃圾回收算法可以定制。

##### 标记清除算法 Mark-Sweep

​		标记阶段，确定所有要回收的对象，并做标记。清除阶段，紧随标记阶段，将标记阶段确定不可用的对象清除。

​		产生大量不连续的内存空间碎片，导致创建需要大片连续存储空间的大对象时，尽管剩余空间足够，也创建不成功。创建对象慢，因为需要查找哪块空间的大小满足需求。

##### 标记整理算法 Mark-Compact

​		不会产生内存空间碎片，要移动覆盖对象，把存活对象往内存的一端移动，提高了内存的利用率。但是耗时。

​		适合存活对象比较多，垃圾比较少的情况。

##### 标记复制算法 Copying

​		把内存分成大小相等的两块，每次使用其中一块，当垃圾回收的时候，把存活的对象复制到另一块连续的内存空间中，然后把这块内存整个清理掉。

​		复制算法实现简单，运行效率高，不会产生碎片。但是由于每次只能使用其中的一半，造成内存的利用率不高。

​		适合存活对象比较少的情况。

##### 分代收集算法 Generational Collection

​		新建的对象再新生代中，如果新生代内存不够，就进行新生代的垃圾回收Minor GC释放掉不活跃对象。垃圾回收的时候把Eden区和一个可用的Survivor区中存活的对象复制到另一个保留的Survivor区中，然后清理到Eden区和一个可用的Survivor区所有内容。如果保留的Survivor区装不下，则通过分配担保机制将部分活跃对象提前转移到老年代。如果还是不够，就进行老年代的垃圾回收MajorGC释放老年代，如果还是不够，JVM会抛出内存不足，发生oom，内存泄漏。

###### Major GC触发时机

1. 老年代空间不足
2. 方法区空间不足
3. 调用 System.gc()
4. 没有足够的连续空间分配给大对象
5. 长期存活的对象转入老年代，空间不足。
6. 新生代垃圾回收存活的对象太多，S1 放不下，老年代担保空间不足。

#### 堆内存分代

​		根据对象的存活时间将堆内存分成新生代、老年代、永久代（即方法区）三个区域。JDK8后，删去永久代，增加元数据区（存于本地内存）。

##### 新生代

​		频繁生成对象，产生大量垃圾。使用标记复制算法。

##### 老年代

​		使用标记整理算法长期存活的对象将进入老年代。长期存活判断标准：

1. 如果对象在Eden出生并经过第一次Minor GC后仍然存活，并且能被Survivor容纳的话，将被移动到Survivor空间中，并且对象年龄设为1。对象在Survivor区中每熬过一次Minor GC，年龄就增加1岁，当它的年龄增加到一定程度，默认是15岁，将会被晋升到老年代中。对象晋升老年代的年龄阈值可以通过参数-XX:MaxTenuringThreshold设置。
2. 如果在Survivor空间中相同年龄所有对象大小的总和大于Survivor空间的一半，年龄大于等于该年龄的对象就可以直接进入老年代，无需等到MaxTenuringThreshold中要求的年龄。

#### finalize()

​		垃圾回收器回收对象前，会调用Object类的finalize()方法。子类重写finalize()，关系系统资源问题，但关闭系统一般放在finally语句中，因为资源放在finalize()里，由于垃圾回收器的不定时出现，而得不到及时的释放。Object中的finalize()默认实现为空，直接调用此方法，对象并不会变成垃圾。
