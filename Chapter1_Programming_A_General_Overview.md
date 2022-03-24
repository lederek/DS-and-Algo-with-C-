# 第1章    程序设计：综述

## 1.1    本书讨论的内容

1. **选择问题** ——设有一组N个数而要确定其中第K个最大者。
   
   + 该问题的解法就是将这N个数读进一个数组中，再通过某种简单的算法。
   
   + **冒泡排序法（bubble sort）**——以递减顺序将数组排序，然后返回位置K上的元素
   
   + 好一点的算法——先把前k个元素读入数组并（以递减的顺序）对其排序。然后，将剩下的元素再逐个读入。当新元素被读到时，如果小于数组中第K个元素则忽略之，否则将其放到数组中的正确位置上，同时将原数组中的最后一个元素寄出数组。当算法终止时，位于第k个位置上的元素作为答案范围。

2. **字谜（word puzzle)** 游戏——输入由一些字母构成的二维数组和一个单词表组成。目标是要找出字谜中的单词，这些单词可能是水平、垂直或在对角线上以任何方向放置的。

## 1.2        数学知识复习

## 1.4  C++ 类

### 1.4.1  基本的Class语法

### 1.4.2  构造函数的附加语法和访问函数

### 1.4.3  接口与实现的分离

### 1.4.4  vector类和string类

1. C++ 定义了两个类：**vector**类和**string**类。

2. vector 用来替代引起无限麻烦的内置C++数组（build-in C++ array)。
   
   1. 内置C++数组的问题在于，其行为不同于第一类对象（first-class object）
   
   2. 如，不能用 = 来复制，一个内置数组并不记忆它能够存储多少项，而且它的下标运算符（indexing operator) 也不检查下标是否合法。
   
   3. 内置的字符串就是一个字符数组，因此也就妨碍它再添加一些字符的操作，如， == 就不能正确地比较两个内置字符串

3. **STL（标准模板库）** 中的vector类和string类把数组和串处理成第一类对象。

4. 一个vector对象知道它本身有多大。两个字符对象可以通过==、<等运算符进行比较。

5. vector和string均可使用=来复制。

6. 如果可能，则应避免使用内置的C++的数组和字符串。
   
   ```cpp
   // vector的一个优良特点，即它容易改变其本身大小。
   // 在许多情况下，vector对象的初始大小为0.它随着需要而增长
   #include<iostream>
   #include<vector>
   
   using namespace std;
   
   int main()
   {   
       vector<int> squares(100);
       for (int i = 0; i < squares.size(); ++i)  // size函数，返回vector对象的大小
           squares[i] = i * i;
   
       for (int i = 0; i < squares.size(); ++i)
           cout << i << " " << squares[i] << endl;
   
       vector<int> daysInMonth = {31, 28, 31, 30, 31, 31, 30, 31, 30, 31, 30, 31};
       vector<int> daysInMonth {12};   // C++11给了初始化列表优先权，实际上是在位置0上有唯一一个元素12的大小为1的vector对象
       vector<int> daysInMonth (12);   // 初始化大小为12的一个vector，必须用圆
                                       // 括号括起来调用填写大小的构造函数
       string str1;
       string str2;
       if (str1 == str2) // 两个字符串的值相同，那么就是TRUE
      {
       cout << str1.length() << " " << str2.length() << endl;          // length 函数，返回字符串的长度
      }
      return 0;
   }
   ```

7. 对数组的基本操作就是用[ ]确定下标。在像数组或vector这样的集合中，依序访问没一个元素的格式是必不可少的操作。C++11在语法中添加了**范围for语句（range for)**

```cpp
int sum = 0;
for (int i = 0; i < squares.size(); ++i)
    sum += squares[i];


// 上面的程序段改写
int sum = 0;
for (int x: squares)
    sum += x;
// 在许多情况下，范围for语句内的类型声明是不需要的
// C++11也允许使用保留字auto以表示表一起将会自动推断适当的类型
int sum = 0;
for (auto x: squares)
    sum += x;
// 只要每一项都被陆续访问并且不需要下标，则范围for循环(range for loop)就适用
```

## 1.5 C++ 细节

### 1.5.1 指针（pointer)

1. 指针变量是一种存放另一对象所占用的地址的变量。它是用于许多数据结构的基本机制

