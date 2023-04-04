序言：这是一个用了C++stl打了两年半Acm的人在看C++primer这本经典名著时发现的那些掉在地板上的知识，也就是查缺补漏，顺便记录加深印象。

# 6 函数

## 6.2 引用传递

当形参是引用类型时，形参是实参的别名，例如下面的函数

``` c++
void Test(int &p){
    p += p;
}
int main(){
    int a = 10;
    Test(a);
    cout << a << '\n';
}
输出：20
```

可以看到a的值变成了20，所以在Test函数内p是a的别名,修改p相当于修改a。

### 6.2.2 使用引用避免拷贝

拷贝大的类型对象比较低效

例：比较两个字符串的长度,字符串可能很长。

```c++
bool cmp(String a,String b);
```

如果这样写，函数调用时会拷贝对应的字符串到函数体内，这样消耗比较大。

``` c++
bool cmp(String &a,String &b);
```

而我们使用引用对象就可以避免函数复制一份字符串，节省了时间。

``` c++
bool cmp(const String &a,const String &b);
```

如果函数无须改变引用形参的值，就将其声明为常量（const）引用。

==注意：尽量使用常量引用==

如果上面的函数定义时，使用普通的

```c++
bool cmp(String &a,String &b);
```

则只能将cmp用于String对象的比较，你可能会问这样有什么问题？我告诉你

```c++
cmp("helloworld",str);
```

**类似这样的调用将会发生错误！原因是不能把const对象，字面值传递给普通的引用形参。**



使用引用还有一个好处是，当一个函数需要返回两个以上的值，我可以将剩余要改变的值通过引用改变。

```c++
int find(const String &s,char c,auto &occurs){
    occurs = s.count(s.begin(),s.end(),c);//这里通过引用已经返回了字符的出现个数
    int firstout = s.find(c);
    //这里顺便拓展String find的两个用法
    //s.find_first_of(c)第一个出现的位置
    //s.find_last_of(c)最后一个出现的位置
    if(firstout == s.npos)return -1;
    return firstout;//返回第一个出现位置
}
```



### 6.2.3 const形参

``` c++
void fcn(const int i);
void fcn(int i);
```

不同函数的形参列表应该有明显区别。这两个同名fcn函数传入的参数可以完全一样，会报错。

使用范围for便利对象时，若要修改其中元素，应该用auto &item去实现获取元素，才能使修改生效。



### 6.2.4 数组形参



### 6.2.5 main处理命令行选项

main函数可以接受两个形参                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   

```c++
int main(int argc, char * argv[])
```

传递给main函数形参的方法是，先生成exe文件。然后在对应目录打开cmd控制台，输入exe文件的名字 <传递的第一个字符串> <传递的第二个字符串> ...。

这些字符串会从下标1开始，依次存放到argv数组里。

### 6.2.6 含有可变形参的函数

initializer_list只能传递常量值，不能传递String对象等非常量值。

``` c++
int add(initializer_list<int> lst) {
	int val = 0;
	for (auto i = lst.begin(); i != lst.end(); i++) {
		val += *i;
	}
	return val;
}
int main(int argc, char* argv[]) {
	int val = add({ 15,20,35 });
	cout << val << '\n';
}
```

 ## 6.3 返回类型与return语句

### 6.3.1 无返回值函数

返回类型是void的函数可以return <另一个返回值是void的函数>。

### 6.3.2 有返回值函数

### 6.3.3 返回数组指针

尾置返回类型

```c++
auto fan(int a, int b) -> int {
	return a + b;
}
```

```c++
int odd[] = { 1,3,5,7,9 };
int even[] = { 2,4,6,8,10};
decltype(odd) *arrPtr(int i) {
	return i % 2 ? &odd : &even;
}
```

decltype(odd) 指明返回类型是5个长度的数组，*arrPtr表示返回的是指针。

return &odd &even 表示返回的是哪个具体数组的指针。

## 6.4 函数重载

main函数不能重载。

**二义性调用**：在编译器调用重载函数时有多于一个函数可以匹配，但每一个都不是明显的最佳选择。

