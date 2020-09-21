---
title: Unity与脚本-2
date: 2020-02-19 20:43:59
categories:
 - Unity
 - C#编程语言
tags:
 - Unity 2d
 - C#
 - Unity工具
---

**向量计算**
在unity中，会用到向量的知识，包括2d向量和3d向量。
简单说一下3d向量：unity中z代表深度，y代表高度，x代表长短。(笛卡尔坐标系)。
3d向量的大小计算方式和2d相同 x2+y2+z2=m2。
unity提供了简化这种计算的帮助函数。
Vector3.magnitude（Vector3代表3d向量，Vector2代表2d向量）
具体使用方法可以查阅文档。
3d向量还有很多有用的函数，比如点乘积(Dot Product)和叉乘积(Cross Product)。

点乘积输入两个向量，产生一个标量，即为一个单纯的数字。
要计算2个向量的点乘积，将他们的组成分解，得到xyz的值，然后分别乘在一起，并且计算出他们乘积的和。(Ax*Bx)+(Ay*by)+(Az*Bz) = Dot Product
用这个方法可以计算出向量之间的一些关系，比如算出两个向量是否相互垂直，可以用这种方法制作一个飞行模拟器。
帮助函数：
Vector3.Dot(VectorA,VectorB)

叉乘积是另外一种组合两个向量的方式，产生的结果不是标量，叉乘积的结果是另外一个向量，这个新的向量与原来的两个向量都垂直，数学计算中，其表示方式是 A^B = C。
Ay*Bz - Az*By = Cx
Az*Bx - Ax*Bz = Cy
AX*By - Ay*Bx = Cz
帮助函数:
Vector3.Cross(VectorA,VectorB)

**组件启用与禁用**
在unity库启用或禁用某个组件，只需要使用enable标志位。
下面来一个案例，来解释enable的用法。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class aboutre : MonoBehaviour {
	    //控制灯光开关
	    private Light myLight;//light组件的引用
	
	    void start() 
	    {
	        myLight = GetComponent<Light>();//用GetComponent设置这个变量，将mylight设置为游戏对象上的Light组件。
	    }
	
	    void Update()
	    {
	        if (Input.GetKeyUp(KeyCode.Space))//等待空格键放下
	        {
	            myLight.enabled = !myLight.enabled;//修改enabled标志位，将其设置为myLight.enable的相反。
	        }
	    }
	}

**游戏对象激活控制**
在脚本中要激活或者停用某个对象时，可以通过使用SetActive函数。
这可以打开或关闭某个游戏对象，根据其在场景的激活状态。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class aboutre : MonoBehaviour {
	    void start() {
	        gameObject.SetActive(false);
	    }//用这个来调整游戏对象的激活状态
	}
但是在处理多层次对象时，父节点被停用，子节点也在场景中停用了，但是他其所在层次中依然是激活状态的。以便你停用单个的对象，但仍然能通过父节点，保持其对群组的控制。
要检查一个对象的激活状态是自己激活函数在层次中激活，你可以通过activeself 和 activeInHierarchy 的状态。
如果一个子节点是未激活的，因为其父节点未激活。这时使用setActive设置为true，无法使这个结点返回激活状态。

**Translate和Rotate函数**
Translate和Rotate是常用来修改游戏对象位置和旋转的函数。
移动
做移动旋转操作时，会乘以Time.DeltaTime。
这意味着移动速度是 米/秒 而不是 米/帧。
transform.Translate(amount in each axis to move by);
transform.Translate(Vector3/Vector2)
旋转
transform.Rotate(Axis around which to rotate,amount to rotate by)
transform.Rotate(Vector3,float)

rotate vector3的方向有up，down，right，left，back，back, forward等，也可以自定义坐标。

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class NewBehaviourScript : MonoBehaviour
	{
	    public bool control = true;//一个控制物体向上移动和逆时针旋转的脚本。
	    public bool MoveRotate = false;
	    public float moveSpeed = 10f;
	    public float turnSpeed = 10f;
	    void Update()
	    {
	        if(control)
	            transform.Translate(new Vector2(0 , 1) * moveSpeed * Time.deltaTime);
	        if(MoveRotate)
	            transform.Rotate(Vector3.back, -turnSpeed * Time.deltaTime);
	        if (Input.GetKeyUp(KeyCode.Space))
	            control = !control;
	        if (Input.GetKeyUp(KeyCode.R))
	            MoveRotate = !MoveRotate;
	    }
	}

