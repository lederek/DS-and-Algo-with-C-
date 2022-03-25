# 第3章  表、栈和队列

1. 最简单和最基础的3种数据结构
   
   + 介绍抽象数据类型（ADT)的概念
   
   + 阐述如何有效地执行对表的操作
   
   + 介绍栈ADT及其在实现递归方面的应用
   
   + 介绍队列ADT及其在操作系统和算法设计中的因通过用

## 3.1 抽象数据类型（ADT)

1. **抽象数据类型（Abstract Data Type，ADT)** 是带有一组操作的一些对象的集合。

2. 抽象数据类型是数学的抽象，在ADT的定义中没有地方提到关于这组操作是如何实现的任何解释。

3. 诸如表、集合、图以及与它们各自操作一起形成的这些对象都可以被看作是抽象数据类型。

4. 抽象数据类型都有与之相关的操作，如add、remove、size以及contains

5. 也可以只要两种操作union和find

## 3.2 表ADT

1. 将处理形如A0, A1, A2, ···, An-1的一般的**表(list)** ,这个表的大小是n

2. 将大小为0的特殊的表为**空表(empty list)** 

3. Ai 后继Ai-1(或继Ai-1之后，i<n)并称Ai-1前驱Ai(i>0)

4. 表中的第一个元素是A0, 而最后一个元素是An-1。不定义A0的前驱元，也不定义An-1的后继元

5. 元素Ai在表中的位置（position)为i

6. 与这些“定义”相关的是要在表ADT上进行操作的集合。
   
   + printList和makeEmpty是常用的操作
   
   + find返回某一项首次出现的位置
   
   + insert和remove一般是从表的某个位置插入和删除某个元素
   
   + findKth则返回（作为参数而被指定的）某个位置上的元素

### 3.2.1  表的简单数组实现

1. 所有上面的操作都可以通过数组来实现

2. 数组是由固定容量所创建，使用数组时需要对表的大小估计

3. vector类在需要时可以使其容量成倍地增长。

4. 插入：在位置0的插入（即在表的前端插入）首先需将整个数组后移一个位置以空出空间，最坏情况O(n)

5. 删除：删除第一个元素则需要将表中的所有元素前移一个位置，最坏情况O(n)

6. 如果所由的操作都发生在**表的尾端（high end of the list）** 那就没有元素需要移动，此时添加和删除则只花费O(1)

7. 如果插入和删除操作遍及整个的表，特别是对标的前端进行，那么**链表（linked list)是一个好的选择

### 3.2.2  简单链表

1. 为避免插入和删除的线性开销，需要暴躁表可以不连续存储，否则表的每个部分都可能需要整体移动

2. ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-25-15-42-24-image.png)

3. 链表由一系列结点组成，这些结点不必再内存中相连。每个结点均含有表元素和一个**链（link)** ，该链指向包含该元素后继元的另一个结点，称之为next链（next link）

4. 最后一个单元的next链指向nullptr。

5. ![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-25-15-45-41-image.png)

6. 删除最后一项，必须找出指向最后节点的项，把它的next链改成nullptr

7. 在经典的链表中，每个节点均存储指向其下一节点的链，而让指向最后节点的链不提供关于最后节点的前驱节点的任何信息

8. **双向链表（doubly linked list)**: 让每一个节点持有指向它在表中的前驱节点的链 

![](C:\Users\Chennc\AppData\Roaming\marktext\images\2022-03-25-15-55-16-image.png)

## 3.3  STL中的vector和list

1. C++语言在其库中包含一些常用数据结构的实现。该语言这一部分叫做**标准模板库（Standard Template Library, STL）** 

2. 表ADT(List  ADT) 是在STL中实现的数据结构之一

3. 一般来说，这些数据结构被称为**集合(collection)或容器(container)**

4. 表ADT有两种流行的实现方法
   
   1. **vector** 提供表ADT的一种可增长的数组实现。
      
      + 优点是以常数时间可索引(indexable)
      
      + 缺点是插入和删除的代价高昂，除非在尾端
   
   2. **list**  提供表ADT的双向链表实现
      
      + 优点是插入新项和删除代价低廉，但假设变动的位置是一致的
      
      + list不容易被索引
   
   3. **vector** 和**list**   两者在执行查找时都是吊的
   
   4. 两者均为类模板，它们用其所存储的项的类型来实例化而成为具体的类。下面对所有的STL容器都是可用的
      
      ```cpp
      int size()const   // 返回容器中元素的个数
      void clear()      // 从容器中删除所有元素
      bool empty() const // 若容器不含元素则返回true,否则返回FALSE
      ```
   
   5. **vector**  和 **list** 两者都支持以常数时间向表的尾端添加和从表的尾端删除的操作。也都支持以常数时间访问表的前端项：
      
      ```cpp
      void push_back(const Object & x) // 把x添加到表的尾端
      void pop_back                    // 删除位于表的尾端的对象
      const Object & back() const  // 返回位于表的尾端处的对象
                                   // 还提供一个返回引用的修改函数）
      const Object & front() const  // 返回位于表的前端处的对象
                                   // 还提供一个返回引用的修改函数）
      ```

