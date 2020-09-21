---
title: unity-2d角色控制器
date: 2020-02-17 10:17:31
categories:
 - Unity
tags:
 - Unity 2d
 - Unity工具
 - Unity功能实现
---

**有关2d的移动控制**
1.直接修改transform
--transform.translate --按输入vector3的方向和大小移动，可以选择输入参照的坐标系。

--vector3
 --vector3.lerp -在两点间计算线性插值，可以用结果修改position，直线移动
 --vector3.slerp -在两点间计算球形插值，用结果修改position，弧线移动
 --vector3.movetowards -输入两点，及限制最大距离，返回一个不超过最大距离的点，直接移动。
 --vector3.SmoothDamp -平滑的从A点逐渐移动到B点，可以设置移动总时间，最大速度，常见的用法是相机跟随目标。

2.修改rigidbody
--velocity -速度
--AddForce -施加力
--MovePosition -移动位置
--使用时应该在fixedUpdate，因为是按照物理引擎更新进度更新的，UpdatehefixedUpdate不是一个节奏，而且需要注意描述里写的条件和影响结果。

3.CharacterController并不适用于2D

**地面检测**
建立空对象，放在游戏对象下，提供游戏对象与地面接触的坐标，可用来检测游戏对象的脚下是不是地面。
linecast，可以通过划线的方式检测地面，详情可以见官方文档。
还有一种方法，overlap覆盖检测

如果用垂直速度来控制跳跃动画的播放，可以优化跳跃动画。
把动画过渡间的过度值去掉，可以减少动画播放的延迟。

**一些UI(待移除条目)**
可以在hierarchy菜单下创建，这个操作将会为我们自动做一些事。
创建Canvas对象，ui成为它的子对象，我们制作的任何ui对象都需要canvas的子对象才能被渲染。
他还创建了一个EventSystem，如果我们需要与按钮或滑块等ui交互，我们需要它。

在锚点预设按住alt键可以编辑ui的位置。

shadow组件可以在对象上添加阴影。

**2d平台游戏角色的移动脚本**
我们将会通过给刚体施加力的方式实现角色在地面上的移动，并且在角色转向的同时巧妙利用缩放信息"Scale"实现角色的sprite水平翻转。利用LayerMask类型来获取游戏对象的图层信息，用Physics2D.OverlapCircle来进行地面检测，来判断角色是否可以跳跃。且添加2段跳的功能。

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class animation : MonoBehaviour
	{   
	    public float maxSpeed = 6f; //设置最大速度
	    bool isfacingright = true;//角色朝向默认设为true
	    Animator anim;
	    Rigidbody2D rbody2D;
	    bool grounded = true; //定义一个判断器来检测角色是不是在地面上。
	    public Transform GroundChecker;//地板判断器的坐标
	    float checkredius = 0.2f;//检测半径
	    public LayerMask whatisground; //决定在哪些层上检测地面
	    public float jumpforce = 700f; //跳跃的力
	
	    //bool doublejump = false;//多段跳的条件
	
	    void Start() {
	        anim = GetComponent<Animator>();//获取animator组件。
	        rbody2D = GetComponent<Rigidbody2D>();//获取刚体组件
	    }
	
	    public void FixedUpdate() {
	        anim.SetFloat("Vspeed", rbody2D.velocity.y);//将Vspeed设置为垂直方向上的速度，这个可以用来做跳跃的动画。
	        grounded = Physics2D.OverlapCircle(GroundChecker.position,checkredius,whatisground);//检测地面碰撞。
	                                                       //GroundChecke为圆心，checkredius为半径进行地面检测。
	        anim.SetBool("grounded",grounded);//设置一个变量，动画控制器里的变量，把它的值和grounded返回的值一样。如果不需要anim这段代码可以忽视
	
	        //doublejump = false;
	
	        float direction = Input.GetAxis("Horizontal");//接收水平方向上的输入，<-取负值,->取正值。
	        anim.SetFloat("Speed", Mathf.Abs(direction * maxSpeed));//设置Speed参数的值，并且对它取绝对值，
	        rbody2D.velocity = new Vector2(direction * maxSpeed, rbody2D.velocity.y);//改变刚体物件的速度
	
	        if (direction > 0 && isfacingright == false)//检测翻转
	        {
	            Flip();
	        }
	        else if (direction < 0 && isfacingright == true)
	        {
	            Flip();
	        }
	    }
	 
	    void Update() {
	        //实现跳跃和多段跳
	        if ( (grounded || !doublejump) && Input.GetKeyDown(KeyCode.Space)) {
	            grounded = false;
	            rbody2D.AddForce(new Vector2(0, jumpforce));//在y轴上添加一个力
	
	            /*if (!grounded && !doublejump)
	            {
	                doublejump = true;
	            }*/
	        }
	    }
	
	    void Flip() {//实现翻转
	        isfacingright = !isfacingright;//对当前朝向取反
	        Vector3 scale = transform.localScale;//角色相对坐标
	        scale.x *= -1;//对坐标取反
	        transform.localScale = scale; 
	    }
	}

**2D弹幕射击游戏的简单控制**
获取horizontal(水平)和Vertical(垂直)方向来控制角色运动

	private void Moving()
		{
			int horizontal = 0;
			int vertical = 0;

			horizontal = (int)(Input.GetAxisRaw("Horizontal"));
			vertical = (int)(Input.GetAxisRaw("Vertical"));
			rb2d.velocity = new Vector2(horizontal * moveSpeed, vertical * moveSpeed);
		}
