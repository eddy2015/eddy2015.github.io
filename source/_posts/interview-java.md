---
title: Android 面试：Java 简述题
date: 2016-10-25 23:15:14
categories:
- Android 面试
tags:
- Android 面试
---

本文收集整理了 Android 面试中会遇到与 Java 知识相关的简述题。<!--more-->

#  面向对象

**Java面向对象的三个特征与含义。**

继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段。

封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口（可以想想普通洗衣机和全自动洗衣机的差别，明显全自动洗衣机封装更好因此操作起来更简单；我们现在使用的智能手机也是封装得足够好的，因为几个按键就搞定了所有的事情）。

多态：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。如果将对象的方法视为对象向外界提供的服务，那么运行时的多态性可以解释为：当A系统访问B系统提供的服务时，B系统有多种提供服务的方式，但一切对A系统来说都是透明的（就像电动剃须刀是A系统，它的供电系统是B系统，B系统可以使用电池供电或者用交流电，甚至还有可能是太阳能，A系统只会通过B类对象调用供电的方法，但并不知道供电系统的底层实现是什么，究竟通过何种方式获得了动力）。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：1. 方法重写（子类继承父类并重写父类中已有的或抽象的方法）；2. 对象造型（用父类型引用引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。

## Java 多态

什么是多态

面向对象的三大特性：封装、继承、多态。从一定角度来看，封装和继承几乎都是为多态而准备的。这是我们最后一个概念，也是最重要的知识点。

多态的定义：指允许不同类的对象对同一消息做出响应。即同一消息可以根据发送对象的不同而采用多种不同的行为方式。（发送消息就是函数调用）

实现多态的技术称为：动态绑定（dynamic binding），是指在执行期间判断所引用对象的实 际类型，根据其实际的类型调用其相应的方法。

多态的作用：消除类型之间的耦合关系。

现实中，关于多态的例子不胜枚举。比方说按下 F1 键这个动作，如果当前在 Flash 界面下弹出的就是 AS 3 的帮助文档；如果当前在 Word 下弹出的就是 Word 帮助；在 Windows 下弹出的就是 Windows 帮助和支持。同一个事件发生在不同的对象上会产生不同的结果。 下面是多态存在的三个必要条件，要求大家做梦时都能背出来！

多态存在的三个必要条件 一、要有继承； 二、要有重写； 三、父类引用指向子类对象。

 多态的好处：

1. 可替换性（substitutability）。多态对已存在代码具有可替换性。例如，多态对圆Circle类工作，对其他任何圆形几何体，如圆环，也同样工作。
2. 可扩充性（extensibility）。多态对代码具有可扩充性。增加新的子类不影响已存在类的多态性、继承性，以及其他特性的运行和操作。实际上新加子类更容易获得多态功能。例如，在实现了圆锥、半圆锥以及半球体的多态基础上，很容易增添球体类的多态性。
3. 接口性（interface-ability）。多态是超类通过方法签名，向子类提供了一个共同接口，由子类来完善或者覆盖它而实现的。如图8.3 所示。图中超类Shape规定了两个实现多态的接口方法，computeArea()以及computeVolume()。子类，如Circle和Sphere为了实现多态，完善或者覆盖这两个接口方法。
4. 灵活性（flexibility）。它在应用中体现了灵活多样的操作，提高了使用效率。
5. 简化性（simplicity）。多态简化对应用软件的代码编写和修改过程，尤其在处理大量对象的运算和操作时，这个特点尤为突出和重要。

Java中多态的实现方式：接口实现，继承父类进行方法重写，同一个类中进行方法重载。

# 语法知识

## & 和 && 的区别

&和&&都是可以作为逻辑运算符的，其逻辑运算规则是相同的。

但&作为逻辑运算符时，即使第一个操作符是false，那么它仍然会计算第二个操作符。&&短路与，如果第一个操作符为false，那么它不会再去计算第二个操作符。

## 用最有效率的方法算出2乘以8等於几?

2<< 3


## Java 中的 SoftReference 是什么

Java 中的 SoftReference 即对象的软引用。如果一个对象具有软引用，内存空间足够，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。使用软引用能防止内存泄露，增强程序的健壮性。

SoftReference的特点是它的一个实例保存对一个Java对象的软引用，该软引用的存在不妨碍垃圾收集线程对该Java对象的回收。也就是说，一旦SoftReference保存了对一个Java对象的软引用后，在垃圾线程对这个Java对象回收前，SoftReference类所提供的get()方法返回Java对象的强引用。另外，一旦垃圾线程回收该Java对象之后，get()方法将返回null

用Map集合缓存软引用的Bitmap对象

```
Map<String, SoftReference<Bitmap>> imageCache = new new HashMap<String, SoftReference<Bitmap>>();
//强引用的Bitmap对象
Bitmap bitmap = BitmapFactory.decodeStream(InputStream);
//软引用的Bitmap对象
SoftReference<Bitmap> bitmapcache = new SoftReference<Bitmap>(bitmap);
//添加该对象到Map中使其缓存
imageCache.put("1",softRbitmap);
..
.
//从缓存中取软引用的Bitmap对象
SoftReference<Bitmap> bitmapcache_ = imageCache.get("1");
//取出Bitmap对象，如果由于内存不足Bitmap被回收，将取得空

Bitmap bitmap_ = bitmapcache_.get();
```

## 谈一下对 Java 中的 abstract 的理解

Abstract(抽象)可以修饰类、方法 

如果将一个类设置为abstract，则此类必须被继承使用。此类不可生成对象，必须被继承使用。 Abstract可以将子类的共性最大限度的抽取出来，放在父类中，以提高程序的简洁性。 Abstract虽然不能生成对象，但是可以声明，作为编译时类型，但不能作为运行时类型。 Final和abstract永远不会同时出现。  

当abstract用于修饰方法时，此时该方法为抽象方法，此时方法不需要实现，实现留给子类覆盖，子类覆盖该方法之后方法才能够生效。  

注意比较： 
private void print(){}；此语句表示方法的空实现。 
Abstract void print()； 此语句表示方法的抽象，无实现。  

如果一个类中有一个抽象方法，那么这个类一定为一个抽象类。 反之，如果一个类为抽象类，那么其中可能有非抽象的方法。  

如果让一个非抽象类继承一个含抽象方法的抽象类，则编译时会发生错误。因为当一个非抽象类继承一个抽象方法的时候，本着只有一个类中有一个抽象方法，那么这个类必须为抽象类的原则。这个类必须为抽象类，这与此类为非抽象冲突，所以报错。  

所以子类的方法必须覆盖父类的抽象方法。方法才能够起作用。 

只有将理论被熟练运用在实际的程序设计的过程中之后，才能说理论被完全掌握！ 

为了实现多态，那么父类必须有定义。而父类并不实现，留给子类去实现。此时可将父类定义成abstract类。如果没有定义抽象的父类，那么编译会出现错误。  
Abstract和static不能放在一起，否则便会出现错误。（这是因为static不可被覆盖，而abstract为了生效必须被覆盖。）

## Overload 和 Override 的区别

方法的重写(Overriding)和重载(Overloading)是Java多态性的不同表现。重写(Overriding)是父类与子类之间多态性的一种表现，而重载(Overloading)是一个类中多态性的一种表现。
如果在子类中定义某方法与其父类有相同的名称和参数，我们说该方法被重写 (Overriding)。子类的对象使用这个方法时，将调用子类中的定义，对它而言，父类中的定义如同被"屏蔽"了。
如果在一个类中定义了多个同名的方法，它们或有不同的参数个数或有不同的参数类型或有不同的参数次序，则称为方法的重载(Overloading)。不能通过访问权限、返回类型、抛出的异常进行重载.

1.方法重载（overload）
概念：简单的说:方法重载就是类的同一种功能的多种实现方式，到底采用哪种方式，取决于调用者给出的参数。 
注意事项：
（1）方法名相同
（2）方法的参数类型、个数、顺序不至少有一项不同
（3）方法返回类型可以不同
（4）方法的修饰符可以不同
如果只是返回类型不一样，不能够构成重载
如果只是控制访问修饰符号不一样，也是不能构成重载的
Overloaded的方法是可以改变返回值的类型。
2.方法覆盖（override）
概念：简单的说：方法覆盖就是子类有一个方法，和父类的某个方法的名称、返回类型、参数一样，那么我们就说子类的这个方法覆盖了父类的那个方法。
注意事项：方法覆盖有很多条件，总的讲有两点一定要注意：
（1）子类的方法的返回类型，参数，方法名称，要和父类方法的返回类型，参数，方法名称完全一样，否则编译出错。
（2） 子类方法不能缩小父类方法的访问权限（反过来是可以的） 

## private和default有什么区别

Java访问控制符的含义和使用情况:

| 修饰符 | 类内部 | 包 | 子类 | 包外部 |
|  |  |  |  |  |
| public | √ | √ | √ | √ |
| protected | √ | √ | √ | × |
| default | √ | √ | × | × |
| private | √ | × | × | × |

区别：

（1）public：可以被所有其他类所访问。

（2）private：只能被自己访问和修改。

（3）protected：自身，子类及同一个包中类可以访问。

（4）default（默认）：同一包中的类可以访问，声明时没有加修饰符，认为是friendly。

## Java 里的常量是怎么定义的

方法一采用接口(Interface)的中变量默认为static final的特性。

方法二采用了Java 5.0中引入的Enum类型。

方法三采用了在普通类中使用static final修饰变量的方法。

方法四类似方法三，但是通过函数来获取常量。

```
    /** 
     * Method One 
     */  
    interface ConstantInterface {  
        String SUNDAY = "SUNDAY";  
        String MONDAY = "MONDAY";  
        String TUESDAY = "TUESDAY";  
        String WEDNESDAY = "WEDNESDAY";  
        String THURSDAY = "THURSDAY";  
        String FRIDAY = "FRIDAY";  
        String SATURDAY = "SATURDAY";  
    }  
    /** 
     * Method Two  
     */  
    enum ConstantEnum {  
        SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY  
    }  
    /** 
     * Method Three 
     */  
    class ConstantClassField {  
        public static final String SUNDAY = "SUNDAY";  
        public static final String MONDAY = "MONDAY";  
        public static final String TUESDAY = "TUESDAY";  
        public static final String WEDNESDAY = "WEDNESDAY";  
        public static final String THURSDAY = "THURSDAY";  
        public static final String FRIDAY = "FRIDAY";  
        public static final String SATURDAY = "SATURDAY";  
    }  
    /** 
     * Method Four 
     * http://www.ibm.com/developerworks/cn/java/l-java-interface/index.html 
     */  
    class ConstantClassFunction {  
        private static final String SUNDAY = "SUNDAY";  
        private static final String MONDAY = "MONDAY";  
        private static final String TUESDAY = "TUESDAY";  
        private static final String WEDNESDAY = "WEDNESDAY";  
        private static final String THURSDAY = "THURSDAY";  
        private static final String FRIDAY = "FRIDAY";  
        private static final String SATURDAY = "SATURDAY";  
        public static String getSunday() {  
            return SUNDAY;  
        }  
        public static String getMonday() {  
            return MONDAY;  
        }  
        public static String getTuesday() {  
            return TUESDAY;  
        }  
        public static String getWednesday() {  
            return WEDNESDAY;  
        }  
        public static String getThursday() {  
            return THURSDAY;  
        }  
        public static String getFirday() {  
            return FRIDAY;  
        }  
        public static String getSaturday() {  
            return SATURDAY;  
        }  
    }  
    public class TestConstant {  
        static final String day = "saturday";  
        public static void main(String[] args) {  
            System.out.println("Is today Saturday?");  
            System.out.println(day.equalsIgnoreCase(ConstantInterface.SATURDAY));  
            System.out.println(day.equalsIgnoreCase(ConstantEnum.SATURDAY.name()));  
            System.out.println(day.equalsIgnoreCase(ConstantClassField.SATURDAY));  
            System.out.println(day.equalsIgnoreCase(ConstantClassFunction  
                    .getSaturday()));  
        }  
    }  
```

## java中abstract,interface,final,static的总结

一,抽象类:abstract

1,只要有一个或一个以上抽象方法的类,必须用abstract声明为抽象类;

2,抽象类中可以有具体的实现方法;

3,抽象类中可以没有抽象方法;

4,抽象类中的抽象方法必须被它的子类实现,如果子类没有实现,则该子类继续为抽象类

5,抽象类不能被实例化,但可以由抽象父类指向的子类实例来调用抽象父类中的具体实现方法;通常作为一种默认行为;

6,要使用抽象类中的方法,必须有一个子类继承于这个抽象类,并实现抽象类中的抽象方法,通过子类的实例去调用;

二,接口:interface

1,接口中可以有成员变量,且接口中的成员变量必须定义初始化;

2,接口中的成员方法只能是方法原型,不能有方法主体;

3,接口的成员变量和成员方法只能public(或缺省不写),效果一样,都是public

4,实现接口的类必须全部实现接口中的方法(父类的实现也算,一般有通过基类实现接口中个异性不大的方法来做为适配器的做法)

三,关键字:final

