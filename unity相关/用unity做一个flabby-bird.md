---
title: 用unity做一个flabby bird
date: 2020-02-21 09:05:11
categories:
 - Unity
tags:
 - Unity 2d
 - Unity项目总结
---
 --通过移动世界而不是移动角色来模拟endless的效果。
 --在摄像机外回收并循环资源，节约性能。
 --障碍通过脚本对象池实现，性能更好。

1.GetMouseButtonDown()
button values are 0 for left button,
1 for right button,
2 for middle button.

2.OnCollisionEnter2D ()
如果附加了此脚本的游戏对象，与其它东西发生碰撞，则会调用此函数。

3.SetTrigger(“nameOfTrigger”,)
可以传入我们想要设置的触发器(多用于动画触发)

4.SetActive(ture or false)
可以触发或关闭组件。

5.重新加载场景。
using UnityEngine.SceneManagement;空间声明。
	void Update()
	    {
	        if (GameOver == true && Input.GetMouseButtonDown(0))
	        {
	            SceneManager.LoadScene (SceneManager.GetActiveScene ().buildIndex);//加载一个场景文件
	        }
	    }

6.调用UI
空间声明：
using UnityEngine.UI;

**7.对象池**
这是一种软件创建设计模式，使用一组准备好使用的初始化对象作为一个池，而不是实时的分配和销毁对象。
池的客服端从池中请求一个对象，并对返回的对象执行操作，当客服端完成操作后，它将对象返回到池中而不是销毁，它将手动或自动完成。
对象池主要用于性能，在某些情况下，对象池可以显著提高性能，这对于游戏开发来说是绝对实用的。
对象池使对象在周期内变得复杂，当从池中获取或返回对象时，实际上并未创建或销毁对象，因此在使用过程中需谨慎。
所以如果它是一个复杂的对象，你可能需要有某种复位函数，以确保一切都重置，当它回到对象池的时候。这全都取决于你的游戏和你的对象。

主功能：
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	using UnityEngine.SceneManagement;
	using UnityEngine.UI;//引入UI，记录分数
	
	public class GameControl : MonoBehaviour
	{
	    public static GameControl instance; //强制对象为单例模式,允许它从我们的任何其它脚本中访问
	    public GameObject GameOverText;
	    public bool GameOver = false;
	    public float scrollSpeed = -1.5f;
	    public Text scoreText;
		
	    private int score = 0;
		
	    void Awake()
	    {
	        if (instance == null)
	        {
	            instance = this; //当初始化时，确保没有其它gamecontrol
	        }
	        else if (instance != this)
	        {
	            Destroy (gameObject);//意味着破坏此脚本所附加的游戏对象
	        }
	    }
	
	    void Update()
	    {
	        if (GameOver == true && Input.GetMouseButtonDown(0))
	        {
	            SceneManager.LoadScene (SceneManager.GetActiveScene ().buildIndex);//加载一个场景文件
	        }
	    }
	
	    public void BirdScored ()
	    {
	        if (GameOver)
	        {
	            return;
	        }
	        score++;
	        scoreText.text = "Score: " + score.ToString ();
	    }
	
	    public void BirdDied()
	    {
	        GameOverText.SetActive (true);
	        GameOver = true;
	    }
	}

角色控制器
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class BirdController : MonoBehaviour
	{
	    public float addForce = 200f;
	
	    private Rigidbody2D rb2d;
	    private bool isDead = false;
	    private Animator anim;
	    void Start()
	    {
	        rb2d = GetComponent<Rigidbody2D>();
	        anim = GetComponent<Animator>();
	    }
	
	    void Update()
	    {
	        if (!isDead && Input.GetMouseButtonDown(0)) //鼠标左键。
	        {
	            rb2d.velocity = Vector2.zero; //重置速度为0，平衡力
	            rb2d.AddForce(new Vector2(0, addForce));
	            anim.SetTrigger("Flap");
	        }
	    }
	
	    void OnCollisionEnter2D() 
	    {
	        rb2d.velocity = Vector2.zero;
	        isDead = true;
	        anim.SetTrigger("Die");
	        GameControl.instance.BirdDied ();
	    }
	}

