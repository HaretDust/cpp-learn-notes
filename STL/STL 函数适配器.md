STL 函数适配器
===
> `#include <functional>`

[函数对象](http://en.cppreference.com/w/cpp/utility/functional)
>cppreference 中的最新条款
***
1. 绑定适配器
    ---
     `bind1st`  `bind2nd` *将在c++17中弃用*  
    >将n元函数对象的指定参数绑定为一个常数，得到n-1元函数对象  
    
    c11标准中建议使用 `std::bind`
    >将 j 元函数对象的指定参数绑定为 k 元对象，j 和 k 均 >= 0 ， 并且可以绑定和传入多个函数对象  
    >**形参和实参数目，顺序都可不同**  
    >**绑定时使用变量，后续调用时就一直是绑定时的值**，仅当未使用`std::ref, std::cref`时  
    
2. 组合适配器 
    ---
    `not1`  `not2`
    >将指定谓词的结果取反    
    >not几 就是针对对几元的  

3. 函数指针适配器  
    ---
     `ptr_fun`  *将在c++17中弃用*  
    >将一般函数指针转换为函数对象，使之能够作为其它函数适配器的输入。   
    >在进行参数绑定或其他转换的时候，通常需要函数对象的类型信息，例如bind1st和bind2nd要求函数对象必须继承于binary_function类型。但如果传入的是函数指针形式的函数对象，则无法获得函数对象的类型信息。

    c++11标准中建议使用 `std::function` 和 `std::ref `  
    >存储，复制，调用任何可调用的目标函数，lambda λ 表达式，bind 表达式，或者其他函数对象，以及指向成员函数的指针和数据成员的指针  
    
4. 成员函数适配器  
    ---
     `ptr_fun` `ptrfun_ref` *将在c++17中弃用*   
    >对成员函数指针使用，把n元成员函数适配为n + 1元函数对象，该函数对象的第一个参数为调用该成员函数时的目的对象  
    >也就是需要将“object->method()”转为“method(object)”形式。将“object->method(arg1)”转为二元函数“method(object, arg1)”。  
    
    c++11标准中建议使用`std::mem_fn` 和 `std::bind`  **个人觉得使用`std::bind`和`std::function`足以解决了**
    >存储并执行成员函数和成员对象  
    >将成员函数（Member function）转化成函数对象（指针（Pointer）版）  
    >c++11中谨慎使用men_fn，可能会出现问题 *在c++14中被改正了，因此语法上稍有区别*  

***
用法
---

#### Polymorphic function wrappers 多态函数封装器
- std::function
  > std::function<函数参数列表,包括返回值和形参> f1 = 函数名;
  >>函数参数列表和实质上是一种函数签名，只不过此处只写出了*形参*和*返回值*而已
  >>内容格式 去掉了函数名字和参数名字，仅保留返回值类型，括号，形参类型
  
  > std::function<函数参数列表> f2 = 匿名函数定义;
  
  > std::function<函数参数列表> f3 = std::bind(函数名, 参数);
  >>和bind一起联用
  
  > std::function<函数参数列表> f4 = &类和结构名字::成员函数;
  >>成员函数
  
  >std::function<函数参数列表> f5 = &类和结构名字::数据名;
  >>data member accessor
  
  >using std::placeholders::_1;  //_1 占位符，形参参数列表
  >std::function<函数参数列表> f6 = std::bind( &类和结构名字::成员函数, 类和结构的实例名字, _1 );
  >>成员函数和对象，调用 member function and object

  >using std::placeholders::_1;  
  >std::function<函数参数列表> f7 = std::bind( &类和结构名字::成员函数, &类和结构的实例名字, _1 );  
  >>成员函数和对象指针，调用 member function and object ptr

  >std::function<函数参数列表> f8 = 函数名();
  >>直接调用函数对象 function object

#### 绑定相关  
- std::bind2nd  
  >bind2nd(函数对象<类型名字>(),第二参数)  
  `bind2nd(greater<int>(), 40)`  
- std::bind
  >auto f = std::bind(函数名, _2, _1, 42, std::cref(n), n);  
  >> _1,_2... 是*参数表*，对应实际使用时的参数，_1表示调用时的第一个参数，_2表示第二个，以此类推    
  >> *参数表* 可以使用多次，允许存在多个 `_1,_2..`
  >> `变量` 直接显示调用变量，不使用`std::erf`和`std::cerf`，使用时实际值为绑定时的值  
  >> `std::cerf(变量) ` 和 `std::erf(变量)` 表示实际使用时，参数位置对应的值为对应变量在函数使用时的值  
  
  >auto f2 = std::bind(函数名, _3, std::bind(g, _3), _3, 4, 5);
  >>bind可以嵌套使用
  
  >auto f3 = std::bind(&类或者结构名::成员函数, &类或者结构的实例的名字, 95, _1);
  >>bind可以bind成员函数和成员对象
  >>此处的实例是 pointer to member function  
  
  >auto f4 = std::bind(&类或者结构名::数据, _1);  
  >>bind可以bind类或者结构的数据
  
#### Negators 谓词结果转换
- not1(一元谓词)
  >not1( `bind2nd(greater<int>(), 15)` )
  >>对一元谓词结果取反
  
- not2（二元谓词）
  >bind2nd( not2( `greater<int>() `), 15))
  >>对二元谓词结果取反
  
***
