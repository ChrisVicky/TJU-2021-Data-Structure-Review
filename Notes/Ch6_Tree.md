# Tree

## 6.1 树的定义

* 由n个结点组成的有限集合T，如果n>0且满足
  * 有且仅有一个结点称为根(root)
  * 当n>1时，其余的结点分别为几个互不相交的有限集合，集合本身又是棵树，

![image-20210519175416231](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519175416231.png)

### 术语

```markdown
* 结点的度：孩子个数;
* 叶子：度为0
* 双亲、孩子：E是B的孩子，B是F、E的双亲
* 兄弟：E、F是兄弟，F、G是堂兄
* 祖先：H、D、A都是M的祖先
* 树的度：最大的结点的度数：3
* 结点的层次：A为第一层
* 树的深度：最大的结点层次数：4
* 有序树：兄弟的左右次序不可互换
* 森林：多个树，根在同一层次
```

![image-20210519175941837](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519175941837.png)

### 抽样数据类型定义

![image-20210519180203331](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519180203331.png)

### 树的逻辑结构

* 一对多（1:n)​
  * 多个直接后继
  * 一个直接前驱（除根以外）

### 树的存储结构

* 顺序存储
  * 记录自己的父节点
  * 对于完全二叉树，还行

* 链式存储
  * 两个指针
  * 三个指针

## 6.2 二叉树

* 定义：（递归定义）
  * 二叉树 或 空树 或是 由一个 **根节点** 加上两颗分别称为 **左子树** 和 **右子树** 的、互不相交的二叉树

![image-20210519181216082](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519181216082.png)

* 五种基本形态

![image-20210519181248065](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519181248065.png)

### 树转二叉树

![image-20210519181511896](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519181511896.png)

![image-20210519181540374](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519181540374.png)

* 方法：加线（兄弟相连）—— 抹线（长兄为父）—— 旋转（孩子靠左）
* 左：长子
* 右：下一个兄弟

#### 二叉树还原为树

* 要点：逆操作：所有右孩子变为兄弟

### 二叉树的性质

1. 低 i 层上，至多共有2^(i-1)个节点

2. 深度为k的二叉树，至多共有 2^(k)-1 个

3. 对任意二叉树，若终端节点(叶子)为n_0，度为2的节点为n_2，则n_0 = n_2 + 1;

   ```markdown
   证明：
   节点总数 n = n_0 + n_1 + n_2; // 二叉树仅有0（叶子）、1、2个入度可能性
   分支总数 e = n_1 + 2 * n_2 // 两个入度的两个分支，一个入度的一个分支
   又因为   e = n - 1 // 每个结点都有唯一前驱（除了根）
   ```

#### 满二叉树

* 深度为k且有 2^(k)-1 个结点

#### 完全二叉树

* 深度为h
* 除 h 层外，其他各层结点数达到最大值
* 且 第 h 层从右向左连续缺若干结点(或不缺)

![image-20210519184315269](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519184315269.png)

##### 性质

* 度为1的结点数：n_1 = 1（n为奇数） 或 0（当n为偶数）
* 叶子节点多：n_0 = n/2（n为偶数） 或 (n+1)/2（n为奇数）

* n个完全二叉树的深度：floor(log_2(n))+1 //floor()为对float类型向下取整

* 自上到下、自左到右编号，对于第i个节点（可以顺序存储）
  * 左孩子：2i
  * 右孩子：2i+1
  * 双亲：i/2 (向下取整)



![image-20210519185255330](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519185255330.png)

### 二叉树的存储结构

* 顺序存储：适合于 **完全二叉树**

* 链式存储

#### 链式存储结构

```cpp
typedef struct node{
    int data;
    node *left_child, *right_child;
}node;
typedef node *tree_pointer;
```

![image-20210519184931052](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519184931052.png)

* 二叉链表

![image-20210519185156160](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519185156160.png)

* 三叉链表

![image-20210519185354499](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519185354499.png)

#### 创建链式二叉树

![image-20210519185640158](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519185640158.png)

```cpp
tree_pointer buildtree()
{
    char c;
    node *p;
    c = getchar();
    if(c=='0') return (0);
    p = new(node);
    p->data=c;
    p->left_child = buildtree();
    p->right_child = buildtree();
    return (p);
}
序列：
	AB0C00D0EF000
```

## 6.3 遍历二叉树和线索二叉树

### 6.3.1 遍历二叉树

