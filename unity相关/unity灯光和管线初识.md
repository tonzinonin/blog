---
title: unity灯光和管线初识
date: 2020-02-13 19:55:16
categories:
 - Unity
tags:
 - Unity 2d
 - Unity工具
---

###2D灯光的简单实现

对灯光照射的SpriteRenderer添加光照材质，一般来说unity自带Default-diffuse材质就行，之后该贴图会变暗。也可以在资源栏建立材质预制体，在shader选项添加sprite/diffuse就行
之后右键hierarchy可添加light对象。
directional light：
添加之后可以调整光的颜色，打光方向，本身是3D光源，需要在3D的sence里调整。
point light：
点光源，控制range来调整在场景中的散射直径(在2D画面可以通过调整z轴，不过效果有别)，intensity调整光源的强度。

###SRP(ScriptableRenderPipeline)
可编程渲染管线，是Unity提供的模块化渲染系统，如果你是通信工程师，你可以使用C#脚本定制unity的渲染过程。

###Unity中的管线:URP与HDRP
//该处笔记相对比较有时代局限性

HDRP针对的是高端硬件设备，如PC,XBOX,PS4。面向高逼真度的游戏，图形demo，建筑渲染，以及所有需要最佳图形效果的东西。
在针对高端图形时，它比内置的渲染器要快得多。
用HDRP做逼真风格的VR游戏对性能要求很高，因为他要给每个眼睛分别渲染一个画面。
延迟渲染和Decal(又名贴花，是一种方便的堆叠纹理的技术)目前只有HDRP支持。
HDRP提供高级的多的光照功能，比如实时全局光照(RealTimeGlobalIllumination)能够模拟光线反射，体积光(VolumetricLighting)能够模拟光穿过空气中的粒子，以及光线追踪(RayTracing)等。
Shader方面，HDRP提供一些高级的shader特性，比如高度，细节，视差纹理，分别用于纹理的位移，细节，以及深度模拟。且支持子面散射(SubsurfaceScattering)用于模拟光线穿过很薄的物体，比如皮肤和衣物。高级Shader，比如stacklit，能够让你同时使用多个材质的特性，比如子面散射，彩虹色，各向异性已及模糊参数化等。
对于处理后效果，环境光遮蔽(AmbientOcclusion),就是在两个相交的平面加上阴影，自动曝光(AutoExposure)模拟人眼对不同光线条件的能力，屏幕空间反射(SpaceReflections),模拟屏幕上可见物体的散射。

URP以前叫作LWRP(LightWeigh RP)，被设计为在如何平台上提供最好性能，所以除非有特定功能只在HDRP，或者自定义渲染管线能提供。你都应该选择URP。
2D光照和阴影为URP独占的。

两个管线都支持一些特性：相机堆叠(CameraStacking)，能让你同时用多个相机渲染。物理相机等。

截止到目前(我写这篇文章的时候)shader graph和visual graph两个管线都支持，这两个是创作shader和粒子特效很棒的工具。

###URP实现2D灯光

使用URP创建一个管线资源pipeline asset
会生成一个渲染描述文件和一个管道文件，这时通用渲染软件同时渲染在3D和2D场景中。在创建一个2D Renderer并将其添加到之前管道文件中的RendererList
edit-projectSettings-graphics添加管线资源。
然后到Edit-rendererPipeline中将素材升级到2d灯光渲染效果(可以选择场景升级或者整个项目升级)。

升级之后，场景通常会变成一片黑，可能有部分特殊的素材没有变化。也可能场景中的素材变成了粉红色，这代表我们的素材没有升级到URP，需要到material调整到Sprite-Lit-Default。
也可以自己创建material，安装URP中的2Dshader。

之后可以在hierarchy窗口创建2DLight了。
一般这里的light只会渲染在对应的图层上(TargetSortingLayer), 可以用SortingGroup来更改。
1. 点光源2D: 和PointLight类似，不过对2D效果进行了巨大的优化，我们可以直接拖拽调整点光源的范围大小，检视窗口也可以改变其光照锐度，角度(可以用来模拟手电筒)，给灯光添加贴图(cookie)等。
2. GlobalLight2D: 可以照亮所有的选定的图层
3. ShapeLight2D: 可以将贴图作为光源
4. FreeFormLight2D：可以编辑灯光的样式，在使用这种灯光之前要将位置调整好，不然投影会出现各种问题。AlphaBlend可以混合场景的灯光
5. parametricLight2D: 可以制作多边形灯光。

选中(多个)灯光可以在检视栏勾选use normal map，这将允许你使用法线贴图。

后处理效果postprocessing：
在hierarchy窗口里创建volume
创建一个profile，之后Add_Override比如Bloom(炫光的效果)，然后在相机里启动postprocessing。