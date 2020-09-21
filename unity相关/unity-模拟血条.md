---
title: unity 模拟简单血条
date: 2020-08-29 16:05:13
categories:
 - Unity
tags:
 - Unity 2d
 - Unity功能实现
---
需要用到gui的slider组件。

首先我们需要一个用来作为血条的sprite，当然你可以多加一个sprite作为血条的背景。
新建一个slider组件。

仅保留slider内的background组件，其余删除，看情况取舍。
更改imageType为Filled，
填充类型为horizontal。

将画好的图片拖到background的image中，然后将carvas的渲染模式改成WorldSpace。

调整大小，位置。

在background下添加脚本，如下void Update()
    {
        if (Input.GetKeyDown(KeyCode.J))
            GetComponent<Image>().fillAmount -= 0.1; //按下J键，血条减少百分之10(在value为1的情况);
    }
触发条件不一定是按键控制，按自己需要去弄