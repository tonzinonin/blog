---
title: unity与脚本-3
date: 2020-02-19 21:13:03
categories:
 - Unity
 - C#编程语言
tags:
 - Unity 2d
 - C#
 - Unity工具
---

**GetComponent函数**
在Unity中，脚本可视为自定义的组件，你会经常需要获取在同一个甚至其他对象上的脚本。
通过GetComponent可以获取到其他的脚本和组件。
GetComponent<Type>();
GetComponent函数的调用方式与我们之前见到的不同，我们在小括号前添加一组尖括号，这组尖括号用来输入一个类型作为参数。

也可以用GetComponent提取其它游戏对象上的组件，只要我们有对这个对象的引用，像OtherGameObject。

GetComponent 会返回一个任意指定类型的，在所调用游戏对象上的组件。
在GetComponent常用于获取其他脚本的同时，它也可以用来获取到没有被API暴露出来的组件。
GetComponent是比较耗费处理能力的，应该尽可能少的调用。较好的用法是在Awake或Start函数中使用，或只在第一次需要时调用一次。

**DeltaTime变量**
Delta这个术语意思是两个值的差别。Time类的deltaTime属性本质是两次update或者fixedupdate的间隔。这可以用来平滑运动过程中使用的数值，以及其它增量的计算。

帧间的时间间隔是不定的，那么如果你在每一帧用固定距离移动物体，最终结果可能不会很平滑，这是因为每一帧完成所花费的时间都不同。尽管移动距离是固定的，如果你用Time.deltaTime来修改移动变量，那么处理时间长的帧移动距离就大一些，处理时间短的帧变化就小一些。最终结果就是一段时间内的变化会显得平滑。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class UsingDeltaTime : MonoBehaviour
	{   //一个控制3d物体运动和光照的脚本
	    public float speed = 8f;
	    public float countdown = 3.0f;
	
	    void Update()
	    {
	        countdown -= Time.deltaTime;
	        if (countdown <= 0.0f)//当倒计时等于或小于0秒时，灯光效果打开
	            light.enabled = true;
	
	        if (Input.GetKey(KeyCode.RightArrow))//当右箭头按下，物体在x轴上移动，增加的量是定义的speed的值
	            //再乘以Time.deltaTime，使运动平滑，速度恒定
	            //这么使用Time.deltatime的效果就是使变化值以秒为单位，而不是以帧为单位。
	            transform.position += new Vector(speed * Time.deltaTime, 0.0f, 0.0f);
	    }
	}

**Datatypes(数据类型)**
基本上所有的变量都有数据类型。
两个基本数据类型分别是值类型(value)和引用类型(reference)。
像 integer，float，double，bool，char 等都是值类型。
此外，还有struct类型的稍复杂的变量，struct是值类型的，内部包含了其他一个或多个变量，两个最常用的struct是vector和quaternion。

引用类型的列表要简单一些，基本上属于某个类(class)的对象的变量，都是引用类型的，unity两个最常用的类，是transform和GameObject。

简单来说，两者的区别在于，值类型的变量里真的保存着一些值，而所有引用类型的变量，只保存了一个值所在的内存地址。
结果就是如果值类型的变量改变了，只有这个特定的变量受影响。如果一个引用类型的变量改变了，那么所有保存这个内容地址的变量都受到影响。

下面以移动一个3d物体为例

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class datatype : MonoBehaviour
	{
	    void Start()
	    {
	        Vector3 pos = transform.position;//声明一个vector类型的变量，保存物体的位置
	        pos = new Vector3(0 , 2 , 0);//pos保存了一个新的向量
	    }
	}
运行游戏，发现物体的位置没有改变，因为改变的只是pos，position并没有被修改。

另一种方式：
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class datatype : MonoBehaviour
	{
	    void Start()
	    {
	        Transform tran = transform;//创建一个transform类型的变量tran。然后设置其等于对象的transform
	        tran.position = new Vector3(0, 2, 0);//把tran的位置设置为新创建的(0,2,0)
	    }
	}
