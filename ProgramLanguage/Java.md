# 一、Java简介  
Java介于编译型语言和解释型语言之间。编译型语言如C、C++，**代码是直接编译成机器码执行**，但是不同的平台（x86、ARM等）CPU的指令集不同，因此，需要编译出每一种平台的对应机器码。解释型语言如Python、Ruby没有这个问题，可以由解释器直接加载源码然后运行，代价是运行效率太低。而Java是**将代码编译成一种“字节码”**，它类似于抽象的CPU指令，然后，针对不同平台编写虚拟机，**不同平台的虚拟机负责加载字节码并执行**，这样就实现了“一次编写，到处运行”的效果。当然，这是针对Java开发者而言。对于虚拟机，需要为每个平台分别开发。为了保证不同平台、不同公司开发的虚拟机都能正确执行Java字节码，SUN公司制定了一系列的Java虚拟机规范。从实践的角度看，JVM的兼容性做得非常好，低版本的Java字节码完全可以正常运行在高版本的JVM上。

随着Java的发展，SUN给Java又分出了三个不同版本：  
- Java SE：Standard Edition
- Java EE：Enterprise Edition
- Java ME：Micro Edition(嵌入式设备)

三者之间的关系：
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/0c118e97-fb5e-48b9-844c-effab7e93396" alt="java版本">
</div>

**学习路线**
1. 首先要学习Java SE，掌握Java语言本身、Java核心开发技术以及Java标准库的使用；

2. 如果继续学习Java EE，那么Spring框架、数据库开发、分布式架构就是需要学习的；

3. 如果要学习大数据开发，那么Hadoop、Spark、Flink这些大数据平台就是需要学习的，他们都基于Java或Scala开发；

4. 如果想要学习移动开发，那么就深入Android平台，掌握Android App开发。

**名词解释**：  
- JDK：Java Development Kit
- JRE：Java Runtime Environment  

具体关系图示：  
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/82efdac9-1600-4090-b881-210b9de42c79" alt="编译流程">
</div>
JRE就是运行Java字节码的虚拟机，要编译成Java字节码，就需要JDK。  

- JSR规范：Java Specification Request
- JCP组织：Java Community Process  
- RI：Reference Implementation
- TCK：Technology Compatibility Kit  

一个JSR规范发布时，为了让大家有个参考，还要同时发布一个“参考实现RI”，以及一个“兼容性测试套件TCK”

# 二、语法简要
为什么是语法简要？因为java既能在不同平台上运行，又是一个面向对象的语言，它是介于编译型语言和解释性语言之间的一门语言，所以面向对象方面和c#很像，基础的语法和c又很像，因此在这里只强调和c、c#之间的不同之处。

## 2.1 变量类型
变量类型分为两类：
1. 值类型(整型、浮点、字符型、布尔型)
2. 引用类型(除了上述之外的所有，例如String)
### 2.1.1 字符串
字符串也可以像python一样通过运算符`+`拼接。  
### 2.1.2 数组
数组是引用类型，其数组里的内容既可以是值类型，也可以是引用类型。  
数组的遍历：
- c中的基本for循环
- c#中的foreach循环。例如：for (String string : args)

## 2.2 常量
不可变的量，不是用`const`而是用`final double\int`来声明常量。

## 2.3 面向对象
### 2.3.1 字段
java中字段的概念和

# 三、命令行运行
java可以通过命令行进行编译，然后执行java程序。
## 3.1 Windows
如下图所示，进入项目文件的src下面，然后在这里运行cmd。也可以通过cd命令进入文件夹下。
<div align="center">
    <img src="https://github.com/xuehao-in-studing/learngit/assets/102791379/f68a6b44-8e41-4af0-aed5-859fdcbbeb0c" alt="java版本">
</div>

执行命令：  

1. `javac Main.java`编译
2. `java Main`运行

如果声明了package，那么命令改变，cmd的运行路径不变：
1. `javac hello\Main.java`
2. `java hello\Main`  

hello为包名，根据你自己的包名进行替换。