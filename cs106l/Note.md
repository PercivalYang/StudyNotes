# Stream
## Output Stream
- type: `std::ostream`
- 只能通过`<<`传送数据
  - 将数据转换为`string`发送给`stream`
```cpp
cout << 5 << endl // 将数字5转换为字符串 "5"，发送给stream输出到console
```
- `std::ofstream`: 将Output stream连接到文件，例如：
  ```cpp
  std::ofstream out("out.txt")
  std::cout << 5 << std::endl // 5转换为"5"，输入到out.txt文件中
  ```
## Output File Stream
- type: `std::ofstream` (注意和上面的`std::ostream`区分！！)
  - Note：
  ```cpp
  void writeToOut(std::ostream& anyOutStream, int number){
       // std::ostream 改为 std::ofstream 的话，传入cout会发生TypeError
       // 函数目的是输出数据给anyOutStream
       anyOutStream << number << endl;
  }
  int main(){
       std::ofstream fileOut("out.txt");
       int myNum = 42;
       writeToOut(std::cout, myNum);
       writeToOut(fileOut, myNum);
  ```

## Input Stream
- type: `std::istream`
- `std::cin`:
  - 从命令行接收输入，遇到enter结束；
  - 每个`>>`遇到whiterspace时停止，例如`std::cin >> a >> b >> c`
## Input File Stream
- type: `std::ifstream`
- 使用任何Input Stream前都要先Initialize
## Othre Stream
- StringStrems
  - Input: `std::istringstream`
  - Output: `std::ostringstream`
    - read from it word/type by word/type
    - e.g.：
      ``` cpp
      ...
      {
        std::ostringstream formatter;
        formatter << name << ", age " << age;
        if (loveCpp) formatter << ", rocks.";
        else formatter << " could be better";
        return formatter.str();
      }
      ```
    - 感觉有点像StringBuffer和StringBuilder？
  - Same as i/ostreams
- 
## std::getline(istream& is, string& str, char delim)
- 参数解释：
  - `is`: Stream to read from;
  - `str`: Place where input from stream is stored
  - `delim`: When to stop reading (default: `\n`)
- 工作步骤：
  - 清空`str`；
  - 从`is`中提取chars并保存到`str`中直到以下条件发生：
    1. `is`接受到文件结束的信号，给`is`设置EOF bit(可通过`is.eof()`check)；
    2. 下一个`is`中的char是`delim`，提取出来但不store；
    3. `str`到了max size，给`is`设置FAIL bit(可通过`is.fail()`check)
  - 如果`is`提取不到任何char，设置FAIL bit
- `>>`和enhance for loop每次只提取single world or type，如果要读取一整行则用`std::getline`
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

# Reference(引用)
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
