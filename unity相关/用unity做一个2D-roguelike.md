---
title: 用unity做一个2D-roguelike
date: 2020-02-29 09:35:14
categories:
 - Unity
tags:
 - Unity 2d
 - Unity项目总结
---

1.使用类继承的方式使玩家和敌人拥有相同的移动方式；

2.对象的单例模式

3.可序列化 serializable

4.利用数组和random函数，随机生成地图

5.虚函数

在某基类中声明为 virtual 并在一个或多个派生类中被重新定义的成员函数，用法格式为：virtual 函数返回类型 函数名（参数表） {函数体}；实现多态性，通过指向派生类的基类指针或引用，访问派生类中同名覆盖成员函数。

6.抽象类

抽象类往往用来表征对问题领域进行分析、设计中得出的抽象概念，是对一系列看上去不同，但是本质上相同的具体概念的抽象。
通常在编程语句中用 abstract 修饰的类是抽象类。在C++中，含有纯虚拟函数的类称为抽象类，它不能生成对象；在java中，含有抽象方法的类称为抽象类，同样不能生成对象。
抽象类是不完整的，它只能用作基类。在面向对象方法中，抽象类主要用来进行类型隐藏和充当全局变量的角色。

7.protected保护类型前缀

8.float.Epsilon代表近似于0的数

9.MoveTowards函数
Vector3 newPosition = Vector3.MoveTowards (rb2D.position, end, inverseMoveTime * Time.deltaTime)
MoveTowards沿着指向目标点的直线移动点，这个点是inverseMoveTime*Time.deltaTime时间内能够移动到的最接近目的地的点。
Vector3.MoveTowards的返回值是一个点，这个点
三个参数，第一个是原位置，第二个是要移动到的位置

10.LayerMask类型

11.out关键字

12.linecast

13.泛型参数

14.RaycastHit2D

15.as关键字

16.IEnumerable
