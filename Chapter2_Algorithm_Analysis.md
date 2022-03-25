# 第2章  算法分析

1. 算法（algorithm)是为求解一个问题需要遵循的、被清楚地指定的简单指令的集合。

2. 对于一个问题，一旦某种算法给定并且（以某种方式）被确定是正确的，那么重要的一步就是确定该算法将需要多少诸如时间或空间等的资源量的问题

3. 在这一章，将讨论：
   
   + 如何估计一个程序所需要的时间
   
   + 如何将一个程序的运行时间从天或年降低到少于1秒
   
   + 盲目使用递归的后果
   
   + 使一个数自乘得到其幂，以及计算两个数的最大公因数的非常有效的算法





## 2.1  数学基础

1. 估计算法资源消耗所需的分析一般说来是一个理论问题 ，因此需要一套正式的系统框架

2. 下列4个定义：
   
   ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-24-20-05-32-image.png)

3. 这些定义的目的是要在函数间建立一种相对的级别。

4. 比较它们的**相对增长率（relative rates of growth）**

5. **大O标记法（Big-Oh notation)** 
   
   ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-24-20-37-54-image.png)

6. 重要结论：
   
   ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-24-20-56-38-image.png)

7. 注意：
   
   + 不要将常数或低阶项放进大O
   
   + ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-24-20-58-05-image.png)
   
   + 极限不存在：二者无关



### 2.2  模型

模型机是一台标准的计算机，在机器中指令被顺序地执行。模型机做任一简单的工作都恰好花费一个时间单元



### 2.3  要分析的问题

1.  要分析的最重要的资源一般说来就是运行时间。

2. 典型的情形是，输入的大小是主要的考虑方面

3. 主要考虑平均情形和最坏情形的运行时间

4. 一般说来，若无其他的指定，则所要求的量是最坏情况的运行时间

### 2.4  运行时间计算

1. 为了简化分析，采纳如下的约定：
   
   + 不存在特定的时间单位
   
   + 抛弃一些前导的常数
   
   + 抛弃低阶项

2. 实际要做的是计算大O运行时间



### 2.4.1 一个简单的例子

1. ```cpp
   int sum(int n)
   {
       int partialSum;
   
   1    partialSum = 0;
   2    for (int i = 1; i <= n; ++i)
   3        partialSum += i * i * i;
    4   return partialSum;
   }   
   // 第1行和第4行各占一个时间单元
   // 第3行每执行一次占用4个时间单元（两次乘法、一个加法和一次赋值）
   // 而执行n次共占用4n个时间单元
   // 第2行在初始化i、测试i<=n和对i的自增运算隐含着开销
   // 初始化1个时间单元、测试为n+1个时间单元，自增运算n个时间单元，共2n+2个时间单元
   // 忽略调用函数和返回的开销，得到总量是6n+4
   // 该函数时O(n)
   ```

### 2.4.2 一般法则

+ 法则1——for循环
  
  + 一个for循环的运行时间至多是该for循环内部那些语句（包括测试）的运行时间乘以迭代的次数

+ 法则2——嵌套的循环
  
  + 从里往外的分析。在一组嵌套循环内部的一条语句总的运行时间为该语句的运行时间乘以该组所有的for循环的大小的乘积
  
  + ```cpp
    for (i = 0; i < n; ++i)
        for (j = 0; j < n; ++j)
            ++k;
    // O（n^2)
    ```

+ 法则3——顺序语句
  
  + 将各个语句的运行时间求和即可（这意味着，其中的最大值就是所得的运行时间）
  
  + ```cpp
    for (i = 0; i < n; ++i)
        a[i] = 0;
    for (i = 0; i < n; ++j)
        for (j = 0; j <  n; ++j)
            a[i] += a[j] + i + j;
    
    // 先是O(n),接着是O(n^2),所以总量是O(n^2)
    ```

+ 法则4——if/else语句
  
  + ```cpp
    if(condition)
        S1
    else
        S2
    ```
  
  + 一个if/else语句的运行时间从不超过判断的运行时间再加上S1和S2中运行时间长者的总的运行时间。
  
  + 显然在某些情况下这么估计有些过高，但绝不会估计过低

