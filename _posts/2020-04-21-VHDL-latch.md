---
layout:     post
title:      "VHDL中的锁存器"
subtitle:   " \"锁存器真的有害吗？\""
date:       2020-04-01 12:00:00
author:     "LiYanXin"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - VHDL
---

# VHDL-learning
VHDL FPGA学习

## 锁存器是什么？

我们经常听说，if-else，case语句必须要补全，if不能缺少else，case不能缺少default，否则会有锁存器生成，带来不好的影响。  
那么锁存器到底是什么呢？

	<div align=center><img width="250" height="250" src="/img/latch.png"></div>
	
锁存器的RTL模型如图所示，它是一种在异步时序电路系统中，对输入信号电平敏感的单元，用来存储信息。
一个锁存器可以存储1bit的信息，通常，锁存器会多个一起出现，如4位锁存器，8位锁存器。锁存器在数据未锁存时，输出端的信号随输入信号变化，
就像信号通过一个缓冲器，一旦锁存信号有效，则数据被锁存，输入信号不起作用。因此，锁存器也被称为透明锁存器，指的是不锁存时输出对于输入是透明的。

## 锁存器与寄存器的区别
	我们知道，FPGA的底层电路最丰富的资源就是寄存器，而锁存器在电路中实际是不存在的，如果我们需要一个锁存器，就需要寄存器和其他资源搭起来。
	寄存器和锁存器两者都是基本存储单元，单锁存器是电平触发的存储器，触发器是边沿触发的存储器。本质是，两者的基本功能是一样的，都可以存储数据。
但一个是组合逻辑的，一个是在时序电路中用的。
	

## 实测latch的产生
  我们可以使用vivado自己写一段程序，实际测试Latch是如何产生的。
  
  <div align=center><img width="250" height="250" src="/img/RTL_latch_code.jpg"></div>
  
  如上所示，我们写一段简单的判断使能为1时，对输出赋值的程序。注意这是**时序逻辑电路**。
  
  * 写else：
  <div align=center><img width="550" height="350" src="/img/RTL_right.jpg"/></div>
  
  * 不写else：     
    <div align=center><img width="550" height="350" src="/img/RTL_right2.jpg"/></div>
	
	可以看出，在时序电路中，写不写else好像没有什么区别，都不会有latch生成。那我们为什么需要else呢？个人认为这是一种良好的习惯，
	你需要考虑else情况下我们究竟需要怎么赋值，可能会发现更好的描述方式或者自己程序里的bug。
	但是在时序电路里，仍然会有必须要写else的情况。
	
	典型例子：用Verilog HDL实现一个锁存器，当输入数据大于127时，将输入数据输出，否则输出0
	* 正确描述
	if (din > 127 )  
        dout = din;  
    else  
        dout = 0;  
	end if;
	* 错误描述
	if (din > 127 )  
        dout = din; 
	end if;
	
	如果不写else，当输入小于127时，输出保持了上次的127，不是0，没有达到设计要求。所以写全else是一个很好的编程习惯。
	
	
　　那么在组合逻辑电路中，不写else是否会有latch产生呢？
	<div align=center><img width="250" height="250" src="/img/RTL_latch_code2.jpg"></div>
	
	<div align=center><img width="250" height="250" src="/img/RTL_latch.jpg"></div>
	
	上面的是组合逻辑电路，可以看出，综合后的电路明显得latch产生了。
	
	因此，在组合逻辑电路中，我们必须补全else或case，避免latch的产生。
    
    
    
    
    
    
    