一般2d用到back（逆时针）和forward（顺时针）就够了。

要注意这些函数都在本地坐标系工作，而不是世界坐标系，所有如果我们使用Vector3.forward或者Vector3.up（包括vector2），都是相对于其应用的坐标系的。
如果要移动带碰撞体的物体，那么有时候它会发生物理交互，这时候就不应该用translatehe和rotate，你应该使用物理类的函数，如果你想移动这个物体，唯一的例外是你有一个运动学的刚体(在rigidbody上选择isKinematic)

**LookAt函数**
LookAt函数可以用来使游戏对象的前方向。指向世界里的另外一个位置。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class on_off : MonoBehaviour
	{
	    public Transform target;//对想要创建的对象建立引用
	
	    void Update() {
	        transform.LookAt(target);//告诉我们的对象要看向哪里
	    }
	}
然后将对象拖拽到要跟踪的变量上，现在点击播放，camera会一直面向这个对象了。
你可以在顶部切换世界或者本地坐标展示(Global)

**Destroy函数**
Destroy函数可以在运行时移除游戏对象，或者对象上的组件。
也可以在移除前加延时。
Destroy(GameObject/Component, optional delay)
如果要删除一个游戏对象。
比如我们可以直接引用脚本所在的这个对象，用Destroy函数直接删除。
有时需要这个脚本做其它的事，但是删除对象的同时脚本也会同时被删，那么，你应该使用对另外一个对象的引用。

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class Destroy : MonoBehaviour
	{
	
	    public GameObject other;//建立一个public的变量other，来引用其他对象。
	
	    void Update() {
	        if(Input.GetKey (KeyCode.Space)){
	            Destroy (other, 3f);//3s后删除对象
	            //Destroy (GetComponent <MeshRenderer>() ) //删除网格渲染器组件
	        }
	    }
	}
然后在检视视图，我们拖拽一个游戏对象到脚本。

**GetKey和GetButton**
在Unity中，GetKey 和 GetButton是获取键盘按键或者手柄按键的方式。
通过unity提供的Input类，两者的核心区别是，GetKey会明确的命名输入的按键。利用KeyCode，比如空格用KeyCode.Space表示。
推荐使用GetButton代替它，可以详细配置你的控制。
输入管理可以让你给输入取名，并且指明其匹配的按键。可以通过选择 Edit - Project Settings - Input，然后调用菜单的时候，可以通过字符串来引用名字。

positive button--官方文档。

当输入GetKey或GetButton时，这些输入有三个状态，会返回布尔值。

GetButtonDown:按下按钮的第一帧触发，返回一帧的true
GetButton:按下按钮的时候一直返回true
GetButtonUp:在按钮松开后的第一帧触发，返回一帧的True。

GetKeyDown，GetKey，GetKeyUp原理和这个相同。

**GetAxis函数**
工作方式类似GetButton和GetKey，但是有些根本不同。
GetButton和GetKey返回布尔值，但是GetAxis返回的是浮点数。在-1到1之间滑动的数。

在输入管理中设置轴。
处理按键按下时，只需要考虑positive button值。但是处理轴输入时，positive和negative都需要考虑。
还有gravity(数字在按键释放后返回0的速度), sensitivity(和gravity属性相对，它用来控制返回值到达1或-1的速度), dead, snap. 等参数。
如果我们使用摇杆来代替轴，可能不需要响应摇杆特别小的运动，可以使用dead来设置盲区，dead值越大，盲区就越大，摇杆就需要移动更远使GetAxis返回非0值。
snap选项允许你在positive和negative按钮同时按下0，用float值接受水平或者垂直方向的输入时，只需要使用
Input.GetAxis(''Horizontal'');
或者Input.GetAxis(''Vertical'');

也可以使用Input.GetAxisRaw只返回整形值没有中间值，这适用于需要准确控制而不是平滑值的2D游戏。

**OnMouseDown函数**
可以检测在碰撞体或者GUI元素上的点击。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class OnMouseDown : MonoBehaviour
	{
	    void OnMouseDown()
	    {
	        Debug.Log("Clicked on the GameObject");
	    }
	}
