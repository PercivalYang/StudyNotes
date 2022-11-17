# const
- 当声明一个variable为`const`类型时，表示该variable在initialize后就无法被更改
- 不能将非`const`的Reference指向一个`const` variable，例如：
    ```cpp
    const std::vector<int> c_vec{7,8};
    std::vector<int>& r_vec = c_vec // Error!
    ```
- subtleties(细节处):
    - 非`const`的`auto`变量，可以copy`const`的变量(对Reference同理)，如下：
      ```cpp
      const std::vector<int> c_vec{2,3}
      auto copy = c_vec // copy不是const类型
      const auto copy = c_vec // copy是const类型
      ```
    - 其实上述思想是用常量`c_vec`去初始化另一个对象，另一个对象是否是`const`无关紧要
- `const`的引用：
    - 对于已经声明的常量，如`const int ci = 1024`，可以用一个`const int&`(及常量引用)的方式引用对象，但不能用非常量的方式去引用，如`int& r = ci // Error`，因为非常量的引用可以改变被引用的变量，这违反了`const`的约定。
    - 