## 6.5 特殊用途语言特性

### 6.5.1 默认实参

```c++
string screen(sz ht = 24,sz wid = 80,char background = ' ');
```

默认实参作为形参的初始值出现在形参列表中。

**一旦某个形参有了默认值，它后面的所有形参都必须有默认值。**

设计含有默认实参的函数时，有默认实参的形参尽量往后放。

通常在声明函数时定义默认实参，并放在头文件中。

**用作默认实参的名字在函数声明的作用域内寻找**

```c++
int wd = 80;
string screen(int a = wd);
void f(){
    int wd = 100;
    window = screen();//调用screen(80),而不是screen(100)
}
```



### 6.5.2 内联函数和constexpr函数

在函数的返回类型前面加上关键字 inline声明为内联函数。内联函数在程序内直接展开，可省去函数调用的开支。

constexpr函数用来检查函数返回值是否是常量，如果不是常量编译器会检查报错。

inline和constexpr一般声明在头文件内

**用constexpr修饰的函数其函数体内不能包含任何执行操作的语句**

```c++
constexpr const int isShorter(const string& s1, const string& s2) {
	return s1.size() < s2.size();//包含了size（）函数，所以编译器报错
}
```

### 6.5.3 调试帮助

**assert断言的用法：**

计算表达式的值是否为0，为0就打印报错信息，否则什么也不做

```c++
#define NDEBUG//添加这句话可以禁用assert的调试作用
#include<cassert>//需要添加这个头文件
int main(){
    int a = 0,b = 1;
    assert(a==b);
    //当表达式的值为0时跳出报错，否则什么也不做;
}
```



注意事项：

1. 使用assert的缺点是，频繁的调用会极大的影响程序的性能，增加额外的开销。
2. 断言语句不会永远被执行，assert不管是在屏蔽还是启用状态下都不能对我们本身代码有所影响
3. 程序一般分为Debug 版本和Release 版本，Debug 版本用于内部调试，Release 版本发行给用户使用。断言assert 是仅在Debug 版本起作用的宏，它用于检查"不应该"发生的情况。

使用规则：

1. 每个assert只检验一个条件，因为同时检验多个条件时，如果断言失败，我们就无法直观的判断哪个条件失败；

   无法直观的判断哪个条件失败：

   ```c++
   assert(nOffset>=0 && nOffset+nSize<=m_nInfomationSize);
   ```

   只检验一个条件，比较直观：

   ```c++
   assert(nOffset >= 0); 
   assert(nOffset+nSize <= m_nInfomationSize);
   ```

2. 不能使用改变环境的语句,在实际编写代码的过程中是不能这样做的；

   例如：

   ```text
   assert(i++ < 100)
   ```

   不好：这是因为如果出错，比如在执行之前i=100,那么这条语句就不会执行，那么i++这条命令就没有执行。

   ```text
   assert(i < 100)
   i++;
   ```

3. 放在函数参数的入口处检查传入参数的合法性；qwq

### 6.6.1 实参类型转换

```c++
void manip(long);
void manip(float);
double a = 3.14;
manip(a);//该调用具有二义性，因为既能转float又能转long
//所有算数类型转换的级别都一样，从int向longlong转换并不比从int向double转换的转换级别高
```

### 6.7 函数指针

```c++
bool lengthCompare(const string &,const string &)
bool (*pf)(const string &,const string &)//pf指向一个函数，pf是一个指针
    
pf = lengthCompare //pf指向名为lengthCompare的函数
pf = &lengthCompare //取地址符是可选的，这两条语句是等价的，都是返回该函数的地址
    
bool b1 = pf("hello","goodbye");//调用函数
bool b2 = (*pf)("hello","goodbye")//等价的调用
bool b3 = lengthCompare("hello","goodbye")//另一个等价的调用
```



## 7 类

### 7.1 定义抽象数据类型

类外部定义成员函数：

```c++
double Sale_data::avg_Price() const{ 
    // 需要包含这个函数所属的类名，如这里的Sale_data
    return revenue;//这里的revenue是调用了作用域Sale_data中的变量
}
```



