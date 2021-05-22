# Map

1. 基本属于
2. 存储结构
3. 图的遍历
4. 图的连通性
5. 图的应用

## 7.1 基本术语

```markdown
* G = (V, E) 
	* V = vertex
	* E = edge
* V是G的顶点集合，是有穷非空集
* E是G的边集合，是有穷集，可空
```



```markdown
* 有向图：每个E都有方向
* 无向图：每个E都无方向
* 完全图：任意两个V都有一条E
  * n个顶点的无向图共 n(n-1)/2 条边 则是无向完全图
  * n个顶点的有向图共 n(n-1) 条边，则是有向完全图
```

![image-20210521121356853](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521121356853.png)

```markdown
* 稀疏图：边少，E < V * log(V)
* 稠密图：边数接近 n * (n-1) 或 n * (n-1) / 2
* 子图：G = (V, E), G' = (V', E'); 当 V'在V中，E'在E中
* 带权图：“边，E” 上带权值
* 网络：带权图
* 连通图：
	1. 无向图
	2. 任意 两个顶点间 都有路径相通
    3. 连通分量：非连通图的极大连通子图
* 强连通图：
	1. 有向图
    2. 任意两个顶点之间都存在一条 **有向路经**
    2. 强连通分量：强连通子图
```

![image-20210521121858814](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521121858814.png)

 ```markdown
 * 生成树：含有全部n个V，但只有 n-1条边
 	* 任意在生成树上添加一条边，必然构成一个环
 	* 若少于n-1条边，则必然非连通
 * 生成森林：生成树的集合，且构成的 边 或 弧 **最少**
 * 邻接点：若(u,v)是E(G)中的一条边，则u和v互为邻接顶点
 * 弧头、弧尾：1、有向边(u,v)为弧；2、从弧头到弧尾
 * 度：与之顶点v相关联的边数，TD(v)；有向图中分出度、入度
 	* 出度：ID(v)，以v为终边
 	* 入度：OD(v)，以v为始边
 ```

```markdown
* 路径：从v_i出发，到v_j，称序列(v_i, v_p1 ... v_pn, v_j)为v_i 到v_j的路径
	* 简单路径：路径顶点不重合（不一定最简）
	* 非简单路径
	* 回路：v_i = v_j
* 路径长度：权值、无权值
```

![image-20210521123540335](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521123540335.png)

## 7.2 图的存储结构

* 图的特点：非线性（m:n)
* 可以用数组描述元素关系：邻接矩阵，关联矩阵
* 链式存储：多重链表：邻接表、十字链表、邻接多重表

![image-20210521123739451](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521123739451.png)

* 基本原则：惟一复原

 ### 邻接矩阵（数组）表示

* 顶点表、邻接矩阵
* 图A = (V,E)有n个顶点，则图的邻接矩阵是一个二维数组A.Edge\[n][m] = 1连通，0不连通

```markdown
* A[n][n]的邻接矩阵
* A[v][i] : 从顶点v到顶点i
```



*  无向图：![image-20210521124143123](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521124143123.png)

* 有向图

  ![image-20210521124424949](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521124424949.png)

* 权值图：初始化时为无穷大

![image-20210521124736649](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521124736649.png)

基本定义

![image-20210521125012564](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521125012564.png)

### 邻接表（链式)表示

* 对每个顶点建立单链表，放信息连接

* 顶点v_i：单链表

  ```cp
  邻接点域：表示v_i邻接点的位置
  链域：指向下一个边或弧的结点
  数据域：与边有关信息
  ```

* 头

  ```cpp
  数据域：存放顶点v_i信息
  链域：指向第一个结点
  ```

![image-20210521125937920](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521125937920.png)

![image-20210521130011386](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521130011386.png)

![image-20210521130601139](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521130601139.png)

![image-20210521130740557](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521130740557.png)

#### 邻接表 邻接矩阵 异同

* 邻接表：稀疏图
* 邻接矩阵：稠密图

![image-20210521131259651](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521131259651.png)

#### 十字链表 与 邻接多重表

![image-20210521131538850](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521131538850.png)

![image-20210521131617083](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521131617083.png)

## 7.3 图的遍历

* 实质：找每个点的邻接点
* 特点：存在回路，可能回到曾经访问过的顶点
* 加一个辅助数组visit[n]

* 深度优先搜索
* 广度优先搜索

### 7.3.1 DFS

* 基本思路：仿照树的先序遍历
* 栈
* 1. 访问起始点v
  2. 若v的第1个邻接点，没访问过，深度遍历之
  3. 若已访问过，找第2个邻接点……