2. 如，为了存储若干项，可以使用一个连续的数组，但在该连续数组的中间进行插入操作则有许多的项需要重新定位。通常我们不是把一个集合存储到一个数组中，而是把每一项存储到分离的、不连续的内存片（piece of memory)中，这种内存片在程序运行时分配给集合的各项。与每个对象相联系的是通向下一个对象的链接。这种链接是一个指针变量。这就是传统的链表

3. 未经初始化指针的使用会使程序崩溃，因为它们常导致对不存在的内存单元的访问。一般来说，给指针提供一个初始值。也可以初始化为空指针nullptr

4. 对象的动态创建
   
   1. 操作符**new** 返回一个指向新创建对象的指针。
   
   2. C++有几种方法使用其零参数构造函数创建一个对象
      
      ```cpp
      m = new IntCell();    // ok
      m = new IntCell{};    // C++
      m = new IntCell;      // 本书首选
      ```

5. 垃圾收集与delete
   
   1. 当一个通过new操作符被分配地址的对象不再被引用时，必须（通过一个指针）对该对象应用delete操作。否则，该对象使用的内存就会丢失(直到程序终止)。这就叫**内存泄漏（memory leak)** .
   
   2. 许多内存泄漏源（source of  memory leaks)可以自动审慎地清楚
   
   3. 在能够使用自动变量（automatic variable)的时候不要使用new操作符。

6. 指针的赋值和比较——是基于指针的值，也即基于指针所存储的内存地址进行的。如果指向相同的对象，两个指针变量相等

7. 通过指针访问对象的成员——   ->操作符访问

8. 取地址操作（&）—— 该操作符返回一个对象所占用的内存地址，并对实现**别名（alias)** 测试有用的

### 1.5.2  左值、右值和引用

1. C++定义了**引用类型（reference type)** 

2. C++11的主要变化之一是新的引用类型额创建，叫做**右值引用(rvalue reference)** 

3. 一个**左值（lvalue)** 是一个标识非临时性对象的表达式

4. 一个**右值（rvalue)** 是一个标识临时性对象的表达式，或一个不与任何对象相联系的值（如字面值常数）

5. C++语言的语法允许函数调用或运算（操作）符重载在返回值类型中指定为左值。如果函数调用计算一个其值在调用前不存在并且一旦调用终止就不再存在的表达式，那么它很有可能是一个右值，除非它在别处被复制

6. 在传统C++中，引用一般只能是一个左值的名字，因为若有一个对临时量的引用，则将导致对理论上已经声明不再需要的对象的访问的能力，这样就可能让它的资源被声明为另外的对象所用

7. C++11,有两种类型的引用：左值引用和右值引用
   
   1. **左值引用（Ivalue reference)** 的声明是通过在某个类型后放置一个符号&
      
      ```cpp
      string str = "hello";
      string & rstr = str;        // rstr 是str的另一个名字
      rstr += 'o';                // 把str改成"hello"
      bool cond = (&str == &rstr);// true； str和rstr是同一个对象
      string & bad1 = "hello";    // 非法: "hello"不是可修改的左值
      string & bad2 = str + "";   // 非法，str+""不是左值
      string & sub = str.substr(0,4);// 非法，str.substr(0,4)不是左值
      ```
   
   2. **右值引用（rvalue reference)** 是通过在某个类型后放置一个符号&&而被声明的。右值引用也可以引用一个右值（即一个临时量）
      
      ```cpp
      string str = "hello";
      string && bad1 = "hello";           // 合法
      string && bad2 = str + "";          // 合法
      string && sub  = str.substr(0,4);   // 合法
      ```

8. 左值引用的用途
   
   1. 给结构复杂的名称起别名——单独使用一个局部引用变量以达到重新命名一个被复杂表达式所知晓的对象的目的。
      
      ```cpp
      auto & whichList = theLists[myhash(x, theList.size())];
      if (find(begin(whichList), end(whichList), x) != end(whichList))
          return false;
      whichList.push_back(x);   
      ```
   
   2. 范围for循环
      
      ```cpp
      // 让一个vector对象所有的值都增1
      for (int i = 0; i < arr.size(); ++i)
          ++arr[i];
      
      // 使用范围for循环会更简洁
      // 这种自然代码做不到，因为x要担任vector中每一个值的拷贝
      for (auto x: arr)    // 行不通
          ++x;
      // x是一个引用，那么容易做到
      for (auto &x: arr)    // 行得通
          ++x;
      ```
   
   3. 避免复制
      
      ```cpp
      // 设有一个函数findMax，它返回一个vector对象或其他大集合中的最大值
      auto x = findMax(arr)
      // 如果这个vector存储的是一些大的对象，那么x将是arr中最大值的一个拷贝
      // 只需要这个值，并不想让x发生任何变化
      // 声明为一个引用（auto将推出常态性(constness);也可以用const明确规定
      // 一个不可更改的引用);
      auto & x = findMax(arr);
      // 1.引用变量常常用于避免越过函数调用界限复制对象（不管在函数调用还是返回中）
      // 为了使用引用代替复制能够进行传递和返回，在函数声明和返回中是需要语法的。
      ```

