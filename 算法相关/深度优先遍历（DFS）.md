---
###2020-01-05 17:08:07
categories:
 - 基本算法
tags:
 - 搜索
---
搜索算法有很多，其中最主要也是最重要的两个就是宽度优先遍历（BFS）和深度优先遍历（DFS），其它如A*之类的很多搜索算法都是他们的延伸。
<!--more-->
DFS模版（红黑瓷砖问题）（kzoj 1188）：

	#include<bits/stdc++.h>
	using namespace std;
	int move_x[] = {-1,0,1,0};
	int move_y[] = {0,-1,0,1};
 
	char a[51][51];
	int ans=0, w,h;
	void Dfs(int dx,int dy){
    a[dx][dy] = '!';
    ans++;
        for(int i = 0; i < 4; i++){
            int x = dx + move_x[i];
            int y = dy + move_y[i]; 
            if((x > 0 && x <= h) && (y > 0 && y <= w) && a[x][y] == '.')
                Dfs(x,y);
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
    Dfs(Wh,Wx);
    printf("%d",ans);
    return 0;
	}
