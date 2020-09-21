---
title: Unity检测物体
date: 2020-09-13 22:06:38
categories:
 - Unity
tags:
 - Unity 2d
 - C#
 - Unity工具
---

1. OnCollisionEnter函数和OnTriggerEnter函数

这篇文章对这两种函数有很好的诠释：

作者：THE_ZERO
来源：CSDN
原文：https://blog.csdn.net/tianyao9hen/article/details/53141141

OnTriggerEnter和OnCollisionEnter的触发条件是不同的，需要在设计的过程中加以关注。

触发的共同要求
碰撞的两个物体A,B，都要有碰撞体（collider），Box Collider，Sphere Collider，Capsule Collider等的任意一种

当A，B都添加刚体（Rigidbody）时
OnCollisionEnter方法
A和Bx相互碰撞时，无论是谁碰撞的谁，两者都能触发OnCollisionEnter方法，前提是两者都没有勾选isTrigger。
OnTriggerEnter方法
A或者B中有一个勾选isTrigger或者两者都勾选isTrigger后，A和B都可以触发OnTriggerEnter方法，但是不可进入OnCollisionEnter方法。
注意：

OnCollisionEnter方法必须是在两个碰撞物体都不勾选isTrigger的前提下才能进入，反之只要勾选一个isTrigger那么就能进入OnTriggerEnter方法。

OnCollisionEnter和OnTriggerEnter是冲突的不能同时存在的。

当A，B有一个添加了刚体（Rigidbody）时
OnCollisionEnter方法

若A添加了刚体，B没有添加刚体，A去碰撞B，则A会被弹开，B不会运动，此时A、B都会触发OnCollisionEnter方法。
若A添加了刚体，B没有添加刚体，B去碰撞A，不会发生碰撞效果，此时A和B都不会触发OnCollisionEnter方法。
OnTriggerEnter方法

只要A和B中有一个添加了刚体，无论谁碰撞谁，两者都会触发 OnTriggerEnter方法
总结：

OnCollisionEnter方法要求碰撞的发起方必须拥有刚体，而被碰撞方有没有刚体并不重要。OnTriggerEnter方法则对此没有要求，只需要碰撞双方有一个具有刚体即可触发。即刚体是一个判断是否实现碰撞的是与否的标志。
刚体对于系统的开销是很大的，所以在使用刚体时，根据可能发生的碰撞触发事件，适当的减少刚体，是一个减少资源消耗的好办法。 比如地面就可以不设置刚体，因为地面是永远不动的，把人物设置刚体就可以实现真实的物理碰撞效果了。
需要让被碰撞者触发OnCollisionEnter的办法
我们可以用一些方法来规避OnCollisionEnter的要求，达到同样的效果。

例如：
A添加了刚体，B没有刚体，但是需要B碰撞A同时触发碰撞效果。此时碰撞发起者是B，而B没有刚体，是无法触发OnCollisionEnter方法的。
解决方法：
我们可以在B的下面创建一个子物体C（可以是空物体），并给C添加collider，勾选isTrigger。
将C的collider大小设置的和B的collider一样大，并且位置重叠。
使用OnTriggerEnter方法，达到同样的目的。A在和B碰撞的同时，也与C碰撞，会触发A与C之间的OnTriggerEnter方法。
