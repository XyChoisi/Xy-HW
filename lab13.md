# **字符游戏-贪吃蛇**
## 伪代码
```

输出字符矩阵
    WHILE not 游戏结束 DO
        ch＝等待输入
        CASE ch DO
        ‘A’:左前进一步，break 
        ‘D’:右前进一步，break    
        ‘W’:上前进一步，break    
        ‘S’:下前进一步，break    
        END CASE
        输出字符矩阵
    END WHILE
    输出 Game Over!!! 
```

## 有下列函数
```
void snakeMove (int, int);		//蛇移动函数 
void put_money (void);			//下放食物 
void output (void);				//输出数组 
void gameover (void);			//打印提示游戏结束 
void snakeExtent (void);		//吃到食物后蛇的伸长 
int judge (void);				//判断游戏是否结束 
char changeUpper (char);		//将字母全部转为大写 

```




### 移动
```
//移动函数
void snakeMove (int x, int y) {
	int i; 
	if(snakeX[0] + x == moneyX && snakeY[0] + y == moneyY) 				//判断是否吃到食物 ,如是便使蛇伸长 
		snakeExtent() ;						
	else 
		map[snakeY[snakeLength - 1]][snakeX[snakeLength - 1]] = ' ';	//如果没有吃到食物,把蛇的最后一段删除 
	//蛇的向前走,蛇身的后面一段继承前面一段的位置 
	for(i = snakeLength - 1; i > 0; i --) {								 
		snakeX[i] = snakeX[i - 1];		
		snakeY[i] = snakeY[i - 1];
		map[snakeY[i]][snakeX[i]] = 'X';
	}
	snakeX[0] += x;
	snakeY[0] += y;
	map[snakeY[0]][snakeX[0]] = 'H';
	//输出数组 
	output ();
}
```

## 放食物
```
//放食物
void put_money (void) {
	//随机设置食物位置 
	moneyX = rand () % 9;
	moneyY = rand ()  % 9;

	//判断位置是否为空 
	if (map[moneyY][moneyX] == ' ') {
		map[moneyY][moneyX]='$';
	}
	else	//不为空重新调用 
		put_money ();		

}
```

## 吃食物
```
//吃食物延长
void snakeExtent (void) {
	put_money ();//蛇吃到食物后重置食物位置
	//小于最长长度 
	if (snakeLength < SNAKE_MAX_LENGTH) { 
		snakeY[snakeLength] = snakeY[snakeLength-1];
		snakeX[snakeLength] = snakeX[snakeLength-1];
		snakeLength ++ ;
	} 
	else 
		map[snakeY[snakeLength-1]][snakeX[snakeLength-1]] = ' '; //大于最长长度 
}
```

判断生死
```
//判断蛇生死
int judge(){
	int i;
	//碰到框架游戏结束 
	if(snakeX[0] == 0||snakeX[0] == 11||snakeY[0] == 0||snakeY[0] == 11)
		return 0;
	//头碰到自己游戏结束 
	for(i=1;i<snakeLength;i++) {
		if(snakeX[0] == snakeX[i]&&snakeY[0]==snakeY[i])
				return 0;
	}
	return 1;
}
```

蛇身
```
char map[10][10] = { "**********",
                     "*XXXXH   *",
                     "*        *",
                     "*        *",
                     "*        *",
                     "*        *",
                     "*        *",
                     "*        *",
                     "*        *",
                     "**********" };

//蛇身的各个位置,[0]是头	
int snakeX[SNAKE_MAX_LENGTH] = {5, 4, 3, 2, 1}; 
int snakeY[SNAKE_MAX_LENGTH] = {1, 1, 1, 1, 1};
```

## 主程序
```
//主程序
int main() {
 	char ch='d';//默认向右走 
	put_money(); 	
	output();
	while(1) {
 		ch = changeUpper(ch);
		switch(ch) {
			case 'A': 
				snakeMove (-1, 0);
				break;
			case 'S':
				snakeMove (0, 1);
				break;
			case 'D':
				snakeMove (1, 0);
				break; 
			case 'W':
				snakeMove (0, -1);
				break;
			}
		Sleep(200);
		if(kbhit())
			ch = getche();
		if(judge() == 0){
			gameover();
			break;
		}
	}
}
```


## 智能蛇新增下列函数
```
void movedis();					//设置可走路线与食物的距离 
char aisnake();					//这是找最小的距离的输出 
void judgearray();				//设置蛇的可走路线	
int autojudge(int , int);		//判断蛇是否可走
```

## 新增变量
```
char array[4][2] = {{'a',1},{'s',1},{'w',1},{'d',1}};
//储存各个动作以及该动作是否可移动 

int distance[4]; 								//储存各个可移动动作与食物的距离 

int xymove[4][2] = {{-1,0},{0,1},{0,-1},{1,0}};
//[][0]为X,[][1]为Y ,储存各个动作的移动方向 

```


## 函数
```
int autojudge(int x, int y) {
	if(map[snakeY[0] + y][snakeX[0] + x] != 'X'&&map[snakeY[0] + y][snakeX[0] + x] != '*')	
    //判断是否会移动到蛇身 
		return 1;
	else
		return 0;
}

void judgearray() {
	int count;

	for(count = 0;count<4;count ++) {

		if(autojudge (xymove[count][X],xymove[count][Y]) )
			array[count][1] = 1;		 //说明该移动是否合理 
		else
			array[count][1] = 0;
	}
}

 

void movedis() {
	int num;
	judgearray();

	for(num=0;num<4;num++) {

		if(array[num][1]==1)

			distance[num] = abs(moneyX-snakeX[0]-xymove[num][X])+abs(moneyY-snakeY[0]-xymove[num][Y]);
            //可移动的算距离 
		else
			distance[num] = 9999;		//不可移动的设置一个超大的值 
	}
}


char aisnake() {
	movedis();
	int num,min=0;

	//选择并返回一个最合适的动作 
	for(num=1;num<4;num++) {			
		if(distance[num] < distance[min])
			min=num; 
	}
	return array[min][0];
}
```