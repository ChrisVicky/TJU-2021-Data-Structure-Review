# Array

## 概念

* 数组：由一组名字相同、下标不同的变量构成
*  特点：
  * 各个元素有统一的类型
  * 下标有固定的上界、下界；定义后，维数、维界不改变
  * 基本操作简单
    *  初始化、销毁、存取元素、改变元素值
* 二维数组：A_m*n：m行，n列

* ！！N 维数组的特点：n个下标，可以看作由若干个 n-1 维数组组成的线性表

![image-20210519143000445](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519143000445.png)

## 数组的顺序存储表示和实现

* 按照**次序**将数组元素排成 **一列** 序列
  * 例如：按列存储、按行存储
* ![image-20210519143253074](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519143253074.png)

* **求地址：**
  * 开始结点
  * 维数、每个维的上下界
  * 每个元素占的单元数
* ![image-20210519143502468](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519143502468.png)

 ### 练习题：求地址

![image-20210519143807402](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519143807402.png)

```mark
行下标从1到6，则共6行；
列下标从0到7，则共8列；
每个占6字节，则共：(6-1+1)*(7-0+1)*6 = 288
```

![image-20210519143836808](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519143836808.png)

```mark
Loc(a_{ij})=Loc(a_{11})+[(j-1)*m + (i-1)]*K
```

![image-20210519144138215](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519144138215.png)

```mark
Loc(a[32][58]) = Loc(a[1][1]) + 2*[60*(58-1)+(32-1)] 
=2048 + 2*(3420+31)
=2048 + 6902
=8950
```

### 三维数组的地址求法


$$
已知：A[x_0...x_n][y_0...y_n][z_0...z_n]
$$
$$
已知：A[x][y][z] 的地址 base，且单位长度 t
$$
$$
求 A[i][j][k]
$$
$$
A[i][j][k] = base + t * [(i-x) * (x_n - x_0 + 1) * (y_n-y_0+1)*(z_n-z_0+1) + (j-y)*(z_n-z_0+1)+(k-z)]
$$



### 顺序存储表示

```cpp
#define MAX_ARRAY_DIM 8 // 假设维数8
typedef struct{
    ElemType *base; // 数组元素基址
    int dim; // 数组维数
    int *bound; // 数组各维长度信息保存区基址
    int *constants; // 数组映像函数常量的基址
}Array;
```

### 补充：数组的链式存储方式

* 用带行指针向量的单链表来表示。

## 矩阵的压缩存储

* 压缩存储
  * 若多个数据元素的 **值** 都相同，则只分配一个元素值的存储空间，且零元素不分配存储空间
* 特点：对称、对角、三角、稀疏矩阵
* 稀疏矩阵：非零元素的个数较少（一般小于 5% )

### 稀疏矩阵的压缩存储

* 将每个非零元素用一个 **3元组** (i, j, a_ij) 里表示
  * i：行下标
  * j：列下标
  * a_ij：元素值

#### 法一：线性表

![image-20210519150131661](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519150131661.png)

#### 法二：十字链表

![image-20210519150217944](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519150217944.png)

#### 法三：三元数组矩阵

![image-20210519150300721](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519150300721.png)

* 针对具体问题、逻辑关系，选择是否压缩、如何压缩

* **失去随机存取的功能**：如何解决？

#### 法四：带辅助向量的三元组

* 用途：更高效地访问

* 方法：增加2个辅助向量

  * 记录 **每行** 非0个数：NUM[i]

  * 记录 **每行** 第一个非0元素在 **三元组** 中的行号：POS[i]

  * ```cpp
    pos[1]=1;
    pos[i]=pos[i-1] + num[i-1];
    ```

![image-20210519151429363](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519151429363.png)

### 稀疏矩阵的操作

#### 矩阵转置

* 已知：原三元组
* 求解：转置后的三元组

* 步骤
* 1. 每个元素行下标、列下标互换
  2. T总行数(mu)和总列数(nu)交换
  3. 重排三元组内个元素顺序

##### 法1.压缩转置

![image-20210519153633669](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519153633669.png)

##### 法2.快速压缩转置

![image-20210519153808080](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519153808080.png)

###### 设计思路：辅助向量

![image-20210519154046338](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519154046338.png)

![image-20210519154221054](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519154221054.png)

#### 分析时间复杂度

![image-20210519154531288](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519154531288.png)

 

## 数组和广义表(Arrays & Lists)

* 特点：特殊的线性表
* 元素的值，并非原子类型，可再分解
* 所有元素仍属于同一类型

### 广义表的定义

![image-20210519162926632](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519162926632.png)

* 小写字母：原子类型
* 大写字母：列表
* 第一个元素：表头
* 其余元素组成的表：表尾

#### 广义表与线性表区别

* 广义表：元素既可以是原子，也可以是列表
* 线性表：每个元素都是原子类型，且相同

#### 特点

* 次序性：前驱、后继
* 长度：表中个数
* 深度：括号的重数
* 递归：自己作为自己的子表
* 共享：略

##### 例1：求广义表的长度

```markdown
1) A = ()	n=0, A是空表
2) B = (e)	n=1, 表中元素e是小写，原子
3) C = (a, (b, c, d))	n=2, a为原子, (b,c,d) 为子表
4) D = (A, B, C)	n=3, 3个元素都是子表
5) E = (a, E)	n=2, a为原子，E为子表
```

##### 例2：求深度

![image-20210519164447288](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519164447288.png)

##### 例3：GetHead, GetTail

![image-20210519164631102](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519164631102.png)

* 备注：
  * Head指的第一个元素
  * Tail为除第一个以外的部分，组成的列表（看第3题）

### 广义表的存储结构

![image-20210519165208149](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519165208149.png)

* 链表
  * ![image-20210519164952793](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519164952793.png)
  * ![image-20210519165047792](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519165047792.png)

* 例：
  * ![image-20210519165102747](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210519165102747.png)

