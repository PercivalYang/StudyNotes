## Exceptions
- Java的error-handling框架定义两个class来表示abnormal software events：
    - `Error class`：表示严重的问题，
    - `Exception class`: 相比Error较轻的问题
- `Error`和`Exception`都继承自Abstract class`throwable`
- `Exception`:
    - `Unchecked Exception`: Unknown to compiler, only known at runtime, are used when when we expect that the caller of the method cannot recover from the exception
    - `Checked Exception`: known to the compiler
        - If we are calling a method that potentially throws a checked exception, it must be handled (or we will get an error from the compiler)
        - are used when we expect that the caller of the method can recover from the exception

## Exception Handler
- `try {}catch{}`
    - 处理程序抛出的exception,并保证程序能够继续运行下去；
    - 例如:
    ```java
    try {
        System.out.println(new Phone("iPhone", numbers[i]));
    }catch (IllegalArgumentException ex){  // ex等同于设置别名
        System.out.println(ex.getLocalizedMessage()); // 通过getLocalizedMessage()获取Exception内的文本信息
    }
    ```
- `finnaly()`:
    - 在`finnaly(){}` block内的程序会一直执行，即使遇到导致程序终止的Error

## Enums
- Syntax example:
    ```java
    enum Stoplight {
      RED,
      YELLOW,
      GREEN
    }
    ```

## Scanner 
- `Scanner`可以read并解析简单的文本信息:
    - 可以将primitive types和String types解析为tokens;
    - 默认用**空格**代表每个word的delimitate，同时也可用正则表达式;
    - 可以从String, System.in, files等不同数据源读取并解析
- Methods:
    - `.next()`: 返回当前第一个word;
    - `.nextline()`：返回当前第一行.
    - `.hasnext()`: check if it is safe to call `.next()`
- Syntax:
    - from `System.in`
    ```java
    Scanner scanner = new Scanner(System.in);
    ```
    - 

## Date
The Date class represents a specific instance in time. We can instantiate a new Date object like so:
```java
    Date date = new Date(); // 默认获取当前时间
```

## Calendar
The Calendar class is an **abstract class** that provides methods for manipulating date and time. The basic syntax for instantiating a new Calendar object looks like this:
```java
    Calendar calendar = Calendar.getInstance();
    calendar.setTime(date); // 获取当前时间
    System.out.println(calendar.getTime()); // 打印当前时间
    calendar.set(2031, 02, 02); // 设置时间
```
- 修改时间格式：
    ```java
    SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
    System.out.println(format.format(date));
    ```
如果想继续了解关于date相关的，可以点这个[链接](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)

## RegEX
- Three classes in RegEX packages:
    - Pattern
    - Matcher
    - PatternSyntaxException  
- To use RegEx in Java, we have to do two main theings:
    - Create a Pattern based on a specialized syntax  
    - Use the Matcher to determine if the pattern exists in the String provided
- Helpful resources
    - [official Java docs on regular expression syntax](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html)
    - [web of trying RegEx](https://regexr.com)  
- Example:
    - 定义匹配邮箱的正则表达式的步骤：  
        1. `String emailRegx = "^(.+)@(.+).(.+)"`
        2. `Pattern pattern = Pattern.compile(emailRegex)`
        3. `Matcher matcher = pattern.matcher("jeff@example.com")`
    - 顺序：`String`->`Pattern`->`Matcher`
