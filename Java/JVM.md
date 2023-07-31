我实在不知道我到底要干什么了,不如先看着JVM吧,反正早晚都要看.

# JVM与Java体系结构

`write once, run anywhere`

Java虚拟机根本不关心运行在其内部的程序到底是使用何种编程语言编写的,它只关心"字节码"文件,也就是说Java虚拟机拥有语言无关性,并不会单纯地与Java语言终身绑定,只要其他编程语言的编译结果满足并包含Java虚拟机内部指令集,符号表以及其他的辅助信息,他就是一个有效的字节码文件,就能够被虚拟机所识别病状并装载运行.

## 什么是Java虚拟机
- Java虚拟机是一台执行Java字节码的虚拟计算机,它拥有独立的运行机制,其运行的Java字节码也未必由Java语言编译而成
- JVM平台的各种语言可以共享Java虚拟机带来的跨平台性,优秀的垃圾回收器,以及可靠的即时编译器
- Java技术的核心就是Java虚拟机(JVM,Java Virtual Machine),因为所有的Java程序都运行在Java虚拟机内部

>作用
Java虚拟机就是二进制字节码的运行环境,负责装在字节码到其内部,解释/编译为对应平台上的机器指令执行,每一条Java指令,Java虚拟机规范中都有详细定义,如怎么取操作数,怎么处理操作数,处理结果放在哪

>特点
- 一次编译,到处运行
- 自动内存管理
- 自动垃圾回收功能

---

JVM的位置:JVM是运行在操作系统之上的,它与硬件没有直接的交互
![第01章_JVM所处位置](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第01章_JVM所处位置.jpg)

## JVM的整体结构
- HotSpot VM是目前市面上高性能虚拟机的代表作之一
- 它采用解释器与即时编译器并存的架构
- 在今天,Java程序的运行性能早已脱胎换骨,已经达到了以C/C++程序一较高下的的地步

![第02章_JVM架构-简图](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第02章_JVM架构-简图.jpg)
*注意,这个图要会画...*

执行引擎(Execution Engine)的图解:
![第02章_JVM架构-中](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第02章_JVM架构-中.jpg)
英文版:
![第02章_JVM架构-英](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第02章_JVM架构-英.jpg)

---

>Java代码执行流程
![20230707102218](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230707102218.png)

## JVM的架构模型

Java编译器输入的指令流基本上是一种`基于栈的指令集架构`,另外一种指令集架构则是基于`寄存器的指令集架构`

具体来说,两种架构之间的区别:
- 基于栈式架构的特点
    - 设计和实现更简单,适用于资源受限的系统
    - 避开寄存器的分配难题,使用零零地址指令方式分配
    - 指令流中的指令大部分是零地址指令,其执行过程依赖于操作栈,指令集更小,编译器容易实现
    - 不需要硬件支持,可移植性更好,更好实现阔平台
- 基于寄存器架构的特点
    - 典型的应用是x86的二进制指令集,比如传统的PC以及Android的Davlik虚拟机
    - **指令集架构则完全依赖硬件,可移植性差**
    - **性能优秀和执行更高效**
    - 花费更少的指令去完成一项操作
    - 在大部分情况下,基于寄存器架构的指令集往往都以一地址指令,二地址指令和三地址指令为主,而基于栈式架构的指令集却是以零地址指令为主

---

总结:
由于跨平台性的设计,**Java的指令都是跟据栈来设计的**,不同平台CPU架构不同,所以不能设计为基于寄存器的,有点事跨平台,指令集小,编译器容器实现,缺点是性能下降,实现同样的功能需要更多的指令

时至今日,尽管嵌入式平台已经不是Java程序的主流运行平台了(准确来说应该是HotSpotVM的宿主环境已经不局限于嵌入式平台了),那么为什么不将架构更换为基于寄存器的架构呢

**简单来说,就是基于栈的架构,也够用了**

## JVM的生命周期

>虚拟机的启动

Java虚拟机的启动是通过引导类加载器(bootstrap class loader)创建一个初始类(initial class)来完成的,这个类是由虚拟机的具体实现指定的

>虚拟机的执行
- 一个运行中的Java虚拟机有着一个清晰的任务:执行Java程序
- 程序开始执行时他才运行,程序结束时他就停止
- 执行一个所谓的Java程序的时候,真真正正在执行的是一个叫做Java虚拟机的进程

>虚拟机的退出
有如下几种情况
- 程序正常执行结束
- 程序在执行过程中遇到了异常或者错误而异常终止
- 由于操作系统出现错误而导致Java虚拟机进行终止
- 某线程调用Runtime类或System类的exit方法,或Runtime类的halt方法,并且Java安全管理器也允许这次exit或halt操作
- 除此之外,JNI(Java Native Interface)规范描述了用JNI Invocation API 来加载或下载Java虚拟机时,Java虚拟机的退出情况

## JVM发展历程

Sun Classic VM
- sun公司发布,是世界上**第一款商用**Java虚拟机,至java1.4时完全被淘汰
- 只提供解释器
- 如果要使用JIT编译器,就需要进行外挂,且**解释器和编译器不能配合工作**
- 现在hotspot**内置了此虚拟机**

Exact VM
- jdk1.2时,sun提供了此虚拟机
- Exact Memory Management:准确式内存管理
    - 也可以叫Non-Conservative/Accurate Memory Management
    - 虚拟机可以知道内存中某个位置的数据具体是什么类型
- 具备现代高性能虚拟机的雏形
    - 热点探测
    - 编译器与解释器混合工作模式
- 只在Solaris平台短暂使用,其他平台上还是classic vm
    - 英雄气短,终被hotspot虚拟机替换

HotSpot VM
- 最初由一家名为"Longview Tecnologies"的小公司设计
- jdk1.3时,HotSpot VM成为默认虚拟机
- 目前HotSpot占有绝对的市场地位
    - 不管是现在仍在广泛使用的JDK6,还是JDK8,默认的虚拟机都是HotSpot
    - Sun/Oracle JDK 和 OpenJDK的默认虚拟机
    - 因此本课程默认介绍的虚拟机都是HotSpot,相关机制也主要是指HotSpot的GC机制
- 从服务器,桌面,到移动端,嵌入式都有应用
- 名称中的HotSpot指的就是它的热点代码探测技术
    - 通过计数器找到最具编译价值的代码,触发即时编译或栈上替换
    - 通过编译器与解释器协同工作,在最优的程序响应时间与最佳执行性能中取得平衡

BEA的JRockit
- **专注于服务器端应用**
    - 它可以不太关注程序启动速度,因此JRockit内部不包含解析器实现,u暗部代码都靠即时编译器编译后执行
