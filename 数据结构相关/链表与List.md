---
####2019-12-22 23:02:05
categories:
 - 数据结构
tags:
 - 基本线性数据结构
---
概念：数据结构，以链表的形式储存数据，链表一般有许多结点，结点相互连接，每个结点下有一个数据域和一个指针域，数据域(->num)用来储存数据，指针域(->next)用来指下一个节点的位置，最后一个节点的指针域一般为NULL。与数组不同，数组是以线性表的方式储存数据的。STL拥有的容器（List）为双向链表。
<!--more-->
- 特性：

可以在任意的位置迅速的查找，插入，删除某一个元素，修改指针即可；

采用动态储存(List)，不会造成内存浪费或溢出；

遍历速度没有数组快（Time）；

占用的空间比数组要大（Space）；

List的迭代器为双向迭代器，支持前移和后移；

插入和删除操作都不会造成List迭代器的失效，这在vector容器中是不成立的

1. 链表的基操：
  
1,插入元素，如果是在链表中间则在链表中间插入一个结点，并把该结点与该结点两边的结点相连；如果是在开头插入则新设置一个头结点并把新结点连到原头结点上；如果是在末端则直接在末端新建一个结点；

2,删除元素，原理很简单，把要删除的元素的结点给“忽略”，也就是把该结点与其它结点的联系给切断，然后通过对旁边的节点进行操作来进行删除；

3,链表的遍历输出，每次把结点通过指针指向下一个结点进行遍历与输出；

- 链表的分类

链表大致可以分为单向链表，双向链表，循环链表，用来处理不同的问题；

单向链表：
最简单的链表，只能向一个方向进行遍历以及其它操作；
手工链表应用举例（kzoj-1322）

	#include<iostream>
	using namespace std;
 
	struct node{
    int num;
    node *next; //定义一个指针，指向下一个结点 
	}; 
	int main(){
  	  int n,x;
  	  node *head,*p,*q,*r;
  	  cin >> n;
  	  head = new node;
  	  head -> next = NULL;//初始化头结点
  	  cin >> head -> num;
  	  q = head;
     
  	  for(int i = 2; i <= n; i++){
        p = new node;
        p -> next = NULL;
        cin >> p -> num;
        q -> next = p;
        q = p;//建立链表 
   	 }
     
   	 cin >> x;
  	 p = head; //回到头结点 ,遍历链表，找到插入位置 
  	 while(p != NULL){
        if(p->num >= x) break;
        q = p;//取前一个元素 
        p = p -> next;
    }
     
    r = new node;
    r -> num = x;
    r -> next = NULL;
    if(p == head){//如果插入位置在链表的头部 ,建立新头结点 
        r -> next = head;
        head = r;
    }
    else if(p == NULL){//如果插入位置在链表地尾部，在尾部加上一个结点 
        q -> next = r;
    } 
    else{//如果插入位置在链表中间，插入结点 
        q -> next = r;
        r -> next = p;
    }
     
    p = head;//输出 
    while(p -> next != NULL){
        cout << p -> num <<' '; 
        p = p->next;
    }
    cout << p -> num <<endl;
    return 0;
	} 

双向链表：
可以往两个方向进行操作，stl中的容器List就是一个双向链表；
双向链表应用举例（kzoj-1323）

	#include<cstdio>
	#include<iostream>
	using namespace std;
	struct node {
    int num;
    node *pre,*next;
	};
	int main(){
    int n,m,d,s;
    node *head,*tail,*p,*k;
    scanf ("%d%d",&n,&m);
    head = new node;
    head->pre = NULL;
    head->next = NULL;
    head->num = 1;
    k = head;
    for (int i = 2; i <= n; i++){
        p = new node;
        p->next = NULL;
        p->pre = NULL;
        p->num = i;
         
        k->next = p;
        p->pre = k;
        k = p;
    }
    tail = k;
    p = head;
    s = 0;
    d = 1;
    while (head != tail){
        s++;
        if(s==m){
            s=0;
            if(p == tail){
                tail = p->pre;
                p = tail;
                d = 2;
                continue;
            }
            if(p == head){
                  head = p->next;
                  p = head;
                    d = 1;
                    continue;
            }
            p->pre->next = p->next;
            p->next->pre = p->pre;
        }
        if(d == 1){
            if(p == tail){
                p = p->pre;
                d = 2;
            }
            else p = p->next;
        }
        else{
            if(p == head){
                p = p->next;
                d = 1;
            }
            else p = p->pre;
        }
    }
    cout<<head->num;
    return 0;
	}
循环链表：
链表首尾相连，很好理解；单向链表应用举例（kzoj-1324）

	#include<cstdio>
	#include<iostream>
	using namespace std;
	struct node {
    int num;
    node *next;
	};
	int main(){
    int n,k,s;
    scanf("%d%d",&n,&k);
    node *head,*p,*q;
    head = new node;
    head->next = NULL;
    head->num = 1;
    q = head; 
    for(int i = 2; i <= n; i++){
        p = new node;
        p->next = NULL;
        p->num = i;
        q->next = p;
        q = p;
    }
    q->next = head;//建立循环链表 
    p = head;
    s = 0;
    while(p->next != p){
        s++;
        if(s == k){
            s = 0;
            printf("%d ",p->num);
            q->next = p->next;
            p = p->next;
        }
        else{
            q = p;
            p = p->next;
        }
    }
    printf("%d",p->num);
    return 0;
	} 

