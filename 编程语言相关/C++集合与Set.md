---
####2020-01-05 13:41:58
categories:
 - Cpp语法以及标准模版库
tags:
 - Cpp标准模板库
---
set又称作集合，是stl中的常用容器之一，分为set和multiset，区别是 是否能够含有相同的元素，底层用二叉树（红黑树）实现
<!--more-->

	#include <cstdio>
	#include <set>
	#include <stdlib.h>
	#include <string> 
	using namespace std;
	void output(set<int>&s){//打印 set 
	for(set<int>::iterator it = s.begin(); it != s.end(); it++){
		printf("%d ",*it);
	}
	putchar('\n');
	}

	void output_multi(multiset<int>&s){//打印 multiset 
	for(multiset<int>::iterator it = s.begin(); it != s.end(); it++){
		printf("%d ",*it);
	}
	putchar('\n');
	}

	struct cmp{//排序的仿函数
	bool operator() (const int &a,const int &b) const{
		return a>b;
	}
	};

	struct person{
		person(int age,string name){
			this->m_age = age;
			this->m_name = name;
		}
	int m_age;
	string m_name; 
	};
	struct person_cmp{
	bool operator() (const person&a,const person&b) const{
		return a.m_age > b.m_age;
	}
	};
	int main(){
	set<int> st_3;
	set<int> st;//定义操作； 
	multiset<int> mtst;
	
	st.insert(12);//插入操作 
	st.insert(24);
	st.insert(6);
	st.insert(12);
	
	pair<set<int>::iterator,bool> judge = st.insert(12);
	if(judge.second) puts("插入成功"); 
	else puts("插入失败");
	
	mtst.insert(16);
	mtst.insert(7);
	mtst.insert(15);
	mtst.insert(15);
	
	set<int> st_2(st); //copy操作
	st_2.insert(100);
	st_3 = st_2; //赋值 
	
	puts("输出测试：");
	output(st);
	output(st_2);
	output(st_3); 
	output_multi(mtst);
	
	//system("pause");
	
	puts("st的大小为:");
	printf("%d\n",st.size());
	puts("mtst的大小为:");
	printf("%d\n",mtst.size());
	
	puts("交换操作：");
	st.swap(st_2);
	printf("st: ");output(st);
	printf("st_2: ");output(st_2);
	
	puts("删除st容器中的100操作：");
	st.erase(100);
	output(st);
	puts("删除st容器中的第一个元素的操作：");//排好序后删除
	st.erase(st.begin());
	output(st);
	
	puts("清空st");
	st.clear(); 
	if(st.empty()) puts("st已清空");
	
	puts("查找st_2元素") ;
	set<int>::iterator pos = st_2.find(12);
	if(pos != st_2.end()) puts("YES");
	else puts("NO"); 
	
	puts("统计mtst元素");//set容器的元素个数不是0就是1，multiset则不一定 
	printf("%d\n",mtst.count(15)); 
	
	puts("setの自定义排序"); 
	set<int,cmp> less_st;//定义时设置排序规则 
	less_st.insert(156);
	less_st.insert(132);
	for(set<int>::iterator it = less_st.begin(); it != less_st.end(); it++){
		printf("%d ",*it);
	}
	putchar('\n');
	
	puts("setの自定义类型的排序");
	
	set<person,person_cmp> st_4;
	person p1(12,"甲"),p2(18,"乙"),p3(32,"丁"); 
	 st_4.insert(p1);
	 st_4.insert(p2);
	 st_4.insert(p3);
	for(set<person,person_cmp>::iterator it = st_4.begin(); it != st_4.end(); it++){
		printf("%d %s ",it->m_age,it->m_name.c_str());
	}
	putchar('\n');
	return 0;
	}