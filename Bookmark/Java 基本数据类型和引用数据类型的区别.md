Java中的数据类型分为两大类，基本数据类型和引用数据类型  
原文地址：https://blog.csdn.net/nanhuaibeian/article/details/105999420

#### 文章目录

+   +   [一、数据类型](#_2)
    +   [二、基本数据类型和引用数据类型的区别](#_19)
    +   +   [1\. 存储位置](#1__20)
        +   [2\. 传递方式](#2__40)
    +   [三、补充知识点](#_96)
    +   [四、装箱和拆箱](#_118)

### 一、数据类型

1.  基本数据类型  
    基本数据类型只有8种，可按照如下分类  
    ①整数类型：`long、int、short、byte`  
    ②浮点类型：`float、double`  
    ③字符类型：`char`  
    ④布尔类型：`boolean`  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7737d91482f55f0cd0320b01e12af1ce.png)

> 对于 boolean 类型，只有真和假两个可能的值，java规范中它的大小并没有明确的定义  
> （1）1 byte：boolean类型的值只有true和false两种逻辑值，在编译后会使用1和0来表示，这两个数在内存中按位算，仅需1位（bit）即可存储，位是计算机最小的存储单位。  
> （2）4 byte：《Java虚拟机规范》虽然定义了boolean这种数据类型，但是只对它提供了非常有限的支持。Java语言表达式所操作的boolean值，在编译之后都使用Java虚拟机中的int数据类型来代替  
> 使用int的原因是，对于当下32位的处理器（CPU）来说，一次处理数据是32位（这里不是指的是32/64位系统，而是指CPU硬件层面），32 位 CPU 使用 4 个字节是最为节省的，哪怕你是 1 个 bit 他也是占用 4 个字节。因为 CPU 寻址系统只能 32 位 32 位地寻址，具有高效存取的特点。

2.  引用类型  
    引用数据类型非常多，大致包括：类、 接口类型、 数组类型、 枚举类型、 注解类型、 字符串型  
    例如，String类型就是引用类型，还有Double，Byte,Long,Float,Char,Boolean,Short（注意这里和基本类型相比首字母是大写）  
    简单来说，所有的非基本数据类型都是引用数据类型

### 二、基本数据类型和引用数据类型的区别

#### 1\. 存储位置

java中的基本数据类型一定存储在栈中的，这句话是错的！

（1）在方法中声明的变量

> 该变量是局部变量，每当程序调用方法时，系统都会为该方法建立一个方法栈，其所在方法中声明的变量就放在方法栈中，当方法结束系统会释放方法栈，其对应在该方法中声明的变量随着栈的销毁而结束，这就局部变量只能在方法中有效的原因

> 在方法中声明的变量可以是基本类型的变量，也可以是引用类型的变量。
> 
> +   当声明是基本类型的变量的时，其变量名及值（变量名及值是两个概念）是放在JAVA虚拟机栈中
> +   当声明的是引用变量时，所声明的变量（该变量实际上是在方法中存储的是内存地址值）是放在JAVA虚拟机的栈中，该变量所指向的对象是放在堆类存中的。  
>     ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/86292cca5788cdf60de2820505af4af6.png)

（2）在类中声明的变量

> 在类中声明的变量是成员变量，也叫全局变量，放在堆中的（因为全局变量不会随着某个方法执行结束而销毁）。

> 同样在类中声明的变量即可是基本类型的变量 也可是引用类型的变量
> 
> +   当声明的是基本类型的变量其变量名及其值放在堆内存中的
> +   引用类型时，其声明的变量仍然会存储一个内存地址值，该内存地址值指向所引用的对象。引用变量名和对应的对象仍然存储在相应的堆中

#### 2\. 传递方式

基本变量类型：在方法中定义的非全局基本数据类型变量，调用方法时作为参数是按数值传递的

```java
//基本数据类型作为方法参数被调用
public class Main{
   public static void main(String[] args){
       int msg = 100;
       System.out.println("调用方法前msg的值：\n"+ msg);    //100
       fun(msg);
       System.out.println("调用方法后msg的值：\n"+ msg);    //100
   }
   public static void fun(int temp){
       temp = 0;
   }
}
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5ff3115dad9ffaace24f3e6190887bb9.png)  
引用变量类型：引用数据类型变量，调用方法时作为参数是按引用传递的

```java
//引用数据类型作为方法参数被调用
class Book{
    String name;
    double price;

    public Book(String name,double price){
        this.name = name;
        this.price = price;
    }
    public void getInfo(){
        System.out.println("图书名称："+ name + "，价格：" + price);
    }

    public void setPrice(double price){
        this.price = price;
    }
}

public class Main{
   public static void main(String[] args){
       Book book = new Book("Java开发指南",66.6);
       book.getInfo();  //图书名称：Java开发指南，价格：66.6
       fun(book);
       book.getInfo();  //图书名称：Java开发指南，价格：99.9
   }

   public static void fun(Book temp){
       temp.setPrice(99.9);
   }
}
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/97fb8bc1452925e7d43a1292782ce011.png)  
引用数据类型是基本数据类型的包装类

### 三、补充知识点

1.  一切的引用数据类型都可以使用Objec进行接收，[具体可参考](https://blog.csdn.net/nanhuaibeian/article/details/103956974)

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0e288b4d312db191675be2a47bc66569.png)

```java
public class Test {
    public static void main(String[] args) {
        int[] list1 = {1, 2, 3};
        //因为数组也是引用，此处可以接收这里的数组应用list1
        Object list2 = list1;
        //但是这里输出时，就需要判断下是否是整型数组，然后再进行输出操作
        if (list2 instanceof int[]){
            //执行向下转型操作
            int[] list3 = (int[]) list2;
            for (int i : list3) {
                System.out.println(i);
            }
        }
    }
}
```

### 四、装箱和拆箱

我们将基本数据类型转化为引用数据类型的过程叫做装箱，相应的，我们把从引用数据类型转化为基本数据类型的过程叫做拆箱

如下列代码：我们首先创建了两个Integer类型的对象。然后使用intValue可以将Integer对象拆箱为int类型变量

```java
package com.Integer;
 
public class IntegerDemo3 {
	public static void main(String[] args) {
		//创建两个Integer对象
		Integer x=new Integer("10");
		Integer y=new Integer("10");
		//创建两个int类型变量
		int m=10;
		int n=10;
		//valueOf的作用是将int变量转化成Integer对象
		//将int类型变量“手动”装箱
		Integer m1=Integer.valueOf(m);
		Integer n1=Integer.valueOf(n);
		
		//intValue的作用是将Integer对象转化成int类型
		//将Integer对象“手动”拆箱
		int v1=x.intValue();
		int v2=y.intValue();
}
```

自动装箱自动拆箱  
java自己的一种机制，叫做自动装箱和自动拆箱

借用反编译器可以看一下class文件中经过处理后的文件，可以看得出，class中自动添加了intValue和valueOf方法，用来适应对方。这就是自动装箱和自动拆箱。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f0af77feba6bfcce21443a7e3807e9af.png)  
参考：https://blog.csdn.net/qq\_41469636/article/details/107369554