* 遍历：顺着某一条搜索路径，巡防二叉树中的结点，使每个结点均被访问一次，且仅被访问一次
* 访问：对结点左各种处理
* 遍历目的：得到树中所有结点的一个线性序列

* 两种思想：
  * 层次遍历（队列）
  * 递归算法（栈）

### 层次遍历

* 从上到下、从左到右依次访问结点
* 顺序存储上的实现：
  1. 补全为完全二叉树
  2. 按从小到大遍历、且忽略空结点
  3. 时间复杂度：稀疏二叉树，补全，导致时间浪费
* 链式存储上的实现：
  1. 队列思想
  2. 根入队
  3. 出队时，访问并将子节点从左到右入队

### 递归算法

![image-20210519191909113](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519191909113.png)

* 先序遍历：根->左->右

  ```cpp
  DLR(node *root)
  {
      if(root==NULL) return;
      cout << root->data;
      DLR(root->lchild);
      DLR(root->rchild);
  }
  cout:
  	A B D H C F I J G
  ```

* 中序遍历：左->根->右

  ```cpp
  LDR(node *root)
  {
      if(root==NULL) return;
      LDR(root->lchild);
      cout << root->data;
      LDR(root->rchild);
  }
  cout:
  	D H B A I F J C G
  ```

* 后序遍历：左->右->根

  ```cpp
  LRD(node *root)
  {
      if(root==NULL) return;
      RDL(root->lchild);
      RDL(root->rchild);
      cout << root;
  }
  cout:
  	H D B I J F G C A
  ```

* 备注：先左后右，根的位置确定先、中、后续遍历

#### 例题1

![image-20210519192401808](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519192401808.png)

```markdown
先序遍历：- + a x b - c d / e f		// 前缀表示
中序遍历：a + b x c - d - e / f		// 中缀表示
后序遍历：a b c d - x + e f / -		// 后缀表示
层次遍历：- + / a x e f b - c d
```

#### 算法分析

* 访问路径相同、时机不同：
  1. 第1次经过，就访问，先序遍历
  2. 第2次经过，访问，中序遍历
  3. 第3次经过，才访问，后序遍历

##### 时间复杂度

* O(n): 每个结点只访问一次

##### 空间复杂度

* 所需辅助栈空间的最大容量为k+1,k为深度

* 最好：完全二叉树，k = floor(log_2(n))+1，O(log_2(n))
* 最坏：单支树n，空间复杂度 O(n)
* 说明：此处的最好和最坏是相对而言的。n为固定值，结点的总数。

#### 例题2

![image-20210519193556494](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519193556494.png)

```markdown
先序遍历：a b c d e f g
中序遍历：c b d a e g f
后序遍历：c d b g f e a
```

### 非递归算法

* 目标：不用递归算法，得到三种遍历序列

* 用栈：保存地址信息
* 用栈来模拟函数递归的过程
  * 三种访问顺序中，压栈顺序相同：先左孩子，后右孩子、
  * 每个结点经过3次，哪次进行访问，即对应哪种递归遍历 
* 改进的非递归先/中序算法：
  * 仅考虑先/中序算法时：每个节点经过2次（第二次经过访问后，直接pop掉当前节点，然后push进当前节点的孩子）

![image-20210519195636656](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519195636656.png)

* 改进的复杂度分析：
  * 时间复杂度：O(n)
  * 空间复杂度：
    * 最好情况：右单支：O(1)
    * 最坏情况：左单支：O(n)

* 再次改进的非递归线序算法

![image-20210519200209515](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519200209515.png)

* 复杂度分析：
  * 时间复杂度：O(n)
  * 空间复杂度：最好的情况：右单支、左单支均为O(1)

### 二叉树递归算法的应用举例

1. 求二叉树的深度
2. 统计叶子节点个数
3. 求第k层节点的个数

* 后序遍历

#### 求深度

* depth[i] = 结点i的左子树、右子树最大深度+1

#### 求叶子总数

* 空树：0
* 根是叶子：1
* 左右子树的叶子之和

根：访问的当前节点

#### 求第k层结点的个数

![image-20210519201338045](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519201338045.png)

### 二叉树的重建

![image-20210519201450002](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519201450002.png)

#### 例题：前序、中序建树

```markdown
前序：abcdefg
中序：cbdaegf
```

![image-20210519202259250](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519202259250.png)

![image-20210520212022749](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520212022749.png)