因为transform是一个类，我们知道tran是一个引用类型，此时运行游戏，游戏对象的位置被修改了。

**类(class)的使用**
在Unity中，每个脚本都包含了一个类的定义。
如果变量是盒子，函数是机器，那么类就是这些机器和盒子所在的工厂。
当你创建一个新的c#脚本的时候，unity会自动帮你定义好类，在javascript中这是隐含的，也就是说你不会看到这行定义。但unity还是把所有脚本内容当做一个类来看待。
这里的类必须和脚本名称相同，这点非常重要，最好不要修改脚本中类的名字。

类是变量和函数的容器，与其它方式相比，它提供了一个友好的方式来把在一起工作的东西整理起来，他们在被称为面向对象编程，或者简称OOP的领域里，属于组织形工具。
OOP的核心理念就是把单个脚本切分成多个脚本，每个脚本承担单一角色，因此类应该专注于单一任务。
可以把功能分成清单，放入不同的类中，这可以使脚本更容易管理，更加易读，写起来更加效率。

	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class Inventory : MonoBehaviour
	{   //定义一个游戏角色库存的类
	    public class Stuff//定义一个子类stuff，它包括3个整型变量
	    {
	        public int bullets;
	        public int grenades;
	        public int rockets;
	    }
	
	    //Creating an Instance (an Object) of the Stuff class;
	    public Stuff myStuff = new Stuff();
		//创建这个类的实例，一般称作为对象，给他一个数据类型(这里实际上就是类名)，然后起一个名字(newStuff)，KeyWord new，再一次类名

	    void Start()
	    {
	        Debug.Log(myStuff.bullets);
	    }
	}

类名后面的括号，暗示这里用了构造函数，你可以使用构造函数来给类中的变量设置初始值，当然，他们也有很多用处。
比如，我们可以设置一个默认构造函数，可以在里面给每个对象设置初始值，这意味着，他们不会初始化为0，而是你设置的数值，我们还可以写自己的构造函数，来设置这些变量。
这意味着当我们创建一个对象时，或者类的实例时，我们可以用括号来定义这些初始值是什么，

	    public class Stuff
	    {
	        public int bullets;
	        public int grenades;
	        public int rockets;
	
	        public Stuff(int bul, int gre, int roc)//可以用来定义在类的实例中这些变量的值。
	        {
	            bullets = bul;
	            grenades = gre;
	            rocket = roc;
	        }
	
	        //constructor
			public Stuff()//初始化
	        {
	            bullets = 1;
	            grenades = 1;
	            rocket = 1;
	        }
	    }

比如 
	public Stuff myStuff = new Stuff(50,3,1);
表示有50发子弹，3发手榴弹，1个火箭。

首先，构造函数名称必须是类名，构造函数没有返回值类型，void都没有。
一个类可以有多个不同的构造函数，但是对象初始化时只会有一个会被调用。
比如，我们第一个构造函数设置3个整型数的值，但是如果我们有更多的数值，比如添加这样一个变量：
	public float fuel;
现在我们第一个构造函数不会设置它的值，但是我们可以写另外一个构造函数来处理这件事。
	public Stuff(int bul,float fu){
		bullets = bul;
		fuel = fu;
	} 
在创建实例时，我们可以通过传入不同的参数来使用不同的构造函数。
	public Stuff myOtherStuff = new Stuff(50, 1.5f)
现在，这个实例就会使用新的构造函数，因为参数是匹配的。

**Instantiate函数**
Instantiate函数用来创建游戏对象的克隆体，通常这是指克隆一个预制体，预制体是一个预先配置好的对象。
保存在项目的素材中。
Instantiate最基本的形态只传入一个参数，就是我们需要克隆的对象。
用法1：
Instantiate(GameObject or Prefab to create an instance or 'clone' of);

代码：
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class instantiate : MonoBehaviour
	{
	    public Rigidbody Prefab;
	    void Update()
	    {
	        if(Input.GetButtonDown("Fire1"))
	        {
	            Instantiate(Prefab);
	        }
	    }
	}
