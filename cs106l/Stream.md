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
## Other Stream
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