5. 双向链表在其前端可以进行高效的改动，而vector不能，下列对**list** 可用
   
   ```cpp
   void push_front(const Object & x)    // 把x添加到表的前端
   void pop_front()                     // 删除位于表的前端处的对象
   ```

6. **vector** 有它自己的方法及，**list** 不具备。
   
   ```cpp
   Object & operator[](int idx)// 返回vector中下标idx的对象，不带界线检验
                               // （还提供一个返回常量引用的访问函数）
   Object & at(int idx)        // 返回vector中下标为idx的对象，带有界限检验
                               // (还提供一个返回常量引用的访问函数)
   int capacity() const        // 返回vector的内部容量
   void reserve（int newCapacity) // 设置新的容量。如果有好的估计可用，
                                  // 那么可以用于避免扩展vector
   ```

### 3.3.1 迭代器

1. 对表的一些操作，尤其那些在表的中间进行精密的插入和删除的操作，需要位置的概念。

2. 在STL中，位置由内嵌类型**iterator** 来表示。特别是，对于list<string>, 位置由类型list<string>::iterator表示；对于vector<int>,位置由vector<int>::iterator

3. 获取迭代器
   
   1. STL表（以及所有其他的STL容器)定义了一对方法
      
      ```cpp
      iterator begin()  // 返回一个适当的迭代器，表示容器中的第一项
      iterator end()    // 返回一个适当的跌宕器，表示容器1中的尾端
                        // （终端）标记（endmarker）(即容器中最后一项之后的位置)
      // end方法返回一个“越界”的迭代器。
      
      for(int i = 0; i != v.size(); ++i)
          cout << v[i] << endl;
      // 使用迭代器改写
      for (vector<int>::iterator itr = v.begin(); itr != v.end();itr.??)
          cout << itr.?? << endl;
      ```

4. 迭代器方法
   
   1. 迭代器可以用!=和==进行比较，并且可能需要定义一些**拷贝构造函数（copy construction)和operator=函数。
   
   2. 对迭代器通常使用操作如下：
      
      ```cpp
      itr++和++itr // 将迭代器推进到下一个位置。前缀形式和后缀形式都可以
      *itr // 返回对存储在迭代器itr的位置上对象的引用。所返回的引用可以允许修改
           // 也可以不允许修改
      itr1 == itr2    // 若itr1和itr2指向同一个位置则返回TRUE，反之FALSE
      itr1!=itr2  // 若itr1和itr2指向不同的位置则返回TRUE，反之FALSE
      ```
   
   3. ```cpp
      for (vector<int>::iterator itr = v.begin(); itr != v.end();++itr)
          cout << *itr << endl;
      // 又可以改写成
      vector<int>::iterator itr = v.begin();
      while (itr != v.end())
          cout << *itr++ << endl;
      ```

5. 需要迭代器的容器操作
   
   ```cpp
   // 需要迭代器的3个最流行的函数是那些从表（或vector、或list)的执行位置上
   // 进行添加或删除的操作
   iterator insert(iterator pos, const Object & x)
   // 把x添加到表中由迭代器pos所给定的位置之前的位置上。
   // 这是对list(但不是对vector)的常数时间的操作
   // 返回值是指向被插入项的位置的一个迭代器
   iterator erase(iterator pos)
   // 删除由迭代器所给定的位置上的对象。
   // 这是对list(但不是对vector)的常数时间的操作
   // 返回值是调用之前pos的后继元素所在的位置
   // 该操作使pos失效，现在它是多余的了，，因为它正在指向的容器项已经被删除
   iterator erase(iterator start, iterator end)
   // 删除从位置start开始直到（但不包括)位置end,为止的所有项
   // 注意，整个表可以通过调用c.erase(c.begin(), c.end())而被删除
   ```

### 3.3.2  例子：对表使用erase

1. 从初始项开始，每隔一项删除表中的一项
   
   ```cpp
   template<typename Container>
   void removeEveryOtherItem(Container * lst)
   {
       auto itr = lst.begin(); // itr 是一个Container::iterator
   
       while (itr != lst.end())
       {
           itr = lst.erase(itr)
           if (itr != lst.end())
               ++itr;
       }
   }
   ```