卷轴
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class ScrollingObject : MonoBehaviour
	{
	    private Rigidbody2D rb2d; 
	    void Start()
	    {
	        rb2d = GetComponent<Rigidbody2D>();
	        rb2d.velocity = new Vector2(GameControl.instance.scrollSpeed, 0);
	    }
	
	    // Update is called once per frame
	    void Update()
	    {
	        if (GameControl.instance.GameOver == true)
	        {
	            rb2d.velocity = Vector2.zero;
	        }
	    }
	}

复制实现卷轴循环
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class RepeatingBackground : MonoBehaviour
	{
	    private BoxCollider2D groundCollider;
	    private float groundHorizontalLength;
	
	    void Start()
	    {
	        groundCollider = GetComponent<BoxCollider2D>();//引入碰撞体
	        groundHorizontalLength = groundCollider.size.x;//获得x轴的大小
	    }
	     
	    void Update()
	    {
	        if (transform.position.x < -groundHorizontalLength)//检查是否需要重新定位
	        {
	            RepositionBackground ();
	        }
	    }
	
	    private void RepositionBackground()
	    {
	        Vector2 groundOffset = new Vector2(groundHorizontalLength * 2, 0);
	        //在重新定位它的时候我们向前迈进了多少距离。
	        transform.position = (Vector2) transform.position + groundOffset;//计算移动多远
	
	    }
	}

引入计分
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class Column : MonoBehaviour
	{
	    private void OnTriggerEnter2D (Collider2D other)
	    {
	        if (other.GetComponent<BirdController>() != null) ;//无论通过什么触发器，检查它是不是鸟
	        {
	            GameControl.instance.BirdScored ();
	        }
	    }
	}

对象池
	using System.Collections;
	using System.Collections.Generic;
	using UnityEngine;
	
	public class ColumnPool : MonoBehaviour
	{
	    public int columnPoolSize = 5;//表示数组初始长度的值。
	    public GameObject columnPrefab;//实例化的对象
	    public float spwanRate = 4f;//在玩家面前从对象池放置新柱子的时间间隔。
	    public float columnMin = -1f;
	    public float columnMax = 3.5f;//以垂直随机偏移量定位柱子
	
	    private GameObject[] columns;//柱子数组。
	    private Vector2 ObjectPoolPosition = new Vector2(-15f, -25f);//一个屏幕外的位置
	    private float timeSinceLastSpawned; //储存自上一次在玩家面前放置一个柱子以来，已经过了多少时间。
	    private float spawnXPosition = 10f;
	    private int currentColumn = 0;//跟踪正在重新定义数组中的柱子
	
	    void Start()
	    {
	        columns = new GameObject[columnPoolSize];//用5个空对象制作了一个新的空数组游戏对象
	        for (int i = 0; i < columnPoolSize; i++)
	        {   //循环遍历我们索引的每一个对象，实例化Prefab,实例化的位置是ObjectPoolPosition,Quaternion.identity设置预制体的旋转
	            columns[i] = (GameObject) Instantiate(columnPrefab,ObjectPoolPosition,Quaternion.identity);
	            //实例化一个对象为游戏对象，储存在数组中
	        }
	    }
	
	    void Update()//计时器
	    {
	        timeSinceLastSpawned += Time.deltaTime;
	
	        if (GameControl.instance.GameOver == false && timeSinceLastSpawned >= spwanRate)
	        {
	            timeSinceLastSpawned = 0;
	            float spawnYPosition = Random.Range(columnMin, columnMax);//生成柱子
	            columns [currentColumn].transform.position = new Vector2(spawnXPosition, spawnYPosition);
	            //生成了一个固定的x坐标，随机的y坐标；
	            currentColumn++;
	            if (currentColumn >= columnPoolSize)
	            {
	                currentColumn = 0;
	            }
	        }
	    }
	}