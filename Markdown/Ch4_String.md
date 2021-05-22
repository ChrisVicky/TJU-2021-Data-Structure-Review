# String

## 引言

#### 串

* 逻辑结构：
  * s='a1a2....an'
* 存储结构：
  * **定长** **顺序** 存储结构
  * **堆** 存储结构
  * **块链** 存储结构
* 操作：
  * 若干函数
  * 模式匹配算法

#### 模式匹配

* 即子串定位算法，如何实现 Index(S, T, pos) 函数
* 精华：KMP算法——快速查找(用 next[j] 或 nextval[j] )

## 串类型的定义

* 有限
* 数据元素为：单个字符
* 隐含结束符 **'\0'** 即 **ASCII** 码NULL
* 为何单独讨论串：
  * 比其他数据类型更复杂
  * 程序设计中处理对象很多是串类型

### 若干术语

* 串长：字符个数，n=0时为空串
* 空格（白）串：由 **空格符** 组成
* 空串与空格串：
  * ![image-20210518151532666](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518151532666.png)

* 子串：给定串S中，任意连续的字符序列，S叫 **主串** 
* 字串位置：子串的 **第一个** 字符在主串中的序号
* 字符位置：字符的序号
* 串相等：串长度相等，且对应位置字符相等
* ![image-20210518151809030](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518151809030.png)

### 串的抽象数据类型定义

![image-20210518151946223](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518151946223.png)

![image-20210518152143188](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518152143188.png)

* 习题

![image-20210518152841188](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518152841188.png)

* 注意：
  * t 的长度为4，不是 5 
  * 但实验出来，string s = "good "; cout << s.length() << endl; 出来是 5 
  * Index(s, 'A') 是 3 不是 2，即第 3 个

## 串的表示和实现

* 与 **线性表** 不同，是以 **串的整体** 作为操作对象

* 三种机内**表示**方法
  * 顺序存储
    * 定长的顺序存储表示
      * **地址连续**
      * **静态存储**
    * 堆分配存储表示
      * **地址连续**
      * **动态分配**
  * 链式存储
    * 链式

### 顺序存储

#### 数组·定长

```cpp
#define MAXstrlen 255
typedef unsigned char SString[MAXstrlen + 1] SString;
	SString s;
```

说明：

* 一般用 **SString[0]** 来存放 **串长** 信息
* **C** 语言约定：在串尾加结束符 **'\0'** ，以利操作加速，但不计入串长
* （用 **首址**和**串长** 或 **首址**和**尾标记 **来描述串数组）
* 若字符串超过 MAXstrlen 则自动截断（静态数组存不进去嘛）

#### 堆·动态分配

```cpp
typedef struct{
    char *ch; //若 不是 空串，按串长分配空间，否则ch = NULL;
    //结合下面的 示例， ch 是指向这个连续物理空间的首地址的
    int length; //串长度
}HString
```

* 特点：仍用 **连续存储单元** ，存储空间在执行过程中 **动态分配** 
  * 动态分配的一个简单例子：
    * 原有1~10的10个地址，现在需要20个且第11个为已占用地址；于是重新分配一片连续的20个地址。

![image-20210518161311450](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518161311450.png)

![image-20210518161207861](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518161207861.png)

**malloc：**在内存中分配一段连续的存储空间

* 建堆

![image-20210518190213638](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518190213638.png)

* 插入字符串

![image-20210518190415929](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518190415929.png)

**注意**：使用了 **realloc** 函数，**重新分配** 了 ( S.length + T.length ) 个空间

### 链式存储

* 易于删除和插入
* 块链结构
  * 多个字符存入一个结点里

![image-20210518192335425](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518192335425.png)

#### 块链类型定义

```cpp
#define CHUNKSIZE 80 // 每块的大小
//定义结点类型
typedef struct Chunk{ 
    char ch[CHUNKSIZE]; //数据域 
    struct Chunk* next; //指针域
}Chunk;

//块链表的整体描述
//定义 串 类型
typedef struct{
    Chunk *head; // 头指针
    Chunk *tail; // 尾指针
    int curLen; // 结点个数
}LString;
```

![image-20210518192416977](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518192416977.png)

## 串的模式匹配算法

* Word 文档查找

* 算法目的：确定主串中所含子串第一次出现的位置
  * 典型函数：Index(S, T, pos)：从S的pos位置开始找
* 算法类型：
  * **BF算法** ：速度慢，带回溯（古典的、经典的、朴素的、穷举的）
  * **KMP算法**：避免回溯，匹配速度快

### BF的实现

```cpp
//BS算法匹配
int Index(const SString& S, const SString& T, const int& pos)
{/*返回子串T在主串S中的第pos个字符之后的位置。若不存在，则函数值为0。其中T非空且 1<=pos<=Strlen(S)*/
    int q = pos, p=1;
    while(q + T[0] <= S[0] && p <= T[0])
    {
        //如果i，j两指针在正常范围
        if(S[i]==T[i]){
            ++i;
            ++j;
        }else{
            i = i - j + 2； // 相当于是 i 从下一个开始
            j = 1;
        }
    }
    if(p > T[0]) return i - T[0]; // T子串指针p正常到队尾，匹配成功
    return 0; //否则i先到尾就不正常
}
```

#### 分析时间复杂度

主串长n，子串长m，可能匹配成功的位置 i = ( 1 ~~ n-m+1 )

* 最好的情况
  * 在第 i 个位置匹配成功，且前 i - 1 个位置都仅匹配了 1 次
  * 第 i 个位置总共比较了 ( i - 1 + m ) 次
  * 则有如下公式

