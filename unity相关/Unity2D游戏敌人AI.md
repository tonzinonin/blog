---
title: Unity2D游戏敌人AI
date: 2020-02-13 19:55:26
categories:
 - Unity
tags:
 - Unity 2d
 - Unity功能实现
---

1. 只会朝一个方向移动，并且到一定的距离就自动削除了的敌人脚本，可以用作子弹脚本

    public float moveSpeed = 6f;
    public float moveAngelMax = 0f; //可以给敌人设置随机的移动角度
    public float moveAngelMin = 0f; 
    public Transform destransform; //自我销毁的位置

    private Rigidbody2D rb2d;
    private float moveAngel;

    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
        moveAngel = Random.Range(moveAngelMin, moveAngelMax);
    }

    void Update()
    {
        rb2d.velocity = new Vector2(-moveSpeed, moveAngel );
        if (gameObject.transform.position.x <= -destransform.position.x)
        {
            Destroy(gameObject);
        }
    }

2. 在两个点之间来回移动并且可以随着时间释放2种类型的子弹的的敌人功能实现函数

以下是伪代码

        private void MovingUpDown()//这个函数被用于Update中
    {
        if (gameObject.transform.position.y < up.position.y && isMove == true)
        {
            rb2D.velocity = new Vector2(0, movingSpeed);
        }
        if (gameObject.transform.position.y >= up.position.y && isMove == true)
        {
            rb2D.velocity = Vector2.zero;
            isMove = false;
        }
        if (gameObject.transform.position.y > down.position.y && isMove == false)
        {
            rb2D.velocity = new Vector2(0, -movingSpeed);
        }
        if (gameObject.transform.position.y <= down.position.y && isMove == false)
        {
            rb2D.velocity = Vector2.zero;
            isMove = true;
        }
    }

    private void Battle()
    {
        if (Time.fixedTime - time1 > bullet1Speed)
        {
            time1 = Time.fixedTime;
            Instantiate(bullet1, gameObject.transform.position, Quaternion.identity);
        }
        if (Time.fixedTime - time2 > bullet2Speed)
        {
            time2 = Time.fixedTime;
            Instantiate(bullet2, gameObject.transform.position, Quaternion.identity);
        }
    }
