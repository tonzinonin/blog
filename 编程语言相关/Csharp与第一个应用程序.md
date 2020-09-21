---
title: 'C#与第一个应用程序'
date: 2020-03-01 09:03:38
categories:
 - C#编程语言
tags:
 - C#
---
vs2010

C#是一个完全面向对象的编程语言

Solution是针对客户需求的总的解决方案。
Project解决具体的某个问题。

分别编写Console、WPS、Windows Forms的Hello World。

类(class)是构成程序的主体
名称空间(namespace)以树形结构组织类(和其它类型)如Button和Path类。

类库引用是使用名称空间的物理基础。不同技术类型的项目会默认引用不同的类库。
DLL引用（黑盒引用，无源代码）
项目引用（白盒引用，有源代码）

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Windows.Forms;
	
	namespace ClassAndInstance
	{
	    class Program 
	    {
	        static void Main(string[] args)
	        {  
	            Form myForm = new Form(); //对象的实例化
	            myForm.Text = "MyForm!";
	            myForm.ShowDialog();
	        }
	    }
	}

类的三大成员：
属性(Property)
储存数据，组合起来表示类或对象当前的状态
方法(Method)
由C语言的函数(function)进化而来，表示类或对象"能做什么"
工作中%90的时间是在与方法打交道，因为它是"真正做事"、"构成逻辑"的成员
事件(Event)
类或对象通知其它类或对象的机制，为C#所持有(Java通过其它办法实现这个机制)
善用事件机制非常重要


