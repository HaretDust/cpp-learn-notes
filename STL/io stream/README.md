# io流类库

> 描述io流相关内容

- 主导程序与外界环境的信息交换
- 流是一种抽象,存在于在数据的生产者和数据的消费者之间
- 流可以文件相互关联,当流与对象关联时,操作流就等同于操作对象

> 读 操作在流数据抽象中被称为(从流中)提取<br>
> 写 操作被称为(向流中)插入

分类

- ios_base
  - ios
    - istream
      - ifstream
      - istringstream
    - ostream
      - ofstream
      - ostringstream
- (istream + ostream) ---->iostream
  - fstream
  - stringstream
- streambuf
  - filebuf
  - stringbuf

## 输出流

- ostream
- ofstream 文件输出流
- ostringstream 字符串输出流

预先定义的输出流对象

- cout 标准输出
- cerr 标准错误输出,没有缓冲,发送给它的内容立即被输出
- clog 类似于cerr,但是有缓冲,缓冲区满时被输出

```c++
//指定输出到文件对象b.out中
ofstream fout("b.out");
//rdbuf是重定向函数
streambuf*  pOld  = cout.rdbuf(fout.rdbuf());  
//…
cout.rdbuf(pOld);
```
---
#### 向文本文件输出
>标准输出设备显示器被系统看作文本文件

插入运算符

- 插入(<<)运算符
- 为所有标准C++数据类型预先设计的,用于传送字节到一个输出流对象.

操纵符(manipulator)

- 插入运算符与操纵符一起工作
  - 控制输出格式.
- 很多操纵符都定义在
  - ios_base类中(如hex())、头文件(如setprecision())
- 控制输出宽度
  - 在流中放入setw操纵符或调用width成员函数为每个项指定输出宽度
- setw和width仅影响紧随其后的输出项,但其它流格式操纵符保持有效直到发生改变
- dec、oct和hex操纵符设置输入和输出的默认进制.

典型操作符

操作符         | 作用                           | 效果
----------- | ---------------------------- | ------------
boolalpha   | 字符化 bool 值                   | 显示true,false
noboolalpha | 取消 bool 值的字符化设置              | 显示为0,1
dec         | 用十进制计数                       |
hex         | 使用十六进制计数                     |
oct         | 使用八进制计数                      |
fixed       | 使用定点数(Fixed floating-point)表示浮点 | 0.010000
scientific  | 科学计数法表示浮点 | 1.000000e-02
hexfloat    | 十六进制科学计数法表示浮点 |  0x1.47ae147ae147bp-7
defaultfloat| 一般表示(按需要) | 0.01
showpoint,noshowpoint | 控制浮点表示是否始终包含小数点 |
showbase,noshowbase | 是否显示前缀 | 0x2a
endl        | 插入新行(Newline)及刷新缓冲区数据(Flush) |
ends        | 插入空(Null)字符 | 一个空白字符
setw        | 更改下个输入/输出域的宽度(有参数) |
setiosflags,resetiosflags | 设置操作符 |
left	| 左校正（添加填充字符到右）
right	| 右校正（添加填充字符到左）

#### 向二进制文件输出
>使用ofstream构造函数中的模式参量指定二进制输出模式；  
以通常方式构造一个流，然后使用setmode成员函数，在文件打开后改变模式；  
通过二进制文件输出流对象完成输出。    

ios_base::binary

#### 向字符串输出  
>将字符串作为输出流的目标，可以实现将其他数据类型转换为字符串的功能

字符串输出流（ ostringstream ）
>用于构造字符串
- 功能  
    - 支持ofstream类的除open、close外的所有操作  
    - str函数可以返回当前已构造的字符串  

```cpp
//11_6.cpp 典型应用 将数值转换为字符串
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

//函数模板toString可以将各种支持“<<“插入符的类型的对象转换为字符串。

template <class T>
inline string toString(const T &v) {
    ostringstream os;   //创建字符串输出流
    os << v;        //将变量v的值写入字符串流
    return os.str();    //返回输出流生成的字符串
}

int main() {
    string str1 = toString(5);
    cout << str1 << endl;
    string str2 = toString(1.2);
    cout << str2 << endl;
    return 0;
}
```
>输出结果：
>5  
1.2