1,可用于修饰:成员变量,非抽象类(不能与abstract同时出现),非抽象的成员方法,以及方法参数

2,final方法:不能被子类的方法重写,但可以被继承;

3,final类:表示该类不能被继承,没有子类;final类中的方法也无法被继承.

4,final变量:表示常量,只能赋值一次,赋值后不能被修改.final变量必须定义初始化;

5,final不能用于修饰构造方法;

6,final参数:只能使用该参数,不能修改该参数的值;

四,关键字:static

1,可以修饰成员变量和成员方法,但不能修饰类以及构造方法;

2,被static修饰的成员变量和成员方法独立于该类的任何对象。也就是说，它不依赖类特定的实例，被类的所有实例共享

3,static变量和static方法一般是通过类名直接访问,但也可以通过类的实例来访问(不推荐这种访问方式)

4,static变量和static方法同样适应java访问修饰符.用public修饰的static变量和static方法,在任何地方都可以通过类名直接来访问,但用private修饰的static变量和static方法,只能在声明的本类方法及静态块中访问,但不能用this访问,因为this属于非静态变量.

五,static和final同时使用 

1,static final用来修饰成员变量和成员方法，可简单理解为“全局常量”！ 

2,对于变量，表示一旦给值就不可修改，并且通过类名可以访问。 

3,对于方法，表示不可覆盖，并且可以通过类名直接访问。

## Switch能否用string做参数？

在Java 5以前，switch(expr)中，expr只能是byte、short、char、int。从Java 5开始，Java中引入了枚举类型，expr也可以是enum类型，从Java 7开始，expr还可以是字符串（String），但是长整型（long）在目前所有的版本中都是不可以的。

## Object有哪些公用方法？

