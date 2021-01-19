## sizeof运算符

1. 是单目运算符
2. 以字节为单位运算占用内存
3. 对类型的运算

```c
/* ANSI C 规定 */
sizeof(char)					= 1;
sizeof(unsigned char) = 1;
sizeof(signed char) 	= 1;
/* 未明确规定，一般来说 */
sizeof(int)            = 4;
sizeof(unsigned int)   = 4;
sizeof(short int)      = 2;
sizeof(unsigned short) = 2;
sizeof(long int)       = 4;
sizeof(unsigned long)  = 4;
sizeof(float)          = 4;
sizeof(double)         = 8;
sizeof(long double)    = 12;
```

4. 指针在32位机中为4字节，在64位机中为8字节
5. 操作数是数组时，为数组总字节数。
6. 操作数是union时，为联合中占用空间最大的成员的字节数。



## 位域

1. 一般用于开关标志等，不需要占用一个完整的存储单元。
2. 声明

```c
struct
{
  type [member_name] : width ;
};
/*
 * 有效的type为int, unsigned int，和signed int。用于指示如何解读位域的值
 */
```

3. 占用空间：缺省情况下，若多个位域的宽度不超过一个单位，则可以只占用一个存储单位；若加上随后的一个宽度超过了一个单位，一般来说不支持跨域，下一个宽度的值将重新起头。



## 宏



## 多维数组

多维数组可以连续赋初值，按照行向下赋值。所以初值的个数可以决定未声明第一维的大小。如a\[\][4]= {0,0}，则第一维大小为1.



## STL

STL主要依赖模板实现，而不是OOP的三个特性：封装、继承和多态。