### 关于建树：找竞赛书看看

* 必须知道中序（得到“根左”-“根”-“根右”此三者的唯一的正确的顺序，然后可以进行递归建树）

## 6.3.2 线索二叉树

* 引入：
  * 共n个结点时：有2n个指针，其中只用了(n-1)个（每个结点指向其直接前驱）
  * 于是可以得出：空指针有：n+1个，比较多，可以用起来

![image-20210519222443238](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519222443238.png)

* 此处的 **前驱** 和 **后继** 指的是在 **遍历顺序** 中的 **“前驱”** 和 **“后继”**

 ### 线索二叉树的生成

* 先进行结构体的改进

![image-20210519223951626](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210519223951626.png)

```markdown
* 对空指针的利用：
* 空左：指前驱
* 空右：指后继
```



![image-20210519223107268](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519223107268.png)

![image-20210519223351767](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210519223351767.png)

* 图中叶子的右指针有点小奇怪

```markdown
* 设计技巧：每次只修改前驱结点的右指针（后继）和本结点的做指针（前驱）
* 设置两个指针：p：当前结点；pre：前趋结点
* 遍历二叉树时，对于每个结点p：
​```cpp
if(p->lchild==NULL) p->Ltag=1,p->lchild=pre; // p的前驱线索应在p结点的左边
if(pre->rchild==NULL) pre->Rtag=1,pre->rchild=p; //pre的后继线索在右边
​```
```

#### 生成算法

![image-20210519224353035](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210519224353035.png)

* 先建立一个普通的树，并把均设置为LINK

![image-20210520152207845](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520152207845.png)

* 然后，先导入一个thrt作为整棵树的头头
* 再按照中序进行遍历和设置空结点

![image-20210520155047297](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520155047297.png)

```cpp
void Middle_Travel(bitree T)
{
    bitree p = T->lchild;
    while(p!=T)
    {
        // 当 有 左孩子时，往死里找
        while(p->LTag==LINK) p=p->lchild; 
        cout << p->data;
        // 当 无 右孩子时，rchild中存其后继
        while(p->RTag==THREAD && p->rchild!=T) 
        {
            p = p->rchild;
            cout << p->data;
        }
        p = p->rchild;
    }
}
```

#### 关于线索树的看法

* 在已知访问顺序：前、中、后序的遍历顺序下，通过一定顺序的遍历，定义出他们空节点指向的地址

* 可以减少空间复杂度（不用栈进行递归）

## 树、森林与二叉树的转换

### 树的表示

#### 双亲表示

![image-20210520165343283](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520165343283.png)

* 数据域、指针域
* **子节点** 记录他们 **双亲** 的所在的“脚标”

```cpp
// 节点定义
typedef struct PTNode{
    ElemType data;
    int parent; //双亲位置域
}PTNode;

// 树定义
typedef struct{
    PTNode nodes[MAX_TREE_SIZE];
    int r,n; //r:root，根节点；n:节点总个数
}PTree
```

#### 孩子链表表示（好）

* 每个结点加一个链表，该链表的数据域是其孩子的“数组脚标”

![image-20210520165851487](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520165851487.png)

```cpp
// 每个结点找孩子的链表节点
typedef struct CTNode{
    int child;
    CTNode *next;
}CTNode, *ChildPtr;

// 树节点定义 
typedef struct{
    Elemtype data;
    int parent;
    ChildPtr firstchild;
}CTBox;

//树定义
typedef struct{
    CTBox nodes[MAX_TREE_SIZE];
    int r,n;
}CTree;
```

#### 树的二叉链表（孩子-兄弟）存储表示法

* 树 --> 二叉树：左孩子为节点的长子（左数第一个），右孩子为节点兄弟
* 其结点的结构和普通二叉树的区别在于其命名

```cpp
typedef struct CSNode{
    ElemType data;
    //“左孩子”装“长子”，“右孩子”装“兄弟” 
    CSNode *firstChild, *nextSibling;
}CSNode, *CSTree;
```

### 6.4.2 森林转换二叉树

* 回顾：树->二叉树、二叉树->树

* 森林->二叉树
  * 各自转、依次连接到一个上
  * 森林直变兄弟，在转二叉树

![image-20210520181220859](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520181220859.png)

二叉树->森林

![image-20210520181334313](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520181334313.png)

### 6.4.3 树和森林的遍历

