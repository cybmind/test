```c

	#include<stdio.h> 
	#include<stdlib.h>
	#include<time.h>
	#include<windows.h>
	#define SNAKE_MAX_LENGTH 20
	#define SNAKE_HEAD 'H'
	#define SNAKE_BODY 'X'
	#define BLANK_CELL ' '
	#define SNAKE_FOOD '$'
	#define WALL_CELL '*'
	#define MAXLENTH 50
	#define GO_ON 1
	#define END 0 
	void snake_move(int,int);
	int main(){
		int flag=GO_ON;
    int snake_x[MAXLENTH]={1,2,3,4,5};
    int snake_y[MAXLENTH]={1,1,1,1,1};
    int head_x=5,head_y=1;
    int length=4;//length--蛇长度 
    int m=length;//m--坐标个数 
    int i,j,n;
    char map[12][13]={{"************"},
					{"*XXXXH     *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"*          *"},
					{"************"}};
    //产生食物
	int food_x=5,food_y=5;
	 map[food_y][food_x]='$';
	//打印地图 
	system("cls");
	for(i=0;i<13;i++){
		for(j=0;j<12;j++){
			printf("%c",map[i][j]);
		}
		printf("\n");
	}
	
	//移动蛇 
	while(flag!=END){
		char action;
		scanf("%c",&action);
		switch(action) {
			case 'a':{
				//判断游戏结束
				if(map[head_y][head_x-1]=='*'||map[head_y][head_x-1]=='X'){
					flag=END;
					break;
				}
				//判断蛇能动的条件
				if(head_x-1!=snake_x[m-1]){
				//没有吃到食物的情况
				if(map[head_y][head_x-1]!='$') {
				//头移动 	
				map[head_y][--head_x]='H';
				//身子移动 
				while(m > 0){
					map[snake_y[m]][snake_x[m]]=map[snake_y[m-1]][snake_x[m-1]];
					m--;
				}
				m=length; 
				//尾巴删除 
				map[snake_y[0]][snake_x[0]]=' ';
				//身子坐标改变 
				for(n=0;n<m;n++){
					snake_y[n]=snake_y[n+1];
					snake_x[n]=snake_x[n+1];
				}
				//头坐标改变 
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				}
				//吃到食物的情况
				if(map[head_y][head_x-1]=='$') {
				//长度加一 
				length++;
				m=length; 
				//头移动 	
				map[head_y][--head_x]='H'; 
				//原来的头变为身体
				map[head_y] [head_x+1]='X';
				//坐标改变
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				//随机产生食物
				while(map[food_y][food_x]!=' ') {
					srand(time(0));
					food_x=rand()%10+1;
					food_y=rand()%10+1;
				}
				map[food_y][food_x]='$';
				}
				}
				//打印地图 
				system("cls");
				for(i=0;i<13;i++){
					for(j=0;j<12;j++){
						printf("%c",map[i][j]);
					}
					printf("\n");
				}
				break;
			}
			case 's':{
				//判断游戏结束
				if(map[head_y+1][head_x]=='*'||map[head_y+1][head_x]=='X'){
					flag=END;
					break;
				}
				//判断蛇能动的条件
				if(head_y+1!=snake_y[m-1]){
				//没有吃到食物的情况
				if(map[head_y+1][head_x]!='$') {
				//头移动 	
				map[++head_y][head_x]='H';
				//身子移动 
				while(m > 0){
					map[snake_y[m]][snake_x[m]]=map[snake_y[m-1]][snake_x[m-1]];
					m--;
				}
				m=length; 
				//尾巴删除 
				map[snake_y[0]][snake_x[0]]=' ';
				//身子坐标改变 
				for(n=0;n<m;n++){
					snake_y[n]=snake_y[n+1];
					snake_x[n]=snake_x[n+1];
				}
				//头坐标改变 
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				}
				//吃到食物的情况
				if(map[head_y+1][head_x]=='$') {
				//长度加一 
				length++;
				m=length; 
				//头移动 	
				map[++head_y][head_x]='H'; 
				//原来的头变为身体
				map[head_y-1] [head_x]='X';
				//坐标改变
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				//随机产生食物
				while(map[food_y][food_x]!=' ') {
					srand(time(0));
					food_x=rand()%10+1;
					food_y=rand()%10+1;
				}
				map[food_y][food_x]='$';
				}
				}
				//打印地图 
				system("cls");
				for(i=0;i<13;i++){
					for(j=0;j<12;j++){
						printf("%c",map[i][j]);
					}
					printf("\n");
				}break;
			}
			case 'd':{
				//判断游戏结束
				if(map[head_y][head_x+1]=='*'||map[head_y][head_x+1]=='X'){
					flag=END;
					break;
				}
				//判断蛇能动的条件
				if(head_x+1!=snake_x[m-1]){
				//没有吃到食物的情况
				if(map[head_y][head_x+1]!='$') {
				//头移动 	
				map[head_y][++head_x]='H';
				//身子移动 
				while(m > 0){
					map[snake_y[m]][snake_x[m]]=map[snake_y[m-1]][snake_x[m-1]];
					m--;
				}
				m=length; 
				//尾巴删除 
				map[snake_y[0]][snake_x[0]]=' ';
				//身子坐标改变 
				for(n=0;n<m;n++){
					snake_y[n]=snake_y[n+1];
					snake_x[n]=snake_x[n+1];
				}
				//头坐标改变 
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				}
				//吃到食物的情况
				if(map[head_y][head_x+1]=='$') {
				//长度加一 
				length++;
				m=length; 
				//头移动 	
				map[head_y][++head_x]='H'; 
				//原来的头变为身体
				map[head_y] [head_x-1]='X';
				//坐标改变
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				//随机产生食物
				while(map[food_y][food_x]!=' ') {
					srand(time(0));
					food_x=rand()%10+1;
					food_y=rand()%10+1;
				}
				map[food_y][food_x]='$';
				}
				}
				//打印地图 
				system("cls");
				for(i=0;i<13;i++){
					for(j=0;j<12;j++){
						printf("%c",map[i][j]);
					}
					printf("\n");
				}break;
			}
			case 'w':{
				//判断游戏结束
				if(map[head_y-1][head_x]=='*'||map[head_y-1][head_x]=='X'){
					flag=END;
					break;
				}
				//判断蛇能动的条件
				if(head_y-1!=snake_x[m-1]){
				//没有吃到食物的情况
				if(map[head_y-1][head_x]!='$') {
				//头移动 	
				map[--head_y][head_x]='H';
				//身子移动 
				while(m > 0){
					map[snake_y[m]][snake_x[m]]=map[snake_y[m-1]][snake_x[m-1]];
					m--;
				}
				m=length; 
				//尾巴删除 
				map[snake_y[0]][snake_x[0]]=' ';
				//身子坐标改变 
				for(n=0;n<m;n++){
					snake_y[n]=snake_y[n+1];
					snake_x[n]=snake_x[n+1];
				}
				//头坐标改变 
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				}
				//吃到食物的情况
				if(map[head_y-1][head_x]=='$') {
				//长度加一 
				length++;
				m=length; 
				//头移动 	
				map[--head_y][head_x]='H'; 
				//原来的头变为身体
				map[head_y+1] [head_x]='X';
				//坐标改变
				snake_y[m]=head_y;
				snake_x[m]=head_x;
				//随机产生食物
				while(map[food_y][food_x]!=' ') {
					srand(time(0));
					food_x=rand()%10+1;
					food_y=rand()%10+1;
				}
				map[food_y][food_x]='$';
				}
				}
				//打印地图 
				system("cls");
				for(i=0;i<13;i++){
					for(j=0;j<12;j++){
						printf("%c",map[i][j]);
					}
					printf("\n");
				}break;
			}
		}
		
	}
	return 0;
	}