### 1.5.3  参数传递

1. **传值调用（call-by-value）** —— 把实参复制到形参

2. **传引用调用（call-by-reference)**
   
   ```cpp
   void swap(double & a, doubkle & b);    // 交换a和b
   ```

3. **传对常量引用的调用（call-by-reference-to-a-constant)** 
   
   ```cpp
   string randomItem(const vector<string> & arr);// 返回arr中的一个随机项
   ```

### 1.5.4  返回值传递

1. **传值返回（return-by-value)** 

2. **传常量引用返回（return-by-constant-reference)**

3. **传引用返回**

### 1.5.5 std::swap 和 std::move

1. 存在函数**std::move** 能够把任何左值（或右值）转换成右值。

2. 交换函数**std::swap** , 它可对任何类型的数据进行交换
   
   ```cpp
   // 通过3次复制的转换
   void swap(double & x, double & y)
   {
       double tmp = x;
       x = y;
       y = tmp;    
   }
   
   void swap(vector<string> & x, vector<string> & y)
    {
    vector<string> tmp = x;
    x = y;
    y = tmp;
    }
   
   // 通过3次移动进行交换
    void swap(vector<string> & x, vector<string> & y)
    {
    vector<string> tmp = static_cast<vector<string> && >(x);
    x = static_cast<vector<string> && > (y);
    y = static_cast<vector<string> && >(tmp);
    }
   
   void swap(vector<string> & x, vector<string> & y)
    {
    vector<string> tmp = std::move(x);
    x = std::move(y);
    y = std::move(tmp);
    }
   ```

### 1.5.6 五大函数：析构函数、拷贝构造函数、移动构造函数、拷贝复制operator=、移动复制operator=

1. 析构函数——只要一个对象运行越出范围，或经受一次**delete** ，则析构函数就要被调用。典型情况下，析构函数的唯一责任就是释放掉在对象使用期间获得的资源，包括关于任意的**new** 操作调用对应的**delete** ，关闭任何打开的文件，等等。默认做法是对每个数据成员应用析构函数。

2. 拷贝构造函数和移动构造函数——用来构造一个新的对象，它被初始化为与另一个同样类型对象相同的状态。
- 如果这个已存在的对象是一个左值，那么就用拷贝构造函数；

- 如果这个已存在的对象是一个右值(即一个迟早要被删除的临时量)，那么就用移动构造函数

- 默认情况下，移动构造函数的实现是通过将拷贝构造函数依次应用到每个数据成员来完成。基本类型的数据成员，进行简单的复制即可。
3. 拷贝赋值和移动赋值（operator=）——当=用于两个先前均被构造过的对象时，则调用复制运算符
- 如果是一个左值，则可以通过使用拷贝赋值符运算符完成

- 如果是一个右值（即一个将要被回收的临时量）,那么可通过移动赋值运算符

- 默认时，拷贝赋值运算符是通过依次把拷贝赋值运算符用于每一个数据成员而被实现的
4. 默认情形——如果一个类由一些数据成员组成，而这些数据成员只是一些基本类型的数据以及对其进行默认处理有意义的对象，那么这个类的默认值通常是有意义的。

5. 浅拷贝（shallow copy)——设该类包含一个数据成员，是个指针。这个指针指向一个动态定址的对象。默认的析构函数无法对这些这震雷削的数据成员起作用（必须delete）。拷贝构造函数和拷贝赋值运算符均复制指针的值而不是指针所指向的对象。则将有两个类实例，都包含指针，而指针又都指向相同的对象

6. 深拷贝(deep copy)——得到整个对象的复制品

7. 当一个类包含指针作为数据成员时，一般必须自己实现析构函、拷贝赋值和拷贝构造函数。这样做排除了移动的默认情形，因此还必须实现移动复制和移动构造函数

8. 作为一般法则，接受对所有5种操作的默认处理，或声明并显式定义所有5个函数的默认情形（使用关键字default），或每个都不予接受(使用关键字delete)