- 大量的行业基准测试显示,JRockit JVM是世界上最快的JVM
    - 使用JRockit产品,客户已经体验到了显著的性能提高(一些超过了70%)和硬件成本的减少(达50%)
- 优势:全面的Java运行时解决方案组合
    - JRockit面向延迟敏感型应用的解决方案是JRockit Real Time提供以毫秒或微秒级的JVM响应时间,适合财务,军事指挥,电信网络的需要
    - MissionControl服务套件,它是一组以极低的开销来监控,管理和分析生产环境中的应用程序的工具
- 后Oracle整合二者,整合的方式是在HotSpot的基础上,移植JRockit的优秀特性
- 高斯林:目前就职于谷歌,研究人工智能和水下机器人

IBM的J9
- 市场定位和HotSpot接近,服务器端,桌面应用,嵌入式等多用途VM
- 广泛用于IBM的各种Java产品
- 有影响力的三大商用虚拟机之一,也号称是世界上最快的Java虚拟机
- 2017年,IBM发布了开源的J9 VM,命名为OpenJ9,交给Eclipse基金会管理,也称为Eclipse OpenJ9

**主要记HotSpot和JRockit以及J9就行了**

# 类加载子系统

## 内存结构概述

![第02章_JVM架构-简图](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第02章_JVM架构-简图.jpg)

- 类加载器子系统负责从文件系统或网络中加载class文件,class文件在文件开头有特定的文件标识
- ClassLoader只负责文件的加载,至于它是否可以运行,则由ExecutionEngine决定
- 加载的类信息存放于一块称为方法去的内存空间,除了类的信息外,方法区中还有存放运行时常量池信息,可能还包括字符串字面量和数字常量(这部分常量信息是Class文件中常量池部分的内存映射)

![20230708103023](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708103023.png)

>类加载器ClassLoader角色
1. class file存在于本地磁盘上,可以理解为设计师画在纸上的模板,而最终这个模板在执行的时候是要加载到JVM当中来根据这个文件实例化出n个一摸一样的实例
2. class flie加载到JVM中,被称为DNA元数据模板,放在方法区
3. 在.class文件->JVM->最终成为元数据模板,此过程就要一个运输工具(类装载器Class loader),扮演一个快递员的角色.

![20230708103708](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708103708.png)

---

类加载过程:
![第02章_类的加载过程](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第02章_类的加载过程.jpg)

还有一张细分的图:
![20230708104839](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708104839.png)

1. 加载
    1. 通过一个类的全限定名获取定义此类的二进制字节流
    2. 将这个字节流所代表的静态存储结构转换为方法区的运行时数据库
    3. 在内存中生成一个代表这个类的java.lang.Class对象,作为方法区这个类的各种数据的访问入口
2. 链接
    1. 验证(Verify)
        - 目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求,保证被加载类的正确性,不会危害虚拟机自身安全
        - 主要包括四种验证,文件格式验证,元数据验证,字节码验证,符号引用验证
    2. 准备(Prepare)
        - 为类变量分配内存并且设置该变量的默认初始值,即零值
        - 这里不包含用final修饰的static,因为final在编译的时候就会分配了,准备阶段会显式初始化
        - 这里不会为实例变量分配初始化,类变量会分配在方法区中,而实例变量是会随着对象一起分配到Java堆中
    3. 解析
        - 将常量池内的符号引用转换为直接引用的过程
        - 事实上,解析操作往往会伴随着JVM在执行完初始化之后执行
        - 符号应用就是一组符号来描述所引用的目标,符号应用的字面量形式明确定义在Java虚拟机规范中的Class文件格式中,直接引用就是直接指向目标的指针,相对偏移量或一个间接定位到目标的句柄
        - 解析动作主要针对类或接口,字段,类方法,接口方法,方法类型等,对应常量池中的CONSTANT_Class_info,ConSTANT_Fieldref_info,CONSTANT_Methodref_info等
3. 初始化
    - 初始化阶段就是执行类构造器方法`<clinit>()`的过程
    - ![20230708163645](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708163645.png)
    - 此方法不需要定义,是Javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来
    - ![20230708163915](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708163915.png)
    - 构造器方法中指令按语句在源文件中出现的顺序执行
    - ![20230708164257](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708164257.png)
    - `<clinit>()`不同于类的构造器(关联:构造器是虚拟机视角下的`<init>()`)
    - 若该类具有父类,JVM会保证子类的`<clinit>()`执行前,父类的`<clinit>()`已经执行完毕
    - 虚拟机必须保证一个类的`<clinit>()`方法在多线程下被同步加锁

如果没有静态变量,或者静态代码块中的变量赋值,是不会有`<clinit>`方法的:
![20230708164903](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708164903.png)

任何一个类声明以后，内部至少存在一个类的构造器,对应的就是`<init>()`方法

## 类加载器分类

- JVM支持两种类型的类加载器,分别为**引导类加载器(bootstrap ClassLoader)**和**自定义类加载器(User-Defined ClassLoader)**
- 从概念上来讲,自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器,但是Java虚拟机规范却没有这么定义,而是将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器
- 无论类加载器的类型如何划分,在程序中我们最常见的类加载器**始终只有3个**

![20230708172205](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230708172205.png)

**这里的四者之间的关系是包含关系,不是上层下层,也不是子父类继承关系**

```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-08 17:25
 */
public class ClassLoaderTest {
    public static void main(String[] args) {
        // 获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader); // sun.misc.Launcher$AppClassLoader@dad5dc

        // 获取其上层： 扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader); // sun.misc.Launcher$ExtClassLoader@16d3586

        // 获取其上层-- 获取不到引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader); // null

        // 对于用户自定义类来说-- 默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader);// sun.misc.Launcher$AppClassLoader@dad5dc

        // String类是使用引导类加载器进行加载的--系统的核心类库都使用引导类进行加载的
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1); // null

    }
}
```

---

虚拟机自带的加载器
1. 启动类加载器(引导类加载器,bootstrap ClassLoader)
    - 这个类加载使用C/C++语言实现的,嵌套在JVM内部
    - 它用来加载Java的核心库(JAVA_HOME/jre/lib/rt.jar,resources.jar或sun.boot.class.path路径下的内容),用于提供JVM自身需要的类
    - 并不继承自java.lang.ClassLoader,没有父加载器
    - 加载扩展类和应用程序类加载器,并指定为他们的父类加载器
    - 出于安全考虑,Bootstrap启动类加载器只加载包名为java,javax,sun等开头的类
2. 扩展类加载器(Extenion classLoader)
    - Java语言编写,由sun.misc.Launcher$ExtClassLoader实现
    - 派生于ClassLoader类
    - 父类加载器为启动类加载器
    - 从java.ext.dirs系统属性所指定的目录中加载类库,或从JDK的安装目录的jre/lib/ext子目录(扩展目录)下加载类库,如果用户创建的jar放在此目录下,也会自动由扩展类加载器加载
