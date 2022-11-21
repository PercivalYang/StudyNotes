${toc}
# Initialize
## Uniform initialize
- 使用花括号进行initialize，适用于所有type，可以在declaration时进行initialize
- e.g.：
  ```cpp
  std::vector<int> vec{1,2,3};   // 前面declaration，后面initialize
  ```
## `auto`
- 让complier推断出varation的类型
- 什么时候用auto？
  - 当type名称过于长(为了使代码简洁)
- 不要过度使用auto
  ```cpp
  int a, b, c   // complie right
  auto a, b, c // complie error!
  ```
  - 过度使用容易使代码less clear
## Initialize from contents of struct
![fig1](/i/1739353b-bd8f-4c98-a3bc-792639998949.jpg)

# Reference
- 来看下面的例子，区分Reference和Copy：
![](/i/9c4074a9-1a5a-4fd5-9c0f-a1618522c24f.jpg)
- Reference to variables
![](/i/48ee26a0-7d94-4563-8b80-201ff612b143.jpg)
- 常见的Reference bug：
  - 在函数内部的copy：
```cpp
void shift(vector<std::pair<int, int>>& nums) {
    for (auto [num1, num2]: nums) {   // 这里等于new initialize了num1,num2将pair类型的nums中的first和second copy出来
      num1++;
      num2++;
    }
}
```
  - Fix：
```cpp
for (auto& [num1, num2]: nums)  //加上指针，将copy改为Reference
```
- Reference只能针对变量创建，例如下面的code是非法的：
```cpp
int& thisWontWork = 5 // Error!
```
- Reference的一些用法：
  ![](https://gitee.com/percivalyang/images/raw/master/images/202211131948280.jpg)
  - 先看函数返回的是整数的Reference，函数内部返回的则是vec[0]，结合下方`front(number)`后number的第一个元素被修改为4。
  - **TODO：这里漏了东西，有时间把他补上！！(课程地址：https://github.com/ChristosBouronikos/typora-nord-theme.git*)*

# Memory
- 例如`array`这样很占用内存的数据，在使用完后为了让它不再占用内存，可以使用：
```cpp
delete[] my_int_array;
```



# Pointer
  - 假设有指针`ptr`指向一个目标的地址，当我们想获取目标中的member variable `var`时，格式如下：
  ```*ptr.var == ptr->var```，两种方式等效
  - **`int *` is the type of an int array variable**
    ```cpp
    int *my_int_array = new int[10] //my_int_array is the type of int array variable
    int first_element = my_int_array[0]
    ```
  - `nullptr` 代表空指针
  - `void*`
  - Dereference(解引用): 假设ptr是个指针，`*ptr`即解引用获取该指针存储的地址所指向的数据
  - 通过Pointer来Ask memory：
  ```cpp
    // char是一个byte
    char* buffer = new char[8] // buffer向memory申请一组8byte的空间
    // buffer是指针，指向该地址的开头
  ```



# Class
  - Difference between `class` and `struct`:
    - `struct`就像是一堆暴露在外的数据集合，并且调用前要先Initialize `struct` variables；而`class`有着很强的encapsulations barrier(private element)，仅提供给用户便捷的API供使用
## Constructor and Destructor
  - Constructor还可以通过Initializer list 来construction,例如：
  ```cpp
  Student::Student(): name(""), age{0}, state{""} {} //注意()后的冒号，和最后的{}
  Student::Student(string name, int age, string state):{
      name{name}, age{age}, state{state} {}
      }
  ```
  - Destructor是为了减少内存占用，在class中的member out of scope时将它从内存中delete
    - 删除变量 `delete[] my_var`
    - 在class内的形式为`~ClassName()`
## Template class
  - Template class不仅有Java中的Generic type一样的功能，同时还具有其他的用法
      - 先来看看generic type 的用法
      ```cpp
      template<typename T> class my_vector<T>{};
      ```
  - 另一种用法，定义一个class`MyArray`如下：
  ```cpp
    template<typename T, int N>
    class MyArray{
        private:
        // T 为generic type的用法
            T m_Array[N]; // 可以用template中的元素来初始化class
    }
    int main(){
        MyArray<5> array; // Declaration时初始化MyArray大小
    }
  ```
   - 另外`template`在程式中，只有当call它时才会发送code，即: "Template don't emit code until instantiated"

# Some details
## g++
- `g++ -c vector.cpp main.cpp`: complie vector.cpp和main.cpp，其中`-c`是complie的简写
- `g++ vector.o main.o -o output`: 链接多个文件生成一个可执行的binary file`output`
## Header Files
- `Interface`常放在header files中，而其对应的`Implements`存放于cpp files中。在main.cpp中习惯于包含要用到的header files,例如`#include "vector.h"`。Q: **main.cpp怎么找到interface对应的implements？**
- A: 在Header Files 中包含Interface的implements文件，如在`vector.h`中`#include "vector.cpp"`
## size_t

# Scope 
- Scope(作用域),在前面Class中的Destructor中有提到：  
> 在class中的member out of scope时将它从内存中delete
- Scope的范围由花括号{}规定，变量名的有效范围**从名字的声明开始，到名字所在的Scope结束为止**，因此到这我们就可以理解前面的out of scope是什么意思了，举个例子：
    ```cpp
    int main(){
        int num = 3;
        std::cout << num << std::endl;
        return 0;
    }
    ```
    - 程式中`num`的Scope就是在`main`的花括号内，而`main`则是一个global scope的名字，即在整个程式内`main`都是有效的。
## Nested Scope
  - 来看个例子：
  ```cpp
  int reused = 32; // 全局变量
  int main(){
    std::cout << reused << std::endl;
    int reused = 0; // 新建局部变量，覆盖了全局变量
    std::cout << reused << std::endl;
    // 如果此时想要访问全局变量的reused
    std::cout << ::reused << std::endl;
  }
  ```
  - 可以看出对全局变量访问的形式是`::reused`，因为全局Scope本身没有名字，因此`::`左边为空。

