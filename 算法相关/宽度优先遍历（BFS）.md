---
###2020-01-05 17:08:32
categories:
 - 基本算法
tags:
 - 搜索
---
搜索算法有很多，其中最主要也是最重要的两个就是宽度优先遍历（BFS）和深度优先遍历（DFS），其它如A*之类的很多搜索算法都是他们的延伸。
<!--more-->
BFS模版（红黑瓷砖问题）：

	#include<bits/stdc++.h>
	using namespace std;
	int move_x[] = {-1,0,1,0};
	int move_y[] = {0,-1,0,1};
	 
	char a[51][51];
	int ans,w,h;
	struct node{
    int x,y;
	};
	void BFS(int Wx,int Wh){
    ans = 1;
    queue <node> q;
    node start, next;
    start.x = Wx;
    start.y = Wh;
    q.push(start);
    while(!q.empty()){
        start = q.front();
        q.pop();
        for(int i = 0; i < 4; i++){
            next.x = start.x + move_x[i];
            next.y = start.y + move_y[i];
            if(( next.x <= h && next.x > 0)&&( next.y <= w && next.y > 0) && a[next.x][next.y] == '.')
            {
                a[next.x][next.y] = '#';
                ans++;
                q.push(next);
            }
        }
    }
	}
	int main(){
    int Wx,Wh,i,j;
    cin>>w>>h;
    for(int i = 1; i <= h; i++)
        for(int j = 1; j <= w; j++){
            cin>>a[i][j];
            if(a[i][j]=='@'){
                Wx = j;
                Wh = i;
            }
        }
    BFS(Wh,Wx);
    printf("%d",ans);
    return 0;
}