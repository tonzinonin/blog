---
title: unity 实现简单场景控制
date: 2020-08-29 16:04:49
categories:
 - Unity
tags:
 - Unity 2d
 - Unity功能实现
---

unity类里包含有很多场景控制的方法
一般要包含 scenemanage 头文件

一些简单的指令：
Application.Quit(); //退出游戏
SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex + 1); //移动到下标为buildIndex的场景，下标可以在buildSetting里设置。
SceneManager.LoadScene(1); //加载下标为1的场景

一些效果的实现方法
1. 加载游戏：
在触发loadScene函数的时候激发loadUI就行。
2. 实现全部游戏画面暂停：
 Time.timeScale = 0;    //其实就是时停嘛Kora！