```cpp
// 再次显式地列出拷贝和移动操作
~IntCell()  {cout << "Invoking destructor" << endl;}    // 析构函数
IntCell（const IntCell & rhs) = default;                // 拷贝构造函数
IntCell(IntCell && rhs) = default;                      // 移动构造函数
IntCell & operator = (const IntCell & rhs) =default;    // 拷贝赋值
IntCell & operator = ( IntCell && rhs) =default;        // 移动赋值

// 不允许对IntCell 对象的所有复制和移动
IntCell（const IntCell & rhs) = delete;                // 无拷贝构造函数
IntCell(IntCell && rhs) = delete;                      // 无移动构造函数
IntCell & operator = (const IntCell & rhs) =delete;    // 无拷贝赋值
IntCell & operator = ( IntCell && rhs) =delete;        // 无移动赋值
```

9. 当默认操作不起作用时，拷贝赋值运算符一般能够通过使用拷贝构造函数创建一个拷贝然后将其与现有的对象交换来实现。移动赋值运算符一般可以通过逐项交换成员来实现
   
   ```cpp
   // 默认操作有问题
   class IntCell
   {
   public:
       explicit IntCell(int initialValue = 0)
           {storedValue = new int {initialValue};}
   
       int read() const
       { return * storedValue;}
       void write(int t)
       {*storedValue = x;}
   
   private:
       int *storedValue;
   };
   // 默认的拷贝赋值运算符和拷贝构造函数复制了指针storedValue。
   // 于是，a.storedValue、b.storedValue、c.storedValue都指向同一个int量
   // 这些复制均为浅拷贝（shallow copy)
   // 被复制的是这些指针而不是被指向的对象。
   // 不太明显的问题是内存泄漏
   // 为了解决这些问题，实现五大函数
   class IntCell
   {
   
   public:
       explicit IntCell(int initialValue = 0)
        { storedValue = new int (initialValue);}  
       ~IntCell()
       {delete storedValue;}                      // 析构函数
   
       IntCell(const IntCell & rhs)              // 拷贝构造函数
       { storedValue = new int {*rhs.storedValue};}    
   
       IntCell(IntCell && rhs):storedValue{rhs.storedValue}// 移动构造函数
       { rhs.storedValue = nullptr;} 
   
       IntCell & operator=(const IntCell & rhs)            // 拷贝赋值
       {
           if (this != &rhs)
               *storedValue = *rhs.storedValue;
           return *this;
       }
   
       IntCell & operator = (IntCell && rhs)               // 移动赋值
       {
           std::swap(storedValue, rhs.storedValue);        
           return *this;
       }
   
       int read() const
       { return *storedValue;}
       void write(int x)
       {*storedValue = x;}
   
   private:
       int *storedValue;
   };
   
   // 在C++11,常常使用拷贝和交换格式（copy-and-swap idiom)编写拷贝赋值
   // 导致另一种实现方法：   
   IntCell & operator=  （const IntCell & rhs)    // 拷贝赋值
   {
       IntCell copy = rhs;
       std::swap(*this, copy);
       return *this
   }
   ```

### 1.5.7   C风格数组和字符串

1. C风格数组

```cpp
// 声明由10个整数组成的数组arr1
int arr1[10];
// arr1实际上是一个指向大到足以存储10个int型量的内存的指针，而不是一个第一类数组类型
// 将=用于数组即企图拷贝两个指针的值而非整个数组
// 由上面的声明可知，这是非法的
// 当arr1被传递到函数时，传递的只是指针的值，而关于指针大小的信息就丢失了
// 数组大小必须作为一个附加的参数被传递过去
// 如果大小位置，必须显式地声明一个指针并通过new[]安排内存
int *arr2 = new int[n];
// arr2不是常量指针，可以指向一大块内存
// 某个时刻必须使用delete[将其释放
delete[] arr2;
// 否则，内存泄漏将发生
```

2. C风格字符串（build-in C-style string)——作为字符数组来实现的
   
   1. 特殊的空终止符（null-terminator)'\0'作为一个字符用来表示字符串的逻辑结尾。
   
   2. 字符串通过strcpy进行复制
   
   3. 使用strcmp进行比较，其长度则由strlen确定
   
   4. 单个的字符可以通过数组下标操作符来访问
   
   5. 当与设计使用C和C++工作的库例程序交互时不得不使用C-style
   
   6. 为提高速度而必须进行优化的代码段中使用C-style有时候（偶尔）也是需要的