3. 应用程序类加载器(系统类加载器-AppClassLoader)
    - java语言编写,由sun.misc.Launcher$AppClassLoader实现
    - 派生于ClassLoader类
    - 父类加载器为扩展类加载器
    - 它负责加载环境变量classpath或系统属性java.class.path指定路径下的类库
    - 该类加载是程序中默认的类加载器,一般来说,java应用的类都是由它来完成加载
    - 通过`ClassLoader#getSystemClassLoader()`方法可以获取到该类加载器

```java
package com.zzmr.java;

import com.sun.javafx.PlatformUtil;
import com.sun.net.ssl.internal.ssl.Provider;
import sun.misc.Launcher;

import java.net.URL;

/**
 * @author zzmr
 * @create 2023-07-09 11:00
 */
public class ClassLoaderTest1 {
    public static void main(String[] args) {
        System.out.println("********启动类加载器**********");
        // 获取BootstrapClassLoader能够加载的api路径
        URL[] urLs = Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
        // 从上面的路径中随意选择一个类，来看看它的类加载器是什么
        ClassLoader classLoader = Provider.class.getClassLoader();
        System.out.println(classLoader);


        System.out.println("********扩展类加载器**********");
        String extDirs = System.getProperty("java.ext.dirs");
        for (String path : extDirs.split(";")) {
            System.out.println(path);
        }
        // 从上面的路径中随意选择一个类，来看看它的类加载器是什么
        ClassLoader classLoader1 = PlatformUtil.class.getClassLoader();
        System.out.println(classLoader1);
    }
}

```

---

用户自定义类加载器

在Java的日常应用程序开发中,类的加载几乎是由上述3种类加载器相互配合执行的,在必要时,我们还可以自定义类加载器,来定制类的加载方式

为什么要自定义类加载器
- 隔离加载类
- 修改类加载的方式
- 扩展加载源
- 防止源码泄露

---

>关于ClassLoader
ClassLoader类,它是一个抽象类,其后所有的类加载器都继承自ClassLoader(不包括启动了加载器)
![20230709112455](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230709112455.png)

获取类的加载器
```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-09 11:53
 */
public class ClassLoaderTest2 {
    public static void main(String[] args) {
        try {
            // 1.
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader);

            // 2.
            ClassLoader classLoader1 = Thread.currentThread().getContextClassLoader();
            System.out.println(classLoader1);

            // 3.
            ClassLoader classLoader2 = ClassLoader.getSystemClassLoader().getParent();
            System.out.println(classLoader2);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```

## 双亲委派机制

Java虚拟机对class文件采用的是**按需加载**的方式，也就是说当需要使用该类才会将它的class文件加载到内存中生成class对象，而且加载某个类的class文件时，Java虚拟机采用的是**双亲委派机制**，即把请求交给父类处理，它是一种任务委派模式

>工作原理
1. 如果一个类加载器收到了类加载请求,它并不会自己先去加载,而是把这个请求委托给父类的加载器去执行
2. 如果父类加载器还存在其他父类加载器,则进一步向上委托,以此递归,请求最终将到达顶层的启动类加载器
3. 如果父类加载器可以完成类加载任务,就成功返回,倘若父类加载器无法完成此加载任务,子加载器才会尝试自己去加载,这就是双亲委派模式
![20230709120828](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230709120828.png)

此时可以做一个测试
一个测试类:
```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-09 12:00
 */
public class StringTest {
    public static void main(String[] args) {
        String str = new String();
        System.out.println("hello,zzmr.club");
    }
}

```
这里创建一个String对象
再创建一个包:
```java
package java.lang;

/**
 * @author zzmr
 * @create 2023-07-09 12:03
 */
public class String {
    static {
        System.out.println("我是自定义的String类的静态代码块");
    }
}

```

此时StringTest并不会使用我们自己定义的String类,而是使用的原始的String
以为String这个类层层往上,会被启动类加载器加载,所以就会使用原始的String

如果这么写:
```java
package java.lang;

/**
 * @author zzmr
 * @create 2023-07-09 12:03
 */
public class String {
    static {
        System.out.println("我是自定义的String类的静态代码块");
    }

    public static void main(String[] args) {
        System.out.println("he");
    }
}

```
会报错:
![20230709121830](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230709121830.png)

双亲委派机制举例图解:
![20230709122003](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230709122003.png)

---

双亲委派机制的优势
1. 避免类的重复加载
2. 保护程序安全,防止核心API被随意篡改

---

自定义String类,但是在加载自定义String类的时候会率先使用引导类加载器加载,而引导类加载器在加载的过程中会先加载jdk自带的文件`(rt.jar包中java\lang\String.class)`,报错信息说没有mian方法,就是因为加载的是rt.jar包中的String类,这样可以保证对java核心源代码的保护,这就是**沙箱安全机制**

## 其他

- 在JVM中表示两个class对象是否为同一个类存在两个必要条件:
    - 类的完整类名必须一致,包括包名
    - 加载这个类的ClassLoader(指ClassLoader实例对象)必须相同
- 换句话说,在JVM中,即使这两个类对象(class对象)来源同一个Class文件,被同一个虚拟机所加载,但只要加载它们的ClassLoader实例对象不同,那么这两个类对象也是不相等的

>对类加载器的引用
JVM必须知道一个类是由启动加载器加载的还是由用户类加载器加载的,如果一个类型是由用户类加载器加载的,那么JVM会将这个类加载器的一个引用作为类型信息的一部分保存在方法区中,当解析一个类型到另一个类型的引用的时候,JVM需要保证这两个类型的类加载器是相同的

---

Java程序对类的使用方式分为:主动使用和被动使用
- 主动使用,又分为七种情况
    - 创建类的实例
    - 访问某个类或接口的静态变量,或者对该静态变量赋值
    - 调用类的静态方法
    - 反射比如`Class.forName("com.zzmr.Test")`
    - 初始化一个类的子类
    - Java虚拟机启动时被标明为启动类的类
    - JDK7开始提供的动态语言支持:*java.lang.invoke.MethodHandle实例的解析结果,REF_getStatic,REF_putStatc,REF_invokeStatic句柄对应的类没有初始化,则初始化*
- 除了以上七种情况,其他使用Java类的方式都被看做是对**类的被动使用,都不会导致类的初始化**

# 运行时数据区概述及线程

运行时数据区简图:
![20230709180551](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230709180551.png)

