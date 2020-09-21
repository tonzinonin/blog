---
title: unity dialogSystem
date: 2020-08-28 13:19:25
categories:
 - Unity
tags:
 - Unity 2d
 - Unity功能实现
---
新建ui-panel对象
render mode - worldspace 
reset canvas
让ui界面移到地图中间。

event camera 决定哪个摄像机进行渲染
更改sortinglayer Scale location 让其达到自己想要的效果
更改图片

加入text与image

加入触发脚本到对象上，这里举几个常用函数：
OnTriggerEnter2D()
OnTriggerExit2D() 这两个函数需要在对象拥有Trigger Collision的情况下才有效。用途是在进入碰撞体时触发函数。

gameObject.SetActive(bool var) 用来控制对象的激活状态。
gameObject.ActiveSelf 用来获取对象激活状态的布尔值。

DialogSystem
[header("TEXT")] 这个方法可以用来在Inspector中建立可视化的头文字，可以用来注释public变量。

TextAsset 这一类型可以把Text文件使用到游戏项目当中，并且帮助转换其中的文本。（txt, html, xml等，不支持mobi, pdf等）
用一个方法.Text可以把整个文件转换为字符型的数据。
使用例：
		B
		仙贝，你怎么在这里
		A
		抱歉，NYN，我必须先干掉你才能去掘皮炎子。
		B
		看来战斗是不可避免了吗

建立一个Text列表

    public int index;

    List<string> textList = new List<string>();

.Split 方法可以用来切割文本。（共有6种方法）
如 .Split('\n'); 可以将文本按行切割。

	void GetTextFromFile(TextAsset file)
	    {
	        textList.Clear();
	        index = 0;
	
	        var lineData = file.text.Split('\n');//将文本按行切割 加到临时stringArray lineData当中。
	
	        foreach (var line in lineData)//遍历lineData的每一元素(文本中的每一行)
	        {
	            textList.Add(line);//将每一行加到列表当中
	        }
	    }

之后可以遍历文字到游戏中。
（ps.有时候直接使用外部txt会产生无法读取的错误，一般发生错误时txt的inspector窗口不会显示文字）

解决一开始对话栏显示为空的问题：
    private void OnEnable()
    {
        textLabel.text = textList[index];
        index++;
    }
这时要把start函数改为Awake函数

注意：
OnEnable函数会在Start之前进行调用
Awake函数会在OnEnble之前调用

startCoroutine协程
协程可以让你在两行代码中间加入执行一行代码，在代码没有执行完之前不会执行以后的代码，一般用IEnumerator方法来定义一个coroutine。
用StartCoroutine来调取一个协程。

WaitForSeconds(waitTime)函数用来等待一段时间
yield return 会把yield return后的程序暂时挂起只执行yield return后的代码。

读取关键字：
很简单，直接把文字的内容作为判断条件，比如在上面的使用例中，用 if(text == "A/r")来读取A关键字。

可以用来制作给对话自动添加人物头像的效果。
如果要更改GUI中的image,
如下实例:
Image.sprite = sprite01;