## 1.6  模板

将描述类型无关的算法[也称为**泛型算法(generic algorithm)** ]

### 1.6.1 函数模板

1. 一个**函数模板（function template）** 不是一个具体的函数，而是可以编程一个函数的型式
   
   ```cpp
   /**
    * @brief 返回数组a中的最大项
    * 假设a.size() > 0
    * 可比较的对象必须提供operator< 和 operator=
    */
   template<typename Comparable>
   const Comparable & findMax(const vector<Comparable> & a)
   {
       int maxIndex = 0;
   
       for (int i = 1; i < a.size(); ++i)
           if (a[maxIndex] < a[i])
               maxIndex = i;
       return a[maxIndex];
   }
   ```

2. 每个新类型的展开都会产生附加的代码，当它发生在大的项目中的时候，就叫做**代码膨胀（code bloat)**  。习惯上在任何模板之前要安排一些注释，解释关于（一些）模板参数都有哪些假设，包括关于需要一些什么类型的构造函数

3. 如果存在一个非模板和一个模板且都匹配，那么非模板有优先权。

4. 如果出现两个同等近似程度的匹配，那么代码非法冰企鹅额编译程序都将二义性

### 1.6.2  类模板

1. MemoryCell模板
   
   ```cpp
   // 假设Object 有一个零参数构造函数、一个拷贝构造函数和一个拷贝赋值运算符
   /**
    * @brief 
    * 一个模拟内存单元的类
    * 
    */
   template<typename Object>
   class MemoryCell
   {
   public:
       explicit MemoryCell(const Object & initialValue = Object{})
       : storedValue{ initialValue} { }
   
       const Object & read() const
       { return storedValue;}
   
       void write(const Object & x)
       { storedValue = x}
   private:
       Object storedValue;
   } ;
   // Object 是通过常量引用传递的
   // 构造函数的 默认参数不是0，因为0不可能是一个合理的Object对象
   ```

### 1.6.3  Object、Comparable和一个例子

1. Square类通过存储边长来表示一个正方形并定义了operator<。还提供了零参数构造函数、operator=以及拷贝构造函数（均为默认）。该模式提供一个教做print的public型成员函数，该函数接受ostream对象作为其参数。这个public 型的函数可以被一个全局的、非类类型的函数operator<<调用，而这个函数接受一个ostream对象和一个将要输出的对象
   
   ```cpp
   class Square
   {
   public:
       explicit Square(double s = 0.0) : side { s }
       { }
   
       double getSide() const
       { return side; }
       double getArea() const
       { return side * side;}
       double getPerimeter() const
       { return 4 * side;}
   
       void print(ostream & out = cout) const
       { out << "(square " << getSide() << ")"; }
   
       bool operator< (const Square & rhs) const
       { return getSide() < rhs.getSide();}
   
   private:
          double side;
   
   };
   
   int main()
   {
      vector<Square> v = { Square{3.0}, Square{2.0}, Square{2.5}};
      cout << "Largest square: " << findMax(v) << endl;
      return 0;
   }
   ```

### 1.6.4 函数对象

1. 模板有一个重要的局限：它支队那些定义了operator<函数的对象有效，

2. 通过把函数放入一个对象之内来传递它。这个对象通常被称为**函数对象（function object）**

3. 想要找出字符串数组中最大的字符串（以字典序位于最后者），那么默认的operator<不能忽视字符的大小写区别

4. 重写findMax函数来接受两个参数：一个是对象的数组，另一个是解释如何决定两个对象中哪个更大、哪个更小的比较函数

5. 不是使用带有名字的函数，而是使用运算符重载。不是使用作为函数的isLessThan方法，而是使用operator()。其次，当调用operator时，cmp.operator()(x,y)可以简写成cmp(x,y)[换句话说，看起来像是函数调用，因而operator()被称为**函数调用操作符（function call operator）** ]。结果，参数名可以改成更有意义的isLessThan, 而调用则是isLessThan(x,y)。再次，提供一个不用函数对象就能工作的findMax版本，实现用到标准库函数对象模板less(定义于头文件functional中)以生成一个强制使用正常默认顺序的函数对象