+ 分析的基本策略
  
  + 从内部（或最深层部分）向外展开工作的。
  
  + 如果有函数调用，那么这些调用要首先分析。
  
  + 如果有递归函数，那么 存在几种选择。
    
    + 若递归实际上只是被表象掩盖的for循环，则分析通常是很贱的
    
    + ```cpp
      long factorial(int n)
      {
          if (n <= 1)
              return 1;
          else
              return n * factorial(n - 1);
      }
      
      // 实际上就是一个简单的循环，从而其运行时间为O(n)
      ```
  
  + 当递归被正常使用时，分析将设计求解一个递推关系，它对队规使用的效率低的可怕
    
    ```cpp
    long fib(int n)
    {
        if ( n <= 1)
            return 1;
        else
            return fib(n - 1) + fib(n - 2);    
    }
    // 令T(n)为调用函数fib(n)的运行时间
    // 如果n = 0或n = 1,则运行时间是某个常数值，即第1行做判断以及返回所用的时间
    // 因为常数并不重要，所以可以说T(0)=T(1)=1
    // 若n > 2,则执行该函数的时间是第一行上的常数工作加上第3行上的工作。
    // 第3行由一次加法和两次函数调用组成
    // 由于函数调用不是简单的运算，必须用他们自己来对它们进行分析
    // 第一次函数调用是fib(n-1),需要T(n-1)
    // 类似可证，第二次函数调用需要T(n-2)
    // 总的时间需求为T(n-1)+T(n-2)+2, 2指的是第1行的工作和第3行的加法。
    // 由归纳法容易证明T(n)>= fib(n)
    // 可以证fib(n) < (5/3)^n,类似，n>4,fib(n) >= (3/2)^n
    // 故这个程序的运行时间以指数的速度增长，这是最坏的情况
    // 通过保留一个简单的数组并使用一个for循环，运行时间可以显著降低
    // 这个程序之所以慢，是因为存在大量多余的工作要做
    
    ```

### 2.4.3  最大子序列和问题的求解

1. 算法1，穷举式地尝试所有的可能。
   
   ```cpp
   /**
    * 最大相连子序列和的立方级（即三次的）算法
    * 
    */
   int maxSubSum1(const vector<int> & a)
   {
   
       int maxSum = 0;
   
       for (int i = 0; i < a.size(); ++i)
           for (int j = i; j < a.size(); ++j)
           {
               int thisSum = 0;
   
               for (int k = i; k <= j; ++k)
                   thisSum += a[k];
   
               if (thisSum > maxSum)
                   maxSum = thisSum;
           }
   
       return maxSum;
   } 
   // 运行时间为O(n^3)
   ```

2. 算法2
   
   ```cpp
   // 可以通过撤除一个for循环来避免3次的运行时间
   /**
    * 最大相连子序列的平方级(即二次的)算法
    * 
    */
   int maxSubSum2(const vector<int> & a)
   {
       int maxSum = 0;
   
       for (int i = 0; i < a.size(); ++i)
       {
           int thisSum = 0;
           for (int j = i; j < a.size(); ++j)
           {
               thisSum  += a[j];
   
               if (thisSum > maxSum)
                   maxSum = thisSum;
           }
       }
   
       return maxSum;
   }
   ```

3. 算法3——递归和相对复杂的O(nlogn)解法
   
   1. 要是没有出现O(n)(线性的)解法，这个算法就会是体现递归威力的极好的范例了
   
   2. 该方法采用一种“分治（divide-and-conquer)"策略。其想法是把问题分成两个大致相等的子问题，然后递归第对它们求解。这是”分“的部分。“治”阶段将两个子问题的解修补到一起并可能再做些小量的附加工作，最后得到整个问题的解
   
   3. 最大子序列和可能在3处出现：
      
      + 整个出现在输入数据的左半部
      
      + 整个出现在右半部
      
      + 跨越输入数据的中部从而位于左右两半部分之中。
   
   4. 前两种情况跨越递归求解。第三种情况的最大和可以通过求出前半部分（包含前半部分最后一个元素）的最大和以及后半部分（包含后半部分第一个元素）的最大和而得到。然后将这两个和加在一起。
      
      ```cpp
      /**
       * 相连最大子序列和的递归算法
       * 找出生成[left..right]的子数组中的最大和
       * 不试图保留具体的最佳序列
       */
      int maxSumRec(const vector<int> & a, int left, int right)
      {
          if(left == right)   // 基准情形
              if(a[left] > 0)
                  return a[left];
              else    
                  return 0;
      
          int center = (left + right) / 2;
          int maxLeftSum = maxSumRec(a, left, center);
          int maxRightSum = maxSumRec(a,center + 1, right);
      
          int maxLeftBorderSum = 0, leftBorderSum = 0;
          for (int i = center; i >= left; --i)
          {
              leftBorderSum += a[i];
              if (leftBorderSum > maxLeftBorderSum)
                  maxLeftBorderSum = leftBorderSum;
          }
      
          int maxRightBoarderSum = 0, rightBoraderSum = 0;
          for (int j = center + 1; j <= right; ++j)
          {
              rightBoraderSum += a[j];
              if (rightBoraderSum > maxRightBoarderSum)
                  maxRightBoarderSum = rightBoraderSum;
          }
          return max3(maxLeftSum, maxRightSum, 
                      maxLeftBorderSum + maxRightBoarderSum);
      
      }
      
      /**
       * 相连最大子序列和分治算法
       * 的驱动程序
       */
      int maxSubSum3( const vector<int> & a)
      {
          return maxSumRec(a, 0, a.size() - 1);
      }  
      // O(nlogn)
      ```

