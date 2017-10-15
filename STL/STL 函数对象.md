
STL提供的函数对象
===
***
用于算术运算的函数对象：
---  

一元函数对象(一个参数) ：`negate  `  
二元函数对象(两个参数) ：`plus、minus、multiplies、divides、modulus  `  

用于关系运算、逻辑运算的函数对象(要求返回值为`bool`)  
---
一元谓词(一个参数)：`logical_not  `  
二元谓词(两个参数)：`equalto、notequalto、greater、less、greaterequal、lessequal、logicaland、logical_or  `  

***
解释
---
类型 |      名字     | 参数数量 |作用
---  |           ---|---|---
算术  *返回结果类型为传入类型* | negate       | 1 | 返回 (-x)
\   |  plus         | 2 |  返回 (x+y)
\   |  minus        | 2 |  返回 (x-y)
\   |  multiplies   | 2 |  返回 (x*y)
\   |  divides      | 2 |  返回 (x/y)
\   |  modulus      | 2 |  返回 (x%y)
关系与逻辑  *结果为`bool`值*  | logical_not | 1 |  返回 (!x)
\   |  equal_to            | 2 |  返回 (x==y)
\   |  notequal_to         | 2 |  返回 (x!=y)
\   |  greater            | 2 |  返回 (x>y)
\   |  less               | 2 |  返回 (x<y)
\   |  greater_equal      | 2 |  返回 (x>=y)
\   |  less_equal         | 2 |  返回 (x<=y)
\   |  logical_and        | 2 |  返回 (x&&y)
\   |  logical_or         | 2 |  返回 (x||y)

***
用法
---
`std::函数对象名<类型名>( )`  
>例如  `std::minus<int>( )`

***
#### 算数类多用于与 *数值算法* 和 `std::transform( )` 配合使用  
>用于数字的连续计算和变换  
`std::accumulate( )`  
`std::transform( )`


#### 关系类多用于与 *搜索算法* 和 *比较算法* *排序算法* 配合使用  
>搜索特定值 依次匹配 特定排序
`std::mismatch( )`


1. ### 范例  
`std::accumulate( )`  
>accumulate(首地址，结束地址(一般是*首地址+N*)，初始值，函数对象<类型名字>( ))  
>>计算方法  
返回值1 = 初始值  op(函数对象) 首地址   指向的数据  
返回值2 = 返回值1 op(函数对象) 首地址+1 指向的数据  
...  
最终结果 = 返回值N op(函数对象) 首地址+N 指向的数据

```cpp
// minus example
#include <iostream>     // std::cout
#include <functional>   // std::minus
#include <algorithm>    // std::transform

int main ( ) {
  int numbers[]={10,20,30};
  int result;
  result = std::accumulate (numbers, numbers+3, 100, std::minus<int>( ));
  std::cout << "The result of 100-10-20-30 is " << result << ".\n";
  return 0;
}
```
>结果
>> The result of 100-10-20-30 is 40.

2. ### 范例  
`std::transform( )`  
>transform(第一运算对象首地址，结束地址(一般是*首地址+N*)，第二个运算对象首地址，结果，函数对象<类型名字>( ))
>>计算方法  
结果1 = 第一运算对象首地址   op(函数对象) 第二个运算对象首地址   指向的数据  
结果2 = 第一运算对象首地址+1 op(函数对象) 第二个运算对象首地址+1 指向的数据  
...  
结果N = 第一运算对象首地址+N op(函数对象) 第二个运算对象首地址+N 指向的数据  

```cpp
#include <iostream>     // std::cout
#include <functional>   // std::plus
#include <algorithm>    // std::transform

int main( ) {
	int first[] = { 1,2,3,4,5 };
	int second[] = { 2,2,2,2,2 };
	int results[5];
	std::transform(first, first + 5, second, results, std::modulus<int>( ));
	for (int i = 0; i < 5; i++)
		std::cout << results[i] << ' ';
	std::cout << '\n';
	return 0;
}
```
>结果
>> 1 0 1 0 1

3. ### 范例  
`std::transform( )`和`std::bind2nd( )`联合使用  
>std::bind2nd( )  作用: 把所需要的其中一个参数固定  
>transform(首地址，结束地址(一般是*首地址+N*)，结果，std::bind2nd( 函数对象<类型名字>( ) ,第二个运算对象 ))  
>>计算方法  
结果1 = 第一运算对象首地址   op(函数对象) 第二个运算对象  指向的数据  
结果2 = 第一运算对象首地址+1 op(函数对象) 第二个运算对象  指向的数据  
...  
结果N = 第一运算对象首地址+N op(函数对象) 第二个运算对象  指向的数据  

