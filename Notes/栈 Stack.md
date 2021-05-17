# 栈 Stack

- 有一定操作限制的线性表
- 只在一端进行插入和删除
  - Push 插入，入栈
  - Pop 
  - Last in first out (LIFO)

![image-20210517145652807](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517145652807.png)

#### Push & Pop

![image-20210517145729638](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517145729638.png)

## 栈的顺序存储实现

<img src="C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517150104367.png" alt="image-20210517150104367"  />

注意：这里的Data[]为一个数组  

### 思考题：用一个数组实现两个栈

![image-20210517161953738](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517161953738.png)

**满栈条件：Top1 == Top2**



## 栈的链式存储

* 在表头进行插入/删除

### Create Stack

![image-20210517162420296](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517162420296.png)

* 每次Push相当于插入，记得申请新空间

**<img src="C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517162948849.png" alt="image-20210517162948849"  />**

<img src="C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517162928509.png" alt="image-20210517162928509"  />

栈的应用

<img src="C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517163216307.png" alt="image-20210517163216307"  />

#### 表达式求值

![image-20210517164544547](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517164544547.png)

* 中缀表达式 --> 后缀表达式：
  * 栈

![image-20210517165003132](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517165003132.png)

例子：

![image-20210517165147004](C:\Users\Christopher Liu\AppData\Roaming\Typora\typora-user-images\image-20210517165147004.png)