$$
平均比较次数=\sum_{i=1}^{n-m+1} p_i(i-1+m)={1\over n-m+1}\sum_{i=1}^{n-m+1}(i-1+m)={1\over 2}(m+n)
$$

$$
最好的情况：平均时间复杂度=O(n+m)
$$



* 最坏的情况
  * 在第 i 的位置匹配成功，且前 i - 1 个位置都匹配了 m 次
  * 第 i 个位置总共比较了 ( i * m ) 次
  * 则有如下公式，

$$
平均比较次数=\sum_{i=1}^{n-m+1}p_i(i\times m)={m\over n-m+1}\sum_{i=1}^{n-m+1}i={1\over 2}m(n-m+2)
$$

$$
设n>>m,最坏的情况下,平均时间复杂度=O(n*m)
$$

### KMP算法

#### 基本思想

* 失配时，i 不变，j 回溯到 k 的位置
* j 和 k 满足："T_1 T_2 ...... T_k-1" == "T_j-(k-1) ...... T_j-1"
  * **j和k** 分别是**匹配串T**的 **子串** 中的  **前后两个** **相等** 的 **子串** 的 **尾结点** 的下标 +1
  * i 是S的指针，不回溯

![image-20210518204038779](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518204038779.png)

* 建立 k = next[j] 的函数关系
* **重点：** 对 next[j]的求解：KMP算法的实现

![image-20210518204531727](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518204531727.png)



#### next[j] 的物理意义

```mark
Next[j]函数表征 模式 T 中 最大的 相同前缀子串 和 后缀字串（真子串）的长度
备注：
1. 关于k的，前缀子串，一定是从下标 1 开始的
2. 模式中相似部分越多，则 next[j]函数越大，即模式 T 字符之间 相关度 越高，也表示第j个位置以前与主串部分匹配的字符数越多
```

![image-20210518204929890](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518204929890.png)

#### next[j] 如何求解

```markd
* 当j=1时，next[j]=0; // next[j]=0 表示不进行字符比较
* 当j>1时，next[j]的值为：模式串的位置从1到j-1构成的串中，所出现的，**首尾相同的子串** 的 **最大长度** 加1 // 无首尾相同的子串时 next[j]=1, 此时从头部开始比较
```

![image-20210518210020775](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518210020775.png)

##### 例题：求next[j]

![image-20210518210552700](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518210552700.png)

#### ！！编程求解next[j]

* 递推方式

```cpp
void get_next(SString T, int &next[]){
    //求模式串T的next函数并放入next中
    i = 1;
    j = 0;
    next[1]=0;
    while(i<T[0]){//T[0]为T串的长度
        if(j==0 || T[i]==T[j])
        {
            ++i,++j;
            next[i]=j;
            continue;
        }
        j=next[j];
    }
}
```

![image-20210518212625109](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518212625109.png)

#### KMP算法的实现

```cpp
Int Index_KMP(SString S, SString T, int pos)
{
    //KMP的Index算法
    i = pos;
    j = 1;
    while(i < S[0] && j < T[0])
    {
        if(S[i] == T[j] || j==0)
        { // 不失配则继续比较后续字符
            ++i,++j;
            continue;
        }
        j = next[j]; // 特点：S的i指针不回溯，而且从T的k位置开始匹配
    }
    if(j > T[0]) return i - T[0]; // i - j + 1; 也是可以的
    return 0;
}// Index_KMP
```

#### KMP的时间复杂度分析

* 最恶劣的情况
* 由于指针 i 无须回溯，比较次数仅为 n ，算 next[j] 时所用的比较次数 m，比较总次数为 n+m = O(n+m)
* BF在一般情况下，时间复杂度也近似于 O(n+m)，仍被广泛使用

#### KMP算法的用途

* 主串指针 i 不必回溯，从外存输入文件时，可以边读入，边查找，流水作业

### 清华版 KMP

#### 大体思路、示意图：

![image-20210518222301651](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518222301651.png)

#### 定义 next 函数

![image-20210518222811206](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518222811206.png)

##### 一些含义

````markdown
* next[7] = 2;
	则 't_6 t_7' = 't_1 t_2'
* next[8] = 3;
	则 't_6 t_7 t_8' = 't_1 t_2 t_3'
````

#### 求解 next 函数的思路

* 递推过程 

```markdown
* 已知：next[1] = 0;
* 假设：next[j] = k; 
	且 T[j] = T[k];
* 则：next[j+1] = k+1;
* 若：T[j] != T[k];
* 则：使 j = next[j]，直到 T[j] = T[k]，相当于模板串的子串和自己进行匹配;
```

![image-20210518230022143](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518230022143.png)

#### NEXT_VAL: next的改良

![image-20210518230918816](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210518230918816.png)

```markdown
next[j] = k; 当T[j]!=T[k]时;
next[j] = next[k]; 当T[j] == T[k]时;

* 说明：next是用来当 T[j] != S[i]时，重新给j赋值的
	所以当 T[j] = T[next[j]=k]时，浪费了一次对比时间
	这样可以有效地提高效率
```

````cpp
void get_next_new(SString T, int &next[]){
    //求模式串T的next函数并放入next中，求的是next_new
    i = 1;
    j = 0;
    next[1]=0;
    while(i < T[0]){//T[0]为T串的长度
        if(j==0 || T[i]==T[j])
        {
            ++i; ++j;
            if(T[j] == T[i]) next[i] = next[j];
            else next[i]=j;
        }
        else j=next[j];
    }
}
````

### 一些关于next[]的感想

1. 从效率的角度上来看，next越小，移动越快，效率越高
2. 从保险、不漏的角度看，next越大，越保险