内存是非常重要的系统资源,是硬盘和CPU的中间仓库及桥梁,承载着操作系统和应用程序的实时运行,JVM内存布局规定了Java在运行过程中内存申请,分配,管理的策略,保证了JVM的高效稳定运行,**不同的JVM对于内存的划分方式和管理机制存在着部分差异**,结合JVM虚拟机规范,来探讨一下经典的JVM内存布局

Java虚拟机定义了若干种程序运行期间会使用到的运行时数据区,其中有一些会随着虚拟机启动而创建,随着虚拟机退出而销毁,另外一些则是与线程一一对应的,这些与线程对应的数据区域会随着线程开始和结束而创建和销毁

灰色的为单独线程私有的,红色的为多个线程共享的,即:
- 每个线程:独立包括程序计数器,栈,本地栈
- 线程间共享:堆,堆外内存(永久代或元空间,代码缓存)

---

**Runtime**实例-每一个JVM(应用程序)对应一个Runtime实例

## 线程

- 线程是一个程序里的运行单元,JVM允许一个应用有多个线程并行的执行
- 在Hotspot JVM里,每个线程都与操作系统的本地线程直接映射
    - 当一个Java线程准备好执行以后,此时一个操作系统的本地线程也同时创建,Java线程执行终止后,本地线程也会回收
- 操作系统负责所有线程的安排调度到任何一个可用的CPU上,一旦本地线程初始化成功,它就会调用Java线程中的run()方法
- 如果你使用jconsole或者任何一个调试工具,都能看到在后台有许多线程在运行,这些后台线程不包括调用`public static void main(String[])`的main线程以及所有这个线程自己创建的线程
- 这些主要的后台系统线程在Hotspot JVM里主要是以下几个:
![20230711185443](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711185443.png)

## 程序计数器(PC寄存器)

![20230711190245](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711190245.png)

JVM中的程序计数寄存器(Program Counter Register)中,Register的命令源于CPU的寄存器,寄存器存储指令相关的现场信息,CPU只有把数据装载到寄存器才能够运行

这里,并非是广义上所指的物理寄存器,或许将其翻译为PC计数器(或者指令计数器)会更加贴切(也称为程序钩子),并且也不容易引起一些不必要的误会,**JVM中的PC寄存器是对物理PC寄存器的一种抽象模拟**

作用:PC寄存器用来存储指向下一条指令的地址,也即将要执行的指令代码,由执行引擎读取下一条指令
1. 它是一块很小的内存空间,几乎可以忽略不记,也是运行速度最快的存储区域
2. 在JVM规范中,**每个线程都有它自己的程序计数器**,是线程私有的,生命周期与线程的生命周期保持一致
3. **任何时间一个线程都只有一个方法在执行,**也就是所谓的**当前方法**,程序计数器会存储当前线程正在执行的Java方法的JVM指令地址;或者,如果是在执行native方法,则是未指定值(undefined)
4. 它是程序控制流的指示器,分支,循环,跳转,异常处理,线程恢复等基础功能都需要依赖这个计数器来完成
5. 字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令
6. 它是唯一一个Java虚拟机规范中没有规定任何`OutOtMemoryError`情况的区域

### PC寄存器的使用举例

![20230711193122](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711193122.png)
- 最左边的数值是**指令地址(偏移地址)**
- 第二列`bipush,iadd什么的`是**操作指令**

对于`5`来说,PC寄存器存储的就是这个`5`,执行引擎会取PC寄存器中存储的指令地址对应的操作指令(操作局部变量表,操作数栈),并翻译为机器指令,CPU再执行该指令

看下图:
![20230711193839](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711193839.png)

### 两个常见问题

>使用PC寄存器存储字节码指令地址有什么用呢?

因为CPU需要不停的切换各个线程,这时候切换回来以后,就得知道接着从哪开始继续执行

>为什么使用PC寄存器记录当前线程的执行地址呢?

JVM的字节码解释器就需要通过改变PC寄存器的值来明确吓一跳应该执行什么样的字节码指令

>PC寄存器为什么会被设定为线程私有?
我们都知道所谓的多线程在一个特定的时间段内只会执行其中某一个线程的方法,CPU会不停地做任务切换,这样必然导致经常中断或恢复,如何保证分号无差呢,**为了能够准确地记录各个线程正在执行的当前字节码指令地址,最好的办法自然是每一个线程都分配一个PC寄存器**,这样一来各个线程之间便可以进行独立计算,从而不会出现相互干扰的情况

由于CPU时间片轮转限制,众多线程在并发执行过程中,任何一个确定的时刻,一个处理器或者多核处理器中的一个内核,只会执行某个线程中的一条指令

这样必然导致经常中断或恢复,如何保证分号无差呢?每个线程在创建后,都会产生自己的程序计数器和栈帧,程序计数器在各个线程之间互不影响
![20230711195425](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711195425.png)

---

CPU时间片
CPU时间片即CPU分配给各个程序的时间,每个线程被分配一个时间段,称作它的时间片
在宏观上:我们可以同时打开多个应用程序,每个程序并行不悖,同时运行
但在微观上:由于只有一个CPU,一次只能处理程序要求的一部分,如何处理公平,一种方法就是引入时间片,每个程序轮流执行
![20230711195707](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230711195707.png)

## 虚拟机栈

### 虚拟机栈概述

出现的背景:
由于跨平台性的设计,Java的指令都是跟据栈来设计的,不同平台CPU架构不同,寄存器结构也就不同,所以不能设计为基于寄存器的

**优点是跨平台,指令集小,编译器容易实现,缺点是性能下降,实现同样的功能需要更多的指令**

---

>有不少Java开发人员一提到Java内存结构,就会非常粗粒度地将JVM中的内存区理解为仅有Java堆(heap)和Java栈(stack)为什么

**栈是运行时的单位,而堆是存储的单位**

即:栈解决程序的运行问题,即程序如何执行,或者说如何处理数据,堆解决的是数据存储问题,即数据怎么放,放在哪

---

- Java虚拟机栈是什么?
    - Java虚拟机栈(Java Virtual Machine Stack),早期也叫Java栈,每个线程在创建时都会创建一个虚拟机栈,其内部保存一个个的栈帧(Stack Frame),对应着一次次的Java方法调用(是线程私有的)
- 生命周期
    - 生命周期和线程一致
- 作用
    - 主管Java程序的运行,它保存方法的局部变量(8种基本数据类型,对象的引用地址),部分结果,并参与方法的调用和返回
    - 局部变量 vs 成员变量(或属性)
    - 基本数据变量 vs 引用类型变量(类,数组,接口)

栈的特点(优点)
- 栈是一种快速有效的分配存储方式,访问速度仅次于程序计数器
- JVM直接对Java栈的操作只有两个
    - 每个方法执行,伴随着进栈(入栈,压栈)
    - 执行结束后的出栈工作
- 对于栈来说不存在垃圾回收问题