1. 树的遍历

   1. 先序遍历

   - 先访问根节点；
   - 依次先序遍历根节点的每一棵树：可以理解为朴素的深度搜索

   2. 后序遍历

   * 依次后序遍历每一棵子树：最后访问根节点，每个子树亦然
   * 最后访问根节点；

![image-20210520182419244](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\image-20210520182419244.png)

* 利用二叉树进行树的遍历

![image-20210520182921877](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520182921877.png)

2. 森林的遍历

   1. 先序遍历
   * 森林为空，返回
   * 依次对每棵树进行先序遍历
   2. 后序遍历
   * 森林为空，返回
   * 依次对每棵树进行后序遍历

![image-20210520183551286](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520183551286.png)

3. 结论：
   1. 树/森林 **先序** 遍历 = 对应二叉树的 **先序** 遍历
   2. 树/森林 **后序** 遍历 = 对应二叉树的 **中序** 遍历

## 6.6 哈夫曼树与哈夫曼编码

* Huffman树
* Huffman编码

![image-20210520184314417](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520184314417.png)

### 引入

![image-20210520184631873](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520184631873.png)

```markdown
* 说明：
	1. W_k：出现概率，图中 W_k[a]=7, W_k[b]=5;
	2. L_k: 叶子到根的距离，图(a)中 W_k[a]=2, W_k[b]=2
```

### 术语

![image-20210520185156131](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520185156131.png)

```markdown
* 路径：由一节点到零一节点的分支所构成
* 路径长度：路径上的分支数目，例如：从a->e 的路径长度：2，a->f路径长度：4
* 树的路径长度：从 **数根** 到每个节点的路径长度之和
* 带权路径长度：**结点** 到 **根** 的路径长度乘上 结点上的权（WPL）
	// Weighted Path Length
* 树的带权路径长度：树中所有叶子结点的带权路径长度之和
* Huffman树：带权路径长度最小的树
```

### 基本思想

* 权值大的结点，短路经；权值小的结点，长路径。

### Huffman算法

1. 给定n个权值，构成n棵只有根结点的“树”的“森林”，定为集合F
2. 在F中 **取出** 两棵根节点权值最小的树作为左右子树，构建一棵新的二叉树，并将根的权值设定为这两者之和，将产生的 根 **放入** 集合F中
3. 重复2.的过程直到F只含一棵树为止

* 生成的树，只有入度为0和2的结点
* 不成文的规矩：合并时，较小者放在左边

#### 例题：求Huffman编码

![image-20210520190837186](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520190837186.png)

![image-20210520190811797](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520190811797.png)

* 上例中WPL = 2.61

### Huffman编码

* 可以编码压缩，广泛用于多媒体编码中

![image-20210520191120070](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520191120070.png)

#### 译码

* 0：左，1：右
* 到叶子结点：译出字符，并返回根继续

#### 唯一性

* 前缀编码
* ![image-20210520191435680](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520191435680.png)

编码两个问题：

	1. 数据的最小冗余
	2. 译码的唯一性

### Huffman树的实现

```cpp
1.结点结构定义：
    |weight|parent|lchild|rchild|
2. 存储结构：
    顺序存储：结点存在HT[],对应的编码存在HC[]
```

建立树：

```markdown
* Huffman树没有度为1的点
* 一棵有n_0个叶子结点的Huffman树共有：
	n个结点：n=n_0+n_2，
	又因为：n-1 = 2 * n_2 + 0 * n_0;
	所以：n = 2 * n_0 + 1；
* 存储在大小为 n = 2 * n_0 - 1 的一维数组中
```

```cpp
typedef struct{
    unsigned int weight;
    unsigned int parent, lchild, rchild;
}HTNode, *HuffmanTree; // 动态分配数组，存储哈夫曼树
typedef char **HuffmanCode;
```

![image-20210520192309693](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520192309693.png)

![image-20210520193114738](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520193114738.png)

![image-20210520193706118](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520193706118.png)

* 找某个叶子的编码：从子往上找，判断其是其父节点的“左”孩子，还是“右”孩子，确定这个部分是1还是0。

![image-20210520193859377](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520193859377.png)

## 小结



* 2叉链表和3叉链表区别：3叉多记录了一个父节点

* 遍历
  * 森林的先序 = 对应二叉树先序
  * 森林的后续 = 对应二叉树中序

![image-20210520194310019](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Tree.assets\image-20210520194310019.png)
