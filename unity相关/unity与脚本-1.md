---
title: unity与脚本语言与C#初入门
date: 2020-02-19 14:57:16
categories:
 - Unity
 - C#编程语言
tags:
 - Unity 2d
 - C#
 - Unity工具
---

**warning--这是很个人化的unity脚本笔记，由于本人有过编程经验(虽然也很渣就是)，所以对于我来说比较基础的或了解过的东西在这里不会细讲，详情可以去官方文档搜索更加详细的脚本教程。**

**理解unity脚本**
在unity中，脚本应该被视为一种行为组件。
与unity的其它组成相同，他们也可以添加到游戏对象上。
可以为游戏对象添加更多的行为模式。
可以在项目栏创建脚本，并且把脚本拖到游戏对象上。
也可以到“添加组件”里，添加在你项目栏存在的游戏脚本。
也可以到组件-script 添加你想要的脚本。
unity的脚本支持 Javascript，C#，Boo（Boo是python的衍生，很少用）编写。

**变量与函数**
C#声明变量和函数的方式，和C++类似。
变量： type nameOfVariable = value;
可以对变量进行各种赋值，运算的操作。
start函数，在脚本依附的对象进入场景时调用，可以通过Debug.Log()来获取变量值，如果我们想看到这个值，可以在unity 播放模式下的控制台中看到（前提是你的脚本放在了游戏对象中）。
函数：typeOfReturn nameOfFunction(type nameOfparameter)
不作任何的返回值的操作的函数的type一般是void。
以下是代码：
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;

	public class NewBehaviourScript : MonoBehaviour//声明类
	{
	    int myint = 6;//声明一个变量，并且赋值为6.
	
	    void Start()
	    {
	        myint = Text(myint);//调用函数
	        Debug.Log(myint);//调用变量
	    }
	
	   int Text(int x)//声明一个int类型的变量，定义整型参数为x
	    {
	        return x * 2;//返回参数
	    }
	}

**脚本规范与语法**
点操作符，可以让你分开或者访问到unity里一个复合对象的元素（复合对象就是包含了很多元素的对象）。
句号-出现在代码的中间，它的工作方式类似于写地址。
分号-分号用来完结一个语句（除了使用花括号的语句）。
花括号-缩进-可以展现出代码的功能结构（shift+Tab可以对选中内容缩进）。
右斜杠-注释，双右斜杠进行代码注释，注释的内容将不会被执行。
或者用这种方式多行注释：
	/*How
	 are
	 you?
	 * */

**C#与Javascript脚本的区别**

unity的脚本里都包含一个或更多的类
C#脚本会写上class声明。
Javascript则被隐藏了
意思是你在这里写都会自动包含到类中。

C#的声明变量type nameOfVariable = value;其中type可以是int或float之类的类型。
在js中，变量声明以var关键字为起始。var nameOfVariable :type = value。
顶部的#pragma strict会强制你选择一种类型。

C#的函数声明typeOfReturn nameOfFunction(type nameOfparameter)无返回值时typeOfReturn为void。
Js的函数声明，以function关键字开始。function nameOfFunction(nameOfparameter : type)，可以用冒号指定返回值类型。

默认访问修饰符(Default Private)：
C#是private。
Javascript里默认是public。

**分支与循环**
C#的ifelse语句和c++类似，满足if()括号里的条件执行if下的语句，如果条件不满足而且if语句下接了else的话，执行else语句。可以用这种方式处理分支结构。

C#的循环语句和C++类似，有while循环，do while循环，for循环，foreach循环。
foreach适合遍历集合，比如数组。
foreach循环用foreach关键字，后接圆括号，在括号定义一个与需要遍历的集合元素相同类型的变量。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class aboutre : MonoBehaviour
	{
	    void Start()
	    {
	        string[] strings = new string[3];//创建含有三个字符串的数组
	
	        strings[0] = "first";
	        strings[1] = "second";
	        strings[2] = "third";
	
	        foreach(string item in strings) {//定义一个与需要遍历的集合元素相同类型的变量 item，
	                                        //后接关键字in,后接需要遍历的集合的名字。
	            print(item);//在循环体里打印上面创建的item的值，每次循环后，item的值会变成集合下一个元素的值。
	        }
	    }
	}
需要注意的是，用foreach时不能修改被遍历的集合中的元素。

**unity的作用域和访问控制符**
变量的作用域就是指，代码中变量可以被使用的范围，就如c++的局部变量和全局变量的概念。

public和private访问控制符，可以定义在类中的变量。
与定义在函数中的不同，他们有访问控制符的属性。
访问控制符是声明变量时放在数据类型前的关键字，其作用是定义变量或者函数的可见范围。
通常，如果其它脚本需要访问变量或者函数，那么它应该是public的，否则就是private的。

变量声明为public意味着它可以在类之外被访问，同时意味着这个变量可以在Unity的检视视图显示并且修改，使用户在测试游戏时修改变量。
如果变量在类里初始化为某个数，那么它会被检视视图设置的值覆盖。
但是如果值设置在start()和Awake()这类函数里，这些都发生在检视视图操作以后，就不会被覆盖。所以，尽管public的变量可以在组件里修改，你还是可以在运行时通过脚本来修改它。

private的变量通常只能在类内部修改，C#中不特殊指明时，默认的访问控制符是private。

**Awake()和Start()**
Awake和Start函数在脚本被加载时自动被调用。
Awake会在脚本不使用时先被调用。适合在里面设置脚本间的引用和初始化操作。
Start在Awake之后被调用，在第一次调用Update之前，但只在脚本被使用时出现，这意味着你可以使用Start来处理任何你需要在脚本使用后才能发生的事，这允许你推迟任何初始化代码直到其真正需要为止。
*比如一个enemy进入游戏，使用Awake将弹药数量添加到他的身上，但是只能在Start函数中赋予其射击的能力，在脚本组件被使用时。*
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class aboutre : MonoBehaviour
	{
	    void Awake() {//播放时不执行脚本，控制台上只执行这段命令
	        Debug.Log("Hello");
	    }
	    void Start() {//播放时执行脚本，控制台也会执行这段命令
	        Debug.Log("World!");
	    }
	}
需要注意，Start和Awake在脚本的生命周期内只会被调用一次。所以不能去通过使用被使用的脚本来重复使用Start函数。
*Start是唯一一个可以绕过自己编制计数器使用延迟的函数。*

**Update()和FixedUpdate()**
Update可能是Unity里最常用的函数。
他在每帧，每个使用它的脚本上被调用一次。
几乎所有需要周期性修改调整的东西都发生在这。
  *非物理对象的移动*
  *简单的计时器*
  *输入检测*
都是常在Update里做的事。
Update不是按照规则的时间间隔被调用的，如果每帧的处理时间比其后的长。

FixedUpdate和Update类似，但是有几处不同。
他被调用的时间线是规则的，调用之间的时间间隔也是不同的。
FixedUpdate结束之后，紧接着，任何必要的物理计算开始进行。
因此，任何影响物理对象的操作，都该在FixedUpdate里执行。
在FixedUpdate处理物理效果时，最好用力来触发运动。

