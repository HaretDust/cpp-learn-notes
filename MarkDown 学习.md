H1(大标题)
====

H2(小标题)
----

# 1
## 2
### 3
#### 4
##### 5
引用
>引用  
1
>>嵌套引用  


* 无序列表  
+  
-  

1. ### 有序列表
3. #### 后续数字不影响实际生成的序号  
4. 第一层
    1.  第二层  
        2.第三层  
        + 无序列表嵌套

5\. 可以使用：数字\. 来取消显示为列表  



传说中的分割线  


---

*  * *
_ _   _
123

普通链接：
[Google](http://www.google.com/)  
指向本地文件的链接：
[icon.png](./images/icon.png)  
包含 'title' 的链接:
[Google](http://www.google.com/ "鼠标移上去显示的文字")  
[Google][link]    

[link]: http://www.google.com/ "Google"  
这个link前面得空一行
[Bing][]

[Bing]: http://bing.com/ "显示 "

每一行中的代码 `#include <stdio.h>`  
插入代码块（简单的那种，不支持语法高亮）  

    // Tab开头
    Markdown()[;/]
    // 四个空格开头  
多行代码，代码高亮
```cpp
window.addEventListener('load', function() {
  console.log('window loaded');
});
```
***加粗和斜体***  
___下划线也行___  
*一个是斜体*  
__两个是加粗__  
这就是 ~~删除线~~  
表格

name | age
---- | ---
LearnShare | 12
Mike |  32

:--- 代表左对齐  
:--: 代表居中对齐  
---: 代表右对齐  

  | left | center | right |  
  | :--- | :----: | ----: |  
  | aaaa | bbbbbb | ccccc |  
  | a    | b      | c     |
任务列表    
- [ ] Eat
- [x] Code
  - [x] HTML
  - [x] CSS
  - [x] JavaScript
- [ ] Sleep
