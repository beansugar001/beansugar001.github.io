---
layout:     post   				    # 使用的布局（不需要改）
title:      Java单例模式和main()的理解			# 标题 
subtitle:   Java  #副标题
date:       2024-3-4 				# 时间
author:     beansugar 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Java
---


### 单例模式  

何为单例模式 ：采取一定方法保证在整个软件系统中,对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法   
      
实现思路 ：如果我们要让类在虚拟机中只能产生一个对象，我们必须将类的构造器的访问权限设置为private，这样就不能用new操作符在类的外部产生类的对象了，但在类内部仍然可以产生改类的对象。因为在类外部开始还无法得到类的对象，只能调用该类的某个静态方法以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的该类对象的变量也必须定义成静态的    

```java
//代码示例1("立即加载"，随着类的加载，当前唯一的实例就创建了)
//优点：写法简单，在内存中加载较早，使用更快更方便；线程安全
//缺点：内存中占用时间长
public class BankTest {
    public static void main(String[] args){
        Bank bank1 = Bank.getInstance();
        Bank bank2 = Bank.getInstance();
        System.out.println(bank1 == bank2);//true
        //可以将Bank类中的instance看作是一个static属性，所以在外部每次new的时候，都是同一个instance
    }	
}


class Bank{
    //类的构造器私有化
    private Bank(){

    }

    //在类的内部创建当前类的实例,静态属性只能调用静态的属性和方法
    private static Bank instance = new Bank();

    //提供一个get方法来使用私有实例,必须声明为静态的
    public static Bank getInstance(){
        return  instance;
    }
}
```

```java
//代码实例2("延迟加载"，在需要使用时进行创建)
//缺点：线程不安全
class Bank{
    private Bank(){
     
    }
    //将该属性赋值为null
    private static Bank instance = null;
    //通过get方法获取当前实例，若为null（未创建对象），则在方法内部创建
    public static Bank getInstance(){
        if(instance == null){
			instance = new Bank();
        }
       	return instance;
	} 
}
```


### main()方法的理解

```java
	public static void main(String[] args){
	
	}
```

	- 理解1: 看作是一个普通的静态方法
	- 理解2: 看作是主程序的入口，格式是固定的

共勉！
