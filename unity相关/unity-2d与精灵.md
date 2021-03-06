---
title: unity与sprite类型
date: 2020-02-16 16:39:44
categories:
 - Unity
tags:
 - Unity 2d
 - Unity工具
---
**精灵类型（sprite type）**
以精灵类型导入图片素材时，材质需要设置为sprite。如果项目已经被设置为2D，素材导入器会使用Sprite作为默认类型导入新的图片素材。如果图片素材导入时没有自动设置为sprite类型，可以手动在导入器中修改。
精灵类型有三个选项，单个，多个和图形。
当导入的图片素材只包含一个元素时选择单个，包含多个元素时选择多个。
打包标记：对素材打包进行标记，以后再补充
还可以调节一些参数：比如pixels to units（每单位像素数）决定了素材导入后的比例。
轴心：代表物体的中心，选中多个模式时，轴心设置选项为不可用。
需要对每个sprite在sprite editor中单独设置，点击sprite editor按钮，将用编辑器打开这个素材。
精灵类型素材不需要改动filter mode和per platform overrides（？）选项。
需要注意，图集和精灵表单的规格，可能超出图片素材默认的最大值2048，如果超出最大值，在导入过程中将会被调整为最大值。

**精灵渲染器（sprite renderer）**
用来显示以精灵类型导入的图片素材，以便在2d和3d场景中使用。
这是一个精灵到渲染器的应用。

颜色：可以改变sprite的颜色，colour也可以通过调整颜色的alpha通道值，控制sprite的透明度，。

材质：默认情况下，新加入的精灵渲染器组件，使用的是默认材质，用默认材质渲染的精灵将不会被场景中的光照影响，将简单的最大亮度来渲染这个精灵。要使用光照的话，所选的材质必须需带有能够响应光照的shade（着色器）。
sprites diffuse类型专门为使用在精灵素材上优化过，当然也可以使用其它类型的shader，注意有一些shader是跟精灵渲染器不兼容的。
精灵使用标准的unity材质，但是精灵渲染器对材质的处理与mesh renderer（网格渲染器是不同的），主要区别是，不需要材质的纹理属性，精灵渲染器会使用一个兼容的材质中的shader，颜色以及其它属性，但不使用纹理属性。
如果所使用的材质中有一个调色（tint colour）属性，那么他将会与精灵的calour属性组合起来。
光照和材质会影响游戏性能，所以在使用光照和更加复杂的着色器时，最好找到场景中光照效果和性能之间的平衡点。

对sprite进行翻转，更改绘制模式。

sorting layer（排序图层）和order in layer（图层顺序）是系统决定sprite在哪层被绘制出来的参数。
所有的精灵渲染器，在创建时都会被放置在一个默认的sorting layer。

**精灵编辑器（sprite editor）**
用来处理含有多个元素的图片，如图集或精灵表单。
只有精灵模式设置为多个时才能使用编辑器，设置为单个时无法使用编辑器。
点击sprite editor进入精灵编辑器的界面，编辑器是一个标准的unity窗口。
在编辑器内点击并圈入图中想要的元素，就可以创建出精灵，可以对边框进行自由变换的操作。
选中一个精灵时，编辑器窗口右下角出现一个精灵面板，显示出选中的精灵的详细信息。

精灵编辑器顶部也有一些工具，讲一些主要的，切割菜单用来从图片中切割元素，自动创建精灵，切割类型可以是自动或者网格，自动适合切割单个的不规则的元素，比如一个图集，选择自动切割时，minimum size值决定了切割每个元素的最小大小，如果这个值设置的太大了，相互临近的小一些的元素会被合并在一起，形成一个大的精灵。
pivot（轴心）设置了所有精灵，从图片被切出来时的默认轴心点。
RGB Alpha按钮用来切换编辑器中的图片，以颜色模式还是以alpha通道模式来显示。
以上的步骤都完成后，点击slice按钮切割图片，生成单独的精灵并应用。
之后点击精灵旁边的三角符号可以看到它的子精灵“chidren”

**排序图层（sorting layers）**
制作一个2d游戏时，必须要精确的控制物体的渲染顺序。
一种方法是控制精灵到摄像头之间的距离。
另一种方法是sorting layers。
排序图层与摄像头的位置无关，各排序图层之间被指定一定的顺序，在排序图层内的物体将按这一个顺序进行渲染。
在摄像头视野范围内精灵才会被渲染，如果需要更精确的控制，可以给单个精灵添加order in layer（图层顺序）值。

有两种方式进入tags和layers设置面板。
从精灵渲染器组件选择sorting layer - add sorting layer或在编辑器内选择Edit - project setting - tags and layers。
在打开的面板中，可以对sorting layer 添加删除或者重新排序。
列表中有一个default layer，它不能被重命名或者删除，但是可以重新排序。
点击+，+一个图层。
点击名称区域可重命名图层
sorting layer列表顺序是从顶部到底部。
最先渲染的在后面。之后的层再前一层之上渲染。

将一个精灵添加到某个sorting layer的操作是，选中一个精灵，在精灵渲染器组件里，使用sorting layer下拉菜单，选择一个sorting layer。

需要注意，sorting layer和order in layer的参赛是全局的，在unity内所有的渲染器中都可以获取。
每个渲染器，包括粒子渲染器（particle renderers）都能指定一个sorting layer和order in layer
尽管只有精灵渲染器在检视视图里提供了这些参数，其他渲染器需要在脚本中使用这些参数。
设置渲染器的sorting layer时，把renderer.sortingLayerName参数设置为某个sorting layer的名称或者ID。
设置order in layer使用renderer.sortingorder参数。
