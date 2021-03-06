---
title: unity 2d与物理 1
date: 2020-02-16 19:20:46
categories:
 - Unity
tags:
 - Unity 2d
 - Unity工具
---
**2D物理引擎**
现在unity提供了2套不同的游戏引擎，2D一个、3D一个，3D引擎中有第三维度（z轴）。
physic 2d组件与对应的3d版本行为很相似，但尽管3D和2D物理能存在于同一场景中，但彼此之间不会交互。
2d和3d引擎的最决定性区别是在2d引擎中没有深度的概念，所有的2d物理只在x,y轴表现，这个片面的z轴是0值。
由于可以在3d模式下查看2d对象，也可以修改2d物理引擎作用下的游戏对象位置的z轴坐标。
需要注意的是所有的物理交互都发生在z轴的0值处。

2d物理引擎有一些项目级别的设置。
edit-project settings-physics2D。
这些设置中包含一些详细参数。比如重力的方向和大小。默认材质，层间碰撞矩阵，还有很多其他。

rigidbody2D，对游戏对象添加这个组件可以使它处在2d物理引擎的控制下。
rigidbody2D组件与碰撞体一起进行碰撞检测，能接受力和扭距，处理各种类型的关节，以及其他特别类型的2D物理行为，这个组件也用来跟踪，其所在物理对象的重要的物理属性，他拥有物体的质量参数，线性阻力和角阻力，受重力比例，以及其它属性。

在每一个2d项目，在physics 2d setting里都有一个预设的重力值。
重力将会影响所有的添加了rigidbody的游戏对象。除非rigidbody被设置为忽略重力（将gravity scale设置为0）任何其它数值，都将会改变作用于物体的重力值，可以对每个游戏对象单独设置。

如果对游戏对象添加一个2d碰撞体，他可以参与物理碰撞，和触发器事件。2d碰撞体允许游戏对象在2d场景中拥有物理形状，或者物理实体。有多种类型2d碰撞体可以与rigidbody2d（以下简称rb2d）一起工作，圆形，盒状，线型，多边形 碰撞体。每一个碰撞体都有一组不同的行为，可以在不同的环境中选择使用。

添加了rb2d组件的游戏对象，可以使用一些joint 2d（2d关节类型）组件，添加了该组件游戏对象可以参与更复杂的，特殊的物理行为。2d关节组件允许游戏对象以场景内某点或某个对象为锚点，并对此锚点做出物理反应。

碰撞体的表面属性由特殊类型的材质决定。这种材质是2d物理材质资源。两个拥有2d碰撞体的物体碰撞时，2d物理材质用来控制其摩擦和弹跳行为。

**rigidbody2D刚体组件**
点击add component - physics 2d - rb2d可以设置物体的物理参数。
所有参与交互的对象都必须拥有2D碰撞组件，同时其中至少有一个拥有rb2d插件。
点击component-physics 2d-box collider
2d碰撞体会自动匹配精灵的大小。
rb2d组件，除了允许游戏对象被2d物理影响，也用来定义游戏对象的一些重要物理属性。

2d刚体（rigidbody）的质量越大，就需要更多的力来移动，与其它对象碰撞时也会产生更大的效果。
线型阻力影响2d刚体的移动速度。
角阻力会影响它的旋转或者角速度。
重力比例控制重力如何影响2D刚体，与直接修改全局的重力值不同，重力比例允许对每个游戏对象进行精确控制，重力比例越小。物体下落速度越慢，设置为0时，忽略重力。
选中fixed angle（固定角度）时，rb2d会受到2d物理中力的影响，但不会旋转。
选中is kinematic（是否运动学）rb2d被认为是运动学物体。不会被2d物理的力影响，包括重力和碰撞，这种情况一般发生在，不使用物理引擎和物理力，去移动一个有2d碰撞体的对象。在游戏进行移动一个2d碰撞体时，非常建议把rb2d组件也加上，然而我们不想这个平台受到其它作用力影响，这种情况下可以把is kinematic打开。
interpolate（插入）设置用来平滑游戏对象的运动，当游戏对象被rb2d移动时发生抖动，使用interpolate来帮助平滑它的变换运动（根据上一帧的位置来平滑）
sleeping mode控制刚体如何休眠来节约处理时间。never sleep将禁止休眠，start awake保证rb2d在被实体化时是唤醒状态，start sleeping将初始化rb2d为休眠，但是可以被碰撞唤醒。
collision detection设置，用于控制rb2d上的碰撞检测机制类型。离散类型：此时，只有游戏对象的2d碰撞体，在一个物理更新时，与其他碰撞体接触，才产生碰撞事件；连续检测适用于快速移动的物体，当游戏对象在两个更新之间可能发生了碰撞时，就产生碰撞事件。

**2d碰撞组件**
一个游戏对象必须使用碰撞体组件来定义它的物理外形，同时与相关的物理引擎交互。
添加2d碰撞体组件，有四种2d碰撞组件可供选择：
2d圆形碰撞体，2d方形碰撞体，2d多边形碰撞体，2d边缘碰撞体。
这些碰撞体组件有很多相同的参数，因为都继承自同一个通用基础2d碰撞体，但每一个碰撞体都为一种特殊的形状做了优化。

多边形碰撞体和边缘碰撞体很相似，他们都由一组点或者顶点，及点间的连线组成，他们的主要区别是，多边形碰撞体必须是覆盖一块区域的封闭图形，边缘碰撞体必须是开放的，用于定义一个或多个段的边缘，所以它适合用于创造单个的固定表面。

所有的碰撞体在检视视图中有2个相同参数；
is trigger（是否触发器）用来将碰撞体设置为触发器碰撞体，2d触发器碰撞体不参与碰撞，而且发出的是触发器消息，而不是碰撞消息，这些触发器消息可以用来在场景里加人新的动作；
material（材质）参数是对2d碰撞体所用的2d物理材质的引用，可以设置为none（空），物理材质用来定义当碰撞体相互碰撞时的反应，可以定义弹跳系数和表面摩擦力。
其余独有参数用来定义特定碰撞体的形状。

2d碰撞体必须被添加到游戏对象上使用，可以点击Add Component button选择physics 2d选择一个collider2d组件添加2d碰撞体。

添加了碰撞体组件后，unity会尝试将碰撞体尺寸匹配精灵的尺寸，如果对碰撞体的形状和尺寸不满意，可通过修改检视视图的参数修改形状，或者在场景视图内直接修改碰撞体（在场景视图编辑碰撞体时，按住shift，这会在2d碰撞体gizmo上显示一些操作点，这些操作点可以拖拽）
对于边缘碰撞体和多边形碰撞体，拖拽一个已存在的操作点，会移动碰撞体的顶点，改变其形状，而不是调整大小，将鼠标放到边缘碰撞体和多边形碰撞体的一个空白的边上，点击就能创建一个新的顶点，按住ctrl点击顶点可以将其删除。

一个性能最优的常用模式是：只添加2d碰撞体，不添加rigidbody组件，对所有参与2d物理交互的，静态或不会移动的物体。
同时添加2d碰撞体和rigidbody组件，对场景内的交互中，动态且可移动的对象。