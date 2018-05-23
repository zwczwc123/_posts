---
title: 面试笔记
date: 2018-04-22 16:54:03
tags: java软件开发工程师面试笔记
toc: true
---

### Part1：记录面试中遇到的问题
1-1.java内存模型  
1-2.java缓存机制  
<!-- more -->
1-3.快速排序算法  
1-4.sql调优  
1-5.java Error和Exception  
1-6.垃圾回收  
1-7.json数据格式  
1-8.递归算法（递归计算阶乘，菲波那切数列某一项的值）  
1-9.java遍历集合，遍历Map  
1-10.spring ioc和aop  
1-11.集合与Map，以及底层实现原理  
1-12.String以及String判断==  
1-13.Integer的陷阱  
### Part2：java基础


### Part3:做笔试面试题中的笔记总结。
一.关于构造方法   
1.类中的构造方法可以省略，如果省略不写，编译器会提供一个默认的构造方法。   
2.构造方法必须与该构造方法所在类的类名保持一致。   
3.类中的方法名也可以与类名相同，但是实际开发过程中不建议这样写。   
4.当一个对象被new的时候必定会调用构造方法。
5.构造方法可以重载，一个类可以有多个构造方法。   
二.关于main方法   
1.在JAVA语言中，main方法是程序的入口，一个程序要想运行，必须要有main方法，但是只有满足特定条件的main方法，才能作为程序的入口方法。   
2.java是面向对象语言，所有的属性与方法都必须定义在类里面，main方法也要定义在类里面。   
3.java程序可以有多个main方法，但是只有public static void main (String [] args)方法才是java程序的入口，其他main方法都不是。并且这个入口方法必须被定义在类名与文件名相同的被public修饰的类中。如下所示：   

    class T {   
        public static void main(String[] args){   
            System.out.println("T main");
        }   
    }
    public class Test {   
        //程序入口方法
        public static void main(String[] args){   
            System.out.println("Test main");
        }   
    }
    //程序运行结果为Test main
4.在java语言中，不管方法体有几条语句，都需要用大括号{}括起来   
5.在java语言中，一个文件中可以有多个类的存在，被public关键字修饰的类的名字必须与文件名相同。   