STL的主要组件，容器(container)、算法(algorithm、迭代器(iterator)。

### 容器

常用的容器：

序列容器：vector、list、deque

关联容器：set、map、multisite、multimap

适配器容器：stack、queue、priority_queue

#### 序列容器

也叫顺序容器，元素的顺序就是插入的顺序，和值无关。

序列容器：vector、list、deque

#### 关联容器

关联容器内的数据是排序的。

关联容器：set、map、multisite、multimap

关联容器一般使用平衡二叉树实现。

#### 容器适配器

在以上容器的基础上，STL在这些容器的基础上屏蔽了一些功能，突出或增加一些其他功能，实现了栈、队列和优先队列。

适配器容器：stack、queue、priority_queue

### STL中的“大”、“小”和“相等”的概念

#### 小 :  <

容器中的比较大小默认是```<```运算符号，代表以下三个等价的概念：

1. x比y小
2. 表达式```x < y```为真
3. y比x大

#### 大

STL中对```>```运算符是没有规定的，也就无法通过```>```进行比较的运算。```x > y```可能是没有定义的。

#### 相等：==

在未排序的区间上运算，比较两个元素是按照两个元素是否想等运算的。

而在排序区间上运算，```x == y```等价于```x <  y 和 y < x同时为假```

### pair类模板

关联容器中一些成员函数的返回值是pair对象，map和multimap容器中的元素也都是pair对象。

#### 定义

pair的定义如下：

```c++
template <class_Tl, class_T2>
struct pair
{
    _T1 first;
    _T2 second;
    pair(): first(), second() {}  //用无参构造函数初始化 first 和 second
    pair(const _T1 &__a, const _T2 &__b): first(__a), second(__b) {}
    template <class_U1, class_U2>
    pair(const pair <_U1, _U2> &__p): first(__p.first), second(__p.second) {}
};
```

pair中有两个成员变量，first和second

#### 构造

```c++
#include <iostream>
using namespace std;
int main()
{
    pair<int,double> p1;
    cout << p1.first << "," << p1.second << endl; //输出  0,0   
    pair<string,int> p2("this",20);
    cout << p2.first << "," << p2.second << endl; //输出  this,20
    pair<int,int> p3(pair<char,char>('a','b'));
    cout << p3.first << "," << p3.second << endl; //输出  97,98
    pair<int,string> p4 = make_pair(200,"hello");
    cout << p4.first << "," << p4.second << endl; //输出  200,hello
    return 0;
}
```



### 容器的使用

#### vector

vector的作用类似于数组，但是不需要在声明时就固定长度。vector在头文件`<vector>` 中

##### 初始化

![初始化](https://cdn.jsdelivr.net/gh/sim41/pic/gbnote/c_stl_vector_1.png)

int类型默认初始化为0，string类型默认初始化为空字符串

##### 添加元素

使用push_back()方法将元素添加到vector的尾部。

##### 常用方法

![常用方法](https://cdn.jsdelivr.net/gh/sim41/pic/gbnote/c_stl_vector_2.png)

示例

```c++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

template <typename T>
void showvector(vector<T> v)
{
	for (vector<T>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it;
	}
	cout << endl;
}

int main()
{
	vector<string> v6 = { "hi","my","name","is","lee" };
	v6.resize(3);  //重新调整vector容量大小
	showvector(v6);

	vector<int> v5 = { 1,2,3,4,5 }; //列表初始化,注意使用的是花括号
	cout << v5.front() << endl; //访问第一个元素
	cout << v5.back() << endl; //访问最后一个元素

	showvector(v5);
	v5.pop_back(); //删除最后一个元素
	showvector(v5);
	v5.push_back(6); //加入一个元素并把它放在最后
	showvector(v5);
	v5.insert(v5.begin()+1,9); //在第二个位置插入新元素
	showvector(v5);
	v5.erase(v5.begin() + 3);  //删除第四个元素
	showvector(v5);
	v5.insert(v5.begin() + 1, 7,8); //连续插入7个8
	showvector(v5);
	v5.clear(); //清除所有内容
	showvector(v5);

	system("pause");
	return 0;
} 
```

#### multiset

multiset是排序集合，且允许有相同的元素。在头文件```<set>```中

multiset中的元素不能直接修改值，否则会打乱原有的顺序，正确的做法是删除元素并重新进行插入。

##### 定义

```c++
template <class Key, class Pred = less<Key>, class B = allocator<Key> > class multiset {
    ...
};
```

multiset中包含三个类型参数，第一个参数是成员的类型，都是Key类型。第二个参数是排序的优先级，默认是```less<Key>```类型。第三个是内存分配参数，一般使用默认值。

##### 成员函数

| 成员函数或函数模板                                      | 作用                                                         |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| iterator find (const T & val);                          | 在容器中查找值为 val 的元素，返回其迭代器。如果找不到，返 回 end() |
| iterator insert( const T & val);                        | 将 val 插入容器中并返回其迭代器                              |
| void insert(iterator first, iterator last);             | 将区间 [first, last) 中的元素插人容器                        |
| int count( const T & val);                              | 统计有多少个元素的值和 val 相等                              |
| iterator lower_bound( const T & val);                   | 查找一个最大的位置 it，使得 [begin(), it) 中所有的元素者比 val 小 |
| iterator upper_bound( const T & val);                   | 查找一个最小的位置 it，使得 [it, end()) 中所有的元素都比 val 大 |
| pair <iterator, iterator > equal_range (const T & val); | 同时求得 lower_bound 和 upper_bound                          |
| iterator erase(iterator it);                            | 删除 it 指向的元素，返回其后面的元素的迭代器（Visual Studio 2010 中如此，但是在 [C++](http://c.biancheng.net/cplus/) 标准和 Dev C++ 中，返回值不是这样，所以先暂存迭代器） |
| iterator erase(iterator first, iterator last);          | 删除区间 [first, last)，返回 last（Visual Studio 2010 中如此，但是在 C++ 标准和 Dev C++ 中，返回值不是这样） |

##### erase后的迭代器

erase 成员函数删除一个元素后，返回下一个元素的迭代器应该是很合理的，

但是 C++ 标准委员会认为，返回下一个元素的迭代器也是需要时间开销的，

如果程序员不想要这个返回值，那么这个开销就是浪费的——因此在遵循 C++ 标准的 Dev C++ 中，本行无法编译通过。

但是微软公司认为应该对这一点做出改进，因此 Visual Studio 2010 将 erase 成员函数处理成返回被删元素下一个元素的迭代器。

不论在哪种编译器中，用 erase 成员函数删除迭代器 i 指向的元素后，迭代器 i 即告失效， 

此时不能指望 ++i 后 i 会指向被删除元素的下一个元素；

相反，++i 可能立即导致出错。如果想要得到被删除元素后面那个元素的迭代器，

可以在删除前获取其迭代器并保存起来（这同样适用于 set、map、multimap 的 erase 成员函数）。

事实上，如果得到了某关联容器的迭代器，则该迭代器并不会因为容器中元素的插入以及其他元素的删除而失效。

只要该迭代器指向的元素没有被删除，就可以一直使用它。

#### set

set是有序唯一的集合。向set中添加元素，会自动进行排序，而向set中添加已经存在的元素时，本次操作不会执行。

使用方法与multiset类似。在头文件`<set>` 中

不能直接修改 set 容器中元素的值。因为元素被修改后，容器并不会自动重新调整顺序，于是容器的有序性就会被破坏，再在其上进行查

找等操作就会得到错误的结果。因此，如果要修改 set 容器中某个元素的值，正确的做法是先删除该元素，再插入新元素。

##### 添加元素

由于set中不能存在重复元素，所以insert成员函数与multiset有所区别，insert的原型为：

```c++
pair<iterator, bool> insert(const T & val);
```

如果 set 的 insert 成员函数的返回值是 pair 模板类对象 x，如果 x.second 为 true，则说明插入成功，此时 x.first 就是指向被插入元素的

迭代器；如果 x.second 为 false，则说明要插入的元素已在容器中，此时 x.first 就是指向原有那个元素的迭代器。

#### list

list是双向链表容器，单向链表是forward_list。在头文件`<list>`中

#### map

map是一组k-v对的数据。

### 迭代器

迭代器(Iterator)类似于指针，提供了访问容器中对象的方法。

指针也可以看作是一种迭代器。

#### 迭代器的类型：

Input iterators 提供对数据的只读访问。

Output iterators 提供对数据的只写访问。

Forward iterators 提供读写操作，并向前推进迭代器。

Bidirectional iterators 提供读写操作，并可以向前向后双向操作。

Random access iterators 提供读写操作，并能随机访问。 



## string

### string的使用

string是可变字符串。头文件为`<string>`

#### 初始化

1. 使用"="初始化，为拷贝初始化
2. 其他为直接初始化

```c++
string s1;                   //初始化字符串，空字符串
string s2=s1;                //拷贝初始化，深拷贝字符串
string s3="Hello world"; 		 //直接初始化，s3存了字符串Hello world
string s4(10,'a');           //s4存放的字符串的10个a，即aaaaaaaaaa
string s5(s4);               //拷贝初始化，深拷贝字符串
string s6("Hello world"); 	 //直接初始化
string s7=string(6,'c');     //拷贝初始化，s7中6个c，即cccccc 
```

#### 常用方法

![string常用方法](https://cdn.jsdelivr.net/gh/sim41/pic/gbnote/C_STL_string.png)

#### 利用迭代器进行访问

![迭代器](https://cdn.jsdelivr.net/gh/sim41/pic/gbnote/c_stl_string_2.png)

创建语法`string::iterator`， 可以使用`string::const_interator` 创建只读的迭代器

在`<cctype>` 头文件中还提供了一些用于string类的方法

![cctype](https://cdn.jsdelivr.net/gh/sim41/pic/gbnote/c_stl_string_3.png)

### string中追加字符的方法

string中向字符尾部追加字符可以使用`+=`、`append`和`push_back`三种方法。

其区别是：

+= 是追加单个参数

append 可以追加多个参数

push_back 只可以追加单个字符

|                            | append() | +=    | push_back |
| -------------------------- | -------- | ----- | --------- |
| 全字符串(string)           | √        | √     | ×         |
| 部分字符串(substring)      | √        | ×     | ×         |
| 字符数组(char array)       | √        | √     | ×         |
| 单个字符(char)             | ×        | √     | √         |
| 迭代器范围(iterator range) | √        | ×     | ×         |
| 返回值(return value)       | *this    | *this | none      |
| cstring(char *)            | √        | √     | ×         |

### 构造函数

```c++
string s1();							//s1 = ""
string s2("abc");					//s2 = "abc"
string s3(4, 'a');				//s3 = "aaaa"
string s4("abcde", 1, 3)	//s4 = "bcd"
```

### 赋值

可以使用char*类型的变量、常量，以及char类型的变量、常量进行赋值。

```c++
string s1;
s1 = "aaa";
s2 = 'k';
```

也可以使用成员函数assign

```c++
string s1("abcde"), s2;
s2.assign(s1);
s2.assign(4, 'a');
s2.assign("abcde", 1, 3);
s2.assign(s1, 1, 3);
```

### 长度

length或size成员函数

### 字符串的拼接

1.使用+对两个string进行拼接。

2.使用append成员函数

```c++
string s1("123"), s2("abc");
s1.append(s2);  						// s1 = "123abc"
s1.append(s2, 1, 2);  			// s1 = "123abcbc"
s1.append(3, 'K');  				// s1 = "123abcbcKKK"
s1.append("ABCDE", 2, 3);  	// s1 = "123abcbcKKKCDE"，添加 "ABCDE" 的子串(2, 3)
```

### 比较

1.可以利用比较运算符 <、<=、==、!=、>=、> 比较两个string

2.使用compare成员函数，返回小于0表示比当前字符串小，等于0表示两字符串相等，大于0表示比当前字符串大

```c++
string s1("hello"), s2("hello, world");
int n = s1.compare(s2);
n = s1.compare(1, 2, s2, 0, 3);  //比较s1的子串 (1,2) 和s2的子串 (0,3)
n = s1.compare(0, 2, s2);  // 比较s1的子串 (0,2) 和 s2
n = s1.compare("Hello");
n = s1.compare(1, 2, "Hello");  //比较 s1 的子串(1,2)和"Hello”
n = s1.compare(1, 2, "Hello", 1, 2);  //比较 s1 的子串(1,2)和 "Hello" 的子串(1,2)
```

### 子串

成员函数substr用来求子串

```c++
string s1("abcde");
string s2 = s1.substr(n, m);
//如果缺省m，或者m越界，则为n到字符串结束。
```

### 交换两个字符串

使用swap成员函数交换两个string对象。

```c++
string s1("abc"), s2("def");
s1.swap(s2);
```

### 查找子串和字符

该类函数查询后返回子串或字符在string中的下标，如果查找不到，则返回string::npos。

```string::npos```的定义是```size_t npos -1```由于size_t是无符号类型，则下溢出代表最大的size_t。

```
find: 	从前往后查找
rfind: 	从后往前查找
find_first_of:							从前往后查找第一次出现目标字符串中任意字母的位置
eg: s1.find_first_of("abc") 查找第一次出现a或b或c的位置
find_last_of:								从后往前查找第一次出现目标字符串中任意字母的位置
find_first_not_of:					从前往后查找何处出现另一个字符串中没有包含的字符
find_last_not_of:						从后往前查找第一次非目标字符串中字母的位置
```

### 替换子串

使用replace成员函数可以对string中的子串进行替换

用法类似于compare

### 删除子串

erase成员函数可以删除string中的子串

```c++
string s1("abcde");
s1.erase(1, 3);	//删除子串(1,3)即"bcd"
s1.erase(1);		//删除下标1以及1以后的所有字符
```

### 插入字符串

insert可以在string中插入字符串

```c++
string s1("Limitless"), s2("00");
s1.insert(2, "123");  //在下标 2 处插入字符串"123"，s1 = "Li123mitless"
s1.insert(3, s2);  //在下标 2 处插入 s2 , s1 = "Li10023mitless"
s1.insert(3, 5, 'X');  //在下标 3 处插入 5 个 'X'，s1 = "Li1XXXXX0023mitless"
```

### 将string作为流使用

包含头文件sstream，可以使用istringstream和ostringstream将string作为流使用。

```c++
#include <iostream>
#include <string>
#include <sstream>

using namespace std;

int main(){
		string src("Avatar 123 5.2 Titanic K");
    istringstream istrStream(src); //建立src到istrStream的联系
    string s1, s2;
    int n;  double d;  char c;
    istrStream >> s1 >> n >> d >> s2 >> c; //把src的内容当做输入流进行读取
    ostringstream ostrStream;
    ostrStream << s1 << endl << s2 << endl << n << endl << d << endl << c <<endl;
    cout << ostrStream.str();
    return 0;
}
```

### STL

string对象可以看作是一个顺序容器，STL迭代器和算法也可以用于string



## 深拷贝和浅拷贝

对于基本类型和简单的类，它们之间的复制很简单，就是按内存中的二进制位进行拷贝，类似于C中memcpy()的作用。

在这种情况下，默认的拷贝构造函数足以完成任务。

而当类中包含一些复杂的成员，如动态分配的内存，指向其他对象的指针等，默认的拷贝构造函数就不能拷贝这些资源，

此时需要显式地定义拷贝构造函数。

这种将对象所持有的其它资源一并拷贝的行为叫做深拷贝，我们必须显式地定义拷贝构造函数才能达到深拷贝的目的。



## for循环

### 范围循环(Range-Based-For)

c++11中的新特性：for允许简单的范围迭代

```c++
int my_array[5] = {1, 2, 3, 4, 5};
for (int &x : my_array)
{
    x *= 2;
    cout << x << endl;  
}

for (auto &x : my_array) {
    x *= 2;
    cout << x << endl;  
}
```







# 参考

[笔试面试知识整理](https://hit-alibaba.github.io/interview/)

[C++ STL快速入门-Madcola](https://www.cnblogs.com/skyfsm/p/6934246.html)