4. 算法4——
   
   ```cpp
   /**
    * 线性时间最大相连子序列和算法 
    */
   int maxSubSum4(const vector<int> & a)
   {
       int maxSum = 0, thisSum = 0;
   
       for (int j = 0; j < a.size(); ++j)
       {
           thisSum += a[j];
           
           if (thisSum > maxSum)
               maxSum = thisSum;
           else if (thisSum < 0)
               thisSum = 0;
       }
   
       return maxSum;
   } 
   // 如果a[i]是负的，那么它不可能代表最优序列的起点。
   // 类似地，任何负的子序列不可能是最优子序列的前缀。
   // 如果在内循环中检测到从a[i]到a[j]的子序列是负的，那么可以推进i
   // 还可以把它一直推进到j+1
   // 令p为i+1和j之间的任意下标
   // 开始于下标p的任意子序列都不大于在下标i开始并博涵从a[i]到a[p-1]的子序列的对应
   // 的子序列，因为后面这个子序列不是负的
   // j是使得从下标i开始其值称为负值的序列的第一个下标
   ```

5. 该算法的一个附带的优点是，它只对数据进行一次扫描，一旦a[i]被读入并被处理，它就不再需要被记忆。因此，如果数组在磁盘上或通过互联网传送，那么它就可以被顺序读入，在主存中不必存储数组的任何部分。不仅如此，在任意时刻，算法都能被它已经读入的数据给出子序列问题的正确答案。具有这种特性的算法叫做**联机算法（on-linealgorithm）**



### 2.4.4  运行时间中的对数

1. 分析算法最混乱的方法大概几种在对数上面

2. 某些**分治算法（divide-and-conquer algorithm)** 将以O(nlogn)时间运行，

3. 对数最常出现的规律：如果一个算法用常数（O(1)）时间将问题的大小削减为其一部分（通常是1/2）, 那该算法就是O(logn)。另一方面，如果使用常数时间只是把问题减少一个常数的数量（如将问题减少1）,那么这种算法就是O(n) 

4. 只有一些特殊种类的问题才能O(logn)型。如，输入N个数，则一个算法只要把这些数读入就必须耗费O(n)的时间量。故，当谈到这些问题的O(logn)算法时，通常都是假设输入数据已经提前读入。
   
   + 折半查找（binary search）
     
     ```cpp
     /**
      * 每次循环使用两次比较以执行标注你的折半查找
      * 找到时返回所求项的下标，找不到则返回-1
     */
     template<typename Comparable>
     int binarySearch(const vector<Comparable> & a,const Comparable & x)
     {
         int low = 0, high = a.size() - 1;
     
         while(low <= high)
         {
             int mid = (low + high) / 2;
     
             if (a[mid] < x)
                 low = mid + 1;
             else if(a[mid] > x)
                 high = mid - 1;
             else   
                 return mid;     // 已找到
         } 
         return NOT_FOUND;       // NOT_FOUND被定义为-1
     } 
     // 每次迭代在循环内全部工作的花费是O(1),因此分析需要确定循环的次数
     
     // 循环从high-low = n-1开始并在high-low <= -1结束
     // 每次循环后high - low的值至少将该次循环前的值这版。
     ```
   
   + 欧几里得算法——计算最大公因数
     
     ```cpp
     long long gcd(long long m, long long n)
     {
         while (n != 0)
         {
             long long rem = m % n;
             m = n;
             n = rem ;
         }
         return m;
     } 
     // 通过连续计算余数直到余数是0为止，最后的非零余数就是最大公余数
     ```
   
   + 幂运算
     
     ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-25-12-09-54-image.png)
     
     ```cpp
     // 将用乘法的次数作为运行时间的度量
     // 计算X^N的明显算法是使用N-1次乘法自乘
     // 
     long long pow(long long x, int n)
     {
         if (n == 0)
             return 1;
         if (n == 1)
             return x;
         if (isEven(n))
             return pow(x*x, n/2)
         else   
             return pow(x*x,n/2)*x;
     }
     ```

![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-25-12-17-41-image.png)



### 2.4.5  最坏情形分析的局限性