把这个脚本传入instantiate函数，但这样预制体就会在默认位置生成，也就是坐标0点世界中心，也是预制体的中心。

用法2：
instantiate(Prefab to clone,Position,Rotation to use)
对应需要生成的对象，还有新克隆对象的位置，旋转。
Quaternion.identity不旋转

可以创建一个空白的游戏对象，定义位置，我们可以在脚本中新创建的参数BarrelEnd中用到它，我们可以使用这个组件的位置会旋转，作为新克隆预制体的位置和旋转值。
如果再对复制体施加力的作用，可以产生抛物线的效果。
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class instantiate : MonoBehaviour
	{
	    public Rigidbody rocketPrefab;
	    public Transform barrelEnd;
	
	    void Update()
	    {
	        if(Input.GetButtonDown("Fire1"))
	        {
	            Rigidbody rocketInstance;
	            rocketInstance = Instantiate(rocketPrefab, barrelEnd.position, barrelEnd.rotation) as Rigidbody;
	            rocketInstance.AddForce(barrelEnd.forward * 500);
	        }
	    }
	}
值得注意的是，你复制的对象在场景中不会消失，这时最好考虑写一个脚本清除多余的对象。

**数组**
数组是保存一组同类型数据的一种方式。C#数组的声明方式类似于C++；
type[] nameofArray = new type[number of elements];
访问并且赋值元素时：
nameOfArray[index of element to access] = value;

直接初始化赋值
type[] nameOfArray = {value,value,value...};

如果把数组设置为public，就可以在检视视图里看到，可以在那里保存数据。
比如
	public GameObject[] player

unity里有一些函数可以帮助我们处理这个。
我们想要数组保存场景中所有的特定对象，FindGameObjectsWithTag函数会返回场景中所有带特定标签的对象，它有一个优点，就是很方便循环，假如我们要记录所有场景中player的名字，我们可以用for循环来遍历数组的每个元素。

数组有一个长度属性，用Length函数可以返回数组中的元素数量。

**invoke函数**
invoke函数允许你安排一个延时一定时间调用的函数，这使得我们可以构造一个非常有用的，有时效性的调用系统。
在start函数中我们调用invoke函数，它需要两个参数，一个代表要调用的函数名的字符串，和一个要延时的时长，单位是秒。

需要注意的是，只有不需要传入参数，而且返回类型是void的函数才能用 invoke调用。

invokeRepeating函数可以反复调用一个函数。
这个函数有3个参数，第一个是函数名字符串，第二个是延时数秒，第三个后续调用之间的间隔。

如果要停止invokeRepeating函数的调用，可以使用cancelinvoke()函数，括号里填上需要停止调用的函数名。

**枚举**
在unity中写脚本时，有时我们需要的变量，其取值范围只在一组常数中。
枚举Enumeration是一种特殊的数据类型，含有一组指定的可取值。

枚举可以创建在类里面或者外面，如果只有这个类需要访问这个枚举，就把他定义在类里，如果还有其它类需要访问，就放在类的外面。

枚举声明的语法是，先写enum关键字，接着写上名字，这里我们叫 Direction,注意这里我们最后首字母大写，因为我们实际声明的是一个类型，而不是变量。然后用双括号，在双括号里列举出想要使用的常数，语句的结尾要打分号。
每个在这里定义的常数都有一个数值，默认情况下从0开始，然后按照序列递增。
如果需要的话，这里的值类型以及具体取值都可以被覆盖，常数的类型可以修改成任何类型的整数。
修改类型时，在名称后加上冒号，再加上类型。
修改枚举类型的一个原因是为了优化，但一般情况不需要担心这个。

声明完枚举，就可以创建枚举类型的变量了。
枚举类型，也可以被作为入参或者返回值。我们只要使用枚举类型的名字，在枚举类型的函数下只能取枚举类型的值。

**switch语句**
switch语句类似多分枝化的 if else 语句，常用于枚举类型的判断。
和c++类似，这里不再赘述。

**OnTriggerEnter**
该函数可以检测碰撞体