![20230712220103](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230712220103.png)


---

面试题:开发中遇到的异常有哪些

栈中可能出现的异常
Java虚拟机规范允许**Java栈的大小是动态的或者是固定不变的**
- 如果采用固定大小的Java虚拟机栈,那每一个线程的Java虚拟机栈容量可以在创建的时候独立选定,如果线程请求分配的栈容量超过Java虚拟机栈允许的最大容量,Java虚拟机会抛出一个`StackOverflowError`异常
- 如果Java虚拟机栈可以动态扩展,并且在尝试扩展的时候无法申请到足够的内存,或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈,那Java虚拟机会抛出一个`OutOtMemoryError`异常


```java
package com.zzmr.java;

/**
 * 演示栈中的异常
 *
 * @author zzmr
 * @create 2023-07-12 22:21
 */
public class StackErrorTest {
    public static void main(String[] args) {
        main(args);
    }
}
```
报错:![20230712222201](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230712222201.png)

---

### 设置栈内存大小

我们可以使用参数-Xss选项来设置线程的最大栈空间,栈的大小直接决定了函数调用的最大可达深度
![20230712222936](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230712222936.png)
```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-12 22:23
 * 默认情况，count=6295
 * 设置栈的大小为 -Xss256k
 * 变成了4950
 */
public class StackDeepTest {
    private static int count = 1;

    public static void main(String[] args) {
        System.out.println(count);
        count++;
        main(args);
    }
}
```

### 栈的存储单位

栈中存储什么?
1. 每个线程都有自己的栈,栈的数据都是以`栈帧(Stack Frame)`的格式存在
2. 在这个线程上正在执行的每个方法都各自对应一个栈帧
3. 栈帧是一个内存区快,是一个数据集,维系着方法执行过程中的各种数据信息
4. JVM直接对Java栈的操作只有两个,就是对栈帧的**压栈**和**出栈**,遵循先进后出的原则
5. **在一条活动线程中,一个时间点上,只会有一个活动的栈帧**,即只有当前正在执行的方法的栈帧(栈顶栈帧)是有效的,这个栈帧(Current Frame),与当前帧帧相对应的方法就是当前方法(Current Method),定义这个方法的类就是当前类(Current Class)
6. 执行引擎运行的所有字节码指令只针对当前帧帧进行操作
7. 如果在该方法中调用了其他方法,对应新的栈帧就会被创建出来,放在栈的顶端,成为新的当前帧

```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-21 12:05
 */
public class StackFrameTest {
    public static void main(String[] args) {
        StackFrameTest test = new StackFrameTest();
        test.method1();
    }

    public void method1() {
        System.out.println("method1()开始执行...");
        method2();
        System.out.println("method1()执行结束");

    }

    private int method2() {
        System.out.println("method2()开始执行...");
        int i = 10;
        int m = (int) method3();
        System.out.println("method2()即将结束");
        return i + m;
    }

    private double method3() {
        System.out.println("method3()开始执行...");
        double j = 20.0;
        System.out.println("method3()即将结束");
        return j;
    }
}
```

![20230721121322](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230721121322.png)
就是一个嵌套的感觉

![第05章_方法与栈桢](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第05章_方法与栈桢.jpg)

---

>栈运行原理
- 不同线程中所包含的栈帧是不允许存在相互引用的,即不可能在一个栈帧之中应用另外一个线程的栈帧
- 如果当前方法调用了其他方法,方法返回之际,当前栈帧会传回此方法的执行结果给前一个栈帧,接着,虚拟机会丢弃当前栈帧,使得前一个栈帧重新成为当前栈帧
- Java方法有两种返回函数的方式,一种是正常的函数返回,使用return指令,另外一种是抛出异常,不管使用哪种方式,都会导致栈帧被弹出

### 栈帧的内部结构

每个栈帧中存储着:
- 局部变量表(Local Variables)
- 操作数栈(Operand Stack)或表达式栈
- 动态链接(Dynamic Linking)或指向运行时常量池的方法引用
- 方法返回地址(Return Address)或方法正常退出或异常退出的定义
- 一些附加信息
![第05章_栈桢内部结构](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/第05章_栈桢内部结构.jpg)

#### 局部变量表

- 局部变量表也被称之为局部变量数组或本地变量表
- **定义为一个数字数组,主要用于存储方法参数和定义在方法体内的局部变量**,这些数据类型包括各类基本数据类型,对象引用(reference)以及returnAddress类型
- 由于局部变量是建立在线程的栈上,是线程的私有数据,**因此不存在数据安全问题**
- **局部变量表所需的容量大小是在编译期确定下来的**,并保存在方法的Code属性的`maximum local variables`数据项中,在方法运行期间是不会改变局部变量表的大小的
- **方法嵌套调用的次数由栈的大小决定,一般来说,栈越大,方法嵌套调用次数越多**,对一个函数而言,它的参数和局部变量越多,使得局部变量表膨胀,它的栈帧就越大,以满足方法调用所需传递的信息增大的需求,进而函数调用就会占用更多的栈空间,导致其嵌套调用次数就会减少
- **局部变量表中的变量只在当前方法调用中有效**,在方法执行时,虚拟机通过使用局部变量表完成参数值到参数列表的传递过程,当方法调用结束后,**随着方法栈帧的销毁,局部变量表也会随之销毁**

main方法:
![20230721132426](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230721132426.png)

#### 关于Slot的理解

- 参数值的存放总是在局部变量数组的index0开始的,到数组长度-1的索引结束
- 局部变量表,最基本的存储单元是`slot(变量槽)`
- 局部变量表中存放编译期可知的各种基本数据类型(8种),引用类型(reference),returnAddress类型的变量
- 在局部变量表中,32位以内的类型只占用一个slot(包括returnAddress类型),64位的类型(long和double)占用两个slot
    - byte,short,char在存储前被转换为int,boolean也被转换成int,0表示false,非零表示true
    - long和double则占据两个slot
![20230722162102](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722162102.png)

```txt
这里刚好能理解为什么static方法中不能使用this了
因为this变量不存在于当前静态方法的局部变量表中,所以引用不到
```

---

比如下面这个非静态方法:
```java
    public void test1() {
        Date date = new Date();
        String name1 = "atguigu.com";
        String info = test2(date, name1);
        System.out.println(date + name1);
    }
```

它的局部变量表中就有4个变量,也就是说存在`this`,且放在首位
![20230722162657](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722162657.png)

test2中,由于weight是double类型的,会占用两个slot,weight的开始索引是3,所以下一个gender的开始索引就是5
![20230722163137](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722163137.png)