```cpp
// 泛型findMax, 带有一个函数对象，vector #1
// 前提：a.size() > 0
template<typename Object, typename Comparator>
const Object & findMax(const vector<Object> & arr, Comparator cmp)
{
    int maxIndex = 0;

    for (int i = 1; i < arr.size(); ++i)
        if (cmp.isLessThan(arr[maxIndex], arr[i]))
            maxIndex = i;

    return arr[maxIndex];
} 

class CaseInsensitiveCompare
{
public:
    bool isLessThan(const string & lhs, const string & rhs) const
    { return strcasecmp(lhs.c_str(), rhs.c_str())<0;}
};

int main()
{
    vector<string> arr = {"ZEBRA", "alligator", "crocodile"};
    cout << findMax(arr, CaseInsensitiveCompare{}) << en
dl;

    return 0;
} 

// 泛型findMax， 用到一个函数对象，C++风格
// 前提：a.size()>0
template<typename Object, typename Comparator>
const Object & findMax(const vector<Object> & arr, Comparator cmp)
{
    int maxIndex = 0;

    for (int i = 1; i < arr.size(); ++i)
        if (isLessThan(arr[maxIndex], arr[i]))
            maxIndex = i;

    return arr[maxIndex];
} 

// 泛型findMax,使用默认的排序
#include<functional>
template<typename Object>
const Object & findMax(const vector<Object> & arr)
{
    return findMax(arr, less<Object>{});
}

class CaseInsensitiveCompare
{
public:
    bool isLessThan(const string & lhs, const string & rhs) const
    { return strcasecmp(lhs.c_str(), rhs.c_str())<0;

};



int main()
{
    vector<string> arr = {"ZEBRA", "alligator", "crocodile"};
    cout << findMax(arr, CaseInsensitiveCompare{}) << endl;
    cout << findMax(arr) << endl;

    return 0;
}
```

### 1.6.5  类模板的分离式编译

## 1.7  使用矩阵

1. 二维数组，一般称之为矩阵（matrix）

2. C++不提供matrix类。但可以写。基本思路是使用一些向量的向量来完成

3. 为其定义operator[ ], 即数组下标运算符(array-indexing operator)
   
   ```cpp
   // 一个完整的matrix类
   #ifndef MATRIX_H_
   #define MATRIX_H_
   
   #include<vector>
   using namespace std;
   
   template<typename Object>
   
   class matrix
   {
   public:
       matrix(int rows, int cols): array(rows)
       {
           for (auto & thisRow:array)
               thisRow.1(cols);
       }
   
       matrix(vector<vector<Object>> v): array{v}
       { }
       matrix(vector<vector<Object>> && v): array{std::move(v)}
       { }
   
       const vector<Object> & operator[] (int row) const
       { return array[row];}
       vector<Object> & operator[] (int row)
       { return array[row];}
   
       int numrows() const
       {return array.size();}
       int numcols() const
       {return numrows()? array[0].size():0;}
   
   private:
       vector<vector<Object>> array;
   };
   #endif
   ```

### 1.7.1  数据成员、构造函数和基本访问函数

1. 矩阵通过array型数据成员来表示，该数据成员被声明为**vector<Object>** 的一个

一个vector类的对象。

2. 构造函数首先把array构造成具有rows个元素，每个元素都是vector<Object>类型对象，而这些对象均是由零参数构造函数构造而得的向量。因此，有rows个零长度的Object的向量

3. 进入构造函数的函数体，每行的大小被调整为有cols个列位置。

### 1.7.2  0perator[ ]

1. operator [ ]的思路：如果有一个矩阵m, 那么m[i]就应该返回对应的matrix m中的第i行的向量。若是这样，则经过正常的vector的下标运算，m[i][j]将给出向量m[i]中位置j上的元素。因此，matrix 的operator[ ] 返回一个vector<Object>类型的实体，而不是Object对象。

2. 返回？
   
   ```cpp
   void copy(const matrix<int> & from, matrix<int> & to)
   {
       for (int i = 0; i < to.numrows; ++i);
           to[i] = from[i];
   }
   ```

3. 在copy 函数中，试图将matrix from的每一行复制到matrix to 对应的行上。 operator[ ] 返回一个常量引用，那么to[i]就不能出现在赋值语句的左边。因此返回一个引用。但from[i]=to[i]这样的表达式会编译，即使from是常量矩阵，from[i]也不会是一个常向量。

4. 让operator[ ] 返回一个from的常量引用，而不是to的简单引用。由于成员函数的**定常性（const-ness）** （即一个函数是否是访问函数或是修改函数）是特征的一部分，能够让operator[ ]的访问函数版返回一个常量引用，并让修改函数版返回一个简单的引用。

### 1.7.3  五大函数

这5个函数均可被自动地处理，因为vector已经对其做了处理。