```cpp
// modulus example
#include <iostream>     // std::cout
#include <functional>   // std::modulus, std::bind2nd
#include <algorithm>    // std::transform

int main ( ) {
  int numbers[]={1,2,3,4,5};
  int remainders[5];
  std::transform (numbers, numbers+5, remainders, std::bind2nd(std::modulus<int>( ),2));
  for (int i=0; i<5; i++)
    std::cout << numbers[i] << " is " << (remainders[i]==0?"even":"odd") << '\n';
  return 0;
}
```
>结果
>>1 is odd  
2 is even  
3 is odd  
4 is even  
5 is odd  

4. ### 范例  
`std::mismatch `范例1  
>std::mismatch( )  作用: 检索是否匹配，返回一个`pair`  
>std::mismatch(第一比较对象首地址，结束地址(一般是*首地址+N*)，第二比较对象首地址，函数对象<类型名字>( ) )  
>>计算方法  
依次对两个对象比较,比较方法如下    
第一对象1 op 第二对象1  
第一对象2 op 第二对象2  
...  
第一对象N op 第二对象N  
* 当全部相等时，返回的结果pair的first部分和第一对象结束地址相等，second结果和第二比较对象对应地址相等  
 `p.first == std::end(第一个对象) && p.second == std::end(第二个对象)`  *仅当两对象长度一致时*  
 实际使用中可仅 `p.first == std::end(第一个对象)`
* 若两比较对象有不符合传入函数对象操作时，**mismatch立即停止**，返回的结果pair的first部分和第一对象相应位置相等，second结果和第二比较对象对应地址相等  
 `*p.first == 第一比较对象n位 && p.second == 第二个对象对应的n位`
  
```cpp
#include <iostream>     // std::cout
#include <utility>      // std::pair
#include <functional>   // std::equal_to
#include <algorithm>    // std::mismatch

int main() {
	std::pair<int*, int*> ptiter;
	int foo[] = { 10,20,30,40,50 };
	int bar[] = { 10,20,40,80,160 };
	ptiter = std::mismatch(foo, foo + 5, bar, std::equal_to<int>());
	std::cout << "First mismatching pair is: " << *ptiter.first;
	std::cout << " and " << *ptiter.second << '\n';

	std::pair<int*, int*> ptiter1;
	int foo1[] = { 10,20,30,40,50 };
	int bar1[] = { 10,20,30,40,50 };
	ptiter1 = std::mismatch(foo1, foo1 + 5, bar1, std::equal_to<int>());
	if (*ptiter1.first == *(foo1 + 5) || *ptiter1.second == *(bar1 + 5))
	{
		std::cout << "first is equal to second" << std::endl;
	}
	else
	{

		std::cout << "First mismatching pair is: " << *ptiter1.first;
		std::cout << " and " << *ptiter1.second << '\n';
	}

	return 0;
}
```
>结果
>>First mismatching pair is: 30 and 40  
first is equal to second  

`std::mismatch `范例2  
```cpp
#include <iostream>
#include <algorithm>
#include <functional>
#include <vector>

namespace ClassFoo {
	void Mismatch_2() {
		std::vector<int> BigVector = { 8, 23, 5, 6, 7, 29, 0, 5, 6, 7, 1, 1 };
		std::vector<int> SmallVector = { 8, 23, 5, 6, 3, 29, 0, 13, 6, 7, 1, 0 };
		// 等价比较
		auto p = std::mismatch(
			std::begin(BigVector),
			std::end(BigVector),
			std::begin(SmallVector),
			std::equal_to<int>()
		);
		if (p.first != std::end(BigVector)) {
			std::cout << "找到: " << *p.first << '\n';
			std::cout << "前一个元素是: " << *(p.first - 1) << '\n';
		}
		// 自定谓词比较
		p = std::mismatch(
			std::begin(BigVector),
			std::end(BigVector),
			std::begin(SmallVector),
			[](int &m, int &n) { return m == n; });
		if (p.first != std::end(BigVector)) {
			std::cout << "找到: " << *p.first << '\n';
			std::cout << "前一个元素是: " << *(p.first - 1) << '\n';
		}
	}
}
int main()
{
	ClassFoo::Mismatch_2();
	return 0;
}
```
>结果
>>找到: 7
前一个元素是: 6
找到: 7
前一个元素是: 6
