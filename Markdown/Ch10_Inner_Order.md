# Order

* 定义
  * 内部排序：在内存中排序
  * 外部排序：待排数据放在外部存储器，需要内外存交换数据

* 排序效率 和待排数据的数据结构 密切相关
  * 集合
  * 数型结构
* 数据表：有限集合
* 稳定性：排序不改变“关键码相同”的对象的顺序
* 排序的时间、空间开销：时间复杂度、空间复杂度



* 插入排序
* 交换排序
* 选择排序
* 归并排序
* 基数排序

## 10.1 插入排序

* 基本方法：每一步，将待排对象插入到一组已排好序的对象的相应位置

### 直接插入排序

* 按顺序逐一进行比较，得到插入位置。

* 分析：
  * 若共有n个，则进行n-1次插入
  * 最好的情况下：每趟只需与前面的有序对象序列的最后一个对象比较1次，移动2次对象。插入n个元素，比较n-1次，总移动次数2(n-1)
  * 最坏的情况下：第i趟插入时，第i个对象和前面i-1个对象进行比较，且每次比较需要移动1次数据，则总的次数为：n^2/2
  * ![image-20210522023838120](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522023838120.png)
  * 直接插入排序的时间复杂度：O(n^2)，考虑一般情况
  * 稳定排序

#### 折半插入排序

* 对直接插入排序进行改进
* 通过排序过程中部分元素有序，提高排序效率
* 分析：
  * 查找快
  * 比较次数：n * log_2(n) 次
  * 也是稳定的

#### 表插入排序

* 目标：
  * 减少插入排序中，移动元素的次数
  * 尽量不移动元素
* 方法：链式存储（静态链表）

* 和表头形成循环链表
  * ![image-20210522024628243](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522024628243.png)
  * ![image-20210522024705038](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522024705038.png)

#### 希尔排序

* 基本思想：分组排序，每组分别进行直接插入排序
* 对于数据量大时挺好用

* 基本过程：
  * n个对象，先取一个整数gap<n为间隔，
  * 所有距离为gap的对象在同一个子序列中，每个子序列中分别进行直接排序
  * 缩小gap，重复，直到gap取1
    * gap为“脚标”之差
  * ![image-20210522025242252](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522025242252.png)

* 时间复杂度：平均为 n^{1.25} ~ 1.6 * n^{1.25} 

## 10.2 交换排序

### 冒泡排序

* 分析：
  * 最好的情况：n-1次比较，不移动
  * 最坏的情况：比较次数1/2 * n(n-1)，移动次数：3/2 * n(n-1)
* 稳定的排序
* O(n^2)的时间复杂度

### 快速排序

* 基本思想：
  * 在冒泡排序上进行大范围交换
  * 采用：树形结构，借鉴二叉树的思想
  
* 基本过程：
  * 任取一个对象，作为基准
    * 左侧序列都小于或等于基准
    * 右侧都大于基准
  * 对两个子序列重复上述方法
  * 类似于二叉排序树，在一维数组层次上进行存储或者处理
  * 设定i，j指针先后扫描，当i==j时扫描结束；扫描期间，大者向后，小者向前
  * ![image-20210522031205132](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522031205132.png)
  * ![image-20210522031904035](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522031904035.png)

#### 快速排序分析

  * 快排是递归的，需要使用栈
  * 可以证明：quicksort的平均计算时间：O(n * log_2(n))
  * 最大递归调用层于递归树的深度一样，存储开销：O(log_2(n))
  * 每次划分确定对象定位后，左侧序列和右侧序列长度相同，则下一步将是对两个长度减半的子序列进行排序，理想，趟数最少
  * 最坏的情况：比较次数：n^2 / 2
    * 例如 **从小到大** 的已经排好的序列
    * 递归树成为单支树
    * 改进：选基准元素：中间的而不是两端的
* 快排是 **不稳定** 的排序方法
* 小结：
  * T(n) = O(n * log_2(n))
  * S(n) = O(log_2(n))
  * 不稳定

## 10.3 选择排序

* 基本思想：每一趟在n-i个待排序对象中，选出最小/最大的对象

### 直接选择排序

* 算法分析：
  * 重复扫描，时间复杂度：O(n^2)

### 锦标赛排序

* 基本思想：
  * 消除直接排序中的重复扫描
  * 采用树形结构进行选择排序
  
* 具体过程：
  * 进行两两对比，直到决出“冠军”，比较次数为 n-1
  * ![image-20210522033247382](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522033247382.png)

  

#### 锦标赛排序分析
  * 满 完全二叉树，深度为 上取整(log_2(n)) + 1
  * 时间复杂度：O(n * log_2(n))
  * 需要附加空间：n-1个结点
  * 稳定
  * ![image-20210522033501795](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522033501795.png)
  * 小结：
      * T(n) = O(n * log_2(n))
      * S(n) = O(n)
      * **稳定**

### 堆排序

* 基本目标：
  * 实现 O(n * log_2(n))的时间复杂度
  * 减少/消除辅助空间
* 基本思想：
  * 二叉树中所有节点均存放在待排元素，而不是只将待排元素放在叶子节点上

1. 堆的概念

   ![image-20210522033957119](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522033957119.png)

   * 本质：在逻辑上看成一棵完全二叉树，所有节点的值均大于(小于 )其左右节点的值，于是，根节点就是“冠军”
   * 大根堆 / 小根堆
   * 如何建这个堆呢？

2. 流程

   ![image-20210522034438222](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522034438222.png)

3. 堆的建立

   * 先思考输出顶点后如何调整：

   * 轻的下沉，重的上浮

     ![image-20210522034632996](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522034632996.png)

     ![image-20210522034857015](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522034857015.png)

   * 算法流程

     ![image-20210522035221034](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522035221034.png)

   * 堆的建立：

     * 从最后一个非终端节点开始，往前逐步调整，直到根节点为止，序列变为堆。

4. 基于初始堆的堆排序

![image-20210522035803131](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522035803131.png)

![image-20210522035828107](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522035828107.png)

![image-20210522035902999](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522035902999.png)

![image-20210522035926355](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522035926355.png)

* 堆排序算法

![image-20210522040039794](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522040039794.png)

#### 堆排序分析

* 调用 n-1 次 HeapAdjust()，时间复杂度O(n * log_2(n))
* 附加存储，在for里面的临时空间，空间复杂度为 O(1)
* 堆排序，不稳定
* 小结：
  * T(n) = O(n * log_2(n))
  * S(n) = O(1)
  * **不稳定**

## 10.5 归并排序

* 基本思路：就是MergeSort

  ![image-20210522040554113](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522040554113.png)

#### 归并排序分析
  * T(n) = O(n* log_2(n))
  * 归并时用栈
  * **稳定**
  
  * 小结：
    * T(n) = O(n * log_2(n))
    * S(n) = O(n)
    * **稳定**

## 10.6 基数排序

* 基本思想：关键字不同的“位值”进行排序
  * 例如扑克牌：先用“花色”排序，再用“面值”排序
* 假定n个对象序列：{ V_0, V_1 ...... V_{n-1} }

* 进队列扫描，出队列即可排序

* 算法分析：

  ![image-20210522043355289](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522043355289.png)

## 排序方式的比较

![image-20210522043422601](C:\Christopher\TJU\转专业\Preparation\数据结构学习笔记\data_-structure_-source\Notes\Ch10_Inner_Order.assets\image-20210522043422601.png)

