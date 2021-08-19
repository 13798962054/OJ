# 什么是BFS
- 英文：Breadth-First Search，全称：广度优先搜索算法。
- 算法特性：从根节点出发，沿着树（图）的宽度便利树（图）的节点
- 中止条件：所有节点均被访问完，则算法结束
- 缺点：是一种盲目的算法

## 迷宫问题一：


## 迷宫问题二：

### 题目描述
小华一不小心进入了一个迷宫.这个迷宫有一个出口.小华可以上下左右移动.迷宫里也有障碍物,如果这个地方是有障碍的,那么这个地方是不能走进去的.
现在小华想知道他能否走出迷宫,若能走出迷宫至少需要多少步.

- 解答要求
时间限制：1000ms, 内存限制：100MB

- 输入
对于每个输入文件,第一行输入一个整数N(1<=N<=500),接下来N行,每行N个字符,表示这个迷宫的状态.
其中’S’表示小华的位置,’E’表示终点,’#’表示障碍物,’.’表示可以走的地方.

- 输出
如果可以走到终点,请输出走到终点的最少步数,否则输出-1.

- 样例
输入样例 1
```
3
#S#
...
E##
```
- 输出样例 1
```
3
```
### 解析
1、迷宫的起点位置和终点位置是位置的，我们一开始必须先找到起点的位置才能进行下一步的搜索工作。
2、小华的移动方式有上下左右四种方向，我们可以通过改变数组下标的方式让小华进行相应的移动。
3、题目要求找出到达终点的最小步数，意味着我们需要对走出迷宫的步数进行判定。
4、程序运行的时限为1000ms，如果使用递归需要谨慎。

### 解题方法一：函数内while死循环，用return跳出。
#### 思路
1、for循环找出起点"S"的位置并记录为startX和startY。同时，利用字符串的split()方法将输入的字符串迷宫转换成二维矩阵保存在rows中。
2、定义stepObj对象记录坐标点是否已经走过和走到改点上的已走步数。
3、定义二维数组queue记录走过的点，将起点记录为第一个点。
4、将起点位置记录在stepObj对象中，定义起点步数为1，格式为stepObj[startX + "" + startY] = 1.
5、执行while死循环，直到找出终点"E"后以return跳出。
6、while循环内执行for循环遍历queue数组上的点，让这些点都尝试移动。
7、while循环内让目前点尝试往上下左右四个方向行动，只要下一个点不越界或者不是墙"#"就在stepObj对象上记录步数，格式为stepObj[nextPosX + '' + nextPosY] = currentStepCount + 1。
8、将这些能走的点添加到queue数组中，执行下一次while循环。

#### 该方法的优缺点
优点：
1、利用循环执行BFS相比递归会高效很多。
2、由于每执行一次while循环只会同步移动一步，所以一经找到终点，其步数一定是最小的步数，可以省去判定是否为走出迷宫的最小步数。
缺点：
利用循环执行BFS需要程序员有更清晰的逻辑，需要更多的临时变量辅助函数。
#### JS代码
```javascript
// please define the JavaScript input here(you can also rewrite the input). 
// following is the default: 
process.stdin.resume();
process.stdin.setEncoding('utf-8');
let input = '';
process.stdin.on('data', (data) => {
  input += data;
});
process.stdin.on('end', () => {
  let inputArray = input.split('\n');
  // please finish the function body here.
  let maxIndex = parseInt(inputArray[0]);
  let queue = [];
  let rows = [];
  let startX = 0,startY = 0;
  for(let i = 1; i < inputArray.length; i++) {
      let cols = inputArray[i].split('');
      let yIndex = cols.indexOf('S');
      if(yIndex != -1) {
          startX = i - 1;
          startY = yIndex;
      }
      rows.push(cols);
  }
  queue.push([startX, startY]); 
  let direactionX = [0, 0, 1, -1];  //X坐标方向
  let direactionY = [1, -1, 0, 0];  //Y坐标方向
  let stepObj = {};
  stepObj[startX + '' +startY] = 1;    //记录走过的位置防止死循环
  while(queue.length != 0) {
      let newQueue = [];
      for(let i = 0; i < queue.length; i++) {
          let curPosX = queue[i][0];
          let curPosY = queue[i][1];
          let currentStepCount = stepObj[curPosX + '' + curPosY];  //当前步数
          for(let j = 0; j < 4; j++) {
              let nextPosX = curPosX + direactionX[j];
              let nextPosY = curPosY + direactionY[j];
              if(stepObj[nextPosX + '' + nextPosY] > 0) {  //如果有步数，说明这个点走过
                  continue;
              }
              if(nextPosX < 0 || nextPosX > maxIndex || nextPosY < 0 || nextPosY > maxIndex) {
                  continue;
              }
              if(rows[nextPosX][nextPosY] == 'E') {
                  console.log(currentStepCount);
                  return;
              }
              if(rows[nextPosX][nextPosY] == '#') {
                  continue;
              }
              newQueue.push([nextPosX, nextPosY]);
              stepObj[nextPosX + '' + nextPosY] = currentStepCount + 1;
          }
      }
      queue = newQueue;
  }
 
  console.log(-1);
  // please define the JavaScript output here. For example: console.log(result);
  process.exit();
});
```