* 递归

![image-20210521133400879](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521133400879.png)

效率分析：

* 稠密图：邻接矩阵
* 稀疏图：邻接链表

### 7.3.2 BFS

* 基本思路：仿照树的按层遍历
* 队列
* 1. 访问起始点v
  2. 依次访问v的邻接点
  3. 不是递归

![image-20210521134257591](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521134257591.png)

## 7.4图的其他运算

* 图的生成树
* 最小生成树
* 最短路径
* 关键路径

### 1. 生成树、生成森林

* 对图进行遍历：得到连通子图
  * 深度优先搜索生成树
  * 广度优先搜索生成树
  * 生成树森林（非连通图）

![image-20210521134943092](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521134943092.png)

![image-20210521135438263](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521135438263.png)

### 2. 最小生成树、生成森林

* 在网络中的多个生成树中，寻找一个各边权值之和最小的生成树
  * 类似于Huffman树
  * 针对：有权图
* 使用不同方法遍历，得到不同生成树
* 生成树的边来自于连通网络的n个顶点，和n-1条边
* 准则
  * 使用该网络中的边构造
  * 仅用 n-1 条边连结
  * 不产生回路

* 用途

![image-20210521135944762](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521135944762.png)

* 设计思想

```markdown
* 以权值给边排序，最小者放入生成树内，逐个递增，舍去回路边
```

* 最常用的两种算法

  * Kruskal（库鲁斯卡尔）算法

  ```markdown
  将边进行归并，适用于 **稀疏网**
  * 按边的权值排序，依次放入图中，并舍去构成回路的边
  
  * 算法效率分析：O(e * log_2(e))
  * 边少的图，稀疏图，邻接表
  ```

  ![image-20210521141334097](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521141334097.png)

  * Prim（普里姆）算法

  ```markdown
  将点进行归并，与边数无关，适用于 **稠密网**
  * 按点递归，在已选顶点集中选A，未选顶点集中选B，使得E(AB)最短，且一定没有回路
  
  * 算法效率：O(n^2)
  * 边多的图，稠密网
  ```

  ![image-20210521142413782](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521142413782.png)

### 3.最短路径

* 源点：起点
* 终点：其他点
* 目标：各边权值之和最小

1. 单源最短路径：Dijkstra 算法
2. 所有顶点间的最短路径：Floyd 算法

#### Dijkstra算法

* 限定正值
* 按照路径长度递增的次序，主次产生最短路径

#### Floyd算法

### 图的应用

#### 活动图

##### AOV网

活动网络 Activity on Vertices

Vi 必须先于 Vj 进行，有向网络

##### AOE网

边表示活动：时间

Activity on Edges

![image-20210521143325150](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521143325150.png)

![image-20210521143747184](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Untitled.assets\image-20210521143747184.png)

## 7.7拓扑排序的方法

### 基本思想：

1. 输入AOV网络，令n为顶点个数
2. 在AOV中选一个“没有直接前驱”的顶点，输出
3. 在图中删去之（同时删去边）
4. 重复2、3，直到（a或b）
   1. 全部顶点均已输出
   2. 还有为输出的顶点，说明必有有向环

### 算法

1. 建立入度为0的顶点栈
2. while(栈不空)：
   1. 退出一个顶点，输出之
   2. 在AOV中删除它，及其边（发出边，输入边可以不删）
   3. 更改其余入度
   4. 如果有入度为0的点，push进栈
3. 如果输出顶点数小于AOV顶点，报告存在有向环

### AOE网

* 由于活动信息记录在Edge上，所以“完成整个工程所需的时间取决于从“源点”到“汇点”的最长路径长度”，该最长路径，即是**关键路径**





# 错题集

## 课后习题

```json
{
"12":"["对于一个具有n个结点和e条边的无向图，用邻接表表示，则顶点表大小为(), 所有边链表中结点的总个数为()","n,2e"]",
"15":"["设G采用邻接表存储，则拓扑排序算法的时间复杂度为","O(n+e)"]",

}
```

* 普里姆算法：
  * 滚动搜索结点，将所有结点v从“图集合”“搬运”到“生成树集合”，故时间复杂度：O(n^2)。
  * 注意，选择时一定是A从“图集合”，B从“生成树集合”
* 克鲁斯卡尔算法：
  * 将边排序，根据大小顺序依次生成“生成树集合”
  * 时间复杂度：O(e * log(e))