>**栈帧中的局部变量表中的槽位是可以重用的**,如果一个局部变量过了其作用域,那么在其作用域之后申明的新的局部变量就很有可能会复用过期局部变量的槽位,从而**达到节省资源额目的**
```java
    public void test4() {
        int a = 0;
        {
            int b = 0;
            b = a + 1;
        }
        // 变量c使用之前已经销毁的变量b占据的slot的位置
        int c = a + 1;
    }
```

b的作用域只在代码块中,出了代码块,b就被销毁了,此时b的空间还在,所以c就直接使用的b的空间:
![20230722164207](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722164207.png)

---

静态变量与局部变量的对比
- 参数表分配完毕之后,再根据方法体内定义的变量的顺序和作用域分配
- 我们知道类变量表有两次初始化的机会,第一次是在准备阶段,执行系统初始化,对类变量设置零值,另一次则是在初始化阶段,赋予程序员在代码中定义的初始值
- 和类变量初始化不同的是,局部变量表不存在系统初始化的过程,这意味着一旦定义了局部变量则必须认为的初始化,否则无法使用
```txt
变量的分类：按照数据类型分：① 基本数据类型  ② 引用数据类型
                按照在类中声明的位置分：
① 成员变量：在使用前，都经历过默认初始化赋值
                                类变量： linking的prepare阶段：给类变量默认赋值  ---> initial阶段：给类变量显式赋值即静态代码块赋值
                                实例变量：随着对象的创建，会在堆空间中分配实例变量空间，并进行默认赋值
② 局部变量：在使用前，必须要进行显式赋值的！否则，编译不通过
```

---

补充:

- 在栈帧中,与性能调优琯溪最为密切的部分就是前面提到的局部变量表,在方法执行时,虚拟机使用局部变量表完成方法的传递
- 局部变量表中的变量也是重要的垃圾回收根节点,只要被局部变量表中直接或间接引用的对象都不会被回收

#### 操作数栈

- 每一个独立的栈帧中除了包含局部变量表以外,还包含一个**后进先出**的操作数栈,也可以称之**表达式栈**
- **操作数栈,在方法执行过程中,根据字节码指令,往栈中写入数据或提取数据,即入栈/出栈**
    - 某些字节码指令将值压入操作数栈,其余的字节码指令将操作数去除栈,使用它们后再把结果压入栈
    - 比如:执行复制,交换,求和等操作
- ![20230722170500](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722170500.png)
- 操作数栈,**主要用于保存计算过程的中间结果,同时作为计算过程中变量临时的存储空间**
- 操作数栈就是JVM执行引擎的一个工作区,当一个方法刚开始执行的时候,一个新的栈帧也会随之被创建出来,**这个方法的操作数栈是空的**
- 每一个操作数栈都会拥有一个明确的栈深度用于存储数值,其所需的最大深度在编译器就定好了,保存在方法的Code属性中,为max_stack的值
- 栈中的任何一个元素都是可以任意的Java数据类型
    - 32bit的类型占用一个栈单位深度
    - 64bit的类型占用两个栈单位深度
- 操作数栈**并且采用访问索引的方式来进行数据访问的**,而是只能通过标准的入栈,出栈操作来完成一次数据访问

---

代码如下:
```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-22 17:23
 */
public class OperandStackTest {
    public void testAddOperation() {
        byte i = 15;
        int j = 8;

        int k = i + j;
    }
}
```

得到的字节码指令:
![20230722173635](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722173635.png)

- byte,short,char,boolean,都以int型来保存,所以是`bipush`,然后将15放到操作数栈中
- 第二行,`istore_1`,表示将上一步放入操作数栈的数据放到局部变量表中,索引为1(因为此方法是非静态的,所以0索引是this变量)
- 下面的`bipush 8`和`istore_2`也同理
- `iload_1`和`iload_2`表示将数据从局部变量表中取出1,2,然后放到操作数栈中,入栈
- `iadd`就是出栈,然后将两数相加,最终将结果放到操作数栈的栈顶
- `istore_3`将结果存入局部变量表中,索引为3
![20230722174428](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722174428.png)
- 如果被调用的方法带有返回值的话,其返回值将会被压入当前栈帧的操作数栈中,并更新PC寄存器中下一条需要执行的字节码指令
- 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配,这由编译器在编译期间进行验证,同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证
- 另外,我们说Java虚拟机的**解释引擎是基于栈的执行引擎**,其中的栈指的就是操作数栈

```java

    public int getSum() {
        int m = 10;
        int n = 20;
        int k = m + n;
        return k;
    }

    public void testGetSum() {
        // 获取上一个栈帧返回的结果，并保存在操作数栈中
        int i = getSum();
        int j = 10;
    }
```
执行了`aload_0`
![20230722181644](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722181644.png)


---

```java
    /**
     * 程序面试题:
     * i++和++i的区别:放到字节码篇章再介绍
     */
    public void add() {
        // 第一类问题
        int i1 = 10;
        i1++;

        int i2 = 10;
        ++i2;

        // 第二类问题
        int i3 = 10;
        int i4 = i3++;

        int i5 = 10;
        int i6 = ++i5;

        // 第三类问题
        int i7 = 10;
        i7 = i7++;

        int i8 = 10;
        i8 = ++i8;

        // 第四类问题
        int i9 = 10;
        int i10 = i9++ + ++i9;
    }
```

这么恶心的题吗哈哈哈操
![20230722182213](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230722182213.png)

第一个问题,可以根据字节码指令来看,会发现字节码指令是完全一样的,这也就说明,第一种是一样的情况
后面几个问题后续再讲

>栈顶缓存(Top-of-Stack Cashing)技术

前面提到,基于栈式架构的虚拟机所使用的零地址指令更加紧凑,但完成一项操作的时候必然需要使用**更多的入栈和出栈指令**,这同时也就意味着将需要更多的指令分派次数和内存读/写次数

由于操作数是存储在内存中的,因此频繁地执行内存读/写操作必然会影响执行速度,为了解决这个问题,就提出了栈顶缓存技术:**将栈顶元素全部缓存在屋里CPU的寄存器中,以此降低对内存的读/写次数,提升执行引擎的执行效率**

*寄存器:指令更少,执行速度更快*

#### 动态链接

方法返回地址,动态链接,一些附加信息三者被称为**帧数据区**

- 每一个栈帧内部都包含一个指向*运行时常量池***中该栈帧所属方法的引用**,包含这个引用的目的就是为了支持当前方法的代码能够实现动态链接(Dynamic Linking),比如:invokedynamice指令
- 在Java源文件被编译到字节码文件中时,所有的变量和方法引用都作为符号引用(Symbolic reference)保存在class文件的常量池里,比如:描述一个方法调用了另外的其他方法时,就是通过常量池中指向方法的符号用来表示的,那么**动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用**

#### **方法的调用**

