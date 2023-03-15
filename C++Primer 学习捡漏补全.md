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

使用范围for便利对象时，若要修改其中元素，应该用auto &item去实现获取元素，才能使修改生效



### 6.2.4 数组形参







