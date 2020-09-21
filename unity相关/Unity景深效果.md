---
title: Unity景深效果
date: 2020-09-13 23:04:04
categories:
 - Unity
tags:
 - Unity 2d
 - Unity功能实现
---
在unity中实现景深效果

让背景，中景，前景等的图片以不同的速度移动。

将下面的脚本放置到对象(背景)上，把cam设为camera来获取camera的坐标，然后进行坐标的操作。



    public class Parallax : MonoBehaviour
    {

        public Transform cam; //获取摄像机的位置
        public float moveRate; //跟随的幅度
        public bool lockY = false; //是否锁定Y轴

        private float startPointX, startPointY;

        void Start()
        {
            startPointX = transform.position.x;
            startPointY = transform.position.y;
        }
        
        void Update()
        {
            if (lockY)
            {
                transform.position = new Vector2(startPointX + cam.position.x * moveRate, transform.position.y);
            }
            else
            {
                transform.position = new Vector2(startPointX + cam.position.x * moveRate, startPointY + cam.position.y);
            }
        }
    }