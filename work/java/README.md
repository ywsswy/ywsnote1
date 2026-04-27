## 查看版本
java -version # 准确说是jre/jdk的版本，输出示例：Runtime Environment (build 1.8.0_282-b08)

## 术语
Java是一门编程语言，分为三大版本，
Java SE即标准版，包含了Java核心类库，主要用来开发桌面应用；
Java EE即企业版，包含SE，又有扩展部分（Servlet，JDBC等）；
JDK(Java Development Kit) 是开发工具包，包含了JRE，同时还包含了编译java源码的编译器javac等工具，是提供给程序员使用的；
JRE(Java Runtime Environment)是运行时环境，包含java基础类库(Java SE API)和Java虚拟机(JVM)，是提供给用户使用的；
JRE根据不同操作系统和不同提供商（IBM,ORACLE等）有很多版本，最常用的是Oracle的、以及开源的OpenJDK；


【Java8 对应 Java SE 8 对应 JDK1.8】，目前最普遍的版本， 其次Java 11/17
Java17 对应 Java SE17 对应 JDK17


源代码：Hello.java
经过javac编译-> Hello.class（字节码，不是机器码，跟平台无关），可以打包成.jar文件
然后不同的平台都可以执行这个.class：
  ┌────────────────────────────────────┐
  │  Windows JVM  →  翻译成 Windows 机器码  │windows上运行jar应用 java.exe -jar xxx.jar
  │  Linux JVM    →  翻译成 Linux 机器码    │
  │  Mac JVM      →  翻译成 Mac 机器码      │
  └────────────────────────────────────┘ 