[http://www.cnblogs.com/yumo/p/4908315.html](http://www.cnblogs.com/yumo/p/4908315.html)

1．clone方法

保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。

主要是JAVA里除了8种基本类型传参数是值传递，其他的类对象传参数都是引用传递，我们有时候不希望在方法里讲参数改变，这是就需要在类中复写clone方法。

2．getClass方法

final方法，获得运行时类型。

3．toString方法

该方法用得比较多，一般子类都有覆盖。

4．finalize方法

该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。

5．equals方法

该方法是非常重要的一个方法。一般equals和==是不一样的，但是在Object中两者是一样的。子类一般都要重写这个方法。

6．hashCode方法

该方法用于哈希查找，可以减少在查找中使用equals的次数，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

一般必须满足obj1.equals(obj2)==true。可以推出obj1.hashCode()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

如果不重写hashCode(),在HashSet中添加两个equals的对象，会将两个对象都加入进去。

7．wait方法

wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

（1）其他线程调用了该对象的notify方法。

（2）其他线程调用了该对象的notifyAll方法。

（3）其他线程调用了interrupt中断该线程。

（4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

8．notify方法

该方法唤醒在该对象上等待的某个线程。

9．notifyAll方法

该方法唤醒在该对象上等待的所有线程。

## foreach与正常for循环效率对比。

[http://904510742.iteye.com/blog/2118331](http://904510742.iteye.com/blog/2118331)

直接for循环效率最高，其次是迭代器和 ForEach操作。作为语法糖，其实 ForEach 编译成 字节码之后，使用的是迭代器实现的，反编译后，testForEach方法如下：

```
public static void testForEach(List list) {  
    for (Iterator iterator = list.iterator(); iterator.hasNext();) {  
        Object t = iterator.next();  
        Object obj = t;  
    }  
}  

```

可以看到，只比迭代器遍历多了生成中间变量这一步，因为性能也略微下降了一些。

# 基本数据类型

## int char long 各占多少字节数

| 类型 | 位数 | 字节数 |
|  |  |  |
| byte | 8 | 1 |
| short | 16 | 2 |
| int | 32 | 4 |
| long | 64 | 8 |
| float | 32 | 4 |
| double | 64 | 8 |
| char | 16 | 2 |

## int 和 integer 的区别

1. int是基本的数据类型；
2. Integer是int的封装类；
3. int和Integer都可以表示某一个数值；
4. int和Integer不能够互用，因为他们两种不同的数据类型；

# String

## 是否可以继承String类?

不能，因为 String 为 final 类。

## swtich是否能作用在byte上，是否能作用在long上，是否能作用在String上?

- switch语句中的表达式只能是byte，short，char ，int以及枚举（enum），所以当表达式是byte的时候可以隐含转换为int类型，而long字节比int字节多，不能隐式转化为int类型，所以switch语句可以用在byte上而不可以用在long上。
- 由于在JDK7.0中引入了新特性，所以switch语句可以接收一个String类型的值，switch语句可以作用在String上。

## 常量final string str=“ab”可不可以变成”abd”，为什么？

不能，因为使用 final 修饰的变量不可改变。

## StringBuffer的作用？

StringBuffer类和String一样，也用来代表字符串，只是由于StringBuffer的内部实现方式和String不同，所以StringBuffer在进行字符串处理时，不生成新的对象，在内存使用上要优于String类。

所以在实际使用时，如果经常需要对一个字符串进行修改，例如插入、删除等操作，使用StringBuffer要更加适合一些。

## String StringBuffer StringBuilder 的区别

三者在执行速度方面的比较：StringBuilder >  StringBuffer  >  String 

- String 字符串常量，不可变
- StringBuffer 字符串变量（线程安全）
- StringBuilder 字符串变量（非线程安全）

简要的说， String 类型和 StringBuffer 类型的主要性能区别其实在于 String 是不可变的对象, 因此在每次对 String 类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用String ，因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后,JVM 的 GC 就会开始工作，那速度是一定会相当慢的。

而如果是使用 StringBuffer 类则结果就不一样了，每次结果都会对 StringBuffer 对象本身进行操作，而不是生成新的对象，再改变对象引用。所以在一般情况下我们推荐使用 StringBuffer ，特别是字符串对象经常改变的情况下。而在某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：

String S1 = “This is only a” + “ simple” + “ test”;

StringBuffer Sb = new StringBuilder(“This is only a”).append(“ simple”).append(“ test”); 你会很惊讶的发现，生成 String S1 对象的速度简直太快了，而这个时候 StringBuffer 居然速度上根本一点都不占优势。其实这是 JVM 的一个把戏，在 JVM 眼里，这个  String S1 = “This is only a” + “ simple” + “test”; 其实就是：  String S1 = “This is only a simple test”; 所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象的话，速度就没那么快了，譬如：  String S2 = “This is only a”; String S3 = “ simple”; String S4 = “ test”; String S1 = S2 +S3 + S4; 这时候 JVM 会规规矩矩的按照原来的方式去做

在大部分情况下 StringBuffer > String

StringBuffer

Java.lang.StringBuffer线程安全的可变字符序列。一个类似于 String 的字符串缓冲区，但不能修改。虽然在任意时间点上它都包含某种特定的字符序列，但通过某些方法调用可以改变该序列的长度和内容。

可将字符串缓冲区安全地用于多个线程。可以在必要时对这些方法进行同步，因此任意特定实例上的所有操作就好像是以串行顺序发生的，该顺序与所涉及的每个线程进行的方法调用顺序一致。

StringBuffer 上的主要操作是 append 和 insert 方法，可重载这些方法，以接受任意类型的数据。每个方法都能有效地将给定的数据转换成字符串，然后将该字符串的字符追加或插入到字符串缓冲区中。append 方法始终将这些字符添加到缓冲区的末端；而 insert 方法则在指定的点添加字符。

例如，如果 z 引用一个当前内容是“start”的字符串缓冲区对象，则此方法调用 z.append("le") 会使字符串缓冲区包含“startle”，而 z.insert(4, "le") 将更改字符串缓冲区，使之包含“starlet”。

在大部分情况下 StringBuilder > StringBuffer

java.lang.StringBuilder

java.lang.StringBuilder一个可变的字符序列是5.0新增的。此类提供一个与 StringBuffer 兼容的 API，但不保证同步。该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuffer 要快。两者的方法基本相同

## String s=new String(“abc”); new了几个对象

两个

## 数组有没有length()这个方法? String有没有length()这个方法？

- 数组中没有length()这个方法，但是数组中有length这个属性。用来表示数组的长度。
- String中有length()这个方法。用来得到字符串的长度。

## String 源码分析

[String源码分析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/String%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90.md)

# 内部类

## Static Inner Class 和 Inner Class 的不同

- 静态内部类不持有外部类的引用；非静态内部类持有外部类的引用。
- 静态内部类可以有静态成员（方法、属性），而非静态内部类则不用有静态成员（方法、属性）。
- 静态内部类只能访问外部类的静态成员和静态方法，而非静态内部类则可以访问内部类的所以成员（方法、属性）。
- 实例化一个静态内部类不依赖于外部类的实例，直接实例化内部类对象；实例化一个非静态内部类依赖于外部类的实例，通过外部类的实例生成内部类的实例。
- 调用静态内部类的方法或静态变量，直接通过类名调用。

## 内部类机制

为什么内部类拥有外部类的所有元素的访问权？
当某个外围类对象创建一个内部连对象时，内部类对象必定会捕获一个指向那个外围类对象的引用。内部类对象只能在与其外部类对象关联的情况下才能被创建（在内部类非static时），构建内部类需要一个外部类的引用，内部类正是利用这个引用去访问外部类的。

内部类的种类
按照内部类所在的位置不同，内部类可以分为以下几种：

1. 成员内部类
2. 方法内部类
3. 匿名内部类
4. 静态内部类

## 内部类的作用？

- 内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立。
- 在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。
- 创建内部类对象的时刻并不依赖与外围类对象的创建。
- 内部类并没有令人迷惑的“is-a”关系，它就是一个独立的实体。
- 内部类提供了更好的封装，除了外围类，其他类都不能访问。

## 静态内部类、内部类、匿名内部类，为什么内部类会持有外部类的引用？持有的引用是this？还是其它？

```
静态内部类：使用static修饰的内部类
匿名内部类：使用new生成的内部类
因为内部类的产生依赖于外部类，持有的引用是类名.this。
```

# 继承

## 父类的静态方法能否被子类重写

不能。

父类的静态方法是不能被子类重写的，其实重写只能适用于实例方法，不能用于静态方法，对于上面这种静态方法而言，我们应该称之为隐藏。

Java静态方法形式上可以重写，但从本质上来说不是Java的重写。因为静态方法只与类相关，不与具体实现相关。声明的是什么类，则引用相应类的静态方法(本来静态无需声明，可以直接引用)。并且static方法不是后期绑定的，它在编译期就绑定了。换句话说，这个方法不会进行多态的判断，只与声明的类有关。

# 抽象类和接口

## 抽象类和接口的区别

- 默认的方法实现。

抽象类可以有默认的方法实现完全是抽象的。接口根本不存在方法的实现。

- 实现。

使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现。

- 构造器。

抽象类可以有构造器。接口不能有构造器。

- 与正常Java类的区别。

除了你不能实例化抽象类之外，它和普通Java类没有任何区 接口是完全不同的类型

- 访问修饰符

抽象方法可以有public、protected和default这些修饰符 接口方法默认修饰符是public。你不可以使用其它修饰符。

- main方法
  抽象方法可以有main方法并且我们可以运行它。接口没有main方法，因此我们不能运行它。
- 多继承

抽象类在java语言中所表示的是一种继承关系，一个子类只能存在一个父类，但是可以存在多个接口。

- 速度

抽象类比接口速度要快。接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。

- 添加新方法

如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。如果你往接口中添加方法，那么你必须改变实现该接口的类。

------

参考：http://www.cnblogs.com/dolphin0520/p/3811437.html

1.语法层面上的区别

　　1）抽象类可以提供成员方法的实现细节，而接口中只能存在public abstract 方法；

　　2）抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的；

　　3）接口中不能含有静态代码块以及静态方法，而抽象类可以有静态代码块和静态方法；

　　4）一个类只能继承一个抽象类，而一个类却可以实现多个接口。

2.设计层面上的区别

　　1）抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。举个简单的例子，飞机和鸟是不同类的事物，但是它们都有一个共性，就是都会飞。那么在设计的时候，可以将飞机设计为一个类Airplane，将鸟设计为一个类Bird，但是不能将 飞行 这个特性也设计为类，因此它只是一个行为特性，并不是对一类事物的抽象描述。此时可以将 飞行 设计为一个接口Fly，包含方法fly( )，然后Airplane和Bird分别根据自己的需要实现Fly这个接口。然后至于有不同种类的飞机，比如战斗机、民用飞机等直接继承Airplane即可，对于鸟也是类似的，不同种类的鸟直接继承Bird类即可。从这里可以看出，继承是一个 "是不是"的关系，而 接口 实现则是 "有没有"的关系。如果一个类继承了某个抽象类，则子类必定是抽象类的种类，而接口实现则是有没有、具备不具备的关系，比如鸟是否能飞（或者是否具备飞行这个特点），能飞行则可以实现这个接口，不能飞行就不实现这个接口。

　　2）设计层面不同，抽象类作为很多子类的父类，它是一种模板式设计。而接口是一种行为规范，它是一种辐射式设计。什么是模板式设计？最简单例子，大家都用过ppt里面的模板，如果用模板A设计了ppt B和ppt C，ppt B和ppt C公共的部分就是模板A了，如果它们的公共部分需要改动，则只需要改动模板A就可以了，不需要重新对ppt B和ppt C进行改动。而辐射式设计，比如某个电梯都装了某种报警器，一旦要更新报警器，就必须全部更新。也就是说对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动。
​    

## 接口的意义

- 提供一个规范，要求类必须实现指定的方法。
- 解决 Java 中的单继承问题，可以用接口来实现多继承的功能，简单化多重继承中继承树的复杂程度。
- 增强程序的扩展性。

## 抽象类的意义

为其子类提供一个公共的类型、封装子类中的重复内容、定义抽象方法，子类虽然有不同的实现，但是定义是一致的。

## 接口是否可继承接口? 抽象类是否可实现(implements)接口? 抽象类是否可继承实体类(concreteclass)?

1.接口可以继承接口..但是要使用extends~而不是用implements

如:interface a{}
interface b extends a{}

2.抽象类可以实现接口..

比如java.util中的AbstractCollection类就是实现的Collection接口

3.抽象类可以继承实体类

下面这段执行无误的代码说明的所有的问题:

```
interface MyInterface {

}

interface AnotherInterface extends MyInterface {

}

class EntityClass {

}

abstract class AbstractClass extends EntityClass implements MyInterface {

}
```

## abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized?

都不行。

abstract的method不可以是static的，因为抽象的方法是要被子类实现的，而static与子类扯不上关系！

native方法表示该方法要用另外一种依赖平台的编辑语言实现的，不存在者被子类实现的问题，所以，他也不能是抽象的，不能与abstract混用。

关于synchronized中avstract合用的问题，我觉得也不行，因为我觉得 synchronized应该是作用在一个具体的方法上才有意义。而且，方法上的synchronized同步所使用的同步锁对象是this，而抽象方法 上无法确定this是什么。 

# final

## 类使用 final 修饰符的用处？

当用final修饰一个类时，表明这个类不能被继承。也就是说，如果一个类你永远不会让他被继承，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意final类中的所有成员方法都会被隐式地指定为final方法。

在使用final修饰类的时候，要注意谨慎选择，除非这个类真的在以后不会用来继承或者出于安全的考虑，尽量不要将类设计为final类。

参考：http://www.cnblogs.com/dolphin0520/p/3736238.html

## finally final finalize的作用？

final用于声明属性，方法和类，分别表示属性不可交变，方法不可覆盖，类不可继承。

finally是异常处理语句结构的一部分，表示总是执行。

finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，供垃圾收集时的其他资源回收，例如关闭文件等。

# 容器

## List, Set, Map是否继承自Collection接口?

List，Set是，Map不是。

```
Collection

　　├List

　　│├LinkedList

　　│├ArrayList

　　│└Vector

　　│　└Stack

　　└Set

　　Map

　　├Hashtable

　　├HashMap

　　└WeakHashMap
```

## hashCode方法的作用

对于包含容器类型的程序设计语言来说，基本上都会涉及到hashCode。在Java中也一样，hashCode方法的主要作用是为了配合基于散列的集合一起正常运行，这样的散列集合包括HashSet、HashMap以及HashTable。

　　为什么这么说呢？考虑一种情况，当向集合中插入对象时，如何判别在集合中是否已经存在该对象了？（注意：集合中不允许重复的元素存在）

　　也许大多数人都会想到调用equals方法来逐个进行比较，这个方法确实可行。但是如果集合中已经存在一万条数据或者更多的数据，如果采用equals方法去逐一比较，效率必然是一个问题。此时hashCode方法的作用就体现出来了，当集合要添加新的对象时，先调用这个对象的hashCode方法，得到对应的hashcode值，实际上在HashMap的具体实现中会用一个table保存已经存进去的对象的hashcode值，如果table中没有该hashcode值，它就可以直接存进去，不用再进行任何比较了；如果存在该hashcode值， 就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址，所以这里存在一个冲突解决的问题，这样一来实际调用equals方法的次数就大大降低了，说通俗一点：Java中的hashCode方法就是根据一定的规则将与对象相关的信息（比如对象的存储地址，对象的字段等）映射成一个数值，这个数值称作为散列值。下面这段代码是java.util.HashMap的中put方法的具体实现：

```
public V put(K key, V value) {
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key.hashCode());
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
 
        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```

　　put方法是用来向HashMap中添加新的元素，从put方法的具体实现可知，会先调用hashCode方法得到该元素的hashCode值，然后查看table中是否存在该hashCode值，如果存在则调用equals方法重新确定是否存在该元素，如果存在，则更新value值，否则将新的元素添加到HashMap中。从这里可以看出，hashCode方法的存在是为了减少equals方法的调用次数，从而提高程序效率。
　　
hashCode返回的就是对象的存储地址，事实上这种看法是不全面的，确实有些JVM在实现时是直接返回对象的存储地址，但是大多时候并不是这样，只能说可能存储地址有一定关联。

因此有人会说，可以直接根据hashcode值判断两个对象是否相等吗？肯定是不可以的，因为不同的对象可能会生成相同的hashcode值。虽然不能根据hashcode值判断两个对象是否相等，但是可以直接根据hashcode值判断两个对象不等，如果两个对象的hashcode值不等，则必定是两个不同的对象。如果要判断两个对象是否真正相等，必须通过equals方法。　　

- 如果equals方法得到的结果为true，则两个对象的hashcode值必定相等；
- 如果equals方法得到的结果为false，则两个对象的hashcode值不一定不同；
- 如果两个对象的hashcode值不等，则equals方法得到的结果必定为false；
- 如果两个对象的hashcode值相等，则equals方法得到的结果未知。

## 当x.equals(y)等于true时，x.hashCode()与y.hashCode()可以不相等，这句话对不对?

如果equals方法得到的结果为true，则两个对象的hashcode值必定相等；

## Java 中 == 和 equals 和 hashCode 的区别

1. '=='是用来比较两个变量（基本类型和对象类型）的值是否相等的， 如果两个变量是基本类型的，那很容易，直接比较值就可以了。如果两个变量是对象类型的，那么它还是比较值，只是它比较的是这两个对象在栈中的引用（即地址）。 对象是放在堆中的，栈中存放的是对象的引用（地址）。由此可见'=='是对栈中的值进行比较的。如果要比较堆中对象的内容是否相同，那么就要重写equals方法了。 
2. Object类中的equals方法就是用'=='来比较的，所以如果没有重写equals方法，equals和==是等价的。 通常我们会重写equals方法，让equals比较两个对象的内容，而不是比较对象的引用（地址）因为往往我们觉得比较对象的内容是否相同比比较对象的引用（地址）更有意义。 
3. Object类中的hashCode是返回对象在内存中地址转换成的一个int值（可以就当做地址看）。所以如果没有重写hashCode方法，任何对象的hashCode都是不相等的。通常在集合类的时候需要重写hashCode方法和equals方法，因为如果需要给集合类（比如：HashSet）添加对象，那么在添加之前需要查看给集合里是否已经有了该对象，比较好的方式就是用hashCode。 
4. 注意的是String、Integer、Boolean、Double等这些类都重写了equals和hashCode方法，这两个方法是根据对象的内容来比较和计算hashCode的。（详细可以查看jdk下的String.java源代码），所以只要对象的基本类型值相同，那么hashcode就一定相同。
5. equals()相等的两个对象，hashcode()一般是相等的，最好在重写equals()方法时，重写hashcode()方法； equals()不相等的两个对象，却并不能证明他们的hashcode()不相等。换句话说，equals()方法不相等的两个对象，hashcode()有可能相等。 反过来：hashcode()不等，一定能推出equals()也不等；hashcode()相等，equals()可能相等，也可能不等。在object类中，hashcode()方法是本地方法，返回的是对象的引用（地址值），而object类中的equals()方法比较的也是两个对象的引用（地址值），如果equals()相等，说明两个对象地址值也相等，当然hashcode()也就相等了。

有许多人学了很长时间的Java，但一直不明白hashCode方法的作用，我来解释一下吧。首先，想要明白hashCode的作用，你必须要先知道Java中的集合。　　总的来说，Java中的集合（Collection）有两类，一类是List，再有一类是Set。你知道它们的区别吗？前者集合内的元素是有序的，元素可以重复；后者元素无序，但元素不可重复。那么这里就有一个比较严重的问题了：要想保证元素不重复，可两个元素是否重复应该依据什么来判断呢？这就是Object.equals方法了。但是，如果每增加一个元素就检查一次，那么当元素很多时，后添加到集合中的元素比较的次数就非常多了。也就是说，如果集合中现在已经有1000个元素，那么第1001个元素加入集合时，它就要调用1000次equals方法。这显然会大大降低效率。    于是，Java采用了哈希表的原理。哈希（Hash）实际上是个人名，由于他提出一哈希算法的概念，所以就以他的名字命名了。哈希算法也称为散列算法，是将数据依特定算法直接指定到一个地址上。如果详细讲解哈希算法，那需要更多的文章篇幅，我在这里就不介绍了。初学者可以这样理解，hashCode方法实际上返回的就是对象存储的物理地址（实际可能并不是）。   这样一来，当集合要添加新的元素时，先调用这个元素的hashCode方法，就一下子能定位到它应该放置的物理位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。所以这里存在一个冲突解决的问题。这样一来实际调用equals方法的次数就大大降低了，几乎只需要一两次。   所以，Java对于eqauls方法和hashCode方法是这样规定的：1、如果两个对象相同，那么它们的hashCode值一定要相同；2、如果两个对象的hashCode相同，它们并不一定相同    上面说的对象相同指的是用eqauls方法比较。    你当然可以不按要求去做了，但你会发现，相同的对象可以出现在Set集合中。同时，增加新元素的效率会大大下降

## Set里的元素是不能重复的，那么用什么方法来区分重复与否呢? 是用==还是equals()? 它们有何区别?

使用equals()区分。

==是用来判断两者是否是同一对象（同一事物），而equals是用来判断是否引用同一个对象。

set里面存放的是对象的引用，所以当两个元素只要满足了equals()时就已经指向同一个对象，也就出现了重复元素。所以应该用equals()来判断。

## HashMap的底层实现

参考：https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/HashMap%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md

## HashMap 的实现原理

- HashMap概述：HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。
- HashMap的数据结构：在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。

从上图中可以看出，HashMap底层就是一个数组结构，数组中的每一项又是一个链表。当新建一个HashMap的时候，就会初始化一个数组。

## 列举 Java 的集合和它们的继承关系

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-19/59789853.jpg)

## Collection包结构，与Collections的区别。

Collection是一个接口，它是Set、List等容器的父接口；Collections是一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包括对容器的搜索、排序、线程安全化等等。

## 多线程环境中安全使用集合API

在集合API中，最初设计的Vector和Hashtable是多线程安全的。例如：对于Vector来说，用来添加和删除元素的方法是同步的。如果只有一个线程与Vector的实例交互，那么，要求获取和释放对象锁便是一种浪费，另外在不必要的时候如果滥用同步化，也有可能会带来死锁。因此，对于更改集合内容的方法，没有一个是同步化的。集合本质上是非多线程安全的，当多个线程与集合交互时，为了使它多线程安全，必须采取额外的措施。

在Collections类 中有多个静态方法，它们可以获取通过同步方法封装非同步集合而得到的集合：

-public static Collection synchronizedCollention(Collection c)
-public static List synchronizedList(list l)
-public static Map synchronizedMap(Map m)
-public static Set synchronizedSet(Set s)
-public static SortedMap synchronizedSortedMap(SortedMap sm)
-public static SortedSet synchronizedSortedSet(SortedSet ss)

这些方法基本上返回具有同步集合方法版本的新类。比如，为了创建多线程安全且由ArrayList支持的List，可以使用如下代码：

```
List list = Collection.synchronizedList(new ArrayList());

```

注意，ArrayList实例马上封装起来，不存在对未同步化ArrayList的直接引用（即直接封装匿名实例）。这是一种最安全的途径。如果另一个线程要直接引用ArrayList实例，它可以执行非同步修改。

下面给出一段多线程中安全遍历集合元素的示例。我们使用Iterator逐个扫描List中的元素，在多线程环境中，当遍历当前集合中的元素时，一般希望阻止其他线程添加或删除元素。安全遍历的实现方法如下：

```
import java.util.*;  

public class SafeCollectionIteration extends Object {  
    public static void main(String[] args) {  
        //为了安全起见，仅使用同步列表的一个引用，这样可以确保控制了所有访问  
        //集合必须同步化，这里是一个List  
        List wordList = Collections.synchronizedList(new ArrayList());  

        //wordList中的add方法是同步方法，会获取wordList实例的对象锁  
        wordList.add("Iterators");  
        wordList.add("require");  
        wordList.add("special");  
        wordList.add("handling");  

        //获取wordList实例的对象锁，  
        //迭代时，阻塞其他线程调用add或remove等方法修改元素  
        synchronized ( wordList ) {  
            Iterator iter = wordList.iterator();  
            while ( iter.hasNext() ) {  
                String s = (String) iter.next();  
                System.out.println("found string: " + s + ", length=" + s.length());  
            }  
        }  
    }  
}  

```

这里需要注意的是：在Java语言中，大部分的线程安全类都是相对线程安全的，它能保证对这个对象单独的操作时线程安全的，我们在调用的时候不需要额外的保障措施，但是对于一些特定的连续调用，就可能需要在调用端使用额外的同步手段来保证调用的正确性。例如Vector、HashTable、Collections的synchronizedXxxx（）方法包装的集合等。

## Java 集合框架

[Java集合框架](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/Java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6.md)

## 集合类的源码分析

[ArrayList源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/ArrayList%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[LinkedList源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/LinkedList%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[Vector源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/Vector%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[HashMap源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/HashMap%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[HashTable源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/HashTable%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[LinkedHashMap源码剖析](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaSE/LinkedHashMap%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

# 容器类之间的区别

## HashMap 和 HashTable 的区别

第一，继承不同。

```
public class Hashtable extends Dictionary implements Map
public class HashMap  extends AbstractMap implements Map
```

第二

Hashtable 中的方法是同步的，而HashMap中的方法在缺省情况下是非同步的。在多线程并发的环境下，可以直接使用Hashtable，但是要使用HashMap的话就要自己增加同步处理了。

第三

Hashtable中，key和value都不允许出现null值。

在HashMap中，null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null。当get()方法返回null值时，即可以表示 HashMap中没有该键，也可以表示该键所对应的值为null。因此，在HashMap中不能由get()方法来判断HashMap中是否存在某个键， 而应该用containsKey()方法来判断。

第四，两个遍历方式的内部实现上不同。

Hashtable、HashMap都使用了 Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式 。

第五

哈希值的使用不同，HashTable直接使用对象的hashCode。而HashMap重新计算hash值。

第六

Hashtable和HashMap它们两个内部实现方式的数组的初始大小和扩容的方式。HashTable中hash数组默认大小是11，增加的方式是 old*2+1。HashMap中hash数组的默认大小是16，而且一定是2的指数。 

## ArrayMap 和 HashMap 的区别

- 存储方式不一样。ArrayMap 使用两个数组来存储，HashMap 则使用一个链表数组来存储。
- 扩容处理方法不一样。ArrayMap 使用 System.arraycopy() copy 数组，而 HashMap 使用 new HashMapEntry 重新创建数组。
- ArrayMap 提供了数组收缩的功能，在clear或remove后，会重新收缩数组，节省空间。
- ArrayMap 采用二分法查找。

## ArrayList Vector LinkedList 三者的区别

- 三者都实现了List 接口；但 LinkedList 还实现了 Queue 接口。
- ArrayList 和 Vector 使用数组存储数据；LinkedList 使用双向链表存储数据。
- ArrayList 和 LinkedList 是非线程安全的；Vector 是线程安全的。
- ArrayList 数据增长时空间增长50%；而 Vector 是增长1倍；
- LinkedList 在添加、删除元素时具有更好的性能，但读取性能要低一些。
- LinkedList 与 ArrayList 最大的区别是 LinkedList 更加灵活，并且部分方法的效率比 ArrayList 对应方法的效率要高很多，对于数据频繁出入的情况下，并且要求操作要足够灵活，建议使用 LinkedList；对于数组变动不大，主要是用来查询的情况下，可以使用 ArrayList。 

## TreeMap、HashMap、LinkedHashMap的底层实现区别。

[http://blog.csdn.net/lolashe/article/details/20806319](http://blog.csdn.net/lolashe/article/details/20806319)

## Set、List之间的区别是什么?*

[http://developer.51cto.com/art/201309/410205_all.htm

## Map、Set、List、Queue、Stack的特点与用法。

[http://www.cnblogs.com/yumo/p/4908718.html](http://www.cnblogs.com/yumo/p/4908718.html)

Collection 是对象集合， Collection 有两个子接口 List 和 Set

List 可以通过下标 (1,2..) 来取得值，值可以重复

而 Set 只能通过游标来取值，并且值是不能重复的

ArrayList ， Vector ， LinkedList 是 List 的实现类

ArrayList 是线程不安全的， Vector 是线程安全的，这两个类底层都是由数组实现的

LinkedList 是线程不安全的，底层是由链表实现的   

Map 是键值对集合

HashTable 和 HashMap 是 Map 的实现类
HashTable 是线程安全的，不能存储 null 值
HashMap 不是线程安全的，可以存储 null 值  

Stack类：继承自Vector，实现一个后进先出的栈。提供了几个基本方法，push、pop、peak、empty、search等。

Queue接口：提供了几个基本方法，offer、poll、peek等。已知实现类有LinkedList、PriorityQueue等。

# 内存相关知识

参考：[Android内存泄漏总结](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93.md)

## Java 内存分配策略

Java 程序运行时的内存分配策略有三种,分别是静态分配,栈式分配,和堆式分配，对应的，三种存储策略使用的内存空间主要分别是静态存储区（也称方法区）、栈区和堆区。

- 静态存储区（方法区）：主要存放静态数据、全局 static 数据和常量。这块内存在程序编译时就已经分配好，并且在程序整个运行期间都存在。
- 栈区 ：当方法被执行时，方法体内的局部变量（其中包括基础数据类型、对象的引用）都在栈上创建，并在方法执行结束时这些局部变量所持有的内存将会自动被释放。因为栈内存分配运算内置于处理器的指令集中，效率很高，但是分配的内存容量有限。
- 堆区 ： 又称动态内存分配，通常就是指在程序运行时直接 new 出来的内存，也就是对象的实例。这部分内存在不使用时将会由 Java 垃圾回收器来负责回收。

## 栈与堆的区别：

在方法体内定义的（局部变量）一些基本类型的变量和对象的引用变量都是在方法的栈内存中分配的。当在一段方法块中定义一个变量时，Java 就会在栈中为该变量分配内存空间，当超过该变量的作用域后，该变量也就无效了，分配给它的内存空间也将被释放掉，该内存空间可以被重新使用。

堆内存用来存放所有由 new 创建的对象（包括该对象其中的所有成员变量）和数组。在堆中分配的内存，将由 Java 垃圾回收器来自动管理。在堆中产生了一个数组或者对象后，还可以在栈中定义一个特殊的变量，这个变量的取值等于数组或者对象在堆内存中的首地址，这个特殊的变量就是我们上面说的引用变量。我们可以通过这个引用变量来访问堆中的对象或者数组。

举个例子:

```java
public class Sample {
    int s1 = 0;
    Sample mSample1 = new Sample();

    public void method() {
        int s2 = 1;
        Sample mSample2 = new Sample();
    }
}

Sample mSample3 = new Sample();
```

Sample 类的局部变量 s2 和引用变量 mSample2 都是存在于栈中，但 mSample2 指向的对象是存在于堆上的。mSample3 指向的对象实体存放在堆上，包括这个对象的所有成员变量 s1 和 mSample1，而它自己存在于栈中。

结论：

局部变量的基本数据类型和引用存储于栈中，引用的对象实体存储于堆中。—— 因为它们属于方法中的变量，生命周期随方法而结束。

成员变量全部存储与堆中（包括基本数据类型，引用和引用的对象实体）—— 因为它们属于类，类对象终究是要被new出来使用的。

## Java是如何管理内存

Java的内存管理就是对象的分配和释放问题。在 Java 中，程序员需要通过关键字 new 为每个对象申请内存空间 (基本类型除外)，所有的对象都在堆 (Heap)中分配空间。另外，对象的释放是由 GC 决定和执行的。在 Java 中，内存的分配是由程序完成的，而内存的释放是由 GC 完成的，这种收支两条线的方法确实简化了程序员的工作。但同时，它也加重了JVM的工作。这也是 Java 程序运行速度较慢的原因之一。因为，GC 为了能够正确释放对象，GC 必须监控每一个对象的运行状态，包括对象的申请、引用、被引用、赋值等，GC 都需要进行监控。

监视对象状态是为了更加准确地、及时地释放对象，而释放对象的根本原则就是该对象不再被引用。

为了更好理解 GC 的工作原理，我们可以将对象考虑为有向图的顶点，将引用关系考虑为图的有向边，有向边从引用者指向被引对象。另外，每个线程对象可以作为一个图的起始顶点，例如大多程序从 main 进程开始执行，那么该图就是以 main 进程顶点开始的一棵根树。在这个有向图中，根顶点可达的对象都是有效对象，GC将不回收这些对象。如果某个对象 (连通子图)与这个根顶点不可达(注意，该图为有向图)，那么我们认为这个(这些)对象不再被引用，可以被 GC 回收。以下，我们举一个例子说明如何用有向图表示内存管理。对于程序的每一个时刻，我们都有一个有向图表示JVM的内存分配情况。以下右图，就是左边程序运行到第6行的示意图。

[![img](https://camo.githubusercontent.com/ba01b8ae9af4a5e588251316c826bf3e0e695f35/687474703a2f2f7777772e69626d2e636f6d2f646576656c6f706572776f726b732f636e2f6a6176612f6c2d4a6176614d656d6f72794c65616b2f312e676966)](https://camo.githubusercontent.com/ba01b8ae9af4a5e588251316c826bf3e0e695f35/687474703a2f2f7777772e69626d2e636f6d2f646576656c6f706572776f726b732f636e2f6a6176612f6c2d4a6176614d656d6f72794c65616b2f312e676966)

Java使用有向图的方式进行内存管理，可以消除引用循环的问题，例如有三个对象，相互引用，只要它们和根进程不可达的，那么GC也是可以回收它们的。这种方式的优点是管理内存的精度很高，但是效率较低。另外一种常用的内存管理技术是使用计数器，例如COM模型采用计数器方式管理构件，它与有向图相比，精度行低(很难处理循环引用的问题)，但执行效率很高。

## GC引用计数法循环引用会发生什么情况

## 什么是Java中的内存泄露

在Java中，内存泄漏就是存在一些被分配的对象，这些对象有下面两个特点，首先，这些对象是可达的，即在有向图中，存在通路可以与其相连；其次，这些对象是无用的，即程序以后不会再使用这些对象。如果对象满足这两个条件，这些对象就可以判定为Java中的内存泄漏，这些对象不会被GC所回收，然而它却占用内存。

在C++中，内存泄漏的范围更大一些。有些对象被分配了内存空间，然后却不可达，由于C++中没有GC，这些内存将永远收不回来。在Java中，这些不可达的对象都由GC负责回收，因此程序员不需要考虑这部分的内存泄露。

通过分析，我们得知，对于C++，程序员需要自己管理边和顶点，而对于Java程序员只需要管理边就可以了(不需要管理顶点的释放)。通过这种方式，Java提高了编程的效率。

[![img](https://camo.githubusercontent.com/4845ffe2ed44807b01b7bb93647dbff85de6300f/687474703a2f2f7777772e69626d2e636f6d2f646576656c6f706572776f726b732f636e2f6a6176612f6c2d4a6176614d656d6f72794c65616b2f322e676966)](https://camo.githubusercontent.com/4845ffe2ed44807b01b7bb93647dbff85de6300f/687474703a2f2f7777772e69626d2e636f6d2f646576656c6f706572776f726b732f636e2f6a6176612f6c2d4a6176614d656d6f72794c65616b2f322e676966)

因此，通过以上分析，我们知道在Java中也有内存泄漏，但范围比C++要小一些。因为Java从语言上保证，任何对象都是可达的，所有的不可达对象都由GC管理。

对于程序员来说，GC基本是透明的，不可见的。虽然，我们只有几个函数可以访问GC，例如运行GC的函数System.gc()，但是根据Java语言规范定义， 该函数不保证JVM的垃圾收集器一定会执行。因为，不同的JVM实现者可能使用不同的算法管理GC。通常，GC的线程的优先级别较低。JVM调用GC的策略也有很多种，有的是内存使用到达一定程度时，GC才开始工作，也有定时执行的，有的是平缓执行GC，有的是中断式执行GC。但通常来说，我们不需要关心这些。除非在一些特定的场合，GC的执行影响应用程序的性能，例如对于基于Web的实时系统，如网络游戏等，用户不希望GC突然中断应用程序执行而进行垃圾回收，那么我们需要调整GC的参数，让GC能够通过平缓的方式释放内存，例如将垃圾回收分解为一系列的小步骤执行，Sun提供的HotSpot JVM就支持这一特性。

同样给出一个 Java 内存泄漏的典型例子，

```java
Vector v = new Vector(10);
for (int i = 1; i < 100; i++) {
    Object o = new Object();
    v.add(o);
    o = null;   
}
```

在这个例子中，我们循环申请Object对象，并将所申请的对象放入一个 Vector 中，如果我们仅仅释放引用本身，那么 Vector 仍然引用该对象，所以这个对象对 GC 来说是不可回收的。因此，如果对象加入到Vector 后，还必须从 Vector 中删除，最简单的方法就是将 Vector 对象设置为 null。

## 详细Java中的内存泄漏

1.Java内存回收机制

不论哪种语言的内存分配方式，都需要返回所分配内存的真实地址，也就是返回一个指针到内存块的首地址。Java中对象是采用new或者反射的方法创建的，这些对象的创建都是在堆（Heap）中分配的，所有对象的回收都是由Java虚拟机通过垃圾回收机制完成的。GC为了能够正确释放对象，会监控每个对象的运行状况，对他们的申请、引用、被引用、赋值等状况进行监控，Java会使用有向图的方法进行管理内存，实时监控对象是否可以达到，如果不可到达，则就将其回收，这样也可以消除引用循环的问题。在Java语言中，判断一个内存空间是否符合垃圾收集标准有两个：一个是给对象赋予了空值null，以下再没有调用过，另一个是给对象赋予了新值，这样重新分配了内存空间。 

2.Java内存泄漏引起的原因

内存泄漏是指无用对象（不再使用的对象）持续占有内存或无用对象的内存得不到及时释放，从而造成内存空间的浪费称为内存泄漏。内存泄露有时不严重且不易察觉，这样开发者就不知道存在内存泄露，但有时也会很严重，会提示你Out of memory。

Java内存泄漏的根本原因是什么呢？长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄漏，尽管短生命周期对象已经不再需要，但是因为长生命周期持有它的引用而导致不能被回收，这就是Java中内存泄漏的发生场景。具体主要有如下几大类：

1、静态集合类引起内存泄漏：

像HashMap、Vector等的使用最容易出现内存泄露，这些静态变量的生命周期和应用程序一致，他们所引用的所有的对象Object也不能被释放，因为他们也将一直被Vector等引用着。 

例如

```java
Static Vector v = new Vector(10);
for (int i = 1; i<100; i++)
{
Object o = new Object();
v.add(o);
o = null;
}
```

在这个例子中，循环申请Object 对象，并将所申请的对象放入一个Vector 中，如果仅仅释放引用本身（o=null），那么Vector 仍然引用该对象，所以这个对象对GC 来说是不可回收的。因此，如果对象加入到Vector 后，还必须从Vector 中删除，最简单的方法就是将Vector对象设置为null。

2、当集合里面的对象属性被修改后，再调用remove()方法时不起作用。

例如：

```java
public static void main(String[] args)
{
Set<Person> set = new HashSet<Person>();
Person p1 = new Person("唐僧","pwd1",25);
Person p2 = new Person("孙悟空","pwd2",26);
Person p3 = new Person("猪八戒","pwd3",27);
set.add(p1);
set.add(p2);
set.add(p3);
System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:3 个元素!
p3.setAge(2); //修改p3的年龄,此时p3元素对应的hashcode值发生改变

set.remove(p3); //此时remove不掉，造成内存泄漏

set.add(p3); //重新添加，居然添加成功
System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:4 个元素!
for (Person person : set)
{
System.out.println(person);
}
}
```

3、监听器

在java 编程中，我们都需要和监听器打交道，通常一个应用当中会用到很多监听器，我们会调用一个控件的诸如addXXXListener()等方法来增加监听器，但往往在释放对象的时候却没有记住去删除这些监听器，从而增加了内存泄漏的机会。

4、各种连接 

比如数据库连接（dataSourse.getConnection()），网络连接(socket)和io连接，除非其显式的调用了其close（）方法将其连接关闭，否则是不会自动被GC 回收的。对于Resultset 和Statement 对象可以不进行显式回收，但Connection 一定要显式回收，因为Connection 在任何时候都无法自动回收，而Connection一旦回收，Resultset 和Statement 对象就会立即为NULL。但是如果使用连接池，情况就不一样了，除了要显式地关闭连接，还必须显式地关闭Resultset Statement 对象（关闭其中一个，另外一个也会关闭），否则就会造成大量的Statement 对象无法释放，从而引起内存泄漏。这种情况下一般都会在try里面去的连接，在finally里面释放连接。

5、内部类和外部模块的引用

内部类的引用是比较容易遗忘的一种，而且一旦没释放可能导致一系列的后继类对象没有释放。此外程序员还要小心外部模块不经意的引用，例如程序员A 负责A 模块，调用了B 模块的一个方法如：public void registerMsg(Object b);这种调用就要非常小心了，传入了一个对象，很可能模块B就保持了对该对象的引用，这时候就需要注意模块B 是否提供相应的操作去除引用。

6、单例模式 

不正确使用单例模式是引起内存泄漏的一个常见问题，单例对象在初始化后将在JVM的整个生命周期中存在（以静态变量的方式），如果单例对象持有外部的引用，那么这个对象将不能被JVM正常回收，导致内存泄漏，考虑下面的例子：

```java
class A{
public A(){
B.getInstance().setA(this);
}
....
}
//B类采用单例模式
class B{
private A a;
private static B instance=new B();
public B(){}
public static B getInstance(){
return instance;
}
public void setA(A a){
this.a=a;
}
//getter...
} 
```

显然B采用singleton模式，它持有一个A对象的引用，而这个A类的对象将不能被回收。想象下如果A是个比较复杂的对象或者集合类型会发生什么情况

## heap(堆) 和 stack(栈) 有什么区别。

Java 的堆是一个运行时数据区,类的(对象从中分配空间。这些对象通过new、newarray、anewarray和multianewarray等指令建立，它们不需要程序代码来显式的释放。堆是由垃圾回收来负责的，堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，因为它是在运行时动态分配内存的，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。

栈的优势是，存取速度比堆要快，仅次于寄存器，栈数据可以共享。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。栈中主要存放一些基本类型的变量（,int, short, long, byte, float, double, boolean, char）和对象句柄。栈有一个很重要的特殊性，就是存在栈中的数据可以共享。

- Stack存取速度仅次于寄存器，存储效率比heap高，可共享存储数据，但是其中数据的大小和生存期必须在运行前确定。
- Heap是运行时可动态分配的数据区，从速度看比Stack慢，Heap里面的数据不共享，大小和生存期都可以在运行时再确定。

## Java内存回收机制，GC 垃圾回收机制,垃圾回收的优点和原理。并考虑2种回收机制。

1. java语言最显著的特点就是引入了垃圾回收机制，它使java程序员在编写程序时不再考虑内存管理的问题。
2. 由于有这个垃圾回收机制，java中的对象不再有“作用域”的概念，只有引用的对象才有“作用域”。
3. 垃圾回收机制有效的防止了内存泄露，可以有效的使用可使用的内存。
4. 垃圾回收器通常作为一个单独的低级别的线程运行，在不可预知的情况下对内存堆中已经死亡的或很长时间没有用过的对象进行清除和回收。
5. 程序员不能实时的对某个对象或所有对象调用垃圾回收器进行垃圾回收。

垃圾回收机制有分代复制垃圾回收、标记垃圾回收、增量垃圾回收。

## 垃圾回收算法

1. 引用计数法：缺点是无法处理循环引用问题
2. 标记-清除法：标记所有从根结点开始的可达对象，缺点是会造成内存空间不连续，不连续的内存空间的工作效率低于连续的内存空间，不容易分配内存
3. 复制算法：将内存空间分成两块，每次将正在使用的内存中存活对象复制到未使用的内存块中，之后清除正在使用的内存块。算法效率高，但是代价是系统内存折半。适用于新生代(存活对象少，垃圾对象多)
4. 标记－压缩算法：标记－清除的改进，清除未标记的对象时还将所有的存活对象压缩到内存的一端，之后，清理边界所有空间既避免碎片产生，又不需要两块同样大小的内存快，性价比高。适用于老年代。
5. 分代



## Java 虚拟机的特性

Java 语言的一个非常重要的特点就是与平台的无关性，而 Java 虚拟机就是实现这一特点的关键。一般的高级语言如果要在不同的平台上运行，至少需要编译成不同的目标代码。而引入Java语言虚拟机后，Java语言在不同平台上运行时不需要重新编译。Java语言使用模式Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。

[JVM基础知识](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JVM/JVM.md)

[JVM类加载机制](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JVM/JVM%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6.md)

[Java内存区域与内存溢出](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JVM/Java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F%E4%B8%8E%E5%86%85%E5%AD%98%E6%BA%A2%E5%87%BA.md)

## 那些情况下的对象会被垃圾回收机制处理掉

Java 垃圾回收机制最基本的做法是分代回收。内存中的区域被划分成不同的世代，对象根据其存活的时间被保存在对应的世代的区域中。一般的实现是划分为3个世代：年轻、年老和永久。内存的分配是发生在年轻世代中的。当一个对象存活时间足够长的时候，它就会被复制到年老世代中。对于不同的世代可以使用不同的垃圾回收算法。进行世代划分的出发点是对应用中对象存活时间进行研究之后得出的统计规律。一般来说，一个应用中的大部分对象的存活时间都很短。比如局部变量的存活时间就只在方法的执行过程中。基于这一点，对于年轻世代的垃圾回收算法就可以很有针对性。

## 状态机

## Java的四种引用，强弱软虚，用到的场景。

JDK1.2之前只有强引用,其他几种引用都是在JDK1.2之后引入的.

- 强引用（Strong Reference）最常用的引用类型，如Object obj = new Object(); 。只要强引用存在则GC时则必定不被回收。
- 软引用（Soft Reference）用于描述还有用但非必须的对象，当堆将发生OOM（Out Of Memory）时则会回收软引用所指向的内存空间，若回收后依然空间不足才会抛出OOM。一般用于实现内存敏感的高速缓存。当真正对象被标记finalizable以及的finalize()方法调用之后并且内存已经清理, 那么如果SoftReference object还存在就被加入到它的 ReferenceQueue.只有前面几步完成后,Soft Reference和Weak Reference的get方法才会返回null
- 弱引用（Weak Reference）发生GC时必定回收弱引用指向的内存空间。和软引用加入队列的时机相同
- 虚引用（Phantom Reference)又称为幽灵引用或幻影引用，虚引用既不会影响对象的生命周期，也无法通过虚引用来获取对象实例，仅用于在发生GC时接收一个系统通知。当一个对象的finalize方法已经被调用了之后，这个对象的幽灵引用会被加入到队列中。通过检查该队列里面的内容就知道一个对象是不是已经准备要被回收了.虚引用和软引用和弱引用都不同,它会在内存没有清理的时候被加入引用队列.虚引用的建立必须要传入引用队列,其他可以没有

# 多线程

## 进程和线程的区别

简而言之,一个程序至少有一个进程,一个进程至少有一个线程。

线程的划分尺度小于进程，使得多线程程序的并发性高。

另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。

线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别。

进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位.

线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源.

一个线程可以创建和撤销另一个线程;同一个进程中的多个线程之间可以并发执行.

进程和线程的主要差别在于它们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响，而线程只是一个进程中的不同执行路径。线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉，所以多进程的程序要比多线程的程序健壮，但在进程切换时，耗费资源较大，效率要差一些。但对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。如果有兴趣深入的话，我建议你们看看《现代操作系统》或者《操作系统的设计与实现》。对就个问题说得比较清楚。

## 什么会导致线程阻塞

线程被堵塞可能是由下述五方面的原因造成的：

1. 调用sleep(毫秒数)，使线程进入“睡眠”状态。在规定的时间内，这个线程是不会运行的。
2. 用suspend()暂停了线程的执行。除非线程收到resume()消息，否则不会返回“可运行”状态。
3. 用wait()暂停了线程的执行。除非线程收到nofify()或者notifyAll()消息，否则不会变成“可运行”。
4. 线程正在等候一些IO（输入输出）操作完成。
5. 线程试图调用另一个对象的“同步”方法，但那个对象处于锁定状态，暂时无法使用。

## Atomic、volatile、synchronized区别

面Java基础的时候遇上了这个问题，说如果只有一个i++;的时候，`volatile`和`synchronized`能否互换。当时也不知道，感觉`volatile`作为修饰变量的时候，变量自加会出现加到一半发生线程调度。再看看当时蒙对了。

`volatile` 
可以保证在一个线程的工作内存中修改了该变量的值，该变量的值立即能回显到主内存中，从而保证所有的线程看到这个变量的值是一致的。但是有个前提，因为它
不具有操作的原子性，也就是它不适合在对该变量的写操作依赖于变量本身自己。就比如i++、i+=1;这种。但是可以改为num=i+1;如果i是一个 `volatile` 类型，那么num就是安全的，总之就是不能作用于自身。

`synchronized`是基于代码块的，只要包含在`synchronized`块中，就是线程安全的。

既然都说了线程安全，就多了解几个：

`AtomicInteger`，一个轻量级的`synchronized`。使用的并不是同步代码块，而是Lock-Free算法(我也不懂，看代码就是一个死循环调用了底层的比较方法直到相同后才退出循环)。最终的结果就是在高并发的时候，或者说竞争激烈的时候效率比`synchronized`高一些。

`ThreadLocal`，线程中私有数据。主要用于线程改变内部的数据时不影响其他线程，使用时需要注意`static`。

详细分析见[这篇文章](http://blog.csdn.net/u010687392/article/details/50549236) 。

再补一个，才学到的。利用`clone()`方法，如果是一个类的多个对象想共用对象内部的一个变量，而又不想这个变量static，可以使用浅复制方式。(查看设计模式原型模式)

## 并发编程中实现内存可见的两种方法比较：加锁和volatile变量

1. volatile变量是一种稍弱的同步机制在访问volatile变量时不会执行加锁操作，因此也就不会使执行线程阻塞，因此volatile变量是一种比synchronized关键字更轻量级的同步机制。


1. 从内存可见性的角度看，写入volatile变量相当于退出同步代码块，而读取volatile变量相当于进入同步代码块。
2. 在代码中如果过度依赖volatile变量来控制状态的可见性，通常会比使用锁的代码更脆弱，也更难以理解。仅当volatile变量能简化代码的实现以及对同步策略的验证时，才应该使用它。一般来说，用同步机制会更安全些。
3. 加锁机制（即同步机制）既可以确保可见性又可以确保原子性，而volatile变量只能确保可见性，原因是声明为volatile的简单变量如果当前值与该变量以前的值相关，那么volatile关键字不起作用，也就是说如下的表达式都不是原子操作：“count++”、“count = count+1”。

当且仅当满足以下所有条件时，才应该使用volatile变量：

1. 对变量的写入操作不依赖变量的当前值，或者你能确保只有单个线程更新变量的值。
2. 该变量没有包含在具有其他变量的不变式中。

总结：在需要同步的时候，第一选择应该是synchronized关键字，这是最安全的方式，尝试其他任何方式都是有风险的。尤其在、jdK1.5之后，对synchronized同步机制做了很多优化，如：自适应的自旋锁、锁粗化、锁消除、轻量级锁等，使得它的性能明显有了很大的提升。

## 启动一个线程是用run()还是start()?

启动一个线程是调用start()方法，使线程就绪状态，以后可以被调度为运行状态，一个线程必须关联一些具体的执行代码，run()方法是该线程所关联的执行代码。

## 多线程有几种实现方法,都是什么?

多线程有两种实现方法，分别是继承Thread类与实现Runnable接口。

Java 5以前实现多线程有两种实现方法：一种是继承Thread类；另一种是实现Runnable接口。两种方式都要通过重写run()方法来定义线程的行为，推荐使用后者，因为Java中的继承是单继承，一个类有一个父类，如果继承了Thread类就无法再继承其他类了，显然使用Runnable接口更为灵活。

实现Runnable接口相比继承Thread类有如下优势：

1. 可以避免由于Java的单继承特性而带来的局限
2. 增强程序的健壮性，代码能够被多个程序共享，代码与数据是独立的
3. 适合多个相同程序代码的线程区处理同一资源的情况

补充：Java 5以后创建线程还有第三种方式：实现Callable接口，该接口中的call方法可以在线程执行结束时产生一个返回值，代码如下所示：

```java
class MyTask implements Callable<Integer> {  
    private int upperBounds;  

    public MyTask(int upperBounds) {  
        this.upperBounds = upperBounds;  
    }  

    @Override  
    public Integer call() throws Exception {  
        int sum = 0;   
        for(int i = 1; i <= upperBounds; i++) {  
            sum += i;  
        }  
        return sum;  
    }  

}  

public class Test {  

    public static void main(String[] args) throws Exception {  
        List<Future<Integer>> list = new ArrayList<>();  
        ExecutorService service = Executors.newFixedThreadPool(10);  
        for(int i = 0; i < 10; i++) {  
            list.add(service.submit(new MyTask((int) (Math.random() * 100))));  
        }  

        int sum = 0;  
        for(Future<Integer> future : list) {  
            while(!future.isDone()) ;  
            sum += future.get();  
        }  

        System.out.println(sum);  
    }  
}  
```

## 同步有几种实现方法,都是什么?

同步的实现方面有两种，分别是synchronized,wait与notify

## 锁的等级

方法锁、对象锁、类锁

## 同步和异步的区别？

同步：A线程要请求某个资源，但是此资源正在被B线程使用中，因为同步机制存在，A线程请求不到，怎么办，A线程只能等待下去

异步：A线程要请求某个资源，但是此资源正在被B线程使用中，因为没有同步机制存在，A线程仍然请求的到，A线程无需等待

------

在进行网络编程时，我们通常会看到同步、异步、阻塞、非阻塞四种调用方式以及他们的组合。

其中同步方式、异步方式主要是由客户端（client）控制的，具体如下：

同步（Sync）

所谓同步，就是发出一个功能调用时，在没有得到结果之前，该调用就不返回或继续执行后续操作。

异步（Async）

异步与同步相对，当一个异步过程调用发出后，调用者在没有得到结果之前，就可以继续执行后续操作。当这个调用完成后，一般通过状态、通知和回调来通知调用者。对于异步调用，调用的返回并不受调用者控制。

## sleep和wait有什么区别？

一个是用来让线程休息，一个是用来挂起线程

- 对于sleep()方法，我们首先要知道该方法是属于Thread类中的。而wait()方法，则是属于Object类中的。
- sleep()：正在执行的线程主动让出CPU（然后CPU就可以去执行其他任务），在sleep指定时间后CPU再回到该线程继续往下执行。注意：sleep方法只让出了CPU，而并不会释放同步资源锁！

wait()：则是指当前线程让自己暂时退让出同步资源锁，以便其他正在等待该资源的线程得到该资源进而运行，只有调用了notify()方法，之前调用wait()的线程才会解除wait状态，可以去参与竞争同步资源锁，进而得到执行

sleep()方法是线程类（Thread）的静态方法，导致此线程暂停执行指定时间，将执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复
（线程回到就绪（ready）状态），因为调用sleep 不会释放对象锁。wait()是Object 
类的方法，对此对象调用wait()方法导致本线程放弃对象锁(线程暂停执行)，进入等待此对象的等待锁定池，只有针对此对象发出notify 
方法（或notifyAll）后本线程才进入对象锁定池准备获得对象锁进入就绪状态。

## 进程和线程的区别

程序是一段静态的代码。一个进程可以有一个或多个线程，每个线程都有一个唯一的标识符。进程和线程的区别为：进程空间大体分为数据区 、代码区、栈区、堆区。多个进程的内部数据和状态都是完全独立的；而线程共享进程的数据区、代码区、堆区，只有栈区是独立的，所以线程切换比进程切换的代价小。

多线程技术是使得单个程序内部可以在同一时刻执行多个代码段，完成不同的任务，这种机制称为多线程。同时并不是指真正意义上的同一时刻，而是指多个线程轮流占用CPU的时间片。

## Java线程池，线程同步

## 详细讲解一下 synchronized

在并发编程中，多线程同时并发访问的资源叫做临界资源，当多个线程同时访问对象并要求操作相同资源时，分割了原子操作就有可能出现数据的不一致或数据不完整的情况，为避免这种情况的发生，我们会采取同步机制，以确保在某一时刻，方法内只允许有一个线程。

采用synchronized修饰符实现的同步机制叫做互斥锁机制，它所获得的锁叫做互斥锁。每个对象都有一个monitor(锁标记)，当线程拥有这个锁标记时才能访问这个资源，没有锁标记便进入锁池。任何一个对象系统都会为其创建一个互斥锁，这个锁是为了分配给线程的，防止打断原子操作。每个对象的锁只能分配给一个线程，因此叫做互斥锁。

这里就使用同步机制获取互斥锁的情况，进行几点说明：

1. 如果同一个方法内同时有两个或更多线程，则每个线程有自己的局部变量拷贝。
2. 类的每个实例都有自己的对象级别锁。当一个线程访问实例对象中的synchronized同步代码块或同步方法时，该线程便获取了该实例的对象级别锁，其他线程这时如果要访问synchronized同步代码块或同步方法，便需要阻塞等待，直到前面的线程从同步代码块或方法中退出，释放掉了该对象级别锁。
3. 访问同一个类的不同实例对象中的同步代码块，不存在阻塞等待获取对象锁的问题，因为它们获取的是各自实例的对象级别锁，相互之间没有影响。
4. 持有一个对象级别锁不会阻止该线程被交换出来，也不会阻塞其他线程访问同一示例对象中的非synchronized代码。当一个线程A持有一个对象级别锁（即进入了synchronized修饰的代码块或方法中）时，线程也有可能被交换出去，此时线程B有可能获取执行该对象中代码的时间，但它只能执行非同步代码（没有用synchronized修饰），当执行到同步代码时，便会被阻塞，此时可能线程规划器又让A线程运行，A线程继续持有对象级别锁，当A线程退出同步代码时（即释放了对象级别锁），如果B线程此时再运行，便会获得该对象级别锁，从而执行synchronized中的代码。
5. 持有对象级别锁的线程会让其他线程阻塞在所有的synchronized代码外。例如，在一个类中有三个synchronized方法a，b，c，当线程A正在执行一个实例对象M中的方法a时，它便获得了该对象级别锁，那么其他的线程在执行同一实例对象（即对象M）中的代码时，便会在所有的synchronized方法处阻塞，即在方法a，b，c处都要被阻塞，等线程A释放掉对象级别锁时，其他的线程才可以去执行方法a，b或者c中的代码，从而获得该对象级别锁。
6. 使用synchronized（obj）同步语句块，可以获取指定对象上的对象级别锁。obj为对象的引用，如果获取了obj对象上的对象级别锁，在并发访问obj对象时时，便会在其synchronized代码处阻塞等待，直到获取到该obj对象的对象级别锁。当obj为this时，便是获取当前对象的对象级别锁。
7. 类级别锁被特定类的所有示例共享，它用于控制对static成员变量以及static方法的并发访问。具体用法与对象级别锁相似。
8. 互斥是实现同步的一种手段，临界区、互斥量和信号量都是主要的互斥实现方式。synchronized关键字经过编译后，会在同步块的前后分别形成monitorenter和monitorexit这两个字节码指令。根据虚拟机规范的要求，在执行monitorenter指令时，首先要尝试获取对象的锁，如果获得了锁，把锁的计数器加1，相应地，在执行monitorexit指令时会将锁计数器减1，当计数器为0时，锁便被释放了。由于synchronized同步块对同一个线程是可重入的，因此一个线程可以多次获得同一个对象的互斥锁，同样，要释放相应次数的该互斥锁，才能最终释放掉该锁。

## 当一个线程进入一个对象的一个synchronized方法后，其它线程是否可进入此对象的其它方法?

- 可以调用此对象的其他非 synchronized 方法；
- 可以调用此对象的 synchronized static 方法；

```
public class A {
     
    /**
     * 静态方法
     */
    public synchronized static void staticMethod(){}
     
    /**
     * 实例方法
     */
    public synchronized void instanceMethod(){}
    public static void main(String[] args) {
         
        //A实例的创建过程
        Class c = Class.forName("A");
        A a1 = c.newInstance();
        A a2 = c.newInstance();
        A a3 = c.newInstance();
    }
}
```

如上代码所示，你看一看main方法里面A实例的创建过程，这个要先理解
staticMethod这个静态方法，无法你实例化多少次，它都只是存在一个，就像Class c指向的对象，它在jvm中也只会存在一个，staticMethod方法锁住的是c指向的实例。

instanceMethod这个实例方法，你创建多少个A实例，这些实例都存在各自的instanceMethod方法，这个方法前加synchronized关键词，会锁住该instanceMethod方法所在的实例。如a1的instanceMethod方法会锁住a1指向的实例，a2的instanceMethod会锁住a2指向的实例。

由此得出结论，staticMethod与instanceMethod锁住的对象是不可能相同的，这就是两个方法不能同步的原因。 

## 内存可见性

加锁（synchronized同步）的功能不仅仅局限于互斥行为，同时还存在另外一个重要的方面：内存可见性。我们不仅希望防止某个线程正在使用对象状态而另一个线程在同时修改该状态，而且还希望确保当一个线程修改了对象状态后，其他线程能够看到该变化。而线程的同步恰恰也能够实现这一点。

内置锁可以用于确保某个线程以一种可预测的方式来查看另一个线程的执行结果。为了确保所有的线程都能看到共享变量的最新值，可以在所有执行读操作或写操作的线程上加上同一把锁。下图示例了同步的可见性保证。

[![img](https://camo.githubusercontent.com/e55eb94078f86c197d053aa7d05d005fc8bb7d0e/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313331323132323131303239313235)](https://camo.githubusercontent.com/e55eb94078f86c197d053aa7d05d005fc8bb7d0e/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313331323132323131303239313235)

当线程A执行某个同步代码块时，线程B随后进入由同一个锁保护的同步代码块，这种情况下可以保证，当锁被释放前，A看到的所有变量值（锁释放前，A看到的变量包括y和x）在B获得同一个锁后同样可以由B看到。换句话说，当线程B执行由锁保护的同步代码块时，可以看到线程A之前在同一个锁保护的同步代码块中的所有操作结果。如果在线程A unlock M之后，线程B才进入lock M，那么线程B都可以看到线程A unlock M之前的操作，可以得到i=1，j=1。如果在线程B unlock M之后，线程A才进入lock M，那么线程B就不一定能看到线程A中的操作，因此j的值就不一定是1。

现在考虑如下代码：

```
public class  MutableInteger  
{  
    private int value;  

    public int get(){  
        return value;  
    }  
    public void set(int value){  
        this.value = value;  
    }  
}  

```

以上代码中，get和set方法都在没有同步的情况下访问value。如果value被多个线程共享，假如某个线程调用了set，那么另一个正在调用get的线程可能会看到更新后的value值，也可能看不到。

通过对set和get方法进行同步，可以使MutableInteger成为一个线程安全的类，如下：

```
public class  SynchronizedInteger  
{  
    private int value;  

    public synchronized int get(){  
        return value;  
    }  
    public synchronized void set(int value){  
        this.value = value;  
    }  
}  

```

对set和get方法进行了同步，加上了同一把对象锁，这样get方法可以看到set方法中value值的变化，从而每次通过get方法取得的value的值都是最新的value值。     

## 生产者和消费者问题

[生产者和消费者问题](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaConcurrent/%E7%94%9F%E4%BA%A7%E8%80%85%E5%92%8C%E6%B6%88%E8%B4%B9%E8%80%85%E9%97%AE%E9%A2%98.md)

## 守护线程与阻塞线程的四种情况

Java中有两类线程：User Thread(用户线程)、Daemon Thread(守护线程)

用户线程即运行在前台的线程，而守护线程是运行在后台的线程。 守护线程作用是为其他前台线程的运行提供便利服务，而且仅在普通、非守护线程仍然运行时才需要，比如垃圾回收线程就是一个守护线程。当VM检测仅剩一个守护线程，而用户线程都已经退出运行时，VM就会退出，因为如果没有了守护者，也就没有继续运行程序的必要了。如果有非守护线程仍然活着，VM就不会退出。

守护线程并非只有虚拟机内部提供，用户在编写程序时也可以自己设置守护线程。用户可以用Thread的setDaemon(true)方法设置当前线程为守护线程。

虽然守护线程可能非常有用，但必须小心确保其它所有非守护线程消亡时，不会由于它的终止而产生任何危害。因为你不可能知道在所有的用户线程退出运行前，守护线程是否已经完成了预期的服务任务。一旦所有的用户线程退出了，虚拟机也就退出运行了。因此，不要再守护线程中执行业务逻辑操作(比如对数据的读写等)。

还有几点：

1. setDaemon(true)必须在调用线程的start()方法之前设置，否则会跑出IllegalThreadStateException异常。
2. 在守护线程中产生的新线程也是守护线程
3. 不要认为所有的应用都可以分配给守护线程来进行服务，比如读写操作或者计算逻辑。

## 死锁

当线程需要同时持有多个锁时，有可能产生死锁。考虑如下情形：

线程A当前持有互斥所锁lock1，线程B当前持有互斥锁lock2。接下来，当线程A仍然持有lock1时，它试图获取lock2，因为线程B正持有lock2，因此线程A会阻塞等待线程B对lock2的释放。如果此时线程B在持有lock2的时候，也在试图获取lock1，因为线程A正持有lock1，因此线程B会阻塞等待A对lock1的释放。二者都在等待对方所持有锁的释放，而二者却又都没释放自己所持有的锁，这时二者便会一直阻塞下去。这种情形称为死锁。

下面给出一个两个线程间产生死锁的示例，如下：

```
public class Deadlock {

    private String objID;

    public Deadlock(String id) {
        objID = id;
    }

    public synchronized void checkOther(Deadlock other) {
        print("entering checkOther()");  
        try { Thread.sleep(2000); }   
        catch ( InterruptedException x ) { }  
        print("in checkOther() - about to " + "invoke 'other.action()'");  
      //调用other对象的action方法，由于该方法是同步方法，因此会试图获取other对象的对象锁  
        other.action();  
        print("leaving checkOther()");  
    }

    public synchronized void action() {  
        print("entering action()");  
        try { Thread.sleep(500); }   
        catch ( InterruptedException x ) { }  
        print("leaving action()");  
    }  

    public void print(String msg) {
        threadPrint("objID=" + objID + " - " + msg);
    }

    public static void threadPrint(String msg) {  
        String threadName = Thread.currentThread().getName();  
        System.out.println(threadName + ": " + msg);  
    }  

    public static void main(String[] args) {
        final Deadlock obj1 = new Deadlock("obj1");  
        final Deadlock obj2 = new Deadlock("obj2");  

        Runnable runA = new Runnable() {  
            public void run() {  
                obj1.checkOther(obj2);  
            }  
        };  

        Thread threadA = new Thread(runA, "threadA");  
        threadA.start();  

        try { Thread.sleep(200); }   
        catch ( InterruptedException x ) { }  

        Runnable runB = new Runnable() {  
            public void run() {  
                obj2.checkOther(obj1);  
            }  
        };  

        Thread threadB = new Thread(runB, "threadB");  
        threadB.start();  

        try { Thread.sleep(5000); }   
        catch ( InterruptedException x ) { }  

        threadPrint("finished sleeping");  

        threadPrint("about to interrupt() threadA"); 

        threadA.interrupt();  

        try { Thread.sleep(1000); }   
        catch ( InterruptedException x ) { }  

        threadPrint("about to interrupt() threadB");  
        threadB.interrupt();  

        try { Thread.sleep(1000); }   
        catch ( InterruptedException x ) { }  

        threadPrint("did that break the deadlock?");  
    }
}


```

运行结果：

```
threadA: objID=obj1 - entering checkOther()
threadB: objID=obj2 - entering checkOther()
threadA: objID=obj1 - in checkOther() - about to invoke 'other.action()'
threadB: objID=obj2 - in checkOther() - about to invoke 'other.action()'
main: finished sleeping
main: about to interrupt() threadA
main: about to interrupt() threadB
main: did that break the deadlock?

```

从结果中可以看出，在执行到other.action（）时，由于两个线程都在试图获取对方的锁，但对方都没有释放自己的锁，因而便产生了死锁，在主线程中试图中断两个线程，但都无果。

大部分代码并不容易产生死锁，死锁可能在代码中隐藏相当长的时间，等待不常见的条件地发生，但即使是很小的概率，一旦发生，便可能造成毁灭性的破坏。避免死锁是一件困难的事，遵循以下原则有助于规避死锁： 

1. 只在必要的最短时间内持有锁，考虑使用同步语句块代替整个同步方法；
2. 尽量编写不在同一时刻需要持有多个锁的代码，如果不可避免，则确保线程持有第二个锁的时间尽量短暂；
3. 创建和使用一个大锁来代替若干小锁，并把这个锁用于互斥，而不是用作单个对象的对象级别锁；

## 可重入内置锁

每个Java对象都可以用做一个实现同步的锁，这些锁被称为内置锁或监视器锁。线程在进入同步代码块之前会自动获取锁，并且在退出同步代码块时会自动释放锁。获得内置锁的唯一途径就是进入由这个锁保护的同步代码块或方法。

当某个线程请求一个由其他线程持有的锁时，发出请求的线程就会阻塞。然而，由于内置锁是可重入的，因此如果摸个线程试图获得一个已经由它自己持有的锁，那么这个请求就会成功。“重入”意味着获取锁的操作的粒度是“线程”，而不是调用。重入的一种实现方法是，为每个锁关联一个获取计数值和一个所有者线程。当计数值为0时，这个锁就被认为是没有被任何线程所持有，当线程请求一个未被持有的锁时，JVM将记下锁的持有者，并且将获取计数值置为1，如果同一个线程再次获取这个锁，计数值将递增，而当线程退出同步代码块时，计数器会相应地递减。当计数值为0时，这个锁将被释放。

重入进一步提升了加锁行为的封装性，因此简化了面向对象并发代码的开发。分析如下程序：

```
public class Father  
{  
    public synchronized void doSomething(){  
        ......  
    }  
}  

public class Child extends Father  
{  
    public synchronized void doSomething(){  
        ......  
        super.doSomething();  
    }  
}  

```

子类覆写了父类的同步方法，然后调用父类中的方法，此时如果没有可重入的锁，那么这段代码件产生死锁。

由于Fither和Child中的doSomething方法都是synchronized方法，因此每个doSomething方法在执行前都会获取Child对象实例上的锁。如果内置锁不是可重入的，那么在调用super.doSomething时将无法获得该Child对象上的互斥锁，因为这个锁已经被持有，从而线程会永远阻塞下去，一直在等待一个永远也无法获取的锁。重入则避免了这种死锁情况的发生。

同一个线程在调用本类中其他synchronized方法/块或父类中的synchronized方法/块时，都不会阻碍该线程地执行，因为互斥锁时可重入的。

## 使用wait/notify/notifyAll实现线程间通信

在Java中，可以通过配合调用Object对象的wait（）方法和notify（）方法或notifyAll（）方法来实现线程间的通信。在线程中调用wait（）方法，将阻塞等待其他线程的通知（其他线程调用notify（）方法或notifyAll（）方法），在线程中调用notify（）方法或notifyAll（）方法，将通知其他线程从wait（）方法处返回。

Object是所有类的超类，它有5个方法组成了等待/通知机制的核心：notify（）、notifyAll（）、wait（）、wait（long）和wait（long，int）。在Java中，所有的类都从Object继承而来，因此，所有的类都拥有这些共有方法可供使用。而且，由于他们都被声明为final，因此在子类中不能覆写任何一个方法。

这里详细说明一下各个方法在使用中需要注意的几点：

1、wait（）

```
public final void wait()  throws InterruptedException,IllegalMonitorStateException

```

该方法用来将当前线程置入休眠状态，直到接到通知或被中断为止。在调用wait（）之前，线程必须要获得该对象的对象级别锁，即只能在同步方法或同步块中调用wait（）方法。进入wait（）方法后，当前线程释放锁。在从wait（）返回前，线程与其他线程竞争重新获得锁。如果调用wait（）时，没有持有适当的锁，则抛出IllegalMonitorStateException，它是RuntimeException的一个子类，因此，不需要try-catch结构。

2、notify（）

```
public final native void notify() throws IllegalMonitorStateException

```

该方法也要在同步方法或同步块中调用，即在调用前，线程也必须要获得该对象的对象级别锁，的如果调用notify（）时没有持有适当的锁，也会抛出IllegalMonitorStateException。

该方法用来通知那些可能等待该对象的对象锁的其他线程。如果有多个线程等待，则线程规划器任意挑选出其中一个wait（）状态的线程来发出通知，并使它等待获取该对象的对象锁（notify后，当前线程不会马上释放该对象锁，wait所在的线程并不能马上获取该对象锁，要等到程序退出synchronized代码块后，当前线程才会释放锁，wait所在的线程也才可以获取该对象锁），但不惊动其他同样在等待被该对象notify的线程们。当第一个获得了该对象锁的wait线程运行完毕以后，它会释放掉该对象锁，此时如果该对象没有再次使用notify语句，则即便该对象已经空闲，其他wait状态等待的线程由于没有得到该对象的通知，会继续阻塞在wait状态，直到这个对象发出一个notify或notifyAll。这里需要注意：它们等待的是被notify或notifyAll，而不是锁。这与下面的notifyAll（）方法执行后的情况不同。 

3、notifyAll（）

```
public final native void notifyAll() throws IllegalMonitorStateException

```

该方法与notify（）方法的工作方式相同，重要的一点差异是：

notifyAll使所有原来在该对象上wait的线程统统退出wait的状态（即全部被唤醒，不再等待notify或notifyAll，但由于此时还没有获取到该对象锁，因此还不能继续往下执行），变成等待获取该对象上的锁，一旦该对象锁被释放（notifyAll线程退出调用了notifyAll的synchronized代码块的时候），他们就会去竞争。如果其中一个线程获得了该对象锁，它就会继续往下执行，在它退出synchronized代码块，释放锁后，其他的已经被唤醒的线程将会继续竞争获取该锁，一直进行下去，直到所有被唤醒的线程都执行完毕。

4、wait（long）和wait（long,int）

显然，这两个方法是设置等待超时时间的，后者在超值时间上加上ns，精度也难以达到，因此，该方法很少使用。对于前者，如果在等待线程接到通知或被中断之前，已经超过了指定的毫秒数，则它通过竞争重新获得锁，并从wait（long）返回。另外，需要知道，如果设置了超时时间，当wait（）返回时，我们不能确定它是因为接到了通知还是因为超时而返回的，因为wait（）方法不会返回任何相关的信息。但一般可以通过设置标志位来判断，在notify之前改变标志位的值，在wait（）方法后读取该标志位的值来判断，当然为了保证notify不被遗漏，我们还需要另外一个标志位来循环判断是否调用wait（）方法。

深入理解：

- 如果线程调用了对象的wait（）方法，那么线程便会处于该对象的等待池中，等待池中的线程不会去竞争该对象的锁。
- 当有线程调用了对象的notifyAll（）方法（唤醒所有wait线程）或notify（）方法（只随机唤醒一个wait线程），被唤醒的的线程便会进入该对象的锁池中，锁池中的线程会去竞争该对象锁。
- 优先级高的线程竞争到对象锁的概率大，假若某线程没有竞争到该对象锁，它还会留在锁池中，唯有线程再次调用wait（）方法，它才会重新回到等待池中。而竞争到对象锁的线程则继续往下执行，直到执行完了synchronized代码块，它会释放掉该对象锁，这时锁池中的线程会继续竞争该对象锁。

## 线程中断

[线程中断](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaConcurrent/%E7%BA%BF%E7%A8%8B%E4%B8%AD%E6%96%AD.md)

# Error 和 Exception

## Error与Exception的区别

Error是Throwable的子类，用于标记严重错误。合理的应用程序不应该去try/catch这种错误。绝大多数的错误都是非正常的，就根本不该出现的。

Exception 是Throwable的一种形式的子类，用于指示一种合理的程序想去catch的条件。即它仅仅是一种程序运行条件，而非严重错误，并且鼓励用户程序去catch它。

checked exceptions: 通常是从一个可以恢复的程序中抛出来的，并且最好能够从这种异常中使用程序恢复。比如FileNotFoundException, ParseException等。

unchecked exceptions: 通常是如果一切正常的话本不该发生的异常，但是的确发生了。比如ArrayIndexOutOfBoundException, ClassCastException等。从语言本身的角度讲，程序不该去catch这类异常，虽然能够从诸如RuntimeException这样的异常中catch并恢复，但是并不鼓励终端程序员这么做，因为完全没要必要。因为这类错误本身就是bug，应该被修复，出现此类错误时程序就应该立即停止执行。 因此，面对Errors和unchecked exceptions应该让程序自动终止执行，程序员不该做诸如try/catch这样的事情，而是应该查明原因，修改代码逻辑。

## Java中的异常处理机制的简单原理和应用。

JAVA语言如何进行异常处理，关键字：throws,throw,try,catch,finally分别代表什么意义？在try块中可以抛出异常吗？

Java通过面向对象的方法进行异常处理，把各种不同的异常进行分类，并提供了良好的接口。在Java中，每个异常都是一个对象，它是Throwable类或其它子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并进行处理。Java的异常处理是通过5个关键词来实现的：try、catch、throw、throws和finally。一般情况下是用try来执行一段程序，如果出现异常，系统会抛出（throws）一个异常，这时候你可以通过它的类型来捕捉（catch）它，或最后（finally）由缺省处理器来处理。用try来指定一块预防所有“异常”的程序。紧跟在try程序后面，应包含一个catch子句来指定你想要捕捉的“异常”的类型。throw语句用来明确地抛出一个“异常”。throws用来标明一个成员函数可能抛出的各种“异常”。Finally为确保一段代码不管发生什么“异常”都被执行一段代码。可以在一个成员函数调用的外面写一个try语句，在这个成员函数内部写另一个try语句保护其他代码。每当遇到一个try语句，“异常”的框架就放到堆栈上面，直到所有的try语句都完成。如果下一级的try语句没有对某种“异常”进行处理，堆栈就会展开，直到遇到有处理这种“异常”的try语句。

## try{ return} catch{} finally{}; return 还是 finally 先执行。

任何执行try 或者catch中的return语句之前，都会先执行finally语句，如果finally存在的话。

如果finally中有return语句，那么程序就return了，所以finally中的return是一定会被return的。

在try语句中，在执行return语句时，要返回的结果已经准备好了，就在此时，程序转到finally执行了。在转去之前，try中先把要返回的结果存放到不同于x的局部变量中去，执行完finally之后，在从中取出返回结果，因此，即使finally中对变量x进行了改变，但是不会影响返回结果。

# 序列化

## 对象Object读写的是哪两个流

ObjectInputStream

ObjectOutputStream

## 什么是Java序列化，如何实现java序列化

序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决在对对象流进行读写操作时所引发的问题。

序列化的实现：将需要被序列化的类实现Serializable接口，该接口没有需要实现的方法，implements Serializable只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流。

## Serializable和Parcelable



# 反射

## 反射机制

JAVA反射机制是在运行状态中, 对于任意一个类, 都能够知道这个类的所有属性和方法; 对于任意一个对象, 都能够调用它的任意一个方法和属性; 这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制.

主要作用有三：

运行时取得类的方法和字段的相关信息。

创建某个类的新实例(.newInstance())

取得字段引用直接获取和设置对象字段，无论访问修饰符是什么。

用处如下：

观察或操作应用程序的运行时行为。

调试或测试程序，因为可以直接访问方法、构造函数和成员字段。

通过名字调用不知道的方法并使用该信息来创建对象和调用方法。

# 泛型

## 泛型的优缺点

优点：

使用泛型类型可以最大限度地重用代码、保护类型的安全以及提高性能。

泛型最常见的用途是创建集合类。

缺点：

在性能上不如数组快。

## 泛型常用特点，`List<String>`能否转为`List<Object>`

能，但是利用类都继承自Object，所以使用是每次调用里面的函数都要通过强制转换还原回原来的类，这样既不安全，运行速度也慢。

# 网络

## http get 和 post 的区别

- GET请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，参数之间以&相连，如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0%E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如：%E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

POST把提交的数据则放置在是HTTP包的包体中。

- GET方式提交的数据最多只能是1024字节，理论上POST没有限制，可传较大量的数据。

首先是"GET方式提交的数据最多只能是1024字节"，因为GET是通过URL提交数据，那么GET可提交的数据量就跟URL的长度有直接关系了。而实际上，URL不存在参数上限的问题，HTTP协议规范没有对URL长度进行限制。这个限制是特定的浏览器及服务器对它的限制。IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系统的支持。

注意这是限制是整个URL长度，而不仅仅是你的参数值数据长度。

- POST的安全性要比GET的安全性高。注意：这里所说的安全性和上面GET提到的“安全”不是同个概念。上面“安全”的含义仅仅是不作数据修改，而这里安全的含义是真正的Security的含义，比如：通过GET提交数据，用户名和密码将明文出现在URL上，因为(1)登录页面有可能被浏览器缓存，(2)其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了。

------

GET - 从指定的服务器中获取数据
POST - 提交数据给指定的服务器处理

GET方法：
使用GET方法时，查询字符串（键值对）被附加在URL地址后面一起发送到服务器：
/test/demo_form.jsp?name1=value1&name2=value2
特点：

```
GET请求能够被缓存
GET请求会保存在浏览器的浏览记录中
以GET请求的URL能够保存为浏览器书签
GET请求有长度限制
GET请求主要用以获取数据
```

POST方法：
使用POST方法时，查询字符串在POST信息中单独存在，和HTTP请求一起发送到服务器：
POST /test/demo_form.jsp HTTP/1.1
Host: w3schools.com
name1=value1&name2=value2
特点：

```
POST请求不能被缓存下来
POST请求不会保存在浏览器浏览记录中
以POST请求的URL无法保存为浏览器书签
POST请求没有长度限制
```



## UDP 和 TCP 的区别

1. 基于连接与无连接；
2. 对系统资源的要求（TCP较多，UDP少）；
3. UDP程序结构较简单；
4. 流模式与数据包模式 ；
5. TCP保证数据正确性，UDP可能丢包，TCP保证数据顺序，UDP不保证。

TCP：面向连接、传输可靠(保证数据正确性,保证数据顺序)、用于传输大量数据(流模式)、速度慢，建立连接需要开销较多(时间，系统资源)。

UDP：面向非连接、传输不可靠、用于传输少量数据(数据包模式)、速度快。

------

TCP是Tranfer Control Protocol的 简称，是一种面向连接的保证可靠传输的协议。通过TCP协议传输，得到的是一个顺序的无差错的数据流。发送方和接收方的成对的两个socket之间必须建 立连接，以便在TCP协议的基础上进行通信，当一个socket（通常都是server socket）等待建立连接时，另一个socket可以要求进行连接，一旦这两个socket连接起来，它们就可以进行双向数据传输，双方都可以进行发送 或接收操作。

UDP是User Datagram Protocol的简称，是一种无连接的协议，每个数据报都是一个独立的信息，包括完整的源地址或目的地址，它在网络上以任何可能的路径传往目的地，因此能否到达目的地，到达目的地的时间以及内容的正确性都是不能被保证的。

比较：

UDP：

1，每个数据报中都给出了完整的地址信息，因此无需要建立发送方和接收方的连接。

2，UDP传输数据时是有大小限制的，每个被传输的数据报必须限定在64KB之内。

3，UDP是一个不可靠的协议，发送方所发送的数据报并不一定以相同的次序到达接收方

TCP：

1，面向连接的协议，在socket之间进行数据传输之前必然要建立连接，所以在TCP中需要连接时间。

2，TCP传输数据大小限制，一旦连接建立起来，双方的socket就可以按统一的格式传输大的数据。

3，TCP是一个可靠的协议，它确保接收方完全正确地获取发送方所发送的全部数据。

应用：

1，TCP在网络通信上有极强的生命力，例如远程连接（Telnet）和文件传输（FTP）都需要不定长度的数据被可靠地传输。但是可靠的传输是要付出代价的，对数据内容正确性的检验必然占用计算机的处理时间和网络的带宽，因此TCP传输的效率不如UDP高。

2，UDP操作简单，而且仅需要较少的监护，因此通常用于局域网高可靠性的分散系统中client/server应用程序。例如视频会议系统，并不要求音频视频数据绝对的正确，只要保证连贯性就可以了，这种情况下显然使用UDP会更合理一些。

## Socket编程的步骤

Server端Listen(监听)某个端口是否有连接请求，Client端向Server 端发出Connect(连接)请求，Server端向Client端发回Accept（接受）消息。一个连接就建立起来了。Server端和Client 端都可以通过Send，Write等方法与对方通信。

对于一个功能齐全的Socket，都要包含以下基本结构，其工作过程包含以下四个基本的步骤：

（1） 创建Socket；

（2） 打开连接到Socket的输入/出流；

（3） 按照一定的协议对Socket进行读/写操作；

（4） 关闭Socket.（在实际应用中，并未使用到显示的close，虽然很多文章都推荐如此，不过在我的程序中，可能因为程序本身比较简单，要求不高，所以并未造成什么影响。）

# IO（IO,NIO，目前okio已经被集成Android包）

## IO框架主要用到什么设计模式

## JDK的I/O包中就主要使用到了两种设计模式：Adatper模式和Decorator模式。

## NIO包有哪些结构？分别起到的作用？

[NIO](https://github.com/GeniusVJR/LearningNotes/blob/master/Part2/JavaConcurrent/NIO.md)

## NIO针对什么情景会比IO有更好的优化？

## OKIO底层实现

# json

## JSON，fastjson 和 GSON的区别

1.json-lib

json-lib最开始的也是应用最广泛的json解析工具，json-lib 不好的地方确实是依赖于很多第三方包，包括commons-beanutils.jar，commons-collections-3.2.jar，commons-lang-2.6.jar，commons-logging-1.1.1.jar，ezmorph-1.0.6.jar，
对于复杂类型的转换，json-lib对于json转换成bean还有缺陷，比如一个类里面会出现另一个类的list或者map集合，json-lib从json到bean的转换就会出现问题。

json-lib在功能和性能上面都不能满足现在互联网化的需求。

2.开源的Jackson

相比json-lib框架，Jackson所依赖的jar包较少，简单易用并且性能也要相对高些。而且Jackson社区相对比较活跃，更新速度也比较快。

Jackson对于复杂类型的json转换bean会出现问题，一些集合Map，List的转换出现问题。Jackson对于复杂类型的bean转换Json，转换的json格式不是标准的Json格式

3.Google的Gson

Gson是目前功能最全的Json解析神器，Gson当初是为因应Google公司内部需求而由Google自行研发而来，但自从在2008年五月公开发布第一版后已被许多公司或用户应用。

Gson的应用主要为toJson与fromJson两个转换函数，无依赖，不需要例外额外的jar，能够直接跑在JDK上。

而在使用这种对象转换之前需先创建好对象的类型以及其成员才能成功的将JSON字符串成功转换成相对应的对象。类里面只要有get和set方法，Gson完全可以将复杂类型的json到bean或bean到json的转换，是JSON解析的神器。

Gson在功能上面无可挑剔，但是性能上面比FastJson有所差距。

4.阿里巴巴的FastJson

Fastjson是一个Java语言编写的高性能的JSON处理器,由阿里巴巴公司开发。无依赖，不需要例外额外的jar，能够直接跑在JDK上。

FastJson在复杂类型的Bean转换Json上会出现一些问题，可能会出现引用的类型，导致Json转换出错，需要制定引用。

FastJson采用独创的算法，将parse的速度提升到极致，超过所有json库。

# xml

## XML，解析XML的几种方式的原理与特点：DOM、SAX、PULL

参考：[http://www.cnblogs.com/HaroldTihan/p/4316397.html](http://www.cnblogs.com/HaroldTihan/p/4316397.html)

# JNI

参考：[http://landerlyoung.github.io/blog/2014/10/16/java-zhong-jnide-shi-yong/](http://landerlyoung.github.io/blog/2014/10/16/java-zhong-jnide-shi-yong/)

## MD5加密原理，可否解密。

# 排序算法

## 常见排序算法的时间复杂度

![](http://o9sn2y8lr.bkt.clouddn.com/16-10-19/70519978.jpg)

## Java 排序算法

参考：http://blog.csdn.net/qy1387/article/details/7752973















