---
title: unity与GUI
date: 2020-02-21 19:20:59
categories:
 - Unity
tags:
 - Unity 2d
 - Unity工具
---

**1.canvas**
canvas是用来控制一组UI元素渲染方式的组件。
所有的UI元素必须是canvas的子节点。
场景中可以创建多个canvas。

每个canvas都有几种不同的渲染模式。
渲染模式可以在RenderMode菜单里选择。

scene Space - Overlay canvas会直接驱动Rect Transform组件的设置。
Ui会覆盖整个屏幕。所有的Ui元素会绘制在场景中其他元素的前面。
在屏幕设置变化时会自动调整大小。
所以canvas上的Rect Transform不能手动编辑。
canvas会设置所有的Rect Transform参数来自动填充整个屏幕。

选中Pixel Perfect时，Ui元素会在渲染时自动调整到最接近的像素。
在某些情况下可能达到锐化外观的效果。

Screen Space - Camera 和 Screen Space - Overlay类似。
但是它是被场景中某个指定的camera来渲染的。
这使得screen space UI可以应用一些camera独有的设置。
最常见用法是使用一个透视摄像头来使UI有一定深度感。
在此模式下，canvas会自动填充摄像头的视野。

当摄像机的viewport设置修改时也会自动调整大小。
在此模式下rect transform组件也会完全由canvas驱动。
此模式下有几个选项，包括pixel perfect。

render camera用来设置渲染此canvas上UI元素的摄像机，此项留空，canvas组件会使用screen space - overlay 的设置来渲染canvas。
当选中screen space - camera 而且设置了一个摄像机时，UI元素会被移动到这个摄像机的视口内并且调整大小。这允许UI与游戏对象在场景中分开渲染。要控制UI在场景中渲染的位置，使用Plane Distance来移动canvas，使其靠近或者远离摄像机。

需要注意，plane distance必须在其选择的摄像头内的clipping planes设置中near和far范围内才能被渲染。

使用Screen Space - Camera时，canvas可以被场景中的任何一个摄像机渲染。
要使一个canvas及其内的元素与场景中其他摄像机隔离的话，调整摄像机的clear flags，culling mask和depth等参数。

World Space渲染模式下，UI元素可以在场景内部被渲染。这些UI元素可以是场景中的静态对象，也可以是移动对象，比如对话气泡，或者场景中跟随游戏对象的角色名签。
首先需要注意的是World space下的canvas，不会再直接驱动rect transform了，canvas可以放置在世界范围内的任意位置，canvas可以放置在世界范围内的任意位置，因为场景中可以有多个canvas，如果需要的话完全可以为world space UI元素创建新的canvas，需要设置Event Camera来接收事件，并且确定哪一个摄像机来检测事件，如UI点击事件，大多数情况下，选择 screen space warld时，这个event camera会这职位渲染整个场景的摄像机。

receives Events 指示是否canvas上的UI元素会接收到点击或者悬浮等事件。不选择这个选项时，canvas的所有子节点都将忽略事件，Sorting Order 和 Order in LayerSorting 用来控制canvas的渲染顺序。顺序指与场景中的其它渲染器比较，Sorting Order和Order in layer会在canvas组件上显示，只能在选择screen space - camra或者world space时设置。

一个canvas内的UI元素按照从上到下的顺序渲染，意思是第一个元素被渲染在最后。要改变UI元素顺序的话，直接修改层次中的顺序就行了。

**Rect Transform**
Rect Transform是一个新的组件，用来在元素上代替常见的transform组件，鉴于transform组件代表了3D对象在场景内的位置，旋转以及播放。
Rect Transform代表了一个2D矩形，其形状由与轴心点关联的宽和高来定义。不过组件内还是包含了一个z轴位置，同时也有旋转和缩放设置，所以这个元素可以在场景中当成3d对象来操作。

rect transform与传统的transform相比，一个重要的区别在于锚点的概念，一个rect transform可以锚定到期父节点上，如果这个父节点也拥有一个rect transform组件的话，锚定之后，UI元素可以基于父节点UI上的rect transform的位置以及大小进行拉伸和移动。

canvas自身也有一个rect transform，那么几乎所有情况下，一个UI元素的父结点都会有一个rect transform，某种意义上UI元素最终都会锚定到其父结点上。

在场景视图中，操作UI元素时，最好使用Rect工具，Rect工具可以通过点击