在JVM中,**将符号引用转换为调用方法的直接引用**与方法的绑定机制相关

- 静态链接:当一个字节码文件被装载进JVM内部时,如果**被调用的目标方法在编译期可知且运行期保持不变时**,这种情况下调用方法的符号引用转换为直接引用的过程称之为静态链接
- 动态链接:如果被调用的方法在编译器无法被确定下来,也就是说,只能够在程序运行期将调用方法的符号引用转换为直接引用,由于这种引用转换过程具备动态性,因此也就称为动态链接

对应的方法的绑定机制为:早期绑定和晚期绑定,**绑定是一个字段,方法或者类在符号引用被替换为直接引用的过程,这仅仅发生一次**

- 早期绑定:早期绑定就是指被调用的**目标方法如果在编译期可知,且运行期保持不变时**,即可将这个方法与所属的类型进行绑定,这样一来,由于明确了被调用的目标方法究竟是哪一个,因此也就可以使用静态链接的方式将符号引用转换为直接引用
- 晚期绑定:如果**被调用的方法在编译期无法被确定下来,只能够在程序运行期间根据实际的类型绑定相关的方法**,这种绑定方式被称为晚期绑定

---

**虚方法与非虚方法**
- 如果方法在编译期就确定了具体的调用版本,这个版本在运行时是不可变的,这样的方法称为**非虚方法**
- 静态方法,私有方法,final方法,实例构造器,父类方法都是非虚方法
- 其他方法称为虚方法

虚拟机中提供了以下几条方法调用指令:
1. 普通调用指令:
    - **invokestatic:调用静态方法,解析阶段确定唯一方法版本**
    - **invokespeial:调用`<init>`方法,私有及父类方法,解析阶段确定唯一方法版本**
    - invokevirtual:调用所有虚方法
    - invokeinterface:调用接口方法
2. 动态调用指令
    - invokedynamic:动态解析出所需要调用的方法,然后执行

前四条指令固化在虚拟机内部,方法的调用执行不可人为干预,而invokedynamic指令则支持由用户确定方法版本,其中invokestatic指令和invokespecial指令调用的方法称为非虚方法,其余(final修饰除外)称为虚方法

---

**方法重写的本质**
1. 找到操作数栈的第一个元素所执行的对象的实际类型,记作C
2. 如果在类型C中找到与常量中的描述符合简单名称都相符的方法,则进行访问权限校验,如果通过则返回这个方法的直接引用,查找过程结束;如果不通过,则返回`Java.lang.IllegalAccessError`异常
3. 否则,按照继承关系从下往上以此对C的各个父类进行第二步的搜索和验证过程
4. 如果始终没有找到合适的方法,则抛出`java.lang.AbstactMethodError`异常

`Java.lang.IllegalAccessError`介绍
程序试图访问或修改一个属性或调用一个方法,这个属性或方法,你没有权限访问,一般的,这个会引起编译器异常,这个错误如果发生在运行时,就说明一个类发生了不兼容的改变

---

**虚方法表**
- 在面向对象的编程中,会很频繁的使用到动态分派,如果在每次动态分派的过程中都要重新在类的方法元数据中搜索合适的目标就可能影响到执行效率,因此,为了提高性能,JVM采用在类的方法区建立一个虚方法表(非虚方法不会出现在此表中)来实现,使用索引表来代替查找
- **每个类中都有一个虚方法表**,表中存放着各个方法的实际入口
- 虚方法表在类加载的链接阶段被创建并开始初始化,类的变量初始值准备完成之后,JVM会把该类的方法表也初始化完成

#### 方法返回地址

- 存放调用该方法的PC寄存器的值
- 一个方法的结束,有两种方式:
    - 正常执行完成
    - 出现未处理的异常,非正常退出
- 无论通过哪种方式退出,在方法退出后都返回到该方法被调用的位置,方法正常退出时,**调用者的PC计数器的值作为返回地址,即调用该方法的指令的下一条指令的地址**,而通过异常退出的,返回地址是通过异常表来确定的,栈帧中一般不会保存这部分的信息


本质上,方法的退出就是当前栈帧出栈的过程,此时,需要恢复上层方法的局部变量表,操作数栈,将返回值压入调用者栈帧的操作数栈,设置PC寄存器值等,让调用者方法继续执行下去

**正常完成出口和异常完成出口的区别在于:通过异常完成出口退出的不会给他的上层调用者产生任何的返回值**

---

当一个方法开始执行后,只有两种方式可以退出这个方法
1. 执行引擎遇到任意一个方法返回的字节码指令(return),会有返回值传递给上层的方法调用者,简称正常完出口
    - 一个方法在正常调用完成之后究竟需要使用哪一个返回指令还需要根据方法返回值的实际数据类型而定
    - 在字节码指令中,返回指令包含`ireturn(当返回值是boolean,byte,char,short和int类型时使用),lreturn,freturn,dreturn,以及areturn`,另外还有一个return指令供声明为void的方法,实例初始化方法,类和接口的初始化方法使用
2. 在方法执行的过程中遇到了异常,并且这个异常没有在方法内进行处理,也就是说只要在本方法的异常表中没有搜索到匹配的异常处理,就会导致方法退出,简称异常完成出口
    - 方法执行过程中抛出异常时的异常处理,存储在一个异常处理表,方便在发生异常的时候找到处理异常的代码
    - ![20230725111939](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230725111939.png)

*各种返回类型对应的情况不同:*
![20230725111330](https://gcore.jsdelivr.net/gh/jimmy66886/picgo_two@main/img/20230725111330.png)

```java
package com.zzmr.java;

import java.io.FileReader;
import java.io.IOException;
import java.util.Date;

/**
 *
 * 返回指令包含ireturn（当返回值是boolean、byte、char、short和int类型时使用）、
 * lreturn、freturn、dreturn以及areturn，另外还有一个return指令供声明为void的方法、
 * 实例初始化方法、类和接口的初始化方法使用。
 *
 * @author shkstart
 * @create 2020 下午 4:05
 */
public class ReturnAddressTest {
    public boolean methodBoolean() {
        return false;
    }

    public byte methodByte() {
        return 0;
    }

    public short methodShort() {
        return 0;
    }

    public char methodChar() {
        return 'a';
    }

    public int methodInt() {
        return 0;
    }

    public long methodLong() {
        return 0L;
    }

    public float methodFloat() {
        return 0.0f;
    }

    public double methodDouble() {
        return 0.0;
    }

    public String methodString() {
        return null;
    }

    public Date methodDate() {
        return null;
    }

    public void methodVoid() {

    }

    static {
        int i = 10;
    }


    //
    public void method2() {

        methodVoid();

        try {
            method1();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void method1() throws IOException {
        FileReader fis = new FileReader("atguigu.txt");
        char[] cBuffer = new char[1024];
        int len;
        while ((len = fis.read(cBuffer)) != -1) {
            String str = new String(cBuffer, 0, len);
            System.out.println(str);
        }
        fis.close();
    }


}
```

>一些附加信息
*在一些课中被省略的部分*
栈帧中还允许携带与Java虚拟机实现相关的一些附加信息,例如:对程序调试提供支持的信息

### 栈的相关面试题

1. 举例栈溢出的情况(StackOverflowError)?
    - 通过-Xss设置栈的大小;OOM(内存不足)
2. 调整栈的大小,就能保证部出现溢出吗?
    - 不能,调整大小,可以让StackOverflowError出现的较晚一些,但是还是可能存在溢出的
3. 分配的栈内存越大越好吗?
    - `治标不治本`
4. 垃圾回收是否会涉及到虚拟机栈?
    - 不会涉及到,`因为栈只有出栈和入栈的情况`
5. 方法中定义的局部变量是否线程安全?
    - 具体问题具体分析
    - 线程安全:如果只有一个线程才可以操作此数据,则必是线程安全的,如果有多个线程操作此数据,则此数据是共享数据,如果不考虑同步机制的话,会存在线程安全问题.

```java
package com.zzmr.java;

/**
 * 面试题：
 * 方法中定义的局部变量是否线程安全？具体情况具体分析
 *
 *   何为线程安全？
 *      如果只有一个线程才可以操作此数据，则必是线程安全的。
 *      如果有多个线程操作此数据，则此数据是共享数据。如果不考虑同步机制的话，会存在线程安全问题。
 * @author shkstart
 * @create 2020 下午 7:48
 */
public class StringBuilderTest {

    int num = 10;

    //s1的声明方式是线程安全的
    public static void method1(){
        //StringBuilder:线程不安全
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        //...
    }
    //sBuilder的操作过程：是线程不安全的
    public static void method2(StringBuilder sBuilder){
        sBuilder.append("a");
        sBuilder.append("b");
        //...
    }
    //s1的操作：是线程不安全的
    public static StringBuilder method3(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1;
    }
    //s1的操作：是线程安全的
    public static String method4(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1.toString();
    }

    public static void main(String[] args) {
        StringBuilder s = new StringBuilder();


        new Thread(() -> {
            s.append("a");
            s.append("b");
        }).start();

        method2(s);

    }

}
```

## 本地方法接口

*先讲这个,然后再回到运行时数据区看本地方法栈*

>什么是本地方法?
**简单地讲,一个Native method就是一个Java调用非Java代码的接口**,一个Native Method是这样一个Java方法:该方法的实现由非Java语言实现,比如C,这个特征并非Java所特有,很多其他的编程语言都有这一机制

在定义一个native method时,并不提供实现体,因为其实现体是由非Java语言在外面实现的

本地接口的作用是融合不同的编程语言为Java所用,它的初衷是融合C/C++程序

```java
package com.zzmr.java;

/**
 * @author zzmr
 * @create 2023-07-26 20:37
 */
public class IHaveNatives {

    public native void Native1(int x);

    native static public long Native2();

    native synchronized private float Native3();

    native void Native4(int[] ary) throws Exception;

}
```

**native和abstract**不能共用

---

>为什么要使用Native Method?
Java使用起来非常方便,然后有些层次的任务用Java实现起来不容易,或者我们对程序的效率很在意时,问题就来了
1. 与Java环境外交互
    - **有时Java应用需要与Java外面的环境交互,这是本地方法存在的主要原因**,Java需要与一些底层系统,如操作系统或某些硬件交换信息时,本地方法正是这样一种交流机制,它为我们提供了一个非常简洁的接口,而且我们无需去了解Java应用之外的繁琐的细节
2. 与操作系统的交互
    - JVM支持着Java语言本身和运行时库,它是Java程序赖以生存的平台,它由一个解释器(解释字节码)和一些连接到本地代码的库组成,然后不管怎样,他毕竟不是一个完整的系统,它经常依赖于一些底层系统的支持,这些底层系统常常是强大的操作系统,**通过本地方法,我们得以用Java实现了jre的与底层系统的交互,甚至JVM的一些部分就是C写的**,还有,如果我们要使用一些Java语言本身没有提供封装的操作系统的特性时,我们也需要使用本地方法
3. Sun's Java
    - **Sun的解释器是C实现的,这使得它能像一些普通的C一样与外部交互**,jre大部分是Java实现的,它也通过一些本地方法与外界交互,例如:类java.lang.Thread的setPriority()方法是用Java实现的,但是它实现调用的是该类里的本地方法setPriority0(),这个本地方法是用C实现的,并被植入JVM内部,在Windows 95的平台上,这个本地方法最终将被调用win32 SetPriority() API,这是一个本地方法的具体实现,由JVM子哦姐提供,更多的情况是本地方法由外部的动态链接库提供,然后被JVM调用.

---

>现状
目前该方法使用的越来越少了,除非是与硬件有关的应用,比如通过Java程序驱动打印机或者Java系统管理生产设备,在企业级应用中已经比较少见,因为现在的异构领域间的通信很发达,比如可以使用Socket通信,也可以使用Web Service等等.

## 本地方法栈

- Java虚拟机栈用于管理Java方法的调用,而本地方法栈用于管理本地方法的调用
- 本地方法栈,也是线程私有的
- 允许被实现成固定或者是可动态扩展的内存大小(在内存溢出方面是相同的)
    - 如果被线程请求分配的栈容量超过本地方法栈允许的最大容量,Java虚拟机将会抛出一个StackOverflowError异常
    - 如果本地方法栈可以动态扩展,并且在尝试扩展的时候无法申请到足够的内存,或者在创建新的线程时没有足够的内存去创建对应的本地方法栈,那么Java虚拟机将会抛出一个OutOfMemoryError异常
- 本地方法是使用C语言实现的
- 它的具体做法是Native Method Stack中登记native方法,在Execution Engine执行时加载本地方法库
- **当某个线程调用一个本地方法时,它就进入了一个全新的并且不再受虚拟机限制的世界,它和虚拟机拥有同样的权限**
    - 本地方法可以通过本地方法接口**来访问虚拟机内部的运行时数据区**
    - 它甚至可以直接使用本地处理器中的寄存器
    - 直接从本地内存的堆中分配任意数量的内存
- *并不是所有的JVM都支持本地方法,因为Java虚拟机规范并没有明确要求本地方法栈的使用语言,具体实现方式,数据结构等,如果JVM产品不打算支持native方法,也可以无需实现本地方法栈
- 在Hotspot JVM中,直接将本地方法栈和虚拟机栈和二为一

## 